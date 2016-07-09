# First practice for Angular2 using TypeScript

**This is from the tutorial at https://angular.io/docs/js/latest/quickstart.html**

The contents below is a summary of that tutorial. This summary is intended for personal use (in case I will not be using Angular2 for a very long time a forget the basics).

##1. Our first Angular component

### a. Add package definition and configuration files

**package.json** 
 - lists packages the QuickStart app depends on and defines some useful scripts.

**tsconfig.json** 
 - is the TypeScript compiler configuration file.

**typings.json** 
 - identifies TypeScript definition files.

**systemjs.config.js**
 - the SystemJS configuration file.
 - QuickStart uses SystemJS to load application and library modules.
 - There are alternatives that work just fine including the well-regarded webpack. SystemJS happens to be a good choice. But we want to be clear that it was a choice and not a preference.
 - We suggest becoming well-versed in the loader of your choice. Learn more about SystemJS configuration here.


### b. Install packages

`npm install`

The typings folder could not show up after npm install. If so, please install them manually.

`npm run typings install`

## 2. Our first Angular component

**app/app.component.ts** - the component file

``` TypeScript
import { Component } from '@angular/core';

@Component({
  selector: 'my-app',
  template: '<h1>My First Angular 2 App</h1>'
})
export class AppComponent { }
```

**AppComponent is the root of the application**

Every Angular app has at least one **root component**, conventionally named AppComponent, that hosts the client user experience. Components are the basic building blocks of Angular applications. A component controls a portion of the screen — a view — through its associated template.

This QuickStart has only one, extremely simple component. But it has the essential structure of every component we'll ever write:

* One or more **import** statements to reference the things we need.
* A **@Component decorator** that tells Angular what template to use and how to create the component.
* A **component class** that controls the appearance and behavior of a view through its template.

We **export** AppComponent so that we can import it elsewhere in our application, as we'll see when we create main.ts.

## 3. Add main.ts

Now we need something to tell Angular to load the root component. Create the file app/main.ts with the following content:

``` TypeScript
import { bootstrap }    from '@angular/platform-browser-dynamic';
import { AppComponent } from './app.component';
bootstrap(AppComponent);
```

We import the two things we need to launch the application:
1. Angular's browser bootstrap function
2. The application root component, AppComponent.

Then we call bootstrap with AppComponent.

**Bootstrapping is platform-specific**

Notice that we import the bootstrap function from @angular/platform-browser-dynamic, not @angular/core. Bootstrapping isn't core because there isn't a single way to bootstrap the app. 

## 4. Add index.html

``` HTML
<html>
  <head>
    <title>Angular 2 QuickStart</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="styles.css">

    <!-- 1. Load libraries -->
     <!-- Polyfill(s) for older browsers -->
    <script src="node_modules/core-js/client/shim.min.js"></script>
    <script src="node_modules/zone.js/dist/zone.js"></script>
    <script src="node_modules/reflect-metadata/Reflect.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>

    <!-- 2. Configure SystemJS -->
    <script src="systemjs.config.js"></script>
    <script>
      System.import('app').catch(function(err){ console.error(err); });
    </script>
  </head>

  <!-- 3. Display the application -->
  <body>
    <my-app>Loading...</my-app>
  </body>
</html>
```

The noteworthy sections of HTML are:

1. The JavaScript libraries
2. Configuration file for SystemJS, and a script where we import and run the app module which refers to the main file that we just wrote.
3. The <my-app> tag in the <body> which is where our app lives!

## 5. Build and run the app!

`npm start`

That command runs two parallel node processes
1. The TypeScript compiler in watch mode
2. A static server called lite-server that loads index.html in a browser and refreshes the browser when application files change

In a few moments, a browser tab should open and display


## What next?

Our first application doesn't do much. It's basically "Hello, World" for Angular 2.

We kept it simple in our first pass: we wrote a little Angular component, created a simple index.html, and launched with a static file server. That's about all we'd expect to do for a "Hello, World" app.

**We have greater ambitions!**

**The good news is that the overhead of setup is (mostly) behind us.** We'll probably only touch the package.json to update libraries. We'll likely open index.html only if we need to add a library or some css stylesheets.

We're about to take the next step and build a small application that demonstrates the great things we can build with Angular 2.

Join us on the [Tour of Heroes Tutorial](https://angular.io/docs/ts/latest/tutorial/)!