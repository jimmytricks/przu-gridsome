<template>
  <Layout>
    <h1 class="text-center">All Websites & Projects</h1>
    <section class="website-posts bg-white">
      <div class="container mx-auto pt-6 md:pt-8 flex flex-wrap">
        <div
          class="inner-card-container w-full sm:w-1/2 lg:w-1/3 mb-8 md:mb-16"
          v-for="post in $page.posts.edges"
          v-bind:key="post.node.id"
        >
          <div class="m-6 rounded overflow-hidden shadow-lg bg-white card-container">
            <g-link class="card-link" v-bind:to="post.node.path">
              <g-image :src="post.node.featureimg" />
              <div class="px-6 py-4">
                <h3>{{ post.node.title }}</h3>
                <p class="desc">{{ post.node.description }}</p>
              </div>
            </g-link>
            <div class="tag-container px-2 py-4">
              <span v-for="tech in post.node.tech" v-bind:key="tech.id" class="tech">{{ tech }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </Layout>
</template>

<script>
export default {
  metaInfo: {
    title: "List of Websites"
  }
};
</script>


<page-query>
  query webPosts {
    posts: allWebPost (filter: { website: { eq: true }}, sortBy: "date", order: DESC) {
      edges {
        node {
          id
          title
          path
          date (format: "D MMMM YYYY")
          description
          tech
          featureimg (width: 500, height: 228, quality: 100)
        }
      }
    }
  }
</page-query>

<style scoped lang="scss">
</style>
