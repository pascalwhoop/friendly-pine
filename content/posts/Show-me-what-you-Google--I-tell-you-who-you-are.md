---
title: 'Show me what you Google, I tell you who you are'
date: '2018-01-19T17:01:15.861Z'
subtitle: Analyzing my own Google Search History from 2009–2016
excerpt: Analyzing my own Google Search History from 2009–2016
thumb_img_path: >-
  images/Show-me-what-you-Google--I-tell-you-who-you-are/1*rCw2K4-Gfw1TCBQQbczdXg.png
layout: post
---
![](/images/Show-me-what-you-Google--I-tell-you-who-you-are/1*rCw2K4-Gfw1TCBQQbczdXg.png)

About a year ago, I largely stopped using Google, replacing it with [duckduckgo,](http://duckduckgo.com/) a search engine that doesn’t track and as a nice bonus, always shows you the first answer on StackOverflow without navigating to SO.

Before I quit Google and many of its services however, I got all my data from their site using their [takeout service](http://takeout.google.com/). Let’s not get hung up on if you should or shouldn’t use Google services, I just decided I’d move to some other services for now.

### Data Importing and Cleaning

Yesterday, I found some time and started playing around with Python to analyze my search data. Each file you receive is a JSON for a number of search queries. I had about 80k queries spread over 30 files. Let’s look at how they are structured

    {  
        "event":[{  
                "query":{  
                    "id"[{"timestamp_usec":"1254341958933293"}],  
                    "query_text":"rollcall"}  
                },   
                ...  
                ]  
    }

So, to import them into Python, my new language of choice, you can do this:

<script src="https://gist.github.com/pascalwhoop/3520daa86aee14828a7d1eba8a6e528e.js"></script>

Now, you have a nice list of searches with tuples of (‘query’, ‘timestamp’)

### Search Times Analysis

Let’s look at the development of my search frequency.

<script src="https://gist.github.com/pascalwhoop/a5ab2cc634076433643435712699ef76.js"></script>

The result is a bar graph, similar to those that show the trading volume in stock graphs. As you can see, from 2009–2016, I obviously searched more and more. However, around 2/3rds through, there was a big dip. I don’t quiet remember why, but I am curious to find out what was the reason for that.

![](/images/Show-me-what-you-Google--I-tell-you-who-you-are/1*EMorpbyjzv4RKr1ZsSta4Q.png)

#### Day of Week Analysis

Let’s see how much I Googled throughout the week in relation to the different years.

<script src="https://gist.github.com/pascalwhoop/5ff4610e9a0ca1cf8611a4b8d06666c5.js"></script>

The result is quiet amusing. If my googling rate correlated to my curiosity about the world, I sure seem to loose interest in it throughout the week. I didn’t know I was a Monday person, but apparently I am!

![](/images/Show-me-what-you-Google--I-tell-you-who-you-are/1*bkI_lI0z3ECTEIbHZwoPRg.png)

#### Time of Day

To generate this for a time of the day, follow the same procedure as for the weekdays but instead of placing the data in a 2D array with year/dow we place it in a year/tod array. The result is again interesting:

![](/images/Show-me-what-you-Google--I-tell-you-who-you-are/1*FHED2Lwv_WsP7a5eSvGmBQ.png)

Obviously, I search more during the day. But what is going on in 2016? I must have really been hitting the keyboard at night, as all other data showed me stopping to use Google in mid 2016, leading to a about 50% reduction compared to 2015. But here, I am going strong at night. The reason is simple: I lived in Australia for 4 months but Google didn’t apply my actual time of the day. Rather they apply either my “home address” time or use some other form of fixing me to GMT+1.

* * *

### Part 2: Terms

Looking at the terms I searched for, there’s some clear winners: **Addresses, weather and exchange rates + technology.** There is also some other things like my previous bank institution (their website was rather hard to navigate) and the usuals.

<script src="https://gist.github.com/pascalwhoop/daeb6a12b08e5bd6309ed59fa681fbe8.js"></script>
    (‘euro dollar’, 210)  
    (‘weather’, 100)  
    (‘Wetter’, 84)  
    (‘angularjs’, 60)  
    (‘Erinnerung hinzufügen’, 43)  
    (‘köln’, 42)  
    (‘facebook’, 42)  
    (‘Im Langen Bruch, Köln’, 40)  
    (‘revealjs’, 36)  
    (‘ing diba’, 36)  
    (‘stargate universe’, 35)  
    (‘spotify’, 35)  
    (‘jquery mobile’, 34)  
    (‘opitz consulting’, 33)  
    (‘bootstrap’, 32)  
    (‘(Aktueller Standort) -> Im Langen Bruch, Köln’, 32)  
    (‘picasa’, 31)  
    (‘jquery’, 30)  
    (‘jdownloader’, 30)  
    (‘walmart’, 30)  
    (‘add reminder’, 30)  
    (‘uni köln’, 29)  
    (‘(Current Location) -> Im Langen Bruch, Köln’, 28)  
    (‘ubuntu’, 26)  
    (‘weather cologne’, 26)  
    (‘kino’, 25)  
    (‘pascal brokmeier’, 25)  
    (‘adf mobile’, 25)  
    (‘skype’, 25)  
    (‘xda transformer’, 24)

But this is not as good as you want it. It’s more interesting to look at single words, not full terms. So, if we split them up by word and then look at the frequency, it’s more telling

<script src="https://gist.github.com/pascalwhoop/bb4266d0b456ad35e002d4f04ec29f8d.js"></script>
    [('köln', 3780),  
     ('to', 3272),  
     ('in', 1820),  
     ('mac', 1662),  
     ('germany', 1572),  
     ('google', 1399),  
     ('android', 1328),  
     ('of', 1302),  
     ('2', 1282),  
     ('java', 1182),  
     ('bergisch', 1174),  
     ('gladbach', 1145),  
     ('location', 1107),  
     ('usa', 1065),  
     ('the', 1038),  
     ('current', 1024),  
     ('angular', 978),  
     ('1', 894),  
     ('from', 834),  
     ('cologne', 830),  
     ('javascript', 822),  
     ('not', 789),  
     ('ca', 769),  
     ('australia', 739),  
     ('9', 735),  
     ('sydney', 730),  
     ('a', 723),  
     ('for', 671),  
     ('windows', 661),  
     ('7', 658),  
     ('schubertstraße', 648),  
     ('3', 642),  
     ('uni', 628),  
     ('im', 617),  
     ('nsw', 616),  
     ('4', 609),  
     ('straße', 595),  
     ('c', 574),  
     ('euro', 559),  
     ('deutschland', 551)]

Again, locations, streetnames, and some trash. Why did I say it’s telling? because of the nice “wordcloud” tool. I know I know. Using a WordCloud is highly nothing special and it has nothing to do with science. But it’s pretty and I was tired at 11.30 and wanted to hit the bed.

<script src="https://gist.github.com/pascalwhoop/42cd70cca8600575591dd88b64f63539.js"></script>

![](/images/Show-me-what-you-Google--I-tell-you-who-you-are/1*rCw2K4-Gfw1TCBQQbczdXg.png)

There you have it. If you ever wondered how websites track your interests (I hope you haven’t wondered about that anymore for the last 3 years), it’s pretty clear. Even if I wasn’t logged into Google, A combination of about 10 searches + my location would probably be enough for them to reattach my unique ID. The terms above are averaged over 6 years. Of course you can get a much higher grade of detail about a person if you get daily updates. What are you looking to eat, researching a new gym, laptop, place to live, flight prices, …

If you want to do the same for yourself, get yourself a python environment and use [this file](https://gist.github.com/pascalwhoop/3c737256fc24366b1f20d21b2f3e890b) to get started. It includes all the code from above. Just place it in the same folder as your `Searches` folder.

And if you liked reading this, consider diving into my other latest post about reinforcement learning

[**Letting RF agents learn by learning from each other in competitive environments**  
*The introductory posting to my upcoming master thesis*medium.com](https://medium.com/curiouscaloo/my-thoughts-on-letting-rf-agents-learn-by-learning-from-each-other-in-competitive-environments-59c368b7c5fb "https://medium.com/curiouscaloo/my-thoughts-on-letting-rf-agents-learn-by-learning-from-each-other-in-competitive-environments-59c368b7c5fb")[](https://medium.com/curiouscaloo/my-thoughts-on-letting-rf-agents-learn-by-learning-from-each-other-in-competitive-environments-59c368b7c5fb)
