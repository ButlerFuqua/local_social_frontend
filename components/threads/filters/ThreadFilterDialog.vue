<template>
  <v-row justify="center">
    <v-dialog
      v-model="showDialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <v-card tile>
        <v-toolbar dark color="secondary">
          <v-btn icon dark @click="cancelUpdatingFilters">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>Filters (0)</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn dark text @click="clearFilters"> Clear All </v-btn>
          </v-toolbar-items>
        </v-toolbar>
        <v-list>
          <ThreadFilterItem
            v-for="filter in filters"
            :key="filter.title"
            :filter="filter"
          />
        </v-list>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import ThreadFilterItem from "./ThreadFilterItem.vue";
export default {
  name: "ThreadFiltersDialog",
  props: ["showDialog", "closeThreadFiltersDialog"],
  components: { ThreadFilterItem },
  data() {
    return {
      filters: [
        {
          title: "Activities",
          type: "checkbox",
          options: ["Running", "Reading", "Video Games"],
        },
        {
          title: "Interests",
          type: "checkbox",
          options: ["Sports", "Video Games", "Coffee"],
        },
        {
          title: "Joined",
          type: "radio",
          options: ["All", "Joined", "Not joined"],
        },
      ],
    };
  },
  methods: {
    cancelUpdatingFilters() {
      // Reset filters to their previous state

      // close dialog
      this.closeThreadFiltersDialog();
    },
    clearFilters() {
      // Clear all filters
    },
  },
};
</script>

<style lang='scss' scoped>
</style>