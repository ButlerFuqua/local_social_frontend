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
  props: ["showConfirmPasswordCodeScreen"],
  components: {},
  data() {
    return {
      emailInput: "",
      passwordInput: "",
    };
  },
  methods: {
    forgotPasswordHandler() {
      console.log("ForgotPassword!!!");
      this.showConfirmPasswordCodeScreen();
    },
  },
};
</script>