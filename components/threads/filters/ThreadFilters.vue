<template>
  <div
    id="filterContainer"
    class="d-flex flex-column justify-center align-center"
  >
    <ThreadFilterDrawer
      :showDialog="showDialog"
      :closeThreadFiltersDialog="closeThreadFiltersDialog"
    />
    <v-btn
      small
      rounded
      elevation="0"
      outlined
      :color="appliedFilters.length > 0 ? 'secondary' : ''"
      @click="showDialog = true"
      >Filters</v-btn
    >
  </div>
</template>

<script>
import ThreadFilterDrawer from "./ThreadFilterDialog.vue";
export default {
  name: "ThreadFilters",
  props: [],
  components: { ThreadFilterDrawer },
  data() {
    return {
      isLoading: true,
      errorMessage: null,
      selected: "All",
      filters: ["All", "Joined", "Not Joined"],
      appliedFilters: [],
      showDialog: false,
    };
  },
  methods: {
    closeThreadFiltersDialog() {
      this.showDialog = false;
    },
    applyFilters() {
      console.log("apply filters");
    },
  },
};
</script>

<style lang="scss" scoped>
#filterContainer {
  position: sticky;
  top: 0;
}
</style>