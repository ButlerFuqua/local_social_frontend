<template>
  <div>
    <v-skeleton-loader
      v-for="num in 8"
      :key="num"
      type="list-item-avatar-two-line, list-item"
    ></v-skeleton-loader>
  </div>
</template>

<script>
export default {
  name: "CommentSkeletons",
};
</script>