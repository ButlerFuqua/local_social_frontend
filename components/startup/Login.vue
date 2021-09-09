<template>
  <v-card>
    <div class="pa-4 text-center">
      <p>Login</p>
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
      <v-btn @click="loginHandler" rounded color="primary" dark>Login</v-btn>
      <v-btn @click="forgotPasswordHandler" text small color="primary"
        >Forgot Password</v-btn
      >
    </div>
  </v-card>
</template>

<script>
export default {
  name: "Login",
  props: ["forgotPasswordAction"],
  components: {},
  data() {
    return {
      emailInput: "",
      passwordInput: "",
    };
  },
  methods: {
    forgotPasswordHandler() {
      this.forgotPasswordAction();
    },
    loginHandler() {
      console.log("LOGIN!!!");
    },
  },
};
</script>