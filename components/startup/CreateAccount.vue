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
  props: [],
  components: {},
  data() {
    return {
      emailInput: "",
      passwordInput: "",
    };
  },
  methods: {
    createAccountHandler() {
      console.log("CreateAccount!!!");
    },
  },
};
</script>