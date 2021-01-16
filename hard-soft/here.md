# Using "here" or "channel"

Let's talk about slack and `@here` & `@channel` keywords.
Those 2 words are useful and dangerous at the same time.


> With great power comes great responsibility

Uncle Ben

## How it works?

It'll basically notify everyone in the channel. Literally everyone

## How to use it right?

With common sense. if channel has 8 members and they all should be notified - don't hesitate. In all other cases - don't use it unless you know what you are doing.

## How to use it wrong?

Almost all remaining use-cases.

Please ask yourself few questions:

* Do you really need to jnotify everyone?
* Is it that important?
  * Remember that stuff important to you doesn't have to be important for someone else
* How big is audience?

# Why such a fuss

Because people tent too notify to wide audience. This is a problem due to a cost & co-existence.

Assuming you have notified 200 engineers on a single channel and 60% of them were focused on real work. What has happen? 120 people has lost focus to read your message. Context switching has just happen. 120 souls has to stop a proces of thinking, debugging or analysis to check why they were directly notified.

Getting back to the original thinking varies from 2 to 30 min(depends on task complexity) once reading message is fast(let's say 10s).

Please imaginge it's 2:52PM.

10% of the engineers have a meeting at 3PM. They have 8 minutes to next meeting. Will they even try to get back to debugging or will they take another cup of the tea?

let's say 2% are working on complicated task and disturb has 20 min impact.

Now let's do some math:
```
200*(0.15+0.6*2+0.1*6+0.02*20) = 470m
470m ~= 1d
```
🎉 you just burned whole day 🎉

what if there is 2000 people on a channel? Impact is 10x biggers - fortnightly salary was just burned.

---

So don't use `@here`