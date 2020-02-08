---
title: Up and running with Vue Router
date: 2020-02-08
description: "A short and simple guide on installing and using Vue's official router"
slug: up-and-running-with-vue-router
blogpost: true
---

## Overview

The [Vue Router](https://router.vuejs.org/) is the official router from Vue, allowing you to declare your single file components and have them map to a view. 

## Installing

In most cases you'll be using NPM, where a `npm install vue-router` will get you up and running. For other installation instructions check out the [official docs](https://router.vuejs.org/installation.html).

Within your main index.js/main.js file for your Vue application, import the router `import router from './router';` then amend your Vue app initialization to use the router: 

```
new Vue({
    router,
    render: function (h) { return h(App) }
  }).$mount('#app')
```

In most cases you will add the above, then create a /router folder that contains an index.js. In here you can then import the Vue router and use like so: 

```
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

## Defining the routes

Inside your newly created index.js file in your Vue router folder, here is an example of defining a route:

```
import Vue from 'vue'
import VueRouter from 'vue-router'
import ExampleComponent from '../views/ExampleComponent.vue'

Vue.use(VueRouter)

const routes = [
    {
        path: '/ExampleComponent',
        name: 'ExampleComponent',
        component: ExampleComponent
    }
]

export default router
```

After importing Vue, the router and any components you want to map a route to, you will then define an array of routes, which in this case is just a single route. 

The path refers to the physical URL address, the name can be used if you're specifically pushing to that page, or referring to it in other areas. The component points to the actual imported component, which would be from the `import ExampleComponent from '../views/ExampleComponent.vue'` line.

Now, if you navigate to youraddress.com/#/ExampleComponent, your Vue app's `<router-view>` (more on that shortly) will load your ExampleComponent.vue single file component. 

So what is the the # in the title, and why do you need it? That would be history mode, as explained below. 

## History mode

One useful feature you may want to add is history mode, which is added into your index.js of your router:

```
const router = new VueRouter({
    mode: 'history',
    routes
})
```

By default Vue will use hash mode, which means your page won't be reloaded when the URL changes, but it will have a /#/ before it. If you implement history mode, this means you'll be able to use yoursite/ExampleComponent rather than yoursite.com/#/ExampleComponent. If you want the former you'll likely need to do some additional server setup as [detailed here](https://router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations).

## The router view

Along with navigating to the route, you will also need to add the router view element to your application: `<router-view></router-view>`

The router view will generate whatever single file component relates to the current route / URL. So for example, if you're on yoursite.com/#/ExampleComponent then inside the router view element it will display the ExampleComponent component. 

If your initial App.Vue is: 

```
<template>
    <div>
        <router-view></router-view>
    </div>
</template>
```

This allows the surrounding elements to remain the same, while the router view changes depending on the route. You may also want to display multiple router views, such as one for main content and another as a sidebar, which is possible via [named routes](https://router.vuejs.org/guide/essentials/named-views.html#nested-named-views).

## Code splitting

You also have the ability to perform code splitting on your routes, which means that only the code that is required for that particular route is loaded. Within your router, you would import the component differently with the following syntax:

```
{
      path: '/howtoplay',
      name: 'How to Play',
      // Lazy loading / code splitting example
      component: () => import('./views/Howtoplay.vue')
    },
```

## Further reading

Practically anything you'll need to know about the Router can be found on the [official documentation](https://router.vuejs.org/) and there are many features not listed, though this will help to get you up and running.