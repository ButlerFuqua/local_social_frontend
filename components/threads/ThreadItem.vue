<template>
  <div>
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title>{{ thread.title }}</v-list-item-title>
        <v-list-item-subtitle>{{ thread.description }}</v-list-item-subtitle>
        <v-divider class="my-4"></v-divider>
      </v-list-item-content>
      <v-btn rounded outlined small color="secondary">View</v-btn>
    </v-list-item>
  </div>
</template>

<script>
export default {
  name: "ThreadItem",
  props: ["thread", "showThread"],
};
</script>

<style lang="scss" scoped>
.justsayingPostContainer {
  background: rgba(255, 255, 255, 0.1);
}
</style>