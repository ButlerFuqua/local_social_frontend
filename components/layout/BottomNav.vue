<template>
  <v-bottom-navigation hide-on-scroll fixed app color="primary" grow>
    <v-btn
      v-for="btn in buttons"
      :key="btn.text"
      :value="btn.text"
      @click="updateNav(btn)"
    >
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
      console.log("value", this.value);
      const { path } = btn;
      if (path !== "POST") this.$nuxt.$router.push(path);
      else console.log(this.$nuxt.$emit("showAddPostDialog"));
    },
    updateValue(newValue) {
      this.value = newValue;
    },
  },
  created() {
    // if (this.startingBtn) this.value = this.startingBtn;
    // const { name } = this.$nuxt.$route;
    // // console.log("name", name);
    // this.value = 2;
    // // else this.value = "bulletin";
  },
};
</script>