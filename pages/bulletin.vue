<template>
  <div
    id="bulletinContainer"
    class="align-stretch align-stretch pa-2 pb-5 mb-5"
    :class="!errorMessage ? '' : 'justify-center'"
  >
    <AddPostDialog
      :showAddPostDialog="showAddPostDialog"
      :closeAddPostDialog="() => (showAddPostDialog = false)"
      :fetchPosts="fetchAllPosts"
    />
    <ErrorMessage v-if="errorMessage" :errorMessage="errorMessage" />
    <BulletinFilters v-if="!errorMessage" :applyFilter="applyFilter" />
    <PostList
      v-if="!errorMessage"
      :loadingPosts="loadingPosts"
      :posts="posts"
      :appliedFilter="appliedFilter"
    />
    <EmptyBulletin v-if="!errorMessage && !loadingPosts && posts.length < 1" />
    <v-btn
      v-if="!errorMessage && !loadingPosts"
      class="bottomRightFab"
      fab
      color="primary"
      @click="showAddPostDialog = true"
    >
      <v-icon>+</v-icon>
    </v-btn>
  </div>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
import BulletinFilters from "../components/bulletin/BulletinFilters.vue";
import PostList from "../components/bulletin/posts/PostList.vue";
import EmptyBulletin from "../components/bulletin/EmptyBulletin.vue";
import ErrorMessage from "../components/progress/ErrorMessage.vue";
import AddPostDialog from "../components/bulletin/posts/AddPostDialog.vue";
export default {
  head: generateHeadTags(
    "Creek bank Bulletin",
    "A public bulletin for citizens of Ohatchee, AL.",
    "thumbnail",
    null,
    true
  ),
  components: {
    BulletinFilters,
    PostList,
    EmptyBulletin,
    ErrorMessage,
    AddPostDialog,
  },
  data() {
    return {
      pageTitle: "Creek bank Bulletin",
      showAddPostDialog: false,
      errorMessage: null,
      loadingPosts: false,
      errorMessage: null,
      appliedFilter: "All",
      posts: [],
    };
  },
  methods: {
    applyFilter(str) {
      this.appliedFilter = str;
    },
    async fetchAllPosts() {
      this.showAddPostDialog = false;
      this.errorMessage = null;
      this.loadingPosts = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const simulateNoPosts = false;
      const fakeFetchPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `All posts fetched`,
                  posts: !simulateNoPosts
                    ? Array(10)
                        .fill(1)
                        .map((num, idx) => ({
                          _id: `${Math.random() * (15000 * idx)}`,
                          postType: "just_saying",
                          username: `User ${idx + num}`,
                          body: `Body of post ${idx + num}`,
                          source:
                            Math.floor(Math.random() * 100) < 50
                              ? "thread"
                              : "friend",
                          comments: Array(Math.floor(Math.random() * 15)).fill(
                            Math.random().toString()
                          ),
                        }))
                    : [],
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
#bulletinContainer {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  overflow: auto;
  background-image: linear-gradient(120deg, #fccb90 0%, #d57eeb 100%);
  background-attachment: fixed;
}
</style>

/**Why you should do it regularly:
https://github.com/browserslist/browserslist#browsers-data-updating
Current version: 1.0.30001170 */