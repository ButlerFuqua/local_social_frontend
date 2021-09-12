<template>
  <v-app dark>
    <MainNav :shown="showNav" :hideNav="() => (showNav = false)" />
    <v-app-bar fixed app flat class="grey lighten-3">
      <v-btn icon :aria-label="`Toggle Side nav`" @click.stop="showNav = true">
        <menu-icon />
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn :aria-label="`Reload to get updates`" @click="reloadWindow" icon>
        <reload-icon />
      </v-btn>
    </v-app-bar>
    <v-main class="grey lighten-5">
      <v-container class="h-100" style="margin: auto">
        <div v-if="isLoading" class="h-100 d-flex align-center justify-center">
          <v-progress-circular
            indeterminate
            color="primary"
          ></v-progress-circular>
        </div>
        <nuxt v-else />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import MenuIcon from "../components/icons/MenuIcon.vue";
import ReloadIcon from "../components/icons/ReloadIcon";
import MainNav from "../components/layout/MainNav.vue";
export default {
  components: {
    ReloadIcon,
    MenuIcon,
    MainNav,
  },
  data() {
    return {
      isLoading: false,
      showNav: false,
      fixed: false,
      navItems: [
        {
          emoji: "🏡",
          title: "Welcome",
          to: "/",
        },
      ],
      title: "",
    };
  },
  methods: {
    reloadWindow() {
      window.location.reload();
    },
  },
  async created() {
    this.$nuxt.$on("pageTitleChange", (title) => (this.title = title));
    this.$nuxt.$on("setMainLoading", (bool) => (this.isLoading = bool));
  },
  beforeDestroy() {
    this.$nuxt.$off("pageTitleChange");
    this.$nuxt.$off("setMainLoading");
  },
};
</script>

<style scoped>
#navMenu {
  max-height: 100% !important;
}
</style>

