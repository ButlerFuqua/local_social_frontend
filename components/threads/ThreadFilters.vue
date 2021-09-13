<template>
  <div id="filterContainer" class="d-flex justify-space-around">
    <v-btn
      v-for="(filter, idx) in filters"
      :key="idx"
      small
      rounded
      elevation="0"
      :color="selected === filter ? 'primary' : ''"
      @click="updateSelection(filter)"
      disabled
      >{{ filter }}</v-btn
    >
  </div>
</template>

<script>
export default {
  name: "ThreadFilters",
  props: ["applyFilter"],
  data() {
    return {
      isLoading: true,
      errorMessage: null,
      selected: "All",
      filters: ["All", "Joined", "Not Joined"],
    };
  },
  methods: {
    updateSelection(text) {
      if (!this.applyFilter) return;

      this.applyFilter(text);
      this.selected = text;
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