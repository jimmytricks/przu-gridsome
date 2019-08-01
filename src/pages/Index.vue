<template>
  <Layout>
    <section class="page-text-content text-center home-text">
      <h1>Welcome to the ZU</h1>
      <logoSVG/>
      <p>I am an English web designer and developer, moving from Vancouver, Canada to live in Edinburgh, UK.</p>
      <p> Feel free to browse around the site or <g-link to="/contact">contact me</g-link> if you wish.</p>
    </section>

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
import logoSVG from '~/assets/img/svg/przu.svg'
export default {
  metaInfo: {
    title: 'Hello, world!'
  },
  components: {
    logoSVG
  }
}
</script>

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

<style>
.home-text {
  display: flex;
  align-items: center;
  flex-direction: column;
}
</style>
