<template>
  <v-card class="my-3">
    <v-list-item three-line>
      <v-list-item-content>
        <div class="d-flex justify-space-between align-center">
          <div class="d-flex align-center">
            <v-list-item-avatar
              circle
              size="40"
              color="grey lighten-3"
              class="ml-0"
            ></v-list-item-avatar>
            <v-chip outlined color="grey darken-1" class="mr-1">{{
              post.username
            }}</v-chip>
          </div>
          <div>
            <v-chip outlined color="teal ">JS</v-chip>
            <v-chip
              outlined
              :color="post.source === 'friend' ? 'blue ' : 'green'"
              >{{ post.source[0].toUpperCase() }}</v-chip
            >
          </div>
        </div>
        <div class="pa-2">
          {{ post.body }}
        </div>
      </v-list-item-content>
    </v-list-item>
    <v-card-actions class="d-flex justify-space-between">
      <v-btn rounded text color="secondary" @click="showPost(post)">
        Comment ({{ post.comments.length }})
      </v-btn>
      <v-btn small rounded text color="red lighten-3"> Report </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "JustSayingPost",
  props: ["post", "showPost"],
};
</script>

<style lang="scss" scoped>
</style>