<template>
  <div id="container">
    <v-chip v-if="errorMessage" class="mb-2" color="red" dark>{{
      errorMessage
    }}</v-chip>
    <v-progress-circular
      v-if="isLoading"
      indeterminate
      color="primary"
    ></v-progress-circular>
    <div v-if="!isLoading" class="d-flex flex-column">
      <Login
        v-if="screen.login"
        :toggleLoading="toggleLoading"
        :updateErrorMessage="updateErrorMessage"
      />
      <CreateAccount
        v-if="screen.createAccount"
        :toggleLoading="toggleLoading"
        :updateErrorMessage="updateErrorMessage"
      />
      <ForgotPassword
        v-if="screen.forgotPassword"
        :showConfirmPasswordCodeScreen="() => updateScreen('confirmCode')"
        :toggleLoading="toggleLoading"
        :updateErrorMessage="updateErrorMessage"
      />
      <ConfirmPasswordCode
        v-if="screen.confirmCode"
        :toggleLoading="toggleLoading"
        :updateErrorMessage="updateErrorMessage"
      />
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
      isLoading: false,
      errorMessage: null,
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
    toggleLoading(bool) {
      if (typeof bool !== "undefined") this.isLoading = bool;
      else this.isLoading = !this.isLoading;
    },
    updateErrorMessage(message) {
      this.errorMessage = message;
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

