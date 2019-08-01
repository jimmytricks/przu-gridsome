<template>
  <div class="layout">
    <header class="header py-2 px-12">
      <section class="container header-container mx-auto py-4">
        <g-link to="/">
          <logoSVG/>
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
    <main class="content-container py-2 px-12">
      <section class="content-wrapper container mx-auto ">
        <slot/>
      </section>
    </main>
    <footer class="footer py-8 px-12">
      <section class="container mx-auto footer-container">
        <div>
          <h3>How did I build this site?</h3>
          <p>Built with Gridsome framework on Vue.JS, using GraphQL.</p>
          <g-link to="/">Read more</g-link>          
          </p>
        </div>
        <div>
          <h3>© PRZU.COM – ALL RIGHTS RESERVED.</h3>
        </div>
    </section>
    </footer>
  </div>
</template>

<script>
import logoSVG from '~/assets/img/svg/logo.svg'
export default {
  metaInfo: {
    title: 'Hello, world!'
  },
  components: {
    logoSVG
  }
}
</script>

<static-query>
query {
  metaData {
    siteName
  }
}
</static-query>

<style lang="scss">

// *** Font

a {
  &:hover {
    color: $color-secondary;
  }
}


// *** General Layout

body {
    font-family: 'montserrat';
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
