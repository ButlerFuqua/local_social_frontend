<template>
  <div id="mainNavContainer" :class="shown ? 'shown' : ''">
    <div id="mainNavContent" calss="pa-2">
      <v-btn id="closeBtn" @click="hideNav" class="my-2" fab small dark
        >X</v-btn
      >
      <br />
      <v-btn
        v-for="(btn, idx) in navButtons"
        :key="idx"
        class="my-2"
        rounded
        :color="$nuxt.$route.fullPath === btn.path ? 'primary' : ''"
        @click="handleNavClick(btn)"
        >{{ btn.text }}</v-btn
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "MainNav",
  props: ["shown", "hideNav"],
  data() {
    return {
      navButtons: [
        {
          text: "Bulletin",
          path: "/bulletin",
        },
        {
          text: "Threads",
          path: "/threads",
        },
        {
          text: "Members",
          path: "/members",
        },
        {
          text: "logout",
          path: "/logout",
        },
      ],
    };
  },
  methods: {
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
    handleLogout() {
      // Remove logout from local Storage

      // Navigate to login
      this.$nuxt.$router.push("/");
    },
  },
};
</script>

<style lang="scss" scoped>
#mainNavContainer {
  position: fixed;
  height: 100%;
  width: 100%;
  z-index: 6;
  background: url("~assets/images/landing_background.jpg");
  background-size: cover;
  background-position: center;

  right: 100%;
  opacity: 0;
  transition: all 0.3s;

  &.shown {
    right: 0;
    opacity: 1;
  }
}
#mainNavContent {
  height: 100%;
  width: 100%;
  background-image: linear-gradient(to top, #c471f5cc 0%, #fa71cdb8 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
}
#closeBtn {
  position: absolute;
  bottom: 10px;
}
</style>