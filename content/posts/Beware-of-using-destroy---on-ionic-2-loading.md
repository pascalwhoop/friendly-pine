---
title: Beware of using destroy() on ionic 2 loading
date: '2016-07-12T09:59:46.253Z'
excerpt: >-
  Quick warning: Do not use destroy() on the ionic2 loading ViewController. It
  may cause undesired effects. Instead use the dismiss()…
thumb_img_path: >-
  images/Beware-of-using-destroy---on-ionic-2-loading/1*LKR07gffKS3HxrI1QDPVvA.gif
layout: post
---
Quick warning: Do not use destroy() on the ionic2 loading ViewController. It may cause undesired effects. Instead use the dismiss() function.

#### Incorrect:

    loading() {  
        **let** loading = Loading.*create*({content: "loading"});  
        setTimeout(()=>{  
            //!!wrong!!  
            loading.destroy();  
        },1000);  
        **this**.nc.present(loading);  
    }

![](/images/Beware-of-using-destroy---on-ionic-2-loading/1*LKR07gffKS3HxrI1QDPVvA.gif)

As you can see the destroy() call on the loading ViewController causes the following <ion-select> element to not function correctly. This was mainly observed within an Ionic 2 Modal. The full for the Modal component is at the end

#### Correct:

    loading() {  
        **let** loading = Loading.*create*({content: "loading"});  
        setTimeout(()=>{  
            //!!right!!  
            loading.dismiss();  
        },1000);  
        **this**.nc.present(loading);  
    }

![](/images/Beware-of-using-destroy---on-ionic-2-loading/1*uEFqGo1xyGqm6n2GTgNQLA.gif)

Full code for you to try yourself:

    **import** {Component} **from** "@angular/core";  
    **import** {NavController, ViewController, Loading} **from** "ionic-angular";  
      
    @Component({  
        template:  
            `  
            <ion-content padding class="about">  
              
              <button (click)="cancel()">Cancel</button>  
              <h2>first:</h2>  
              <button (click)="loading()">trigger loading</button>  
              
              <h2>then:</h2>  
              <ion-list>  
                <ion-item>  
                  <ion-label>Select</ion-label>  
                  <ion-select #selected (ngModelChange)="onSelected($event)">  
                    <ion-option *ngFor="let s of list">s.value</ion-option>  
                  </ion-select>  
                </ion-item>  
              </ion-list>  
            </ion-content>  
      
            `  
      
    })  
    **export class** AboutPage {  
        **constructor**(**private** nc:NavController, **private** viewCtrl:ViewController) {  
        }  
      
        list = [  
            {value: "foo", text: "bar"},  
            {value: "foo2", text: "bar3"},  
            {value: "foo3", text: "bar3"},  
        ];  
      
        cancel() {  
            **this**.viewCtrl.dismiss();  
        }  
      
        loading() {  
            **let** loading = Loading.*create*({content: "loading"});  
            setTimeout(()=>{  
                //!!right!!  
                loading.dismiss();  
            },1000);  
            **this**.nc.present(loading);  
        }  
    }
