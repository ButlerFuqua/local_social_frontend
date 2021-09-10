<template>
  <div>
    <v-progress-circular
      v-if="isLoading"
      indeterminate
      color="primary"
    ></v-progress-circular>
    <v-card v-else-if="!confirmed">
      <div v-if="!isLoading" class="pa-4 text-center">
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
    <v-chip v-if="errorMessage" class="mt-2" color="red" dark>{{
      errorMessage
    }}</v-chip>
  </div>
</template>

<script>
export default {
  name: "ConfirmCode",
  props: [],
  components: {},
  data() {
    return {
      isLoading: false,
      codeInput: "",
      confirmed: false,
      password1Input: "",
      password2Input: "",
      errorMessage: null,
    };
  },
  methods: {
    async confirmCodeHandler() {
      this.errorMessage = null;
      this.isLoading = false;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = true;
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
  },
};
</script>