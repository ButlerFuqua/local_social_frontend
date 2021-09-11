<template>
  <v-card>
    <div class="pa-4 text-center">
      <p>Confirm Account</p>
      <v-form ref="form" v-model="formIsValid">
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
</template>

<script>
export default {
  name: "CreateAccount",
  props: ["toggleLoading", "updateErrorMessage"],
  components: {},
  data() {
    return {
      emailInput: "",
      codeInput: "",
      formIsValid: false,
      codeRules: [(v) => !!v || "Confirmation code is required"],
    };
  },
  methods: {
    async confirmCodeHandler() {
      // Show errors if not valid
      this.$refs.form.validate();

      // Return if not valid
      if (!this.formIsValid) return;

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
                  message: `Account has been confirmed`,
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error confirming your account.`,
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