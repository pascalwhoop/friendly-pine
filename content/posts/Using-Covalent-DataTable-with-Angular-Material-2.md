---
title: Using Covalent DataTable with Angular Material 2
date: '2017-03-30T10:00:49.903Z'
excerpt: >-
  If you are at the point that you know and like angular, use angular 2 a lot
  and just decided upon using their material components, keep…
thumb_img_path: >-
  images/Using-Covalent-DataTable-with-Angular-Material-2/1*S7Dr6abmeFqEoBeMrT15uw.jpeg
layout: post
---
If you are at the point that you know and like angular, use angular 2 a lot and just decided upon using their material components, keep reading. If you now also realized that they are somewhat thinly spread, missing some key elements like a good table, definately keep reading. This is for you!

#### Checking the alternatives

Okay, material looks very much like Google. That’s good, they establish a common use and feel, customers will feel right at home in your app. But now the question arises: Angular ✔ but what UI on top? Bootstrap, [PrimeNG](https://www.primefaces.org/primeng/#/), [Material](http://material.angular.io), …? Well I chose Material but then quickly found myself in a pickle. What about a nice table? Nope not ready. What about cool graphs and colorful donuts? Nope. Alright, searching for alternatives again. I found [Covalent](https://teradata.github.io/covalent/#/) and liked it. It isn’t perfect, its young but it incorporates the Angular Material framework and doesn’t try to reinvent what is already there. They build those things that you would otherwise build *using angular material*.

They also offer some cool optional stuff, like a [http wrapper](https://teradata.github.io/covalent/#/components/http) that helps you build interceptors for your http calls. I feel pretty silly, having built a service that hooks into http’s xhr calls to intercept events now, knowing someone already built that. But let’s not digress further, let’s look into the data-table!

![](/images/Using-Covalent-DataTable-with-Angular-Material-2/1*S7Dr6abmeFqEoBeMrT15uw.jpeg)

<figcaption>the resulting table, because why not start with the&nbsp;results!</figcaption>

#### Build a table

I got some data coming in. I want to display that. Because there can easily be some few hundred elements, I want it sortable and searchable. Makes sense. Maybe even do paging, but that is for later. For now, let’s make use of those 4G networks and just *fetch all the data*.

<script src="https://gist.github.com/pascalwhoop/114192edcccf07735605c926d58b2914.js"></script>

The data is partially nested, but covalent can handle it. You just give it a an array of configurations that describe your data and where to find the values it is supposed to put into the columns and then it figures out the rest.

    columns: ITdDataTableColumn[] = [  
        {name: 'offer.date', label: 'Date', tooltip: 'date ordered'},  
        {name: 'offer.description', label: 'Description'},  
        {name: 'amount', label: 'Price'},  
        {name: 'offer.vegetarian', label: 'Vegetarian'},  
        {name: 'paid', label: 'Paid'}  
    ];

This is the config. I will not post my whole model but it’s obvious from my config anyways. It looks something like

    [{  
        paid: false,  
        //some more Props  
        offer: {  
            description: "foo",  
            date: "2015-05-05",  
            //...  
        },  
    },  
    ...  
    ]

The simple table without search and sorting looks like this now

<script src="https://gist.github.com/pascalwhoop/337b497df2a2dcdf45a9796bc8b65aba.js"></script>

Fairly straight-forward. I give it the data and the data structure using `columns`. Then I can give it templates for data that I want formatted in a certain way. Add a € after the price, display the date properly. That kind of stuff.

#### Wrapping it up

I didn’t explain the whole data-table API, [they have a docs for that](https://teradata.github.io/covalent/#/components/data-table). But you get an idea. And it looks good! On all screen sizes. It doesn’t turn into a list view when you go small but I don’t want it to. It’s a table, it should stay a table even if the screen is small. And it’s ok, because you can scroll from left to right on small screens.

![](/images/Using-Covalent-DataTable-with-Angular-Material-2/1*Xaie_1q5NXEhl5Xip2_vpw.gif)

I’ll keep exploring the project, but it looks promising. It’s not as engineered as angular material or the [material-components-web](https://github.com/material-components/material-components-web), but they are higher level and they are ready today. So I start building my app with them, replacing elements in the future, when the official repositories have implemented their versions of the tools.

The downside: I don’t think these tools are as perfectly tested or oriented for screenreaders and don’t adhere to the same standards of accessibility as angular material does. But that’s the price you pay for getting a library that evolves more quickly.

Overall, it looks promising. [Go star it on GitHub](https://github.com/Teradata/covalent) and keep it in your toolbox!
