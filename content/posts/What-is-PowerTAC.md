---
title: What is PowerTAC?
date: '2018-05-09T00:00:00.000Z'
excerpt: >-
  Energy is a major challenge. We are currently using the futures resources and
  unless we drastically change our behavior, the world as we…
thumb_img_path: images/What-is-PowerTAC/1*Ozrj6N4P2dzIsmhHL9ykeA.jpeg
layout: post
---
![](/images/What-is-PowerTAC/1*Ozrj6N4P2dzIsmhHL9ykeA.jpeg)

Energy is a major challenge. We are currently using the futures resources and unless we drastically change our behavior, the world as we know it will be drastically different in 100 years. In every discussion about the costs of renewables, a common theme is the argument of economic rationality. Current energy sources are x% more effective wher x is usually a low 2 digit number. But it is often dismissed that without a proper change in our behavior, entire continents need to be restructured. Bangladesh, a comparatively small Asian country will have 25 million people who will have to adjust their life by 2050, 2100 can only be extrapolated. Developing countries may leapfrog nation-wide electric grid systems just as they have done with phone lines, going straight to mobile. But industrialized nations require a heavy adaptation of their infrastructure which was designed based on concepts from almost 100 years ago. Networks are going to be heavily distributed, with many suppliers and many consumers being able to store energy and use or release it later. PowerTAC simulates such as scenario by allowing the simulation of a grid market dynamics of a small city. It simulates 3 core markets: Tariff, wholesale and balancing.

![](/images/What-is-PowerTAC/1*Pn12ud07kkfPGY1yCcFPmw.png)

### Tariff market

The first market simulates the creation and adoption of tariffs in a end-user centered market.Brokers, the main actors in the simulation, compete for customers by offering tariffs on the market. Customers evaluate these tariffs and choose one they prefer. They are lazy and suffer limited rationality, just like most normal humans. There are a number of rules that govern the publishing of tariffs but basically, this market is rather idealistic and allows for many interesting tariff options. Rates may change prices based on the time of the day or on the usage per day. They may be adaptive, following the live market price. And they may offer customers to forfeit part of their usage for a strong discount. All these kinds of options allow a broker to balance its portfolio to allow for a decentralized balancing of the grid load. If the market prices are high, customers with variable pricing will tend to reduce their usage. If the portfolio is not balanced, curtailing of customers usage can help recreate an equilibrium. Customers have an incentive to behave economically and brokers have an incentive to precisely estimate their customers usage.

### Wholesale market

This market allows large actors (such as brokers and large-scale producers / consumers) to trade energy usage rights for the near future. The simulation offers a 24h horizon, letting brokers bid on each of the upcoming 24 hours. This means that at any given moment, 24 parallel auctions are performed. This is in line with common market mechanisms and it allows brokers to buy and sell any excessive or missing energy. Prices are calculated based on the supply and demand curves just like many known markets.

### Balancing market

The Balancing market is the last and final trading opportunity for agents and in the sense of the game is at t − 0 meaning that it occurs virtually in parallel to the consummation of energy. Any imbalance during this phase gets corrected for by the distribution utility (DU) which acts as a final balancer for the market. It imposes forced balancing of brokers with an imbalanced portfolio. Brokers with too much supply in their portfolio therefore receive very little reimbursement for it and those whose customers’ usage is higher than the estimated amount pay high prices for the additionally supplied energy. Brokers who have tariffs with economic control abilities can pass this capacity along to the DU who utilizes these capacities to correct the markets imbalances, charging customers’ storage devices if an oversupply is present or depleting their devices in the case of an under-supply. It is therefore economically beneficial for brokers to attract customers with such balancing capabilities since it offers a buffer capacity against the balancing costs otherwise incurred through the actions of the DU.

* * *

### The appeal

The simulation is appealing in many regards. It attacks a problem that is both current and significant. It offers the usage of current state-of-the-art algorithms from machine learning and AI to try and solve these problems. It attempts to avoid political stupidity by simulating various scenarios before acting them out. While the simulation is currently not at a scale where it could be seen as a major guide for evaluating arbitrary future market designs, it does offer the ability to simulate different constructs. Most of all it is a tool game design, offering policy makers and researchers alike the chance to see how AI driven actors will behave under various game designs in a future energy market.

But the simulation also has limits. It is currently only available in Java, lacks scalability and requires more intelligent components. The future will have intelligent agents in hundreds of locations and it should not be limited to simply simulating broker behavior. Researchers should be able to design intelligent customers, intelligent cars and household utilities. The market may be based on non-fully-connected grids (such as those often found in developing worlds), or it may offer a completely peer-to-peer based system based on modern technologies such as block-chains.

Despite its limitations, the appeal is obvious: Attack the biggest problems of our time with the most sophisticated algorithms to coordinate various actors in a market-based environment. See how different market designs and different agent behaviors impact the market and learn before the implementation is complete and changing it would be costly.

### Sources

*   About PowerTAC: [http://www.powertac.org/publications](http://www.powertac.org/publications)
*   Motivation: [https://samharris.org/podcasts/what-you-need-to-know-about-climate-change/](https://samharris.org/podcasts/what-you-need-to-know-about-climate-change/)
*   My GitHub workings: [http://github.com/pascalwhoop/](http://github.com/pascalwhoop/) (masterthesis, grpc-adapter, powertac-server, broker-python)

* * *

*Originally published at* [*pascalbrokmeier.de*](https://pascalbrokmeier.de/technology/2018/05/09/What_is_PowerTAC_.html) *on May 9, 2018.*
