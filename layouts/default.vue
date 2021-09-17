<template>
  <v-app dark>
    <SideNav :shown="showNav" :hideNav="() => (showNav = false)" />
    <TopBar :title="title" :toggleNav="() => (showNav = true)" />
    <v-main class="grey lighten-3">
      <AddPostDialog
        :showAddPostDialog="showAddPostDialog"
        :togglePostDialog="
          (bool) => (showAddPostDialog = bool || !showAddPostDialog)
        "
      />
      <div v-if="isLoading" class="h-100 d-flex align-center justify-center">
        <v-progress-circular
          indeterminate
          color="primary"
        ></v-progress-circular>
      </div>
      <nuxt v-else />
    </v-main>
    <BottomNav />
  </v-app>
</template>

<script>
import MenuIcon from "../components/icons/MenuIcon.vue";
import ReloadIcon from "../components/icons/ReloadIcon";
import SideNav from "../components/layout/SideNav.vue";
import BottomNav from "../components/layout/BottomNav.vue";
import TopBar from "../components/layout/TopBar.vue";
export default {
  components: {
    ReloadIcon,
    MenuIcon,
    SideNav,
    TopBar,
    BottomNav,
  },
  data() {
    return {
      isLoading: false,
      showNav: false,
      showAddPostDialog: false,
      fixed: false,
      value: "recent",
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
    this.$nuxt.$on("showAddPostDialog", () => (this.showAddPostDialog = true));
  },
  beforeDestroy() {
    this.$nuxt.$off("pageTitleChange");
    this.$nuxt.$off("setMainLoading");
    this.$nuxt.$off("showAddPostDialog");
  },
};
</script>

<style scoped>
#navMenu {
  max-height: 100% !important;
}
</style>

