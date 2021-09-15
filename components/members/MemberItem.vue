<template>
  <div>
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title>{{ member.username }}</v-list-item-title>
        <v-divider class="my-4"></v-divider>
      </v-list-item-content>
      <v-btn
        rounded
        outlined
        small
        color="secondary"
        @click="showMember(member)"
        >View</v-btn
      >
    </v-list-item>
  </div>
</template>

<script>
export default {
  name: "MemberItem",
  props: ["member", "showMember"],
};
</script>