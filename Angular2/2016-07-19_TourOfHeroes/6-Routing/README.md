# The Tour of Heroes tutorial

## This tutorial is from https://angular.io/docs/ts/latest/tutorial/

This is a summary of that tutorial. I put/copied it here becuase that tutorial might change in the future and so that I will still have a copy of the tutorial even when I'm offline -- I will be using this as a reference when I use Angular 2 in my projects.


## 1. see folder "1-Introduction"

## 2. see folder "2-TheHeroEditor"

## 3. see folder "3-Master_Detail"

## 4. see folder "4-MultipleComponents"

## 5. see folder "5-Services"

## 6. Routing - We'll add Angular’s Component Router to our app 

### Keep the app transpiling and running
```
npm start
```

### Action plan

Here's our plan:

* Turn AppComponent into an application shell that only handles navigation
* Relocate the Heroes concerns within the current AppComponent to a separate HeroesComponent
* Add routing
* Create a new DashboardComponent
* Tie the Dashboard into the navigation structure

 > Routing is another name for navigation. The router is the mechanism for navigating from view to view.

 ### Splitting the AppComponent

 Our current app loads AppComponent and immediately displays the list of heroes.

Our revised app should present a shell with a choice of views (Dashboard and Heroes) and then default to one of them.

The AppComponent should only handle navigation. Let's move the display of Heroes out of AppComponent and into its own HeroesComponent.

### AppComponent is already dedicated to Heroes. Instead of moving anything out of AppComponent, we'll just rename it HeroesComponent and create a new AppComponent shell separately.

The steps are to rename:

* app.component.ts file to heroes.component.ts
* AppComponent class to HeroesComponent
* Selector my-app to my-heroes

### Create AppComponent

The new AppComponent will be the application shell. It will have some navigation links at the top and a display area below for the pages we navigate to.

The initial steps are:
<ul>
<li>create a new file named <code>app.component.ts</code>.</li>
<li>define an <code>AppComponent</code> class.</li>
<li><code>export</code> it so we can reference it during bootstrapping in <code>main.ts</code>.</li>
<li>expose an application <code>title</code> property.</li>
<li>add the <code>@Component</code> metadata decorator above the class with a <code>my-app</code> selector.</li>
<li>add a template with <code>&lt;h1&gt;</code> tags surrounding a binding to the <code>title</code> property.</li>
<li>add the <code>&lt;my-heroes&gt;</code> tags to the template so we still see the heroes.</li>
<li>add the <code>HeroesComponent</code> to the <code>directives</code> array so Angular recognizes the <code>&lt;my-heroes&gt;</code> tags.</li>
<li>add the <code>HeroService</code> to the <code>providers</code> array because we'll need it in every other view.</li>
<li>add the supporting <code>import</code> statements.</li>
</ul>

``` TypeScript
import { Component } from '@angular/core'

import { HeroService }     from './hero.service';
import { HeroesComponent } from './heroes.component';

@Component({
    selector: 'my-app',
    template: `
        <h1>{{title}}</h1>
        <my-heroes></my-heroes>
    `,    
    directives: [HeroesComponent],
    providers: [
        HeroService
    ]
}) 
export class AppCompoment {
    title = 'Tour of Heroes';
}
```

> REMOVE HEROSERVICE FROM THE HEROESCOMPONENT PROVIDERS
>> Go back to the HeroesComponent and remove the HeroService from its providers array. We are promoting this service from the HeroesComponent to the AppComponent. We do not want two copies of this service at two different levels of our app.

### Add Routing

We're ready to take the next step. Instead of displaying heroes automatically, we'd like to show them after the user clicks a button. In other words, we'd like to navigate to the list of heroes.

We'll need the Angular _Component Router_.

#### Set the base tag
``` HTML
<head>
  <base href="/">
```

> **Why do we need `<base href="/">`?**
>> The Component Router uses the browser's `history.pushState` for navigation. Thanks to pushState, we can make our in-app URL paths look the way we want them to look, e.g. localhost:3000/crisis-center. Our in-app URLs can be indistinguishable from server URLs.

>> Modern HTML 5 browsers were the first to support pushState which is why many people refer to these URLs as "HTML 5 style" URLs.

>> We must add a `<base href>` element tag to the index.html to make pushState routing work.



The Angular router is a combination of multiple provided services (provideRouter), multiple directives (ROUTER_DIRECTIVES), and a configuration (RouterConfig). We'll configure our routes first:

--------------------------------

## I found http://devdocs.io/ where I can download the offline documentation of Angular 2 and other frameworks. So I will stop taking notes here

NOTE: July 31, 2016 - You are now in "Configure and add the router" part.

## Aug 2, 2016 - You decided to continue taking notes here becuase you can use this to review. It's easier to review notes you yourself made.

----------------------------------

#### Routes tell the router which views to display when a user clicks a link or pastes a URL into the browser address bar.

Let's define our first route, a route to the HeroesComponent.

in `app/app.routes.ts`

``` TypeScript
import { provideRouter, RouterConfig }  from '@angular/router';
import { HeroesComponent } from './heroes.component';

const routes: RouterConfig = [
  {
    path: 'heroes',
    component: HeroesComponent
  }
];

export const appRouterProviders = [
  provideRouter(routes)
];
```

The route definition above has two parts:

1. path: the router matches this route's path to the URL in the browser address bar (/heroes).

2. component: the component that the router should create when navigating to this route (HeroesComponent).

### Make the router available.

The Component Router is a service. We have to import our appRouterProviders which contains our configured router and make it available to the application by adding it to the bootstrap array.

inn `app/main.ts`
``` TypeScript
import { bootstrap }    from '@angular/platform-browser-dynamic';

import { AppComponent } from './app.component';
import { appRouterProviders } from './app.routes';

bootstrap(AppComponent, [
  appRouterProviders
]);
```
<h1 id="missing-part">The missing part</h2>

## NOTE: I think something is missing in this part of the tutorial. The `<router-outlet>` needs `directives: [ROUTER_DIRECTIVES]`. But the code for that is given way below of the tutorial. I got confused. But I was able to see the error and found a fix for it.

### At this point we want to remove the `<my-heroes>` and the `import { HeroesComponent }` in the `app.component.ts` because we don't want to display the heroes in there. We now want it to be displayed in the path `/heroes` and we already configured the routes to do that for us.

``` TypeScript
import { Component } from '@angular/core'
import { HeroService }     from './hero.service';

@Component({
    selector: 'my-app',
    template: `
        <h1>{{title}}</h1>
    `,    
    directives: [],
    providers: [
        HeroService
    ]
}) 
export class AppComponent {
    title = 'Tour of Heroes';
}

```

**If we try to navigate to `http://localhost:3000/heroes` in your browser we will only see the title `Tour of Heroes` in your browser. That title came from `app.component.ts`.**

### Router Outlet (This part came from the tutorial but I added the missing code below)

If we paste the path, /heroes, into the browser address bar, the router should match it to the 'Heroes' route and display the HeroesComponent. But where?

We have to tell it where by adding `<router-outlet>` marker tags to the bottom of the template. RouterOutlet is one of the `ROUTER_DIRECTIVES`. The router displays each component immediately below the `<router-outlet>` as we navigate through the application.

In `app.component.ts`:
``` TypeScript
import { Component } from '@angular/core';
import { ROUTER_DIRECTIVES } from '@angular/router';
import { HeroService }     from './hero.service';
@Component({
  selector: 'my-app',
  template: `
    <h1>{{title}}</h1>
    <router-outlet></router-outlet>
  `,
  directives: [ROUTER_DIRECTIVES],
  providers: [
    HeroService
  ]
})
export class AppComponent {
  title = 'Tour of Heroes';
}
```

<h1 id="missing-part-end">End of the missing part</h2>

### Router Links

We don't really expect users to paste a route URL into the address bar. We add an anchor tag to the template which, when clicked, triggers navigation to the HeroesComponent.

In `app.component.ts`:
```
template: `
  <h1>{{title}}</h1>
  <a [routerLink]="['/heroes']">Heroes</a>
  <router-outlet></router-outlet>
`,
```

Notice the `[routerLink]` binding in the anchor tag. We bind the RouterLink directive (another of the ROUTER_DIRECTIVES) to an array that tells the router where to navigate when the user clicks the link.


We define a routing instruction with a link parameters array. The array only has one element in our little sample, the quoted path of the route to follow.

> Learn about the link parameters array in the [Routing](https://angular.io/docs/ts/latest/guide/router.html#link-parameters-array) chapter.


** The AppComponent is now attached to a router and displaying routed views. For this reason and to distinguish it from other kinds of components, we call this type of component a Router Component.**


## Add a Dashboard - Routing only makes sense when we have multiple views. We need another view.

Create new file `app/dashboard.component.ts`:
``` TypeScript
import { Component } from '@angular/core';

@Component({
  selector: 'my-dashboard',
  template: '<h3>My Dashboard</h3>'
})
export class DashboardComponent { }
```

### Configure the dashboard route

In `app.routes.ts`:
``` TypeScript
import { provideRouter, RouterConfig } from '@angular/router';
import { HeroesComponent } from './heroes.component';
import { DashboardComponent } from './dashboard.component';

const routes: RouterConfig = [
  {
    path: 'heroes',
    component: HeroesComponent
  },
  {
    path: 'dashboard',
    component: DashboardComponent
  },
];

export const appRouterProviders = [
  provideRouter(routes)
];
```

#### We want the app to show the dashboard when it starts and we want to see a nice URL in the browser address bar that says /dashboard. Remember that the browser launches with / in the address bar. We can use a redirect route to make this happen.

```
{
  path: '',
  redirectTo: '/dashboard',
  pathMatch: 'full'
},
```

#### Finally, add a dashboard navigation link to the template, just above the Heroes link.

In `app.component.ts`:
``` TypeScript
template: `
  <h1>{{title}}</h1>
  <nav>
    <a [routerLink]="['/dashboard']" routerLinkActive="active">Dashboard</a>
    <a [routerLink]="['/heroes']" routerLinkActive="active">Heroes</a>
  </nav>
  <router-outlet></router-outlet>
`,
```

> We nested the two links within `<nav>` tags. They don't do anything yet but they'll be convenient when we style the links a little later in the chapter.
