<template>
  <v-container>
    <h1>Creek bank Social</h1>
  </v-container>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
export default {
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

