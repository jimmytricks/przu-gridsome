<template>
  <Layout>
    <h1>All Blogs</h1>
    <ul>
        <li v-for="edge in $page.posts.edges" :key="edge.node.title"><a v-bind:href="edge.node.path">{{ edge.node.title }} - {{ edge.node.date }}</a></li>
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
    }
  }
</page-query>
