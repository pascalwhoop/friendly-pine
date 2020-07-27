---
title: 5 IT tools and skills that save seconds every minute
date: '2019-04-08T20:21:49.915Z'
excerpt: >-
  I’ve recently learned yet another tool that I now, only 1 month later, cannot
  believe to have ever lived without. Over the last 7 years I…
thumb_img_path: >-
  images/5-IT-tools-and-skills-that-save-seconds-every-minute/1*t3A-s2eExektSb4S48r0FQ.jpeg
layout: post
---
![](/images/5-IT-tools-and-skills-that-save-seconds-every-minute/1*t3A-s2eExektSb4S48r0FQ.jpeg)

I’ve recently learned yet another tool that I now, only 1 month later, cannot believe to have ever lived without. Over the last 7 years I moved from a tech enthusiastic high school kid to a software and now data engineer and I’ve met many great people that showed me their most valued tools. Looking back, I came to realize that there is a set of tools that shape my day to day work more than anything else. I will share these with you below, but first let me elaborate on my view of what differentiates some people from others:

#### What we do to improve ourselves

We all try to become better at what we do. When I was young, I trained dribbling to be a better basketball player. Kids practice multiplication to become better at math. As an IT professional, learning something new every few weeks is absolutely normal. This sometimes makes it hard to take time and *practice skills.* There is so much to learn day by day that it’s tough to lean back and look at what we do most of our day. Below is a way of thinking about the choices we take every day or only once a lifetime and they usually correlate with the effort it takes to reach the goal that we set ourselves.

![](/images/5-IT-tools-and-skills-that-save-seconds-every-minute/1*xy_3GeuKtPm0_QJlOul6vw.png)

This article focuses on the left hand side. There are plenty of resources that teach you how to live but I don’t want to do that. What I want to do is give you a few tips that save you a few seconds every minute and keep you in *in the flow.* These tips are not an absolute truth by any means. They are just tools that I now see myself wishing others would use when I watch colleagues and customers work with their machines while I sit next to them waiting to see them execute the commands that we already know in our mind but that take too long to translate into execution cycles in the CPU.

### My Top 5

#### 1\. Vim. Seriously

Learn it. Just do it. [What is Vim?](https://medium.com/@fay_jai/what-is-vim-and-why-use-vim-54c67ce3c18e) Others have answered that far better than me. My take on Vim: If your fingers were you and your keyboard were your desk then any other editor is the equivalent of keeping your printer in the bedroom, the monitor in the kitchen, the paper in the living room and the pencils in the basement. Vim on the other hand brings the actions you do most of the time closest to your fingertips. Literally. Delete a line? `dd` delete the current and next 4 lines? `d4j` Want to perform a certain chain of keystrokes again and again? `qq <perform sequence of strokes> q` followed by `n@q` n being the number of times to repeat the sequence. Oh and of course Vim is everywhere. My file manager, IntelliJ, tmux, even browsers, Gmail and GitHub follow the Vi(m) navigation patterns and other keyboard shortcuts. Once you know the basic commands, you will keep recognizing shortcuts in a plethora of tools.

#### 2\. fzf — a command line fuzzy finder

<iframe src="https://giphy.com/embed/L3bgGyWg9RCLfQg6Ak/twitter/iframe" width="435" height="237" frameborder="0" scrolling="no"></iframe>

fzf allows you to quickly search through your history of shell commands or through a tree of files at your current location. Do you want to move a file from somewhere to somewhere else? `cp Ctrl+t <some pattern> Enter Ctrl+t <some-other pattern> Enter Enter` This may be especially nice for those that work with java projects where files are often hidden in a number of package folders, 5+ levels lower. It’s also extremely nice for recovering an SSH command or re-executing code from a few days ago. Sure, you can write scripts for all that stuff but projects change, often every few weeks and then you end up with a bunch of scripts lying around that you never use again.

#### 3\. Tiling / i3 Window Manager

The tiling WM idea is simple: Alt-tabbing your way through life is tedious and just 5% cooler than your grandma’s way of clicking her way through the digital world with the mouse. Tiling always makes use of the entire screen estate at your disposal. Think about it. Your use cases will usually fall into one of three categories:

*   A single app to focus on at a time
*   several apps side by side
*   many powerful apps to switch between, each being full screen while using it

Tiling solves all of this with (1) being the default when only one window is open, (2) happening once you open a second app and (3) being possible with tabs/spaces/stacks.

![](/images/5-IT-tools-and-skills-that-save-seconds-every-minute/1*s3lACaxPMVwoRJf-_CwlKA.png)

<figcaption>Typical full screen use case:&nbsp;VMs</figcaption>

![](/images/5-IT-tools-and-skills-that-save-seconds-every-minute/1*SQEnJax-gqYKrsFpW9DL4Q.png)

<figcaption>side by side views: blog post&nbsp;writing</figcaption>

![](/images/5-IT-tools-and-skills-that-save-seconds-every-minute/1*70rJOD6t0DW6kJksRQkcZQ.png)

<figcaption>no distraction mode: full screen&nbsp;1</figcaption>

> OK nice, I can do this with windows too

Sure, you can. But I swear to you, I navigate around faster than you. `MOD+3` is my browser `MOD+4` my IDE `MOD+1` is my tech screen, `MOD+9`my notes, `MOD+O+[e]mail/[m]usic/[r]anger/[x]plorer` . You get the idea. It allows you to get your most common apps and tasks most closely to your fingertips. If you want to go down the rabbit hole, look at the video below for *many more ways to customize the linux DE.*

<iframe src="https://www.youtube.com/embed/YMiqNJaqvrk?feature=oembed" width="700" height="393" frameborder="0" scrolling="no"></iframe>

#### 4\. AceJump

[AceJump](https://plugins.jetbrains.com/plugin/7086-acejump) is great, especially before you learn Vi(m) bindings. What does it do? Well it follows the simple steps:

1.  Look where you want to go
2.  Hit shortcut
3.  Type what you see
4.  Type what you see
5.  enjoy new cursor position

This sounds tedious. But believe me it’s not. When you code, you start thinking “ah I have to modify the method X which is 28 lines below. I have to add a parameter. So your eyes wander down to where you need to change stuff and while they do, you hit your shortcut, because you know you’ll navigate down there. Because you decide that you will add a new first parameter, you type `(` and a (pair of) letters appears next to the opening bracket of the method parameter list. Type the letter(s) and you’re where you want to go. That’s at most 6 keystrokes to get anywhere on the screen, often less.

![](/images/5-IT-tools-and-skills-that-save-seconds-every-minute/0*XP8GGPiYMiS-WEaX.png)

#### 5\. Recognizing time wasted and learning from others

This is potentially the enabler for all of my above 4. I’d have many more tools to share (like `ranger`, `ack`, `autojump`, `rofi` or `pass`) but this one is most important. I didn’t recognize my wasted time in the shell until I saw a colleague blaze through his project directories. I didn’t realize my double-click mania in the file explorer until I discovered the ranger file manager. Don’t `cd` your way through life without noticing that you are crawling everywhere.

Step back, think about what you are doing and consider if those ~78 keys on your keyboard can’t get you there faster. Learning shortcuts is hard but you know what’s harder? Wasting 10% of your work time doing extremely mundane things like switching directories or navigating to a different location on the screen. You mastered `Ctrl+C/V`, maybe even `Shift+F6` for refactoring or `Shift+F10` for execute tests. Let’s all invest one or two hours a week into improving our second-to-second workflows so that they are better next week and even better the week after that. Then, you get to choose what to do with that extra time. Maybe only work 80% or use the extra time to learn more about AI and leapfrog the others in your environment or just use the time to catch up on your fitness and cooking skills.
