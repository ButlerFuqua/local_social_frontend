<template>
  <v-row justify="center">
    <v-dialog
      v-model="showDialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <AddCommentDialog
        :showAddCommentDialog="showAddCommentDialog"
        :closeAddCommentDialog="() => (showAddCommentDialog = false)"
        :postId="post._id"
        :fetchPost="fetchPost"
      />
      <v-card tile class="pb-5">
        <v-toolbar dark color="secondary">
          <v-btn icon dark @click="closePostDialog">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>{{ post.username }}</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn dark text @click="closePostDialog"> Close </v-btn>
          </v-toolbar-items>
        </v-toolbar>
        <v-list three-line subheader>
          <v-list-item>
            <v-list-item-content> {{ post.body }} </v-list-item-content>
          </v-list-item>
        </v-list>
        <v-divider></v-divider>

        <!-- Skeleton loader -->
        <CommentSkeletons v-if="loadingComments" />

        <!-- Error Message -->
        <div
          v-else-if="errorMessage"
          class="d-flex justify-center align-center pa-3"
        >
          <ErrorMessage :errorMessage="errorMessage" />
        </div>

        <!-- No comments message -->
        <div
          v-else-if="comments.length < 1"
          class="d-flex flex-column justify-center align-center pa-3"
        >
          <p>There are no comments yet.</p>
          <v-btn
            rounded
            text
            color="primary"
            @click="showAddCommentDialog = true"
            >Be the first! 😀</v-btn
          >
        </div>

        <!-- List of comments -->
        <CommentList v-else :comments="comments" />
        <br />
        <div id="addCommentBtnContainer" class="d-flex justify-center pb-2">
          <v-btn rounded color="primary" @click="showAddCommentDialog = true">
            Add Comment
          </v-btn>
        </div>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import ErrorMessage from "../../progress/ErrorMessage.vue";
import CommentSkeletons from "./comments/CommentSkeletons.vue";
import CommentList from "./comments/CommentList.vue";
import AddCommentDialog from "./comments/AddCommentDialog.vue";
export default {
  name: "PostDialog",
  props: ["post", "showDialog", "closePostDialog"],
  components: { ErrorMessage, CommentSkeletons, CommentList, AddCommentDialog },
  data() {
    return {
      loadingComments: true,
      errorMessage: null,
      notifications: false,
      sound: true,
      widgets: false,
      comments: [],
      showAddCommentDialog: false,
    };
  },
  methods: {
    async fetchPost() {
      this.errorMessage = null;
      this.loadingComments = true;
      this.showAddCommentDialog = false;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const simulateNoComments = false;
      const fakeFetchPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `Coimments have been fetched.`,
                  comments: !simulateNoComments
                    ? this.post.comments.map((comment, idx) => ({
                        body: `Comment ${comment} ${idx + 1}`,
                        author: `Author ${idx + 1}`,
                      }))
                    : [],
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error fetching the comments.`,
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
        data: { success, message, comments: resComments },
      } = response;

      this.loadingComments = false;

      if (!success) {
        this.errorMessage = message;
        return;
      }

      // Inject comments
      this.comments = resComments;
    },
  },
  async created() {
    await this.fetchPost();
  },
};
</script>

<style lang='scss' scoped>
#addCommentBtnContainer {
  position: fixed;
  bottom: 0;
  width: 100%;
}
</style>