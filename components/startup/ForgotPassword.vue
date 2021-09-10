<template>
  <v-card>
    <div class="pa-4 text-center">
      <p>Forgot Password</p>
      <v-text-field
        filled
        label="Email"
        placeholder="me@site.com"
        v-model="emailInput"
      ></v-text-field>
      <v-btn @click="forgotPasswordHandler" rounded color="primary" dark
        >Send Code</v-btn
      >
    </div>
  </v-card>
</template>

<script>
export default {
  name: "ForgotPassword",
  props: [
    "showConfirmPasswordCodeScreen",
    "toggleLoading",
    "updateErrorMessage",
  ],
  components: {},
  data() {
    return {
      emailInput: "",
      passwordInput: "",
    };
  },
  methods: {
    async forgotPasswordHandler() {
      this.updateErrorMessage(null);
      this.toggleLoading(true);
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const fakeLoginPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `User is logged in`,
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error logging in.`,
                },
              });
          }, fakeWaitTime);
        });
      //REPLACE TO -------------------------------------------------------

      let response;
      try {
        response = await fakeLoginPromise();
      } catch (error) {
        response = error;
      }

      const {
        data: { success, message },
      } = response;

      this.toggleLoading(false);

      if (!success) return this.updateErrorMessage(message);

      this.showConfirmPasswordCodeScreen();
    },
  },
};
</script>