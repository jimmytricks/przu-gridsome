<template>
  <div class="layout">
    <header class="header py-4 px-12">
      <section class="container header-container mx-auto">
        <g-link to="/">
          <logoSVG />
        </g-link>
        <nav class="nav">
          <g-link class="nav-link" to="/">Home</g-link>
          <g-link class="nav-link" to="/about">About</g-link>
          <g-link class="nav-link" to="/blogs">Blog</g-link>
          <g-link class="nav-link" to="/websites">Portfolio</g-link>
          <g-link class="nav-link" to="/posts">All Posts</g-link>
          <g-link class="nav-link" to="/websites">Contact</g-link>
        </nav>
      </section>
    </header>
    <main class="content-container">
      <slot />
    </main>
    <footer class="footer py-6 px-12">
      <section class="container mx-auto footer-container">
        <div class="w-1/3">
          <p class="text-sm">
            This site is built with the
            <a href="https://gridsome.org/" target="_blank" class="link-exception">Gridsome</a> framework on
            <a href="https://vuejs.org/" target="_blank" class="link-exception">Vue.JS</a>, using GraphQL.
          </p>
          <g-link to="/posts/przu" class="text-sm">Interested? I wrote about it here</g-link>
        </div>
        <div class="w-1/3">
          <p class="text-sm font-bold">© PRZU.COM – All rights reserved.</p>
          <p class="text-sm">Beware of animals in the ZU, they may bite.</p>
        </div>
      </section>
    </footer>
  </div>
</template>

<script>
import logoSVG from "~/assets/img/svg/logo.svg";
export default {
  metaInfo: {
    title: "Hello, world!"
  },
  components: {
    logoSVG
  }
};
</script>

<static-query>
query {
  metaData {
    siteName
  }
}
</static-query>

<style lang="scss">

body {
  font-family: "montserrat", Arial;
}

// *** Font

p {
  font-size: 16px;
  padding-top: 0.6em;
  padding-bottom: 0.6em;
}

a {
  color: $color-secondary;
  border-bottom-width: 1px;
  border-color: rgba($color-secondary, 0.3);
  &:hover {
    color: rgba($color-secondary, 0.8);
  }
}

h1 {
  color: $color-primary;
  font-weight: 700;
  font-size: 1.2em;
  padding: 10px 0;
  text-transform: uppercase;
}

h2 {
  @extend h1;
}

// *** Colours

.link-exception {
  color: #d5d5d5;
}

// *** General Layout

.content-container {
  background-color: $color-tertiary;
  a {
    border-bottom-width: 1px;
    border-color: rgba($color-four, 0.3);
    color: $color-four;
    &:hover {
      color: rgba($color-four, 0.7);
    }
  }
}

// *** Header & nav section
.header {
  background-color: $color-primary;
  border-bottom: 1px solid black;
  a {
    color: white;
    border: none;
    &:hover {
      color: $color-secondary;
    }
  }
}

.layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  .content-container {
    display: flex;
    flex-grow: 1;
    flex-direction: column;
  }
}

.page-text-content {
  max-width: 640px;
  margin: 0 auto;
}

.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-link {
  margin-left: 20px;
}

// *** Footer

footer {
  background-color: $color-primary;
  color: white;
  h3 {
    color: white;
    text-transform: uppercase;
    font-weight: 700;
    font-size: 16px;
  }
  .footer-container {
    display: flex;
    justify-content: space-between;
  }
}
</style>
