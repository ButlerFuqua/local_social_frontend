<template>
  <div id="container">
    <v-progress-circular
      v-if="isLoading"
      indeterminate
      color="primary"
    ></v-progress-circular>
    <div v-if="!isLoading" class="d-flex flex-column">
      <v-chip v-if="errorMessage" class="mb-2" color="red" dark>{{
        errorMessage
      }}</v-chip>
    </div>
  </div>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
export default {
  head: generateHeadTags(
    "Creek bank Threads",
    "A public bulletin for citizens of Ohatchee, AL.",
    "thumbnail",
    null,
    true
  ),
  components: {},
  data() {
    return {
      pageTitle: "Creek bank Threads",
      isLoading: false,
      errorMessage: null,
    };
  },
  methods: {},
  async created() {
    this.$nuxt.$emit("pageTitleChange", this.pageTitle);
  },
};
</script>

<style lang="scss" scoped>
#container {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  overflow: auto;
}
</style>

