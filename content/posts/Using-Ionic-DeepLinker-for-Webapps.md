---
title: Using Ionic DeepLinker for Webapps
date: '2017-03-02T18:35:29.145Z'
excerpt: >-
  I wanted to add deep routing capability to a website that I made using Ionic.
  It was supposed to be a mobile webapp first but then I kind…
thumb_img_path: images/Using-Ionic-DeepLinker-for-Webapps/1*CXjcI_5zHHVGC4gdCl2bIQ.png
layout: post
---
I wanted to add deep routing capability to [a website](http://agm-germany.eu/mobility-fair/) that I made using Ionic. It was supposed to be a mobile webapp first but then I kind of adapted it to also look good on desktop.

The Problem:

![](/images/Using-Ionic-DeepLinker-for-Webapps/1*CXjcI_5zHHVGC4gdCl2bIQ.png)

As you can see, the website is in a child view, but the URL does not represent this. This is not good because it blocks the user from using the browser back button and also doesn’t let you share links easily. But [DeepLinker](http://ionicframework.com/docs/v2/api/navigation/DeepLinker/) to the rescue, the Ionic team has thought of something to solve this.

### Setting it up

Adding some information to my *app.module.ts*

    imports: [  
        IonicModule.*forRoot*(MyApp, {},  
            {  
                links: [  
                    {component: AboutPage, name: 'Home', segment: 'home'},  
                    {component: UniversitiesOverviewPage, name: 'Universities', segment: 'universities'},  
                    {component: UniversityDetailPage, name: "UniversityDetail", segment: "detail/:sectionId"},  
                    {component: WhyStudentsPage, name: 'WhyStudents', segment: 'why-students'},  
                    {component: WhyExhibitorsPage, name: "WhyExhibitors", segment: "why-exhibitors"},  
                    {component: PartnersPage, name: "Partners", segment: "partners"},  
                    {component: PartnerDetailPage, name: "PartnerDetail", segment: "detail/:partnerId"},  
      
                ]  
            })  
    ]

Now the page has a better URL when you open it.

![](/images/Using-Ionic-DeepLinker-for-Webapps/1*qX791X16A_78jlvqini6dA.jpeg)

#### Unexpected behavior

However, navigating to other pages did not cause my URL to update. It just gave me the first page.

![](/images/Using-Ionic-DeepLinker-for-Webapps/1*iG4e3xjQNOOv0WZP0ftK2Q.jpeg)

<figcaption>child page Partners visible, but no updated URL?&nbsp;Odd</figcaption>

I thought I might be the one responsible here, since I tweaked some things here and there and I though I might have broken the framework somewhere. So I created a fresh project and just placed two pages in there, navigating them from home->master->detail by doing a simple this.nav.push(MasterPage) and this.nav.push(DetailPage) respectively. As visible in the Gif below, the URL changes, but it doesn’t do any breadcrumbing. Its still 1 level of depth. But the docs say:

> So, at any point a Page becomes the active page, we just append the URL segment.

![](/images/Using-Ionic-DeepLinker-for-Webapps/1*bXZcy7iukOoRtVFRfku4cg.gif)

This makes sense in a mobile app environment maybe, but it was sort of irritating to see happen. Their code reflects this though. They are looping the NavControllers, not the Pages in the current NavControllers stack.

    // recursivly climb up the nav ancestors  
        // and set each segment's data  
        **while (nav)** {  
          // this could be an ion-nav, ion-tab or ion-portal  
          // if a component and data was already passed in then use it  
          // otherwise get this nav's active view controller  
          if (!component && isNav(nav)) {  
            view = **nav.getActive(true)**;  
            if (view) {  
              component = view.component;  
              data = view.data;  
            }  
          }  
      
          // the ion-nav or ion-portal has an active view  
          // serialize the component and its data to a NavSegment  
          segment = this._serializer.serializeComponent(component, data);  
      
          // reset the component/data  
          component = data = null;  
      
          if (!segment) {  
            break;  
          }  
      
          // add the segment to the path  
          segments.push(segment);  
      
          if (isTab(nav)) {  
            // this nav is a Tab, which is a child of Tabs  
            // add a segment to represent which Tab is the selected one  
            tabSelector = this.getTabSelector(<any>nav);  
            segments.push({  
              id: tabSelector,  
              name: tabSelector,  
              component: null,  
              data: null  
            });  
      
            // a parent to Tab is a Tabs  
            // we should skip over any Tabs and go to the next parent  
            nav = nav.parent && nav.parent.parent;  
      
          } else {  
            // this is an ion-nav  
            **// climb up to the next parent**  
            nav = nav.parent;  
          }  
        }

As you can see, they don’t traverse the page stack that is created by using navCtrl.push(FooPage) but rather the stack of Navs. This is odd, but I can live with it. It is the concept of the target platform (mobile) which is not based on a tree based nav but rather a stack where you could (if you wanted to) keep pushing the same two pages on a stack and then clicking ‘back’ a hundred times.

#### Why is my app not changing URLs?

What does bug me though is the fact, that my App doesn’t do anything at all. It just stays on the root component. Debugging, I noticed, that my components have a different NavController injected than the one that is returned by doing “this.app.getActiveNav()”. So I must have done something differently or broken something, since I am using the same version of ionic.

> Injecting NavController will always get you an instance of the nearest NavController, regardless of whether it is a Tab or a Nav.

So apparently I get the *nearest* but my app thinks another one is the active one and I just keep using a passive one? Sounds odd.

Anyways I fixed it by modifying my constructor of the HomePage (the root page of ion-nav) to this

    **constructor**(**public** navCtrl: NavController, **public** app: App) {  
      **this**.app._setRootNav(**this**.navCtrl)  
    }

Now, my URLs are changing as they are expected to. Well somewhat. My master, detail pages still don’t do a /master/detail/123 but at least they are uniquely reference able.

I looked for a reason for this bug for about two hours but could not find a reason. If you have an idea, or want to look what’s going on, be welcome to do so. The App is hosted [here](https://pascalwhoop.github.io/esn-mobilityfair/App/www/) and the code is on [Github](https://github.com/pascalwhoop/esn-mobilityfair/tree/gh-pages/App)
