<template>
  <div>
    <div v-if="loadingSettings">
      <v-skeleton-loader
        v-for="(num, idx) in Array(skeletonSettingsCount).fill(1)"
        :key="idx"
        type="article"
        class="my-2"
      ></v-skeleton-loader>
    </div>
    <div v-else-if="settings && settings.length > 0">
      <SettingItem
        :updateSetting="updateSetting"
        v-for="(setting, idx) in settings"
        :key="idx"
        :setting="setting"
      />
    </div>
  </div>
</template>

<script>
import SettingItem from "./SettingItem.vue";
export default {
  name: "SettingList",
  props: ["loadingSettings", "settings", "appliedFilter"],
  components: { SettingItem },
  data() {
    return {
      skeletonSettingsCount: 12,
      showDialog: false,
      settingToShow: null,
    };
  },
  methods: {
    closeSettingDialog() {
      this.showDialog = false;
      setTimeout(() => {
        this.settingToShow = null;
      }, 500);
    },
    updateSetting() {
      console.log("update setting");
    },
  },
  computed: {},
};
</script>