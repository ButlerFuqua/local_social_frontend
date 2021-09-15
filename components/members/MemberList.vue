<template>
  <div>
    <ViewMemberDialog
      v-if="memberToShow"
      :memberId="memberToShow._id"
      :showDialog="showDialog"
      :closeMemberDialog="closeMemberDialog"
    />
    <div v-if="loadingMembers">
      <v-skeleton-loader
        v-for="(num, idx) in Array(skeletonMembersCount).fill(1)"
        :key="idx"
        type="article"
        class="my-2"
      ></v-skeleton-loader>
    </div>
    <div v-else-if="filteredMembers && filteredMembers.length > 0">
      <MemberItem
        :showMember="showMember"
        v-for="(member, idx) in filteredMembers"
        :key="idx"
        :member="member"
      />
    </div>
  </div>
</template>

<script>
import MemberItem from "./MemberItem.vue";
import ViewMemberDialog from "./ViewMemberDialog.vue";
export default {
  name: "MemberList",
  props: ["loadingMembers", "members", "appliedFilter"],
  components: { MemberItem, ViewMemberDialog },
  data() {
    return {
      skeletonMembersCount: 12,
      showDialog: false,
      memberToShow: null,
    };
  },
  methods: {
    closeMemberDialog() {
      this.showDialog = false;
      setTimeout(() => {
        this.memberToShow = null;
      }, 500);
    },
    showMember(member) {
      this.memberToShow = member;
      this.showDialog = true;
    },
  },
  computed: {
    filteredMembers() {
      if (!this.appliedFilter) return [...this.members];

      const filter = this.appliedFilter.toLowerCase();

      if (filter === "all") return [...this.members];
      if (filter === "friends")
        return this.members.filter(
          (member) => member.source.toLowerCase() === "friends"
        );
      if (filter === "not friends")
        return this.members.filter(
          (member) => member.source.toLowerCase() === "not friends"
        );
    },
  },
};
</script>