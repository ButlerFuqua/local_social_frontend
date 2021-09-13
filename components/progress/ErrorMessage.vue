<template>
  <v-chip color="red" dark>{{ errorMessage }}</v-chip>
</template>

<script>
export default {
  name: "ErrorMessage",
  props: ["errorMessage"],
};
</script>