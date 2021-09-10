<template>
  <div id="container">
    <Login v-if="screen.login" />
    <CreateAccount v-if="screen.createAccount" />
    <ForgotPassword
      v-if="screen.forgotPassword"
      :showConfirmPasswordCodeScreen="() => updateScreen('confirmCode')"
    />
    <ConfirmPasswordCode v-if="screen.confirmCode" />
    <v-btn
      v-if="screen.login"
      class="mt-2"
      @click="updateScreen('createAccount')"
      rounded
      color="secondary"
      dark
    >
      Create Account
    </v-btn>
    <v-btn
      v-if="screen.createAccount"
      class="mt-2"
      @click="updateScreen('login')"
      rounded
      color="secondary"
      dark
    >
      Login
    </v-btn>
    <v-btn
      v-if="screen.login"
      class="mt-2"
      @click="updateScreen('forgotPassword')"
      text
      rounded
      color="primary"
    >
      Forgot?
    </v-btn>
    <v-btn
      v-if="screen.forgotPassword"
      class="mt-2"
      @click="updateScreen('login')"
      rounded
      color="secondary"
      dark
    >
      Back
    </v-btn>
  </div>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
import Login from "../components/startup/Login.vue";
import CreateAccount from "../components/startup/CreateAccount.vue";
import ForgotPassword from "../components/startup/ForgotPassword.vue";
import ConfirmPasswordCode from "../components/startup/ConfirmPasswordCode.vue";
export default {
  layout: "landing",
  head: generateHeadTags(
    "Creek bank Social",
    "A social app for citizens of Ohatchee, AL.",
    "thumbnail",
    null,
    true
  ),
  components: {
    Login,
    CreateAccount,
    ForgotPassword,
    ConfirmPasswordCode,
  },
  data() {
    return {
      pageTitle: "Creek bank Social",
      screen: {
        login: true,
        createAccount: false,
        forgotPassword: false,
        confirmCode: false,
      },
    };
  },
  methods: {
    updateScreen(screenToShow) {
      for (let prop in this.screen) this.screen[prop] = false;

      this.screen[screenToShow] = true;
    },
  },
  async created() {
    this.$nuxt.$emit("pageTitleChange", this.pageTitle);
  },
};
</script>

<style lang="scss" scoped>
#container {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  overflow: auto;
  background: url("~assets/images/landing_background.jpg");
  background-size: cover;
  background-position: center;
}
</style>

