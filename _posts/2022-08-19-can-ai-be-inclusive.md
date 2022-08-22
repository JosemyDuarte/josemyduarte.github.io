---
layout: post
title: Can AI be racist?
subtitle: Exploring bias on AI image generation
date: 2022-08-19
image: /assets/img/midjourney_logo.png
share-img: /assets/img/midjourney_logo.png
tags:
  - ai
  - midjourney
  - image-generation
published: true
---

If AI painted a "beautiful warrior" would it paint a man or a woman? Would it be Caucasian or Asian? Let's figure it out. But first, can AI be racist?

## Can AI be racist?

Maybe? I would say not necessarily. It all depends on the data fed to train the model. If you train a model to identify bears and you only train it with polar bears, when you try to identify a panda most likely it will fail. Is not that the AI is failing on purpose or that it is racist, is just that wasn't trained for that particular case. I wouldn't say is the AI's fault. Reasons for that to happen? There it gets more complicated...

An AI could be biased for different reasons. This is not an exhaustive list:

### Training data lacks particular cases 

As in our previous example if your model is trained only over polar bears you can't expect it will perform well trying to identify a panda. In this case, it might be a conscious decision or bias of the engineers training the model.

### There is not enough data available

Imagine you want a model that identifies different languages just by speaking a sentence. You would need to train the model with different sentences with all possible accents of every language. For sure you will find more data available in English than in old Latin. So for some use cases is just hard (too expensive or just impossible) to get a diverse data set to train.

### Intentional use case of the model

If the AI goal is to decide who gets a mortgage in order to reduce the risk for the bank, then the model will have a bias against people with high credit records. That's the use case itself, it was tuned and trained to do it like that.

## Exploring AI image generation

In 2022 there has been many AI released allowing image generation from text ([Dall-e 2](https://openai.com/dall-e-2/), [Stable Diffusion](https://stability.ai/), [MidJourney](https://www.midjourney.com/)...). For this exploration, I'll provide the same text to MidJournery and Stable Diffusion and ask them to generate 4 variations. Let's explore how diverse the images generated are.

### Warrior, portrait, highly detailed

On this one, we find both men and women in the design of both AIs. That was a good start. Although is curious how both AI show aborigen traits. 

|                                          MidJourney                                           |                                       Stable Diffusion                                        |
| :-------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------: |
| <img src="/assets/img/warrior_portrait_highly_detailed_mj.png" alt="MJ warrior" width="300"/> | <img src="/assets/img/warrior_portrait_highly_detailed_sd.png" alt="SD warrior" width="300"/> |

### Beautiful warrior, portrait, highly detailed

Exactly the same sentence from the previous experiment, but with *Beautiful* at the beginning. That was enough to get rid of any masculinity in the design. 

|                                                    MidJourney                                                     |                                                 Stable Diffusion                                                  |
| :---------------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------: |
| <img src="/assets/img/beautiful_warrior_portrait_highly_detailed_mj.png" alt="MJ beautiful warrior" width="300"/> | <img src="/assets/img/beautiful_warrior_portrait_highly_detailed_sd.png" alt="SD beautiful warrior" width="300"/> |

### Portrait of a nurse, cinematic, realistic photo

In this case, we notice that even when I didn't ask for any particular sex we got only women on the design of both AIs.

|                                                MidJourney                                                |                                             Stable Diffusion                                             |
| :------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------: |
| <img src="/assets/img/portrait_of_a_nurse_cinematic_realistic_photo_mj.png" alt="MJ nurse" width="300"/> | <img src="/assets/img/portrait_of_a_nurse_cinematic_realistic_photo_sd.png" alt="SD nurse" width="300"/> |

### Portrait of a doctor, cinematic, realistic photo

In contrast with the previous experiment, when changed from *nurse* to *doctor* we get the opposite; only men in the design of both AIs.

|                                                 MidJourney                                                 |                                              Stable Diffusion                                              |
| :--------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------: |
| <img src="/assets/img/portrait_of_a_doctor_cinematic_realistic_photo_mj.png" alt="MJ doctor" width="300"/> | <img src="/assets/img/portrait_of_a_doctor_cinematic_realistic_photo_sd.png" alt="SD doctor" width="300"/> |

### Software Engineer portrait, highly detailed, realistic

This one is similar to the last one, with only men in both designs.

|                                                         MidJourney                                                          |                                                      Stable Diffusion                                                       |
| :-------------------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------: |
| <img src="/assets/img/software_engineer_portrait_highly_detailed_realistic_mj.png" alt="MJ software engineer" width="300"/> | <img src="/assets/img/software_engineer_portrait_highly_detailed_realistic_sd.png" alt="SD software engineer" width="300"/> |

## Comments

Needless to say, that is incredible how those AIs are able to understand the common language and come up with such designs. Not so long ago we thought that wouldn't be possible. The creation of art, music, poems, all of that was a thing of humans. How would it be possible for a machine without thoughts and feelings to be able to create art? Now every year is becoming harder to distinguish what is made by a machine and what is made by a human.

Is not easy to train an AI to be able to do what MidJourney or Dall-e 2 are capable of doing right now. As said before, you need a lot of training data and usually, that data is gathered automatically from what is found over the internet, which causes your model to be biased toward what is published which not necessarily is a reflection of real life. But the question is, how to train a model with data that is not available? I believe progression is better than perfection, so I'm happy to see these technologies available even if they are not perfect. I'm sure we will be moving in the right direction over the years.

