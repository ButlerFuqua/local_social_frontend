<template>
  <div class="text-center">
    <v-dialog v-model="showAddCommentDialog" width="500">
      <v-card
        v-if="isLoading"
        height="250"
        class="d-flex justify-center align-center"
      >
        <v-progress-circular
          indeterminate
          color="primary"
        ></v-progress-circular>
      </v-card>
      <v-card v-else>
        <v-card-title class="text-h6 primary lighten-2">
          Add comment
        </v-card-title>

        <v-form class="pa-2" ref="form" v-model="formIsValid">
          <v-textarea
            filled
            label="Comment"
            placeholder=""
            v-model="commentBody"
            :rules="bodyRules"
          ></v-textarea>
        </v-form>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="secondary" text @click="closeAddCommentDialog">
            Cancel
          </v-btn>
          <v-btn color="primary" text @click="addCommentHandler">
            Submit
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: "AddCommentDialog",
  props: ["showAddCommentDialog", "closeAddCommentDialog", "fetchPost"],
  data() {
    return {
      isLoading: false,
      errorMessage: null,
      formIsValid: false,
      commentBody: "",
      commentAuthor: "",
      bodyRules: [(v) => !!v || "Comment body is required"],
    };
  },
  methods: {
    async addCommentHandler() {
      // Show errors if not valid
      this.$refs.form.validate();

      // Return if not valid
      if (!this.formIsValid) return;

      this.errorMessage = null;
      this.isLoading = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const fakePostPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `Comment has been added to post.`,
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error adding your comment to the post.`,
                },
              });
          }, fakeWaitTime);
        });
      //REPLACE TO -------------------------------------------------------

      let response;
      try {
        response = await fakePostPromise();
      } catch (error) {
        response = error;
      }

      const {
        data: { success, message },
      } = response;

      if (!success) {
        this.errorMessage = message;
        this.isLoading = false;
        return;
      }

      // Close and Initialize parent Component
      this.fetchPost();
      setTimeout(() => this.clearComment(), 500);
    },
    clearComment() {
      this.isLoading = false;
      this.formIsValid = false;
      this.commentBody = "";
      this.commentAuthor = "";
    },
  },
};
</script>