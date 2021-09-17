<template>
  <div
    id="membersContainer"
    class="align-stretch justify-start pa-2 pb-5 mb-5"
    :class="!errorMessage ? '' : 'justify-center'"
  >
    <ErrorMessage v-if="errorMessage" :errorMessage="errorMessage" />
    <div class="mt-1">
      <MemberFilters v-if="!errorMessage" :applyFilter="applyFilter" />
      <v-text-field
        class="mt-2"
        label="Search for Member"
        prepend-inner-icon="mdi-magnify"
        filled
        dense
        v-model="memberSearch"
        @input="fetchSearchedMembers"
      ></v-text-field>
    </div>
    <MemberList
      v-if="!errorMessage"
      :loadingMembers="loadingMembers"
      :members="members"
      :appliedFilter="appliedFilter"
    />
  </div>
</template>

<script>
import generateHeadTags from "../lib/generateMeta";
import MemberList from "../components/members/MemberList.vue";
import ErrorMessage from "../components/progress/ErrorMessage.vue";
import MemberFilters from "../components/members/MemberFilters.vue";
export default {
  head: generateHeadTags(
    "Creek bank Members",
    "People of Ohatchee, Al that have joined Creek Bank Social.",
    "thumbnail",
    null,
    true
  ),
  components: {
    MemberList,
    ErrorMessage,
    MemberFilters,
  },
  data() {
    return {
      pageTitle: "Creek bank Members",
      errorMessage: null,
      loadingMembers: false,
      errorMessage: null,
      appliedFilter: "All",
      members: [],
      memberSearch: "",
    };
  },
  methods: {
    applyFilter(str) {
      return;
      this.appliedFilter = str;
    },
    async fetchAllMembers() {
      this.showAddMemberDialog = false;
      this.errorMessage = null;
      this.loadingMembers = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 600;
      const simulateError = false;
      const simulateNoMembers = false;
      const fakeFetchPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `All members fetched`,
                  members: !simulateNoMembers
                    ? Array(10)
                        .fill(1)
                        .map((num, idx) => ({
                          _id: `${Math.random() * (15000 * idx)}`,
                          username: `username_${idx + num}`,
                          about: `Here is a little about member ${idx + num}`,
                        }))
                    : [],
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error fetching members.`,
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
        data: { success, message, members },
      } = response;

      if (!success) {
        this.errorMessage = message;
      }
      // Load members
      if (!members) {
        this.errorMessage = "There was an error. No members were found.";
      } else {
        this.members = members;
      }
      this.loadingMembers = false;
    },
    async fetchSearchedMembers() {
      if (this.memberSearch.trim() === "") return this.fetchAllMembers();

      if (this.memberSearch.trim().length < 3) return;

      this.showAddMemberDialog = false;
      this.errorMessage = null;
      this.loadingMembers = true;
      //REPLACE FROM -------------------------------------------------------
      const fakeWaitTime = 300;
      const simulateError = false;
      const simulateNoMembers = false;
      const fakeFetchPromise = () =>
        new Promise((resolve, reject) => {
          setTimeout(() => {
            if (!simulateError)
              resolve({
                data: {
                  success: true,
                  message: `All members fetched`,
                  members: !simulateNoMembers
                    ? Array(10)
                        .fill(1)
                        .map((num, idx) => ({
                          _id: `${Math.random() * (15000 * idx)}`,
                          username: `username_${idx + num}`,
                          about: `Here is a little about member ${idx + num}`,
                        }))
                        .filter((member) =>
                          member.username
                            .toLowerCase()
                            .includes(this.memberSearch.toLowerCase())
                        )
                    : [],
                },
              });
            else
              reject({
                data: {
                  success: false,
                  message: `There was an error fetching members.`,
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
        data: { success, message, members },
      } = response;

      if (!success) {
        this.errorMessage = message;
      }
      // Load members
      if (!members) {
        this.errorMessage = "There was an error. No members were found.";
      } else {
        this.members = members;
      }
      this.loadingMembers = false;
    },
    async init() {
      await this.fetchAllMembers();
    },
  },
  async created() {
    this.$nuxt.$emit("pageTitleChange", this.pageTitle);
    await this.init();
  },
};
</script>

<style lang="scss" scoped>
#membersContainer {
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