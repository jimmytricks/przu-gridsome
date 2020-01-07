<template>
  <Layout>
    <div class="content-wrapper pb-8 pt-6 px-4 container mx-auto">
      <section class="page-text-content text-center home-text">
        <h1>Welcome to the ZU</h1>
        <logoSVG />
        <p>Hello! I'm a Web developer from the UK.</p>
        <p>
          Feel free to <g-link to="/contact">contact me</g-link> if you'd like to say hi, or reach out on <a href="https://www.linkedin.com/in/james-h-82685053/" target="_blank" class="link-exception">LinkedIn</a>.
        </p>
        <p>Have a great day!</p>
      </section>
    </div>

    <h2 class="posts-header">Latest Blog Posts</h2>
    <section class="blog-posts">
      <div class="container mx-auto pt-8 flex flex-wrap justify-center">
        <div
          class="inner-card-container  sm:w-1/2 lg:w-1/3"
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
      <div class="container mx-auto">
        <div class="view-all mx-6 mb-6 text-right">
          <g-link to="/blogs" class>View All Blog Posts</g-link>
        </div>
      </div>
    </section>

    <h2 class="posts-header">Latest Websites & Projects</h2>
    <section class="website-posts">
      <div class="container mx-auto pt-12 flex flex-wrap justify-center">
        <div
          class="inner-card-container w-full sm:w-1/2 lg:w-1/3 mb-8"
          v-for="post in $page.webposts.edges"
          v-bind:key="post.node.id"
        >
          <div class="mx-6 rounded overflow-hidden shadow-lg bg-white h-full card-container">
            <g-link class="card-link" v-bind:to="post.node.path">
              <g-image :src="post.node.featureimg" />
              <div class="px-6 py-4">
                <h3>{{ post.node.title }}</h3>
                <p class="desc">{{ post.node.description }}</p>
              </div>
            </g-link>
            <div class="tag-container px-2 py-4">
              <span v-for="tech in post.node.tech" v-bind:key="tech.id" class="tech shadow-lg">{{ tech }}</span>
            </div>
          </div>
        </div>
        <div class="container mx-auto">
          <div class="view-all mx-6 mb-6 text-right">
            <g-link to="/websites" class>View All Websites & Projects</g-link>
          </div>
        </div>
      </div>
    </section>
  </Layout>
</template>

<script>
import logoSVG from "~/assets/img/svg/przu.svg";
export default {
  metaInfo: {
    title: "Welcome to the ZU"
  },
  components: {
    logoSVG
  }
};
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
    webposts: allWebPost (filter: { website: { eq: true }}, limit:6, sortBy: "date", order: DESC) {
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
.home-text {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.posts-header {
  text-align: center;
  background-color: #353535;
  color: white;
}

svg {
  margin-bottom: 10px;
}
</style>
