<template>
  <div>
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title>{{ setting.title }}</v-list-item-title>
        <v-divider class="my-4"></v-divider>
      </v-list-item-content>
    </v-list-item>
  </div>
</template>

<script>
export default {
  name: "SettingItem",
  props: ["setting"],
};
</script>