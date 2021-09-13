<template>
  <JustSayingPost
    v-if="post.postType === 'just_saying'"
    :post="post"
    :showPost="showPost"
  />
</template>

<script>
import JustSayingPost from "./JustSayingPost.vue";
import ErrorMessage from "../../progress/ErrorMessage.vue";
export default {
  name: "PostItem",
  props: ["post", "showPost"],
  components: { JustSayingPost, ErrorMessage },
};
</script>