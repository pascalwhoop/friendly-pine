---
title: Fixing wrap ion-row on iOS with ion-col without defined width
date: '2017-03-02T23:44:06.686Z'
excerpt: 'I ran into this problem the other day:'
thumb_img_path: >-
  images/Fixing-wrap-ion-row-on-iOS-with-ion-col-without-defined-width/1*pDaSVkVVlc9ma2-OZ8evWw.jpeg
layout: post
---
I ran into this problem the other day:

[**Ion-row: wrap attribute doesn't work on ios devices**  
*I found a solution to this, and it's probably something that could be incorporated into the default CSS. If you use a…*forum.ionicframework.com](https://forum.ionicframework.com/t/ion-row-wrap-attribute-doesnt-work-on-ios-devices/50603 "https://forum.ionicframework.com/t/ion-row-wrap-attribute-doesnt-work-on-ios-devices/50603")[](https://forum.ionicframework.com/t/ion-row-wrap-attribute-doesnt-work-on-ios-devices/50603)

![](/images/Fixing-wrap-ion-row-on-iOS-with-ion-col-without-defined-width/1*pDaSVkVVlc9ma2-OZ8evWw.jpeg)

Basically on iOS, the ion-col don’t wrap into the new line, if they don’t have a fixed width defined.

    */*fix for ios*/*  
    @media (max-width: 768px) {  
      ion-content.content-ios{  
        ion-row[wrap] {  
          ion-col {  
            flex-basis: 50%;  
          }  
        }  
      }  
    }
    @media (max-width: 476px) {  
      ion-content.content-ios{  
        ion-row[wrap] {  
          ion-col {  
            flex-basis: 100%;  
          }  
        }  
      }  
    }

This solved it for me.

It makes the columns spread 100% but only if the screen is smaller than 476px, and to 50% on iPads

![](/images/Fixing-wrap-ion-row-on-iOS-with-ion-col-without-defined-width/1*5d59DP8Cy9hEeoD8Z0vxzA.jpeg)

![](/images/Fixing-wrap-ion-row-on-iOS-with-ion-col-without-defined-width/1*UaZOk4vLknVn5sCn06fMXw.jpeg)

As you can see, its not a perfect fix, but it allows you to have cards with undefined widths as ion-row elements and have them grow to the space they need without loosing the nice wrap effect of having too many rows in a column.

    <ion-row wrap>  
        <ion-col *ngFor="let country of (sectionsPerCountry | ota)" class="country-shape-col">  
            <ion-card class="country-shape-card" (click)="goCountry(country)" padding>  
      
                <img src="assets/img/country_shapes/{{country[0].country}}.svg" alt="">  
                <p class="card-title">  
                    {{countries[country[0].country]}}  
                </p>  
                  
            </ion-card>  
        </ion-col>  
    </ion-row>

I hope this helped!
