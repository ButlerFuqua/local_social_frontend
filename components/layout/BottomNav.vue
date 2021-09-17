<template>
  <v-bottom-navigation
    hide-on-scroll
    fixed
    app
    :value="value"
    color="primary"
    grow
  >
    <v-btn v-for="btn in buttons" :key="btn.text" @click="updateNav(btn)">
      <span>{{ btn.text }}</span>
      <v-icon>{{ btn.icon }}</v-icon>
    </v-btn>
  </v-bottom-navigation>
</template>

<script>
export default {
  name: "BottomNaV",
  props: ["startingBtn"],
  data() {
    return {
      value: "bulletin",
      buttons: [
        {
          text: "Buelletin",
          icon: "mdi-bulletin-board",
          path: "/bulletin",
        },
        {
          text: "Threads",
          icon: "mdi-group",
          path: "/threads",
        },
        {
          text: "Members",
          icon: "mdi-account-multiple-outline",
          path: "/members",
        },
        {
          text: "Post",
          icon: "mdi-note-plus-outline",
          path: "POST",
        },
      ],
    };
  },
  methods: {
    updateNav(btn) {
      const { path } = btn;
      if (path !== "POST") this.$nuxt.$router.push(path);
      else console.log(this.$nuxt.$emit("showAddPostDialog"));
    },
    updateValue(newValue) {
      this.value = newValue;
    },
    handleNavClick(btn) {
      this.$nuxt.$emit("setMainLoading", true);

      const { text, path } = btn;

      // Hidenav and return if user selects teh current page
      if (path === this.$nuxt.$route.fullPath) {
        this.$nuxt.$emit("setMainLoading", false);
        this.hideNav();
        return;
      }

      // logout if user selects to logout
      if (text.toLowerCase() === "logout") this.handleLogout();
      // Navigate to selected page
      else
        this.$nuxt.$router.push(path, () => {
          this.hideNav();
          this.$nuxt.$emit("setMainLoading", false);
        });
    },
  },
  created() {
    if (this.startingBtn) this.value = this.startingBtn;
    else this.value = "bulletin";
  },
};
</script>