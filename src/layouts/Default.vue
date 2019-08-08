<template>
  <div class="layout">
    <header class="header py-4 px-4 sm:px-12 sm:flex">
      <section class="container mx-auto">
        <nav class="logo-and-header-container sm:flex justify-between lg:mx-6">
          <g-link to="/" class="exception-no-border">
            <logoSVG />
          </g-link>
          <div class="block sm:hidden hamburger">
            <button
              @click="toggle"
              class="flex items-center px-3 py-2 border rounded text-white hover:text-white hover:border-white"
            >
              <svg
                class="fill-current h-3 w-3"
                viewBox="0 0 20 20"
                xmlns="http://www.w3.org/2000/svg"
              >
                <title>Menu</title>
                <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z" />
              </svg>
            </button>
          </div>
          <div :class="open ? 'block': 'hidden'" class="sm:flex sm:items-center sm:w-auto">
            <div class="text-sm flex flex-col sm:block sm:flex-row">
              <g-link class="nav-link py-1" to="/">Home</g-link>
              <g-link class="nav-link py-1" to="/about">About</g-link>
              <g-link class="nav-link py-1" to="/blogs">Blog</g-link>
              <g-link class="nav-link py-1" to="/websites">Portfolio</g-link>
              <g-link class="nav-link py-1" to="/contact">Contact</g-link>
            </div>
          </div>
        </nav>
      </section>
    </header>
    <main class="content-container">
      <slot />
    </main>
    <footer class="footer py-4 px-4 md:px-12">
      <section class="container mx-auto footer-container flex-col sm:flex-row">
        <div class="sm:w-1/3 lg:mx-6">
          <p class="text-sm">
            This site is built with the
            <a
              href="https://gridsome.org/"
              target="_blank"
              class="link-exception"
            >Gridsome</a> framework on
            <a href="https://vuejs.org/" target="_blank" class="link-exception">Vue.JS</a>, using GraphQL.
          </p>
          <g-link to="/posts/przu" class="text-sm">Interested? I wrote about it here</g-link>
        </div>
        <div class="sm:w-1/2 text-left sm:text-right lg:mx-6">
          <p class="text-sm pb-0 font-bold">© PRZU.COM – All rights reserved.</p>
          <p class="text-sm p-0">Beware of animals in the ZU, they may bite.</p>
        </div>
      </section>
    </footer>
  </div>
</template>

<script>
import logoSVG from "~/assets/img/svg/logo.svg";
export default {
  metaInfo: {
    title: "PRZU.com - Welcome to the ZU"
  },
  components: {
    logoSVG
  },
  data: function() {
    return {
      open: false
    };
  },
  methods: {
    toggle() {
      this.open = !this.open;
    }
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
  font-size: 1em;
  font-weight: 600;
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
    &:hover {
      color: $color-secondary;
    }
  }
  .exception-no-border {
    border: none !important;
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
  max-width: 680px;
  margin: 0 auto;
}

.nav-link {
  margin-right: 20px;
}

// *** Navigation

.hamburger {
  position: absolute;
  right: 25px;
  top: 25px;
}

.header a.active--exact {
  color: $color-secondary;
}

// *** Shared component styling

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

.inner-card-container {
  display: flex;
  justify-content: center;
}

.card-container {
  max-width: 400px;
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
      padding: 6px;
      font-size: 0.6em;
      margin: 4px;
      display: inline-block;
      font-weight: 600;
      text-transform: uppercase;
    }
  }
}

.blog-posts {
  background-color: white;
  a.card-link {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .read-on {
    font-weight: 500;
    font-size: 0.9em;
    border-top: 1px solid #eac652;
    color: $color-primary;
  }
  a {
    border: none;
  }
  .view-all a {
    border-bottom: 1px solid rgba($color-four, 0.3);
  }
}

.website-posts {
  h3 {
    border-bottom: 1px solid $color-tertiary;
    padding-bottom: 4px;
  }
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
