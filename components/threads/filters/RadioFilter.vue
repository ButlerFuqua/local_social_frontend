<template>
  <v-radio-group v-model="radioGroup">
    <v-radio
      v-for="option in filter.options"
      :key="option"
      :label="option"
      :value="option"
    ></v-radio>
  </v-radio-group>
</template>

<script>
export default {
  name: "RadioFilter",
  props: ["filter"],
  data() {
    return {
      radioGroup: 1,
    };
  },
};
</script>