<template>
  <v-card>
    <div class="pa-4 text-center">
      <p>Login</p>
      <v-form ref="form" v-model="formIsValid">
        <v-text-field
          filled
          label="Email"
          placeholder="me@site.com"
          v-model="emailInput"
          :rules="emailRules"
        ></v-text-field>
        <v-text-field
          filled
          type="password"
          label="Password"
          v-model="passwordInput"
          :rules="passwordRules"
        ></v-text-field>
        <v-btn @click="loginHandler" rounded color="primary" dark>Login</v-btn>
      </v-form>
    </div>
  </v-card>
</template>

<script>
export default {
  name: "Login",
  props: ["toggleLoading", "updateErrorMessage"],
  components: {},
  data() {
    return {
      formIsValid: false,
      emailInput: "example@site.com",
      passwordInput: "password",
      emailRules: [
        (v) => !!v || "Email is required",
        (v) =>
          /(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])/.test(
            v
          ) || "E-mail must be valid",
      ],
      passwordRules: [(v) => !!v || "Password is required"],
    };
  },
  methods: {
    async loginHandler() {
      // Show errors if not valid
      this.$refs.form.validate();

      // Return if not valid
      if (!this.formIsValid) return;

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

      // Navigate to bulletin
      this.$nuxt.$router.push("bulletin");
    },
  },
};
</script>