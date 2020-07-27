---
title: 6 Reasons why fitness trackers are less useful than helpful
date: '2016-12-31T11:07:28.515Z'
excerpt: >-
  2016 has surely been a year of trackers. They track your sleep, steps, active
  minutes, remind you to move when you’re a couch potato or in…
thumb_img_path: >-
  images/6-Reasons-why-fitness-trackers-are-less-useful-than-helpful/1*Zz3vXESKO9mdusUNe5wMLA.jpeg
layout: post
---
![](/images/6-Reasons-why-fitness-trackers-are-less-useful-than-helpful/1*Zz3vXESKO9mdusUNe5wMLA.jpeg)

2016 has surely been a year of trackers. They track your sleep, steps, active minutes, remind you to move when you’re a couch potato or in the zone at your desk, track your heart rate and the list gets longer with every iteration.

The goal is simple: With all of our lives being more and more packed and tightly scheduled, fitness often falls short and they remind us to reprioritize our health. And for this they are good. We share us and our data with the company that we decided to go for and in return we get somewhat customized reminders to move our asses again when we lose track of it.

They are also for the fitness junkies who like to jump onto the data collection and self evaluation wagon or simply on the ‘my neighbors wife has one, too’ wagon. Here they help as well. Some data is always better than none and for high performance athletes and their trainers it helps to add another dimension to it.

Some doctors use the data to help their patients, or at least so it seems to the non-informed consumer. The [reality might be different](https://www.technologyreview.com/s/543716/your-doctor-doesnt-want-to-hear-about-your-fitness-tracker-data/). But there are also positive cases, those of data scientist professionals that know the actual worth of the data, the error rate and the applicability to make use of it by changing a kids life that suffers from diabetes (video below).

<iframe src="https://player.vimeo.com/video/81272562?portrait=0&amp;byline=0&amp;title=0" width="700" height="394" frameborder="0" scrolling="no"></iframe>

<figcaption>Vivienne Ming helping her son live with diabetes</figcaption>

However, having now given a non-overoptimistic view of what they CAN do, let me list some things that they DO NOT do or that they do but it’s just plain wrong. So here are

### 5 negative things to deal with when using a fitness tracker

#### 1\. Sick days

We all have them sooner or later. Americans take [8–10 sick days a year](https://www.bls.gov/news.release/ebs.t05.htm), might be we Europeans take more, maybe less. I just spent 3 days being nursed back to health at my girlfriends house after Christmas in the South of Italy. Every 2 hours my Jawbone Up 2 vibrated letting me know I’m not exercising enough. Thanks, it’s not like I chose this.

Its not the fault of the tracker, it can’t be aware of the viral infection that’s in your system. It uses a crude approximation (movement of one arm) to guess your whole physical situation. It is about to be wrong, a lot. But it helps me make my point. *It’s like tracking a cars kilometers by counting your gear shifts.*  My right arms movement surely is correlated to my activity but surely not with a factor close to 0.9 or higher.

#### 2\. Bedtime alerts

Fitness trackers all have a vibration alarm to remind you to go to bed. It is the replacement of our parents when we were little and they told us to go to bed. Mine is set to 23.30 by vibrating six long consecutive times. I’m sure there is a line of code in my trackers event logic that checks if I am already asleep before reminding me. But unlike my parents, it can’t *see* that I am asleep. Its sensors might be off. I might have just fallen asleep minutes ago and it didn’t catch that yet. Or maybe I’m on a bus and the vibrations are throwing its sensors off. Maybe I’m just moving a lot in my first hour of sleep.

But here is the difference to what it replaces: My parents didn’t rip open my bedrooms door at 23.30, then **yelling at me ‘GO TO BED’ six times in a row although I am already sleeping**. But my Jawbone did that (well it vibrates, it doesn’t yell \[yet?\]) repeatedly last week, while I was trying to recover from my flu. It vibrates so loudly even my girlfriend woke up, wondering what is going on. Again, lack of precision causes this. The simple solution to this problem could be to place more sensors on me and get a better picture of what is going on, but I’m not sure if this is the right way to go.

#### 3\. It logs steps when it shouldn’t (plane, bus, train)

This one is short. The same arguments apply as before. The tracker doesn’t have a full view of what is going on with my body, it just has a accelerometer to determine what I am doing and obviously that will be incorrect. It let me do 800 steps on a 3 hour flight without getting up once. I suppose it was a bumpy ride…

#### 4\. The motivation, despite all customization, is still generic

All companies, let it be fitbit, jawbone, garming, withings, polar and all the others, will try to lead you to believe its a customized experience just for you. But, despite their best efforts, it’s still clearly generic. I mean, I understand the approaches, I am a software developer myself. There are limited ways of scaling without losing customer experience. But do I really care that I have completed my first 50k steps with my Jawbone? I mean… do they think I’ll break out a victory dance because I completed 50k steps?

<iframe src="https://giphy.com/embed/Gf3fU0qPtI6uk/twitter/iframe" width="435" height="326" frameborder="0" scrolling="no"></iframe>

#### 5\. It tracks walking, running, biking, … sex?

I don’t know if it says more about me than about the technology but if it can track all the common sports, it sure can track sex. Think about your movements (please don’t get into it too much :-D) and how a machine learning algorithm can learn this pattern based on thousands of datasets. It’s probably even capable of differentiating different positions. Now you can claim you don’t care that they know how much you run because you run a lot or it benefits your insurance premiums (although that crap is only legal in the states anyways), but it sure bothers most people to think that some team of devs can see what positions are preferred by their customers, when and how often they do it , …….

The fact is: If men don’t take off their socks, it’s likely they don’t take off their tracker either. So chances are (even though the UI doesn’t tell you this) that the company tracks it anyways. Just because it can. And because it is interesting after all.

### Even more

There are many other things that need improvement. Like, why don’t they just go on the leg? There used to be a Kickstarter project called [FlyBit](https://www.shopstarter.com/p/522669502/flyfit-unique-ankle-tracker-for-fitness-cycling-an/) but I haven’t heard of it being out in the wild. I am a skater, but my skating is never registered. This is obvious, because my arms don’t move as systematically as they would need to, in order for an AI to learn it. My legs do however. Most exercises (except body workouts like crunches or pushups) have systematic leg movement.

Also, for the paranoid among us, why do they even send the data to some server? Why do I need a server to crunch my numbers? These algorithms can all be applied on my device. Download the neural net model instead and apply it to my data rather than uploading the data. I know its more convenient this way, having all the data in one place makes for a much simpler and faster development. But in a world of data leaks, maybe a customer security centric approach would be a welcome alternative.

Oh and one last thing, then I’ll stop whining: Where is the ‘export csv’ button? If its all about quantifying yourself then why don’t I get easy access to a broad range of analytical tools? If I want to dig in, I want to see steps per minute, accuracy, confidence intervals, error rates, data transaction times, … I don’t just want to see ‘7039 steps today, 9232 steps yesterday’. Who the f\*ck cares?

### Wrapping it up

To conclude I summarize the current state of trackers with a few statements:

1.  (+) They support us in repeatedly telling us to move our asses. That’s good
2.  (+) They are an evolutionary step forward for the quantified self type
3.  (−) They are unprecise
4.  (−) They track more than they should and share it with people they should not
5.  (−) They can be plain annoying and distracting (vibration, battery, writing, having a watch AND a tracker)

In the end I will keep wearing mine until I got where I wanted which is work a continuous workout into my daily routine so that it becomes part of my habits. For me it’s an alternative to asking my girlfriend to continuously nag me to move more. That’s it. And I actually love data analytics and technology in general so it can’t be that I am negatively biased. Maybe in a few years, AI, privacy standards and sensors will have evolved to make it MY personal assistant at last but for now, it’s just my personal annoyer.
