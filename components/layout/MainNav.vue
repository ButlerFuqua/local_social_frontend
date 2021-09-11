<template>
  <div id="mainNavContainer" :class="shown ? 'shown' : ''">
    <div id="mainNavContent" calss="pa-4">
      <v-btn id="closeBtn" @click="hideNav" class="my-2" fab small dark
        >X</v-btn
      >
      <h1>Main Nav</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "MainNav",
  props: ["shown", "hideNav"],
};
</script>

<style lang="scss" scoped>
#mainNavContainer {
  position: fixed;
  height: 100%;
  width: 100%;
  z-index: 6;
  background: url("~assets/images/landing_background.jpg");
  background-size: cover;
  background-position: center;

  right: 100%;
  opacity: 0;
  transition: all 0.3s;

  &.shown {
    right: 0;
    opacity: 1;
  }
}
#mainNavContent {
  height: 100%;
  width: 100%;
  background-image: linear-gradient(to top, #c471f5cc 0%, #fa71cdb8 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
}
#closeBtn {
  position: absolute;
  bottom: 10px;
}
</style>