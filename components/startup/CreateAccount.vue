<template>
  <v-card>
    <div class="pa-4 text-center">
      <p>CreateAccount</p>
      <v-text-field
        filled
        label="Email"
        placeholder="me@site.com"
        v-model="emailInput"
      ></v-text-field>
      <v-text-field
        filled
        type="password"
        label="Password"
        v-model="passwordInput"
      ></v-text-field>
      <v-btn @click="createAccountHandler" rounded color="primary" dark
        >Create Account</v-btn
      >
    </div>
  </v-card>
</template>

<script>
export default {
  name: "CreateAccount",
  props: ["toggleLoading", "updateErrorMessage"],
  components: {},
  data() {
    return {
      emailInput: "",
      passwordInput: "",
    };
  },
  methods: {
    async createAccountHandler() {
      this.updateErrorMessage(null);
      this.toggleLoading(true);
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const fakeCreateAccountPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `Account has been created`,
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error creating your account.`,
                },
              });
          }, fakeWaitTime);
        });
      //REPLACE TO -------------------------------------------------------

      let response;
      try {
        response = await fakeCreateAccountPromise();
      } catch (error) {
        response = error;
      }

      const {
        data: { success, message },
      } = response;

      this.toggleLoading(false);

      if (!success) return this.updateErrorMessage(message);

      // Navigate to bulletin
    },
  },
};
</script>