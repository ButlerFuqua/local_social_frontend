<template>
  <div
    id="settingsContainer"
    class="align-stretch align-stretch pa-2 pb-5 mb-5"
    :class="!errorMessage ? '' : 'justify-center'"
  >
    <ErrorMessage v-if="errorMessage" :errorMessage="errorMessage" />
    <SettingList
      v-if="!errorMessage"
      :loadingSettings="loadingSettings"
      :settings="settings"
      :appliedFilter="appliedFilter"
    />
  </div>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
import SettingList from "../components/settings/SettingList.vue";
import ErrorMessage from "../components/progress/ErrorMessage.vue";
export default {
  head: generateHeadTags(
    "Settings",
    "Social groups for citizens of Ohatchee, AL.",
    "thumbnail",
    null,
    true
  ),
  components: {
    SettingList,
    ErrorMessage,
  },
  data() {
    return {
      pageTitle: "Settings",
      errorMessage: null,
      loadingSettings: false,
      errorMessage: null,
      appliedFilter: "All",
      settings: [],
    };
  },
  methods: {
    applyFilter(str) {
      this.appliedFilter = str;
    },
    async fetchAllSettings() {
      this.showAddSettingDialog = false;
      this.errorMessage = null;
      this.loadingSettings = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const simulateNoSettings = false;
      const fakeFetchPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `All settings fetched`,
                  settings: !simulateNoSettings
                    ? Array(10)
                        .fill(1)
                        .map((num, idx) => ({
                          _id: `${Math.random() * (15000 * idx)}`,
                          title: `Setting ${idx + num}`,
                        }))
                    : [],
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error fetching settings.`,
                },
              });
          }, fakeWaitTime);
        });
      //REPLACE TO -------------------------------------------------------

      let response;
      try {
        response = await fakeFetchPromise();
      } catch (error) {
        response = error;
      }

      const {
        data: { success, message, settings },
      } = response;

      if (!success) {
        this.errorMessage = message;
      }
      // Load settings
      if (!settings) {
        this.errorMessage = "There was an error. No settings were found.";
      } else {
        this.settings = settings;
      }
      this.loadingSettings = false;
    },
    async init() {
      await this.fetchAllSettings();
    },
  },
  async created() {
    this.$nuxt.$emit("pageTitleChange", this.pageTitle);
    await this.init();
  },
};
</script>

<style lang="scss" scoped>
#settingsContainer {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  overflow: auto;
  background-image: linear-gradient(120deg, #fccb90 0%, #d57eeb 100%);
  background-attachment: fixed;
}
</style>

/**Why you should do it regularly:
https://github.com/browserslist/browserslist#browsers-data-updating
Current version: 1.0.30001170 */