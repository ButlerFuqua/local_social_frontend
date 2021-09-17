<template>
  <v-app-bar hide-on-scroll dense fixed app flat class="white">
    <v-btn icon :aria-label="`Toggle Side nav`" @click.stop="toggleNav">
      <v-icon class="white"> mdi-circle </v-icon>
    </v-btn>
    <v-toolbar-title v-text="title" />
    <v-spacer />
    <v-btn :aria-label="`Reload to get updates`" icon>
      <v-icon>mdi-message-outline</v-icon>
      <!-- <v-icon>mdi-message-alert-outline</v-icon> -->
    </v-btn>
    <v-btn :aria-label="`Reload to get updates`" icon>
      <v-icon>mdi-bell-outline</v-icon>
      <!-- <v-icon>mdi-bell-badge-outline</v-icon> -->
    </v-btn>
  </v-app-bar>
</template>

<script>
export default {
  name: "TopBar",
  props: ["title", "toggleNav"],
};
</script>