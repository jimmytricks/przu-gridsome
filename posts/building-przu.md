---
title: How I built PRZU.com with Gridsome, Vue and GraphQL
date: 2019-04-07 07:42:34
description: Gridsome is a fantastic tool for creating static sites. Here is brief write up of how I created PRZU.com 
slug: building-przu
blogpost: true
---

Create a Gridsome site with

`gridsome create new-project`

Add a posts folder, with some mark down files inside
Add a templates for post (WebPost)
Install source file system plugin
`npm install @gridsome/source-filesystem`

Install  transformerremark as dev dependecy
`npm install @gridsome/transformer-remark --save-dev`

Change gridsome config to

```
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
}
```

Install Eslint, Vetur and GraphQL:
https://gridsome.org/docs/dev-tools/
This helps with debugging and creating code, especialy with graphql queries as it gives syntax highlighting

Now we want to split our posts between blog posts and websites. In pages create seperate Posts.vue and Blogs.vue

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

This will display a list of blog posts, is uses the blogpost: variable to see which are true. Here is an example blogpost.md 

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

If you run `gridsome develop` you should be able to go and view your pages, i.e /posts or /blogs, if you have done the same as mine as it will show a corresponding list.

You now have the basics and can go on to add in as much complexity as required. 

To sort by date or title, you can do this:
`posts: allWebPost (filter: { blogpost: { eq: true }}, sortBy: "title", order: ASC)`

To display two different queries, add it to page query. Here I have limited to the last 3 posts:

```
<page-query>
  query blogPosts {
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

To get these posts, I have done: 
`<li v-for="post in $page.posts.edges" v-bind:key="post.node.title">{{ post.node.title }} - {{ post.node.date }}</li>`

#Tags/Categories

Add a new field to add some tags and categories

## Using images in markdown 

`![Alternative](../src/assets/img/sites/didyouwatchthegame.jpg)`

## Add SASS support

Run `npm install -D sass-loader node-sass`

## Support for variables, mixins

`npm i -D style-resources-loader`
Change gridsome config and import in main.js
https://gridsome.org/docs/assets-css/

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

# Install Tailwind 
`npm install -D gridsome-plugin-tailwindcss`

Create a tailwind config file `npx tailwind init`

# Installing font

`npm i -d typeface-montserrat`
Then require it in main.js: `require('typeface-open-sans')`

# Adding SVG Support
`npm i -d vue-svg-loader`

Chain webpack part of config now looks like

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
In your component, import it: `logoSVG from '~/assets/img/przu.svg'` then able to use it like `<logoSVG/>`
Lastly, add it to export
```
  components: {
    logoSVG
  }
```

# Tags/Categories
Then add in support to categories using

`tech: ["WordPress", "PHP", "cURL"]`

And to show on front end: 

` v-for="post in $page.webposts.edges"  v-bind:key="post.node.title"`

and loop categories within post

#feature image
Then add a featureimg field to the post, link it to image source

` <g-image :src="post.node.featureimg"/>`

Within query, you can change the image: `featureimg (width: 720, height: 400, quality: 90)`

# Card styling

To ensure that the tags stay at the bottom I used flex box and space between, while wrapping title and image within a container so they are always level

# Linking to variable/dynamic paths
`<g-link class="px-6 py-4 card-link" v-bind:to="post.node.path">`

# Creating taxonomy tags


```
  refs: {
    tags: {
      typeName: 'Tag',
      route: '/tag/:slug',
      create: true
    }
  }
```
 ^ wasn't able to figure it out

 ## SCSS/CSS

 For styling, I used scss and have all componenets scoped except default.vue, where I put defaults and any shared 

 ```
 <style scoped lang="scss">
</style>
   ```

   # Deployment

   Netlify, signup and add the site from git.
   Select the repo that you have and put the 'dist' folder and 'gridsome build' as the build command