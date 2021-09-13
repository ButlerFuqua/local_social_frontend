<template>
  <div>
    <ViewPostDialog
      v-if="postToShow"
      :post="postToShow"
      :showDialog="showDialog"
      :closePostDialog="closePostDialog"
    />
    <div v-if="loadingPosts">
      <v-skeleton-loader
        v-for="(num, idx) in Array(skeletonPostsCount).fill(1)"
        :key="idx"
        type="article"
        class="my-2"
      ></v-skeleton-loader>
    </div>
    <div v-else-if="filteredPosts && filteredPosts.length > 0">
      <PostItem
        :showPost="showPost"
        v-for="(post, idx) in filteredPosts"
        :key="idx"
        :post="post"
      />
    </div>
  </div>
</template>

<script>
import PostItem from "./PostItem.vue";
import ViewPostDialog from "./ViewPostDialog.vue";
export default {
  name: "PostList",
  props: ["loadingPosts", "posts", "appliedFilter"],
  components: { PostItem, ViewPostDialog },
  data() {
    return {
      skeletonPostsCount: 12,
      showDialog: false,
      postToShow: null,
    };
  },
  methods: {
    closePostDialog() {
      this.showDialog = false;
      setTimeout(() => {
        this.postToShow = null;
      }, 500);
    },
    showPost(post) {
      this.postToShow = post;
      this.showDialog = true;
    },
  },
  computed: {
    filteredPosts() {
      if (!this.appliedFilter) return [...this.posts];

      const filter = this.appliedFilter.toLowerCase();

      if (filter === "all") return [...this.posts];
      if (filter === "threads")
        return this.posts.filter(
          (post) => post.source.toLowerCase() === "thread"
        );
      if (filter === "friends")
        return this.posts.filter(
          (post) => post.source.toLowerCase() === "friend"
        );
    },
  },
};
</script>