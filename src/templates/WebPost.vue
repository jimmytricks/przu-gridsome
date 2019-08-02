<template>
    <layout>
      <section class="page-text-content container posts-page py-12">
        <h1 class="pb-0">{{ $page.post.title }}</h1>
        <p class="py-0 text-sm">{{ $page.post.date }}</p>
        <p class="pt-0 pb-6 text-sm post-info-last">Category: <a v-for="tech in $page.post.tech" v-bind:key="tech.id" href="#">{{ tech }}, </a></p>
        <div class="content pt-4" v-html="$page.post.content">
        </div>
      </section>
    </layout>
</template>

<script>
export default {
  metaInfo () {
    return {
    //   title: this.$page.post.title
    }
  }
}
</script>

<page-query>
    query Posts ($path: String!) {
        post: webPost (path: $path) {
        title
        date (format: "D MMMM, YYYY")
        content
        tech
        }
    }
</page-query>

<style lang="scss">


  .post-info-last {
    border-bottom: 1px solid #D4D4D4;
  }
</style>