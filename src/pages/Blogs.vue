<template>
  <Layout>
    <h1 class="text-center bg-yellow">All Blog Posts</h1>
    <section class="blog-posts">
      <div class="container mx-auto pt-2 md:pt-8 flex flex-wrap">
        <div
          class="inner-card-container w-full sm:w-1/2 lg:w-1/3 mb-8"
          v-for="post in $page.posts.edges"
          v-bind:key="post.node.title"
        >
          <div class="mx-6 mb-6 rounded overflow-hidden shadow-lg bg-white card-container">
            <g-link class="card-link" v-bind:to="post.node.path">
              <div class="flex flex-col px-6 pt-4 pb-2">
                <h3>{{ post.node.title }}</h3>
                <p class="pt-0 date">{{ post.node.date }}</p>
                <p class="desc">{{ post.node.description }}</p>
              </div>
              <p class="read-on px-6 py-3 bg-yellow">Read on...</p>
            </g-link>
          </div>
        </div>
      </div>
    </section>
  </Layout>
</template>



<script>
export default {
  metaInfo: {
    title: "List of blogs"
  }
};
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

<style scoped lang="scss">
.blog-posts {
  background-color: $color-tertiary;
}
</style>
