<template>
  <Layout>
    <h1>Posts</h1>
    <ul>
        <li v-for="edge in $page.posts.edges" :key="edge.node.title"><a v-bind:href="edge.node.path">{{ edge.node.title }}</a></li>
    </ul>
  </Layout>
</template>

<script>
export default {
  metaInfo: {
    title: 'List of posts'
  }
}
</script>


<page-query>
  query Posts {
    posts: allWebPost {
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
