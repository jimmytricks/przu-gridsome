<template>
  <Layout>
    
    <!-- Learn how to use images here: https://gridsome.org/docs/images -->
    <g-image alt="Example image" src="~/favicon.png" width="135" />
    
    <h1>Welcome to the PRZU</h1>
   
    <p>
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Pariatur excepturi labore tempore expedita, et iste tenetur suscipit explicabo! Dolores, aperiam non officia eos quod asperiores
    </p>

    <h2>Latest Blog Posts</h2>
    <section class="blog-posts">
      <li v-for="post in $page.posts.edges" v-bind:key="post.node.title">{{ post.node.title }} - {{ post.node.date }}</li>
    </section>

    <h2>Latest Website posts</h2>
    <section class="website-posts">
        <li v-for="post in $page.webposts.edges" v-bind:key="post.node.title">{{ post.node.title }} - {{ post.node.date }}</li>
    </section>

  </Layout>
</template>

<script>
export default {
  metaInfo: {
    title: 'Hello, world!'
  }
}
</script>

<page-query>
  query blogPosts {
    posts: allWebPost (filter: { blogpost: { eq: true }}, sortBy: "date", order: DESC) {
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
    webposts: allWebPost (filter: { website: { eq: true }}, sortBy: "date", order: DESC) {
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

<style>
.home-links a {
  margin-right: 1rem;
}
</style>
