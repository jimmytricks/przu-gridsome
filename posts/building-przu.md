---
title: How I built PRZU.com with Gridsome, Vue and GraphQL
date: 2019-08-09
description: Gridsome is a fantastic tool for creating static sites. Here is brief write up of how I created PRZU.com 
slug: building-przu
blogpost: true
---

## Overview

In this overview I'll go through some of the initial steps to getting up and running with Gridsome.

## Getting Started

First you will need to install the Gridsome CLI tool `npm install --global @gridsome/cli`

Then create your first gridsome project `gridsome create new-project`

We will use markdown files as blog posts, so first create a new posts folder in your root folder. Then create an example markdown file, which might look something like this:

```
---
title: Example Post
date: 2018-09-15 07:42:34
description: "Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim. Aliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus. Phasellus viverra nulla ut metus varius laoreet."
slug: first
blogpost: true
---

## Example title

Example content
```

Next, install the source file system plugin `npm install @gridsome/source-filesystem` and then transformer remark as a dev dependency `npm install @gridsome/transformer-remark --save-dev`

The source filesystem and transformer remark will make the magic of converting the markdown files into posts we can display on the site. You will need to edit the gridsome config file after doing this, which isn't as worrying as it may first look. 

Change it to

```
module.exports = {
  siteName: 'Example Blog',
  plugins: [
    {
      use: '@gridsome/source-filesystem',
      options: {
        path: 'posts/*.md',
        typeName: 'WebPost',
        route: '/:slug',
      }
    }
  ],
  transformers: {
    remark: {}
  },
}
```
Next up is to install Eslint, Vetur and GraphQL, all of which can read more thoroughly [here](https://gridsome.org/docs/dev-tools/). This helps with debugging and creating code, especially with graphql queries as it gives syntax highlighting. 

Now we want to split our posts between blog posts and websites. In pages create separate Posts.vue and Blogs.vue. Here is an example for a list of blogs:

```
<template>
  <Layout>
    <h1>All Blogs</h1>
    <ul>
        <li v-for="edge in $page.posts.edges" :key="edge.node.title"><a v-bind:href="edge.node.path">{{ edge.node.title }}</a></li>
    </ul>
  </Layout>
</template>

<script>
export default {
  metaInfo: {
    title: 'List of blogs'
  }
}
</script>


<page-query>
  query blogPosts {
    posts: allWebPost (filter: { blogpost: { eq: true }}) {
      edges {
        node {
          id
          title
          path
          date (format: "D. MMMM YYYY")
          description
        }
      }
    }
  }
</page-query>
```

This will display a list of blog posts, is uses the blogpost: variable to see which are true. If you look back at the example markdown file, you will see why we have added blogpost:true.

Go ahead and run `gridsome develop` and you should be able to go and view your pages, i.e /posts or /blogs.

You now have the basics and can go on to add in as much complexity as required. 

To sort by date or title, you can do this: `posts: allWebPost (filter: { blogpost: { eq: true }}, sortBy: "title", order: ASC)`

To display two different queries, add it to page query. Here I have limited to the last 3 posts:

```
    posts: allWebPost (filter: { blogpost: { eq: true }}, limit:3, sortBy: "date", order: DESC) {
      edges {
        node {
          id
          title
          path
          date (format: "D MMMM YYYY")
          description
        }
      }
    },
    webposts: allWebPost (filter: { website: { eq: true }}, limit:3, sortBy: "date", order: DESC) {
      edges {
        node {
          id
          title
          path
          date (format: "D MMMM YYYY")
          description
        }
      }
    }
  }
</page-query>
```

To get these posts to display in our Vue template, I have done: 
`<li v-for="post in $page.posts.edges" v-bind:key="post.node.title">{{ post.node.title }} - {{ post.node.date }}</li>`

## Using images in markdown 

If you want to use images in markdown, you can use: `![Alternative](../src/assets/img/sites/example.jpg)`

## Add SASS support

For adding SASS support, run `npm install -D sass-loader node-sass`. After this you can then change the Vue template so that you can use SASS directly within it by changing your `<style>` tag to `<style scoped lang="scss">`. Note the scoped means that only the elements within that template are effected by the styling.

However, at the time of writing one 'gotcha' is that if you are wanting to style the contents of a v-html, as I have done here with the markdown file, this will need to be styled globally. 

## Support for variables, mixins

`npm i -D style-resources-loader` will give you support for variables, mixins, etc. 
Change gridsome config and import in main.js, with more information available on the [gridsome docs](https://gridsome.org/docs/assets-css/)

My gridsome.config.js at this stage:

```
const path = require('path')

function addStyleResource (rule) {
  rule.use('style-resource')
    .loader('style-resources-loader')
    .options({
      patterns: [
        path.resolve(__dirname, './src/assets/scss/globals.scss')
      ],
    })
}

module.exports = {
  siteName: 'Gridsome',
  plugins: [
    {
      use: '@gridsome/source-filesystem',
      options: {
        path: 'posts/*.md',
        typeName: 'WebPost',
        route: '/:slug',
      }
    }
  ],
  transformers: {
    remark: {}
  },
  chainWebpack (config) {
    // Load variables for all vue-files
    const types = ['vue-modules', 'vue', 'normal-modules', 'normal']
    
    types.forEach(type => {
      addStyleResource(config.module.rule('scss').oneOf(type))
    })
	}
}
```

## Install Tailwind 

Tailwind is one of my favourite libraries, which is especially great for quick prototyping. 

Install it as a dev dependency `npm install -D gridsome-plugin-tailwindcss` and then create a tailwind config file `npx tailwind init`

In here you can add your usual [tailwind configuration](https://tailwindcss.com/docs/configuration/)

## Installing font

It's also likely you will want to add some custom type. I personally installed this via NPM, with `npm i -d typeface-montserrat` and then required it in the main.js: `require('typeface-montserrat')`

## Adding SVG Support

Using SVGs can be setup by doing `npm i -d vue-svg-loader`, then you wil need to add some extra code to your gridsome config file

```
  chainWebpack (config) {
    // Load variables for all vue-files
    const types = ['vue-modules', 'vue', 'normal-modules', 'normal']
    
    types.forEach(type => {
      addStyleResource(config.module.rule('scss').oneOf(type))
    })

    const svgRule = config.module.rule('svg')
    svgRule.uses.clear()
    svgRule
      .use('vue-svg-loader')
      .loader('vue-svg-loader')
	}
}
```

To use an SVG like a component, first import it: `logoSVG from '~/assets/img/example.svg'` then you are able to use it like `<logoSVG/>`
Lastly, add it to export
```
  components: {
    logoSVG
  }
```

## Feature image
For PRZU.com I have setup a featured image that will display on both the home page and also within the blog itself. Within the markdown itself I defined a feature image  `featureimg:/img/example.jpg`.

Then you can use a g-image for referencing this image, to avoid you having to update images in two separate places. `<g-image :src="post.node.featureimg"/>`

Within the GraphQL query, you can also add parameters to the feature image to change the height, image and the quality: `featureimg (width: 720, height: 400, quality: 90)`

## Further development & Deployment 

That should be enough for you to start playing around with Gridsome and GraphQL to create something really cool. When you're ready to deploy, [Netlify](https://www.netlify.com/) offer a great solution that's very easy to set up. Netlify is also free for their low use tier, which is awesome. 

First, push your site to a repo on Github then log into Netlify and hit 'New Site from Git', select github and find your repo. Select your master branch and for the build command enter `gridsome build`. Click deploy the site and voila it should work! (or in my case, need a while tinkering to fix some image path issues).

Now when you want to update your blog, it's as simple as pushing your latest commit to GitHub and Netlify will automatically rebuild the site. How cool is that? Welcome to that [JAMstack](https://jamstack.org/) goodness. 