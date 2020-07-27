---
title: Testing Angular components containing MdDialog
date: '2017-03-22T10:04:08.797Z'
excerpt: >-
  When testing a component that dynamically opened a dialog using Angular
  Material (2), I was getting an error I didn’t immediately expect…
thumb_img_path: images/Testing-Angular-components-containing-MdDialog/0*2Vi_pC9R_GpRMdwq..jpg
layout: post
---
When testing a component that dynamically opened a dialog using Angular Material (2), I was getting an error I didn’t immediately expect. The MdDialogModule was added to the imports, so my TestBed knew what was going on but my dynamically loaded Component caused problems.

> Error: Error in ./AdminHomeComponent class AdminHomeComponent — inline template:21:24 caused by: No component factory found for OfferFormDialogComponent. Did you add it to @NgModule.entryComponents?

Okay, researching the problem online gave me an [issue on Github](https://github.com/angular/angular/issues/10760) for the angular repository. It suggested the following in the beforeEach block:

    TestBed.overrideModule(BrowserDynamicTestingModule, {  
        set: {  
             entryComponents: [OfferFormDialogComponent],  
        },  
    });

Sure, this solved the problem but it then causes an actual dialog to be opened during our tests. Well I really just want to test if the framework class gets called. I don’t want to test if the framework actually does what it promises.

![](/images/Testing-Angular-components-containing-MdDialog/0*2Vi_pC9R_GpRMdwq..jpg)

The tests and the created but uninvited dialog

It turns out, the solution is much cleaner, if you just mock instead of actually calling the MDDialog method.

So instead of

    spyOn(component.dialog, ‘open’).and.callThrough();

we do

    //create a mock MdDialogRef  
    `**let**` `mockDialogRef = **new**` `MdDialogRef(**new**` `OverlayRef(**null**,**null**,**null**,**null**),{});`
    `//and give it an empty version of our component. alternatively, a stub  
    mockDialogRef.componentInstance = **new**` `OfferFormDialogComponent(**null**, **null**);`
    `spyOn(component.dialog, 'open').and.returnValue(mockDialogRef);`

and later in the test spec

    it('should open the dialog on clicking the button', fakeAsync(() => {  
      expect(component.dialog.open).toHaveBeenCalledTimes(0);  
      clickAddNewButton(component, fixture);  
      fixture.detectChanges();  
      tick();  
      expect(component.dialog.open).toHaveBeenCalled();  
    }));

This way, a clean test of the component without too many dependency is possible. Yes, I am creating a new instance of my OfferFormDialogComponent, but since its constructor doesn’t do anything anyways, and its not active itself, it’s OK.

The whole test and class can be found on github/gist  
[Tests](https://gist.github.com/pascalwhoop/ef2c2bfb53f453c796888570d2ca8589)  
[Component](https://gist.github.com/pascalwhoop/057f9ec439d5de220de337a3d4c8e038)
