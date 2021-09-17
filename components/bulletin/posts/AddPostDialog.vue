<template>
  <div class="text-center">
    <v-dialog v-model="showAddPostDialog" width="500">
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
          Add post
        </v-card-title>

        <v-form class="pa-2" ref="form" v-model="formIsValid">
          <v-textarea
            filled
            label="Post"
            placeholder=""
            v-model="postBody"
            :rules="bodyRules"
          ></v-textarea>
        </v-form>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="secondary" text @click="togglePostDialog(false)">
            Cancel
          </v-btn>
          <v-btn color="primary" text @click="addPostHandler"> Submit </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: "AddPostDialog",
  props: ["showAddPostDialog", "togglePostDialog", "fetchPosts"],
  data() {
    return {
      isLoading: false,
      errorMessage: null,
      formIsValid: false,
      postBody: "",
      postAuthor: "",
      bodyRules: [(v) => !!v || "Post body is required"],
    };
  },
  methods: {
    async addPostHandler() {
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
                  message: `Post has been submitted to the bulletin.`,
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error adding your post to the bulletin.`,
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
      const isOnBulletin = this.$nuxt.$route.name === "bulletin";
      if (isOnBulletin) {
        if (this.fetchPosts) this.fetchPosts();
        else this.$nuxt.$emit("fetchAllPosts");
      }
      setTimeout(() => this.clearPost(), 500);
    },
    clearPost() {
      this.isLoading = false;
      this.formIsValid = false;
      this.postBody = "";
      this.postAuthor = "";
      this.togglePostDialog(false);
    },
  },
};
</script>