<template>
  <div>
    <ViewThreadDialog
      v-if="threadToShow"
      :threadId="threadToShow._id"
      :showDialog="showDialog"
      :closeThreadDialog="closeThreadDialog"
    />
    <div v-if="loadingThreads">
      <v-skeleton-loader
        v-for="(num, idx) in Array(skeletonThreadsCount).fill(1)"
        :key="idx"
        type="article"
        class="my-2"
      ></v-skeleton-loader>
    </div>
    <div v-else-if="filteredThreads && filteredThreads.length > 0">
      <ThreadItem
        :showThread="showThread"
        v-for="(thread, idx) in filteredThreads"
        :key="idx"
        :thread="thread"
      />
    </div>
  </div>
</template>

<script>
import ThreadItem from "./ThreadItem.vue";
import ViewThreadDialog from "./ViewThreadDialog.vue";
export default {
  name: "ThreadList",
  props: ["loadingThreads", "threads", "appliedFilter"],
  components: { ThreadItem, ViewThreadDialog },
  data() {
    return {
      skeletonThreadsCount: 12,
      showDialog: false,
      threadToShow: null,
    };
  },
  methods: {
    closeThreadDialog() {
      this.showDialog = false;
      setTimeout(() => {
        this.threadToShow = null;
      }, 500);
    },
    showThread(thread) {
      this.threadToShow = thread;
      this.showDialog = true;
    },
  },
  computed: {
    filteredThreads() {
      if (!this.appliedFilter) return [...this.threads];

      const filter = this.appliedFilter.toLowerCase();

      if (filter === "all") return [...this.threads];
      if (filter === "threads")
        return this.threads.filter(
          (thread) => thread.source.toLowerCase() === "thread"
        );
      if (filter === "friends")
        return this.threads.filter(
          (thread) => thread.source.toLowerCase() === "friend"
        );
    },
  },
};
</script>