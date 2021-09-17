<template>
  <v-card class="my-3" outlined>
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
            <v-chip outlined color="blue lighten-2">JS</v-chip>
            <v-chip
              outlined
              :color="
                post.source === 'friend' ? 'blue lighten-4' : 'green lighten-4'
              "
              >{{ post.source[0].toUpperCase() }}</v-chip
            >
          </div>
        </div>
        <v-card flat class="pa-2">
          {{ post.body }}
        </v-card>
      </v-list-item-content>
    </v-list-item>

    <v-card-actions>
      <v-btn rounded text color="secondary" @click="showPost(post)">
        Comment ({{ post.comments.length }})
      </v-btn>
    </v-card-actions>
    <v-divider></v-divider>
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