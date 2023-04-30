---
layout: post
title: How to shutdown a Go application gracefully 
subtitle: An approach to manage the lifecycle of a Go application
date: 2023-04-24
image: /assets/img/golang-logo.jpg
share-img: /assets/img/golang-logo.jpg
tags:
  - golang
  - examples
published: true
---

All this started by noticing that my Go applications showed an increased number of errors when I was shutting them down. 
After some investigation I found out that the problem was related to the lifecycle of the components of my application.
Why was this happening? Because I was not shutting down my application gracefully. Meaning that I was not giving the
opportunity to the components to finish their work before shutting down all of them. 

Let me explain this with an example.

## The problem

Let's say that we have a web server that is using a database. When we receive a request we need to query the database to get
the information that we need to return to the client. If we are not shutting down the application gracefully, we might
receive an error when we try to query the database because the database connection might be closed before we can use it.
Also, we might still have some requests inflight and all of them will fail if the database connection is closed.

## The solution

Easy, we need to give the opportunity to the components to finish their work before shutting down the application.
In the case of the web server, we need to stop receiving new requests and wait for the inflight requests to finish.
In the case of the database, we need to close the connection only after all the requests that are using it have finished.

I noticed a pattern here. I had to shutdown the components in the reverse order that I started them. Meaning, that I had
to shutdown the web server first and then the database. This way, I could make sure that the web server was not receiving
new requests and that all the inflight requests had finished before closing the database connection. So, I decided to create
a `Manager` struct that will manage the lifecycle of my application. This struct will have a `Start` method that will start
all the components of my application and a `Shutdown` method that will stop all the components of my application.

## The code

Let's see how this looks like in code. First, I defined a common interface for all the components of my application.

```go
type Component interface {
	// Name returns the identifier of the component (mainly for logging purposes)
	Name() string

	// Start is called in order to start the component.
	// The initReady channel should be closed when the component's initialisation has finished. 
	// Some starts are blocking so by closing the channel we signal the manager that it can move on to the next component.
	Start(initReady chan struct{}) error

	// Shutdown is called to stop and cleanup the component.
	// There is a limited time to do so which is controlled by the ctx.
	Shutdown(ctx context.Context) error

	// StartTimeout returns timeout within which the component is expected to be start
	StartTimeout() time.Duration
}
```

Then, I defined the `Manager` struct that will manage the lifecycle of my application. It allows to `Register` new components
and since it is important to shutdown the components in the reverse order that they were started, I decided to store them
in a slice.

```go
// Manager is responsible for managing all the component's lifecycle.
type Manager struct {
	components []Component
}

// Register registers a new component.
func (m *Manager) Register(component Component) {
	m.components = append(m.components, component)
}
```

The `Start` method will start all the registered components of the application. It will iterate over the slice of components and call
the `Start` method of each one of them. It will also wait for the `initReady` channel to be closed before moving on to the
next component. This way, we can make sure that the components are started in the correct order. If the `Start` method of a
component returns an error, the `Start` method of the `Manager` will publish the error in the `errCh` channel and return it. 

```go
func (m *Manager) Start() chan error {
	log.Printf("Starting %d components...\n", len(m.components))
	errCh := make(chan error, len(m.components))

	start := time.Now()

	// Start all the components in order.
	for _, c := range m.components {
		log.Printf("Starting component %s...\n", c.Name())

		// Create a channel to signal that the component's initialization is done.
		initReady := make(chan struct{})

		// Start the component.
		go func() {
			errCh <- c.Start(initReady)
		}()

		// Wait for the component's initialization to complete.
		select {
		case <-initReady:
			log.Printf("Component %s initialization completed\n", c.Name())
		case <-time.After(c.StartTimeout()):
			errCh <- StartTimeoutError{ComponentName: c.Name()}

			return errCh
		}

		// Check for errors during startup.
		select {
		case err := <-errCh:
			if err != nil {
				errCh <- fmt.Errorf("failed to start component %s: %w", c.Name(), err)

				return errCh
			}
		default:
			log.Printf("Component %s started successfully\n", c.Name())
		}
	}

	log.Printf("All components started in %v", time.Since(start))

	return errCh
}
```

The `Shutdown` method will stop all the registered components of the application. It will iterate over the slice of components in reverse order and call
the `Shutdown` method of each one of them and wait for it to finish before moving on to the next component. 
This way, we can make sure that the components are stopped in the correct order, and we can give them the
opportunity to finish their work. If the `Shutdown` method of a component returns an error, it will be added to 
the `errs` slice, and it will continue with the next component. All the errors will be returned at the end of the method in a single error.

```go
func (m *Manager) Shutdown(ctx context.Context, gracePeriod time.Duration) error {
	log.Printf("Shutting down %d components...\n", len(m.components))

	start := time.Now()

	var errs []error

	shutdownDone := make(chan struct{})
	go func() {
		// Start shutting down all the components in reverse order.
		for i := len(m.components) - 1; i >= 0; i-- {
			c := m.components[i]
			log.Printf("Shutting down component %s...\n", c.Name())

			// Check for errors during shutdown.
			if err := c.Shutdown(ctx); err != nil {
				errs = append(errs, fmt.Errorf("failed to shutdown component %s: %w", c.Name(), err))
			} else {
				log.Printf("Component %s shutdown successfully\n", c.Name())
			}
		}

		close(shutdownDone)
	}()

	// Wait for the components shutdown to complete or timeout.
	select {
	case <-shutdownDone:
		log.Printf("Shutdown finished in %v\n", time.Since(start))
	case <-time.After(gracePeriod):
		return ShutdownTimeoutError{TimeOut: gracePeriod}
	}

	// Check if there were any errors during shutdown.
	if len(errs) > 0 {
		return ShutdownError{Errors: errs}
	}

	log.Printf("Shutdown completed successfully\n")

	return nil
}
```

## The result

We end up with a simple way to control the lifecycle of the components of our application. No component should be 
stopped without finishing its work, and no component should be started before all the components that it depends on
have been started. We have complete control over the order in which the components are started and stopped.

Feel free to check out the full code on [GitHub](https://github.com/JosemyDuarte/ComponentManager). There are some tests and 
a simple example of how to use the `Manager` struct. 

Being honest, I don't know if this is the best way to manage the lifecycle of the components of a Golang application, or if I 
just reinvented the wheel. I would love to hear your thoughts on this. If you have any suggestions or improvements, you 
are more than welcome to open a pull request on the repository.


