<template>
  <v-row justify="center">
    <v-dialog
      v-model="showDialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <v-card tile>
        <v-toolbar dark color="secondary">
          <v-btn icon dark @click="closeMemberDialog">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>Member Profile</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn dark text @click="closeMemberDialog"> Close </v-btn>
          </v-toolbar-items>
        </v-toolbar>
        <v-list three-line subheader>
          <v-list-item>
            <v-list-item-content v-if="isLoading && !member">
              Loading...
            </v-list-item-content>
            <v-list-item-content v-else>
              {{ member.about }}
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
  name: "ViewMemberDialog",
  props: ["memberId", "showDialog", "closeMemberDialog"],
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
      member: null,
    };
  },
  methods: {
    async fetchMember() {
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
                  message: `Member has been fetched`,
                  member: {
                    _id: this.memberId,
                    username: "username",
                    about: "About this user...",
                  },
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error fetching the member.`,
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
        data: { success, message, member: fetchedMember },
      } = response;

      this.isLoading = false;

      if (!success) {
        this.errorMessage = message;
        return;
      }

      this.member = fetchedMember;
    },
  },
  async created() {
    await this.fetchMember();
  },
};
</script>

<style lang='scss' scoped>
</style>