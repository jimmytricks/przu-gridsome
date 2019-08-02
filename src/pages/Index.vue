<template>
  <Layout>
    <div class="content-wrapper container mx-auto">
      <section class="page-text-content text-center home-text">
        <h1>Welcome to the ZU</h1>
        <logoSVG />
        <p>I am an English web designer and developer, moving from Vancouver, Canada to live in Edinburgh, UK.</p>
        <p>
          Feel free to browse around the site or
          <g-link to="/contact">contact me</g-link> if you'd like.
        </p>
        <p>Have a great day!</p>
      </section>
    </div>

    <h2 class="posts-header">Latest Blog Posts</h2>
    <section class="blog-posts">
      <div class="container mx-auto py-6 flex">
        <div
          v-for="post in $page.posts.edges"
          v-bind:key="post.node.title"
          class="w-1/3 mx-6 rounded overflow-hidden shadow-lg bg-white card-container"
        >
          <g-link class="card-link" v-bind:to="post.node.path">
            <div class="flex flex-col px-6 pt-4 pb-2">
              <h3>{{ post.node.title }}</h3>
              <p class="pt-0 date">{{ post.node.date }}</p>
              <p class="desc">{{ post.node.description }}</p>
            </div>
            <p class="read-on px-6 py-4 bg-yellow">Read on...</p>
          </g-link>
        </div>
      </div>
    </section>

    <h2 class="posts-header">Latest Websites & Projects</h2>
    <section class="website-posts">
      <div class="container mx-auto py-6 flex">
        <div
          v-for="post in $page.webposts.edges"
          v-bind:key="post.node.title"
          class="w-1/3 mx-6 rounded overflow-hidden shadow-lg bg-white card-container"
        >
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
    </section>
  </Layout>
</template>

<script>
import logoSVG from "~/assets/img/svg/przu.svg";
export default {
  metaInfo: {
    title: "Hello, world!"
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
    webposts: allWebPost (filter: { website: { eq: true }}, limit:3, sortBy: "date", order: DESC) {
      edges {
        node {
          id
          title
          path
          date (format: "D MMMM YYYY")
          description
          tech
          featureimg (width: 400, height: 240, quality: 90)
        }
      }
    }
  }
</page-query>

<style lang="scss">
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

.card-container {
  h3 {
    color: $color-four;
    line-height: normal;
    padding-bottom: 4px;
  }
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  .desc {
    font-size: 0.8em;
  }
  .tag-container {
    text-align: center;
    background-color: $color-tertiary;
    .tech {
      background-color: #ffd64e;
      border-radius: 100px;
      padding: 8px;
      font-size: 0.8em;
      margin: 4px;
      display: inline-block;
      font-weight: 500;
    }
  }
}

.card-link {
  display: block;
  height: 100%;
  &:hover {
    background: rgba($color-tertiary, 0.5);
    opacity: 0.8;
  }
  .desc,
  p {
    color: black;
  }
  .date {
    border-bottom: 1px solid $color-tertiary;
    font-size: 0.8em;
  }
}

.website-posts {
  h3 {
      border-bottom: 1px solid $color-tertiary;
      padding-bottom: 4px;
  }
}

.blog-posts {
  background-color: white;
  a {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .read-on {
    font-weight: 500;
    font-size: 0.9em;
  }
  a {
    border: none;
  }
}
</style>
