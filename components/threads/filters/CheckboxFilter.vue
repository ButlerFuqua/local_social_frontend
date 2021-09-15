<template>
  <div>
    <v-checkbox
      v-for="option in filter.options"
      :key="option"
      v-model="checkboxes[option]"
      :label="option"
      class="my-0"
    ></v-checkbox>
  </div>
</template>

<script>
export default {
  name: "CheckboxFilter",
  props: ["filter"],
  data() {
    return {
      checkboxes: {},
    };
  },
};
</script>