<template>
  <v-row justify="center">
    <v-dialog
      v-model="showDialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <v-card>
        <v-toolbar dark color="secondary">
          <v-btn icon dark @click="closeThreadFiltersDialog">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>Thread Filters</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn dark text @click="closeThreadFiltersDialog"> Close </v-btn>
          </v-toolbar-items>
        </v-toolbar>
        <!-- Error Message -->
        <div class="d-flex justify-center align-center pa-3">
          Thread filters
        </div>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  name: "ThreadFiltersDialog",
  props: ["showDialog", "closeThreadFiltersDialog"],
  components: {},
  data() {
    return {};
  },
  methods: {},
  async created() {},
};
</script>

<style lang='scss' scoped>
</style>