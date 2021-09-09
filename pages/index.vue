<template>
  <div id="container">
    <Login :forgotPasswordAction="forgotPasswordAction" />
  </div>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
import Login from "../components/startup/Login.vue";
export default {
  layout: "landing",
  head: generateHeadTags(
    "Creek bank Social",
    "A social app for citizens of Ohatchee, AL.",
    "thumbnail",
    null,
    true
  ),
  components: {
    Login,
  },
  data() {
    return {
      pageTitle: "Creek bank Social",
    };
  },
  methods: {
    forgotPasswordAction() {
      console.log("forgot password.");
    },
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

