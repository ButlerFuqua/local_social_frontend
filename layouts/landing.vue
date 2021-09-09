<template>
  <v-app dark>
    <nuxt />
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      title: "Creek Bank Social",
    };
  },
  methods: {
    reloadWindow() {
      window.location.reload();
    },
  },
  async created() {
    this.$nuxt.$on("pageTitleChange", (title) => (this.title = title));
  },
  beforeDestroy() {
    this.$nuxt.$off("pageTitleChange");
  },
};
</script>

<style scoped>
html,
body {
  height: 100%;
  overflow: hidden;
  overflow: auto;
}
</style>

