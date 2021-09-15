<template>
  <div
    id="threadsContainer"
    class="align-stretch align-stretch pa-2 pb-5 mb-5"
    :class="!errorMessage ? '' : 'justify-center'"
  >
    <ErrorMessage v-if="errorMessage" :errorMessage="errorMessage" />
    <div v-if="!errorMessage" class="d-flex justify-space-around align-center">
      <ThreadFilters :applyFilter="applyFilter" />
      <AddThread :fetchAllThreads="fetchAllThreads" />
    </div>
    <ThreadList
      v-if="!errorMessage"
      :loadingThreads="loadingThreads"
      :threads="threads"
      :appliedFilter="appliedFilter"
    />
    <EmptyThreads
      v-if="!errorMessage && !loadingThreads && threads.length < 1"
    />
  </div>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
import ThreadFilters from "../components/threads/filters/ThreadFilters.vue";
import AddThread from "../components/threads/AddThread.vue";
import ThreadList from "../components/threads/ThreadList.vue";
import EmptyThreads from "../components/threads/EmptyThreads.vue";
import ErrorMessage from "../components/progress/ErrorMessage.vue";
export default {
  head: generateHeadTags(
    "Creek bank Threads",
    "Social groups for citizens of Ohatchee, AL.",
    "thumbnail",
    null,
    true
  ),
  components: {
    ThreadFilters,
    ThreadList,
    EmptyThreads,
    ErrorMessage,
    AddThread,
  },
  data() {
    return {
      pageTitle: "Creek bank Threads",
      errorMessage: null,
      loadingThreads: false,
      errorMessage: null,
      appliedFilter: "All",
      threads: [],
    };
  },
  methods: {
    applyFilter(str) {
      this.appliedFilter = str;
    },
    async fetchAllThreads() {
      this.showAddThreadDialog = false;
      this.errorMessage = null;
      this.loadingThreads = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const simulateNoThreads = false;
      const fakeFetchPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `All threads fetched`,
                  threads: !simulateNoThreads
                    ? Array(10)
                        .fill(1)
                        .map((num, idx) => ({
                          _id: `${Math.random() * (15000 * idx)}`,
                          title: `Thread ${idx + num}`,
                          description: `Here is the description of thread ${
                            idx + num
                          }`,
                          owner: `GUID_${idx + num}`,
                          members: Array(Math.floor(Math.random() * 15)).fill(
                            Math.random().toString()
                          ),
                        }))
                    : [],
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error fetching threads.`,
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
        data: { success, message, threads },
      } = response;

      if (!success) {
        this.errorMessage = message;
      }
      // Load threads
      if (!threads) {
        this.errorMessage = "There was an error. No threads were found.";
      } else {
        this.threads = threads;
      }
      this.loadingThreads = false;
    },
    async init() {
      await this.fetchAllThreads();
    },
  },
  async created() {
    this.$nuxt.$emit("pageTitleChange", this.pageTitle);
    await this.init();
  },
};
</script>

<style lang="scss" scoped>
#threadsContainer {
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