<template>
  <div class="text-center">
    <v-dialog fullscreen v-model="showAddThreadDialog">
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
      <v-card tile v-else>
        <v-card-title class="text-h6 primary lighten-2">
          Add thread
        </v-card-title>

        <v-form class="pa-2" ref="form" v-model="formIsValid">
          <v-text-field
            filled
            label="Title"
            v-model="threadTitle"
            :rules="threadRules"
          ></v-text-field>
          <ActivitiesComboBox />
          <InterestsComboBox />
          <v-textarea
            filled
            label="Description of thread"
            v-model="threadDescription"
            :rules="descriptionRules"
          ></v-textarea>
        </v-form>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="secondary" text @click="closeAddThreadDialog">
            Cancel
          </v-btn>
          <v-btn color="primary" text @click="addThreadHandler"> Submit </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import ActivitiesComboBox from "./ActivitiesComboBox.vue";
import InterestsComboBox from "./InterestsComboBox.vue";
export default {
  name: "AddThreadDialog",
  props: ["showAddThreadDialog", "closeAddThreadDialog", "fetchThreads"],
  components: { ActivitiesComboBox, InterestsComboBox },
  data() {
    return {
      isLoading: false,
      errorMessage: null,
      formIsValid: false,
      threadTitle: "",
      threadDescription: "",
      threadAuthor: "",
      threadRules: [(v) => !!v || "Thread title is required"],
      descriptionRules: [(v) => true],
      selectedActivities: ["Vuetify", "Programming"],
      availableActivities: ["Programming", "Design", "Vue", "Vuetify"],
    };
  },
  methods: {
    async addThreadHandler() {
      // Show errors if not valid
      this.$refs.form.validate();

      // Return if not valid
      if (!this.formIsValid) return;

      this.errorMessage = null;
      this.isLoading = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const fakeThreadPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `Thread has been submitted to the bulletin.`,
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error adding your thread to the bulletin.`,
                },
              });
          }, fakeWaitTime);
        });
      //REPLACE TO -------------------------------------------------------

      let response;
      try {
        response = await fakeThreadPromise();
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
      this.fetchThreads();
      setTimeout(() => this.clearThread(), 500);
    },
    clearThread() {
      this.isLoading = false;
      this.formIsValid = false;
      this.threadTitle = "";
      this.threadAuthor = "";
      this.closeAddThreadDialog();
    },
  },
};
</script>