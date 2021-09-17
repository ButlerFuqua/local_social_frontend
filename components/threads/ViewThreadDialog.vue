<template>
  <v-row justify="center">
    <v-dialog
      v-model="showDialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <v-card tile>
        <v-toolbar dense dark color="secondary">
          <v-btn icon dark @click="closeThreadDialog">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title v-if="isLoading && !thread"
            >Loading...</v-toolbar-title
          >
          <v-toolbar-title v-else>{{ thread.title }}</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn dark text @click="joinThread"> Join </v-btn>
          </v-toolbar-items>
        </v-toolbar>
        <v-list three-line subheader>
          <v-list-item>
            <v-list-item-content v-if="isLoading && !thread">
              Loading...
            </v-list-item-content>
            <v-list-item-content v-else>
              {{ thread.description }}
            </v-list-item-content>
          </v-list-item>
        </v-list>
        <v-divider></v-divider>

        <!-- Error Message -->
        <div
          v-if="errorMessage"
          class="d-flex justify-center align-center pa-3"
        >
          <ErrorMessage :errorMessage="errorMessage" />
        </div>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import ErrorMessage from "../progress/ErrorMessage.vue";

export default {
  name: "ViewThreadDialog",
  props: ["threadId", "showDialog", "closeThreadDialog"],
  components: { ErrorMessage },
  data() {
    return {
      isLoading: true,
      errorMessage: null,
      notifications: false,
      sound: true,
      widgets: false,
      comments: [],
      showAddCommentDialog: false,
      thread: null,
    };
  },
  methods: {
    async joinThread() {
      console.log("Join Thread");
    },
    async fetchThread() {
      this.errorMessage = null;
      this.isLoading = true;
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
                  message: `Thread has been fetched`,
                  thread: {
                    _id: this.threadId,
                    title: "Title of the thread",
                    description: "Description of the thread",
                  },
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error fetching the thread.`,
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
        data: { success, message, thread: fetchedThread },
      } = response;

      this.isLoading = false;

      if (!success) {
        this.errorMessage = message;
        return;
      }

      this.thread = fetchedThread;
    },
  },
  async created() {
    await this.fetchThread();
  },
};
</script>

<style lang='scss' scoped>
</style>