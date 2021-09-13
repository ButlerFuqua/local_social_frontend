<template>
  <div
    id="container"
    :class="!isLoading ? 'align-stretch' : 'align-center justify-center'"
  >
    <v-progress-circular
      v-if="isLoading"
      indeterminate
      color="primary"
    ></v-progress-circular>
    <div
      v-if="!isLoading"
      class="d-flex flex-column pa-2 h-100"
      :class="!errorMessage ? '' : 'justify-center'"
    >
      <v-chip v-if="errorMessage" class="mb-2" color="red" dark>{{
        errorMessage
      }}</v-chip>
      <div v-if="loadingPosts">
        <v-skeleton-loader
          v-for="(num, idx) in Array(skeletonPostsCount).fill(1)"
          :key="idx"
          type="article"
          class="my-2"
        ></v-skeleton-loader>
      </div>
      <div v-else-if="posts.length > 0">
        <v-card class="my-2" outlined v-for="(post, idx) in posts" :key="idx">
          <v-list-item three-line>
            <v-list-item-avatar
              circle
              size="80"
              color="grey"
            ></v-list-item-avatar>
            <v-list-item-content>
              <div class="text-overline mb-4">{{ post.username }}</div>
              <p>{{ post.body }}</p>
            </v-list-item-content>
          </v-list-item>

          <v-card-actions>
            <v-btn rounded text color="primary"> Comment </v-btn>
          </v-card-actions>
        </v-card>
      </div>
      <div
        v-else-if="!errorMessage"
        class="h-100 d-flex flex-column justify-center align-center"
      >
        <h3 class="headline mb-4">Your Bulletin is empty! 🙀</h3>
        <v-btn class="mb-2" rounded color="primary">Make some friends 😀</v-btn>
        <p class="my-0">Or</p>
        <v-btn class="mt-2" rounded color="primary">Join some Threads 😄</v-btn>
      </div>
    </div>
  </div>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
export default {
  head: generateHeadTags(
    "Creek bank Bulletin",
    "A public bulletin for citizens of Ohatchee, AL.",
    "thumbnail",
    null,
    true
  ),
  components: {},
  data() {
    return {
      pageTitle: "Creek bank Bulletin",
      isLoading: false,
      errorMessage: null,
      loadingPosts: false,
      errorMessage: null,
      skeletonPostsCount: 15,
      posts: [],
    };
  },
  methods: {
    async fetchAllPosts() {
      this.errorMessage = null;
      this.loadingPosts = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const fakeFetchPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `All posts fetched`,
                  posts: Array(30)
                    .fill(1)
                    .map((num, idx) => ({
                      username: `User ${idx + num}`,
                      body: `Body of post ${idx + num}`,
                    })),
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error fetching posts.`,
                },
              });
          }, fakeWaitTime);
        });
      //REPLACE TO -------------------------------------------------------

      let response;
      try {
        response = await fakeFetchPromise();
      } catch (error) {
        response = error;
      }

      const {
        data: { success, message, posts },
      } = response;

      if (!success) {
        this.errorMessage = message;
      }
      // Load posts
      if (!posts) {
        this.errorMessage = "There was an error. No posts were found.";
      } else {
        this.posts = posts;
      }
      this.loadingPosts = false;
    },
    async init() {
      await this.fetchAllPosts();
    },
  },
  async created() {
    this.$nuxt.$emit("pageTitleChange", this.pageTitle);
    await this.init();
  },
};
</script>

<style lang="scss" scoped>
#container {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  overflow: auto;
  background-image: linear-gradient(to top, #ffc3a0 0%, #ffafbd 100%);
}
</style>

