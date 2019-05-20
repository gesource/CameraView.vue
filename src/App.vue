<template>
  <div id="app">
    <CameraSelection v-on:change-camera="onChangeCamera"/>
    <CameraView ref="cameraView" :cameraType="cameraType" :cameraSize="cameraSize"/>
  </div>
</template>

<script>
import CameraSelection from "./components/CameraSelection.vue";
import CameraView from "./components/CameraView.vue";

export default {
  name: "app",
  components: {
    CameraSelection,
    CameraView
  },
  data() {
    return {
      cameraType: { type: "facingMode", device: "environment" },
      cameraSize: null
    };
  },
  methods: {
    onChangeCamera(cameraType, cameraSize) {
      this.cameraType = cameraType;
      this.cameraSize = cameraSize;
      this.$nextTick(() => this.$refs.cameraView.updateCamera());
    }
  }
};
</script>
