<template>
  <v-card class="my-3">
    <!-- <v-card-title>{{ thread.title }}</v-card-title> -->
    <div class="px-3 pt-3">{{ thread.title }} Lorem ipsum dolor sit amet</div>
    <!-- <v-card-text>
      {{ thread.description }}
    </v-card-text> -->

    <v-card-actions>
      <v-btn rounded text color="secondary" @click="showThread(thread)">
        View
      </v-btn>
      <v-btn small rounded text color="primary" @click="showThread(thread)">
        Members (14)
      </v-btn>
    </v-card-actions>
  </v-card>
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