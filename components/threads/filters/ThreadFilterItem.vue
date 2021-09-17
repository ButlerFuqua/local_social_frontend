<template>
  <v-list-item>
    <v-list-item-content>
      <v-list-item-title class="mb-2">{{ filter.title }}</v-list-item-title>
      <CheckboxFilter v-if="filter.type === 'checkbox'" :filter="filter" />
      <RadioFilter v-if="filter.type === 'radio'" :filter="filter" />
      <v-divider class="mt-2"></v-divider>
    </v-list-item-content>
  </v-list-item>
</template>

<script>
import CheckboxFilter from "./CheckboxFilter";
import RadioFilter from "./RadioFilter";
export default {
  name: "ThreadFilterItem",
  props: ["filter"],
  components: { CheckboxFilter, RadioFilter },
};
</script>