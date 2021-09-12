<template>
  <v-progress-circular
    v-if="isLoading"
    indeterminate
    color="primary"
  ></v-progress-circular>
  <div v-else class="d-flex flex-column">
    <v-chip v-if="errorMessage" class="mb-2" color="red" dark>{{
      errorMessage
    }}</v-chip>
    <v-card v-if="!confirmed">
      <div class="pa-4 text-center">
        <p>Confirm Code</p>
        <v-form ref="confirmForm" v-model="confirmFormIsValid">
          <v-text-field
            filled
            label="Confirmation Code"
            v-model="codeInput"
            :rules="codeRules"
          ></v-text-field>
          <v-btn @click="confirmCodeHandler" rounded color="primary" dark
            >Confirm</v-btn
          >
        </v-form>
      </div>
    </v-card>
    <v-card v-else>
      <div class="pa-4 text-center">
        <p>New Password</p>
        <v-form ref="newPasswordForm" v-model="newPasswordFormIsValid">
          <v-text-field
            filled
            type="password"
            label="New Password"
            v-model="password1Input"
            :rules="password1Rules"
          ></v-text-field>
          <v-text-field
            filled
            type="password"
            label="Confirm Password"
            v-model="password2Input"
            :rules="password2Rules"
          ></v-text-field>
          <v-btn @click="newPasswordHandler" rounded color="primary" dark
            >Login</v-btn
          >
        </v-form>
      </div>
    </v-card>
    <v-btn
      class="mt-2"
      @click="updateScreen('login')"
      rounded
      color="secondary"
      dark
    >
      Back to Login
    </v-btn>
  </div>
</template>

<script>
export default {
  name: "ConfirmCode",
  props: ["updateScreen"],
  components: {},
  data() {
    return {
      isLoading: false,
      errorMessage: null,
      confirmFormIsValid: false,
      newPasswordFormIsValid: false,
      codeInput: "777",
      confirmed: false,
      password1Input: "password",
      password2Input: "password",
      codeRules: [(v) => !!v || "Confirmation code is required"],
      password1Rules: [(v) => !!v || "Password is required"],
      password2Rules: [(v) => !!v || "Password confirmation is required"],
    };
  },
  methods: {
    async confirmCodeHandler() {
      // Show errors if not valid
      this.$refs.confirmForm.validate();

      // Return if not valid
      if (!this.confirmFormIsValid) return;

      this.errorMessage = null;
      this.isLoading = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const fakeConfrimPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `User has been confirmed`,
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error with the confirmation.`,
                },
              });
          }, fakeWaitTime);
        });
      //REPLACE TO -------------------------------------------------------

      let response;
      try {
        response = await fakeConfrimPromise();
      } catch (error) {
        response = error;
      }

      const {
        data: { success, message },
      } = response;

      this.isLoading = false;

      if (!success) return (this.errorMessage = message);

      this.confirmed = true;
    },
    async newPasswordHandler() {
      // Show errors if not valid
      this.$refs.newPasswordForm.validate();

      // Return if not valid
      if (!this.newPasswordFormIsValid) return;

      this.errorMessage = null;
      this.isLoading = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const fakeConfrimPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `User has been confirmed`,
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error with the confirmation.`,
                },
              });
          }, fakeWaitTime);
        });
      //REPLACE TO -------------------------------------------------------

      let response;
      try {
        response = await fakeConfrimPromise();
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

      // Send snackbar notification

      // Navigate to Bulletin
      this.$nuxt.$router.push("/bulletin");
    },
  },
};
</script>