<template>
  <v-container fluid>
    <v-combobox
      v-model="model"
      :items="items"
      :search-input.sync="search"
      hide-selected
      hint="Maximum of 5 interests"
      label="Related Interests"
      multiple
      persistent-hint
      small-chips
      outlined
    >
      <template v-slot:no-data>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title>
              No results matching "<strong>{{ search }}</strong
              >". Press <kbd>enter</kbd> to create a new one
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </template>
    </v-combobox>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    items: ["Art", "Sports", "Science", "Jesus", "Gaming"],
    model: [],
    search: null,
  }),

  watch: {
    model(val) {
      if (val.length > 5) {
        this.$nextTick(() => this.model.pop());
      }
    },
  },
};
</script>