<template>
  <v-list three-line subheader>
    <v-subheader>Comments</v-subheader>
    <v-list-item v-for="(comment, idx) in comments" :key="idx">
      <v-list-item-content>
        <div class="d-flex align-center">
          <v-list-item-avatar
            circle
            size="30"
            color="grey lighten-3"
          ></v-list-item-avatar>
          <v-list-item-subtitle>{{ comment.author }}</v-list-item-subtitle>
        </div>
        <v-list-item-title>{{ comment.body }}</v-list-item-title>
        <v-divider class="my-4"></v-divider>
      </v-list-item-content>
    </v-list-item>
  </v-list>
</template>

<script>
export default {
  name: "CommentList",
  props: ["comments"],
};
</script>