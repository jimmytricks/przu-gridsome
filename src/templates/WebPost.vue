<template>
  <layout>
    <section class="page-text-content container posts-page py-12">
      <h1 class="pb-0">{{ $page.post.title }}</h1>
      <p v-if="$page.post.tech.length" class="p-0 text-xs">
        Category:
        <span v-for="(tech, index) in $page.post.tech" v-bind:key="tech.id">
          <span v-if="index != 0 ">,</span>
          {{ tech }}
        </span>
      </p>
      <p class="p-0 text-xs mb-4 pb-4 border-btm">Last updated: {{ $page.post.date }}</p>
      <div v-if="$page.post.featureimg">
        <a v-bind:href="$page.post.websiteurl" class="card-link" target="_blank">
          <g-image :src="$page.post.featureimg" class="shadow-lg"/>
        </a>
        <div class="link-container flex justify-between mt-2 border-btm pb-6 mb-2">
          <a v-bind:href="$page.post.websiteurl" target="_blank">View Site</a>
          <a v-if="$page.post.sourceurl" v-bind:href="$page.post.sourceurl" target="_blank">View Source on GitHub</a>
        </div>
      </div>

      <div class="content" v-html="$page.post.content"></div>
    </section>
  </layout>
</template>

<script>
export default {
  metaInfo() {
    return {
      title: this.$page.post.title
    };
  }
};
</script>

<page-query>
    query Posts ($path: String!) {
        post: webPost (path: $path) {
        title
        date (format: "D MMMM, YYYY")
        content
        tech
        featureimg
        path
        websiteurl
        sourceurl
        }
    }
</page-query>

<style lang="scss">

.border-btm {
  border-bottom: 1px solid #d4d4d4;
}

</style>