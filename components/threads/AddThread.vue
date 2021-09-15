<template>
  <div>
    <AddThreadDialog
      :showAddThreadDialog="showAddThreadDialog"
      :closeAddThreadDialog="() => (showAddThreadDialog = false)"
      :fetchThreads="fetchAllThreads"
    />
    <v-btn
      small
      rounded
      elevation="0"
      outlined
      color="primary"
      @click="showAddThreadDialog = true"
      >Create Thread</v-btn
    >
  </div>
</template>

<script>
import AddThreadDialog from "./AddThreadDialog.vue";
export default {
  name: "AddThread",
  props: ["fetchAllThreads"],
  components: { AddThreadDialog },
  data() {
    return {
      showAddThreadDialog: false,
    };
  },
};
</script>