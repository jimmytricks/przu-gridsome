<template>
  <Layout>
    <section class="page-text-content container px-4 pb-8 pt-6">
      <h1>Contact</h1>
      <form
        name="contact"
        method="post"
        v-on:submit.prevent="handleSubmit"
        action="/success/"
        data-netlify="true"
        data-netlify-honeypot="bot-field"
      >
        <input type="hidden" name="form-name" value="contact" />
        <p hidden>
          <label>
            Don’t fill this out:
            <input name="bot-field" />
          </label>
        </p>
        <div class="sender-info">
          <div>
            <label for="name" class="label block">Your name</label>
            <input type="text" name="name" v-model="formData.name" />
          </div>
          <div>
            <label for="email" class="block">Your email</label>
            <input type="email" name="email" v-model="formData.email" />
          </div>
        </div>

        <div class="message-wrapper">
          <label for="message" class="block">Message</label>
          <textarea name="message" v-model="formData.message"></textarea>
        </div>

        <button type="submit" class="p-4 bg-yellow uppercase font-bold">Talk to me!</button>
      </form>
    </section>

  </Layout>
</template>



<script>
export default {
  metaInfo: {
    title: "Contact"
  },
  data() {
    return {
      formData: {}
    };
  },
  methods: {
    encode(data) {
      return Object.keys(data)
        .map(
          key => encodeURIComponent(key) + "=" + encodeURIComponent(data[key])
        )
        .join("&");
    },
    handleSubmit(e) {
      fetch("/", {
        method: "POST",
        headers: { "Content-Type": "application/x-www-form-urlencoded" },
        body: this.encode({
          "form-name": e.target.getAttribute("name"),
          ...this.formData
        })
      })
        .then(() => this.$router.push("/success"))
        .catch(error => alert(error));
    }
  }
};
</script>

<style scoped lang="scss">
  label {
    width:280px;
    margin-top: 10px;
  }
  textarea {
    height: 100px;
    width: 100%
  }
  input, textarea, button {
    border: 1px solid rgba($color-primary, 0.2)
  }
</style>