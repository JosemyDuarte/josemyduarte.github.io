---
layout: post
title: Lessons from My Journey with Flutter Development
subtitle: Metrics, Errors, and State Management
date: 2024-03-18
image: /assets/img/flutter-logo.jpg
share-img: /assets/img/flutter-logo.jpg
tags:
  - flutter
  - learning
published: true
---

I wanted to build something that I could proudly show to friends and say, "I built this 😎". So, I decided to try [Flutter](https://flutter.dev), a Dart framework that makes it easy to build apps for both iOS and Android. Instead of making a complicated app with lots of fancy features, I wanted to keep it simple. I made an app that uses your device's screen as a flashlight and allows you to change the colors and make it blink in patterns you record with simple taps on your screen.

At first, I was super excited! Flutter made it easy to make pretty buttons and all kinds of common UI components following established design principles. But then, I had to figure out how to organize the app. I'm not a design expert, so it was tricky deciding where to put things. I just did my best to keep it simple and easy to use, but it made me realize that even though I use all kinds of apps every day, it wasn't straightforward deciding how to design my app.

In a broad level, I know that intuitive UX is important, but what does that mean? It is a sum of different small decisions, like the type, color, and size of the different fonts used on your app together with the size, position, and color of your app's buttons, among other million things. Definitely, I learnt that UX isn't a trivial thing, but that wasn't the only problem I faced with this project...

Then came another challenge — keeping track of the settings changed by the user across different screens and updating the UI components accordingly so that the user can see their changes reflected across the app. This is called "state management". Luckily, Flutter has multiple ways to handle it and after a quick modification, I was able to "move" the state across the screens and the UI got updated out of the box by using the [Provider approach](https://docs.flutter.dev/data-and-backend/state-mgmt/simple), which was a nice surprise. I was expecting more struggle to make this work 😅.

Up to this point, I have learnt a couple of things:

1. Crafting beautiful UIs is an art and an important aspect to keep your app intuitive.
2. Flutter provides tools that simplify state management and synchronization of your UI components.

But then after multiple crashes while running the app on the simulator, I wondered how I would know if the app crashed on people's devices once I released it on the app store? I wouldn't be able to add breakpoints to debug or do any other type of deep investigation besides reading their review on the app store which wouldn't be a good one if the app just brings frustration to them.

I figured it would be nice to have a way for my app to report crashes together with the context in which they happened so that I could reproduce the situation and fix it as soon as it started happening.

A quick search on the topic and [Firebase Crashlytics](https://firebase.google.com/docs/crashlytics) popped up. It can be used for both iOS and Android and it has an SDK for Flutter so I didn't look further. It allows you to report all crashes and also add context to it, like custom fields from your app or information from the device itself, which proved to be very useful to understand the context in which your app crashed.

That led me to think on how I could iterate and improve my app. The more you know your audience and their needs, the better you can customize your product to better serve them, right? People usually use [Google Analytics](https://analytics.google.com/) on their websites with this motive. So I was pleased to know that you can use [Google Analytics with Firebase](https://firebase.google.com/docs/analytics) with Flutter really easily. Allowing me to add custom events with all the context I wanted to record. In my case for example, I wanted to know how frequently people would actually use different colors than the default one for their flashlight. That was just adding a custom event for that particular screen and adding the color picked as a custom field.

You might be wondering where the app is? Well, right in the middle of the development of this app, [Google changed the rules of their app store](https://techcrunch.com/2023/11/09/google-play-tightens-up-rules-for-android-app-developers-to-require-testing-increased-app-review/). Now, releasing your app on the store required not only paying [the developer's fee](https://support.google.com/googleplay/android-developer/answer/6112435?hl=en#zippy=%2Cstep-pay-registration-fee) but also getting 20 testers for your app before it can be officially released.

I understand Google's intention is to increase the quality of the apps published on their store, which is great, but since I already had the app on my phone, I felt I had already gotten what I wanted from this journey — being able to show my friends "I built this 😎". Not only that, but I also learned a lot while doing it, so I didn't feel the need to go through that process.