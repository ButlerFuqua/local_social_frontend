<template>
  <div>
    <v-card v-if="!confirmed">
      <div class="pa-4 text-center">
        <p>Confirm Code</p>
        <v-text-field
          filled
          label="Confirmation Code"
          v-model="codeInput"
        ></v-text-field>
        <v-btn @click="confirmCodeHandler" rounded color="primary" dark
          >Confirm</v-btn
        >
      </div>
    </v-card>
    <v-card v-else>
      <div class="pa-4 text-center">
        <p>New Password</p>
        <v-text-field
          filled
          type="password"
          label="New Password"
          v-model="password1Input"
        ></v-text-field>
        <v-text-field
          filled
          type="password"
          label="Confirm Password"
          v-model="password2Input"
        ></v-text-field>
        <v-btn @click="confirmCodeHandler" rounded color="primary" dark
          >Login</v-btn
        >
      </div>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "ConfirmCode",
  props: ["toggleLoading", "updateErrorMessage"],
  components: {},
  data() {
    return {
      codeInput: "",
      confirmed: false,
      password1Input: "",
      password2Input: "",
    };
  },
  methods: {
    async confirmCodeHandler() {
      this.updateErrorMessage(null);
      this.toggleLoading(true);
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

      this.toggleLoading(false);

      if (!success) return this.updateErrorMessage(message);

      this.confirmed = true;
    },
  },
};
</script>