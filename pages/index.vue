<template>
  <div id="container">
    <v-card class="pa-4 text-center">
      <h1>Creek bank Social</h1>
      <p>A locals-only social media app.</p>
      <p>Check back often for updates.</p>
      <h3>Coming soon!</h3>
    </v-card>
  </div>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
export default {
  layout: "landing",
  head: generateHeadTags(
    "Creek bank Social",
    "A social app for citizens of Creek bank, AL.",
    "thumbnail",
    null,
    true
  ),
  components: {},
  data() {
    return {
      pageTitle: "Creek bank Social",
    };
  },
  async created() {
    this.$nuxt.$emit("pageTitleChange", this.pageTitle);
  },
};
</script>

<style lang="scss" scoped>
#container {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: auto;
  background: url("~assets/images/landing_background.jpg");
  background-size: cover;
  background-position: center;
}
</style>

