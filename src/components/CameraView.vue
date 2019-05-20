<template>
  <video playsinline autoplay></video>
</template>

<script>
import "webrtc-adapter";

export default {
  name: "CameraView",
  props: {
    cameraType: Object,
    cameraSize: Object
  },
  mounted() {
    this.video = this.$el;
    this.video.onresize = () => {
      console.log(
        `width: ${this.video.videoWidth} x ${this.video.videoHeight} px.`
      );
    };
    this.updateCamera();
  },
  methods: {
    _getConstraints() {
      const constraints = {};
      if (this.cameraType) {
        constraints[this.cameraType.type] = this.cameraType.device;
      }
      if (this.cameraSize) {
        constraints.width = { exact: this.cameraSize.width };
        constraints.height = { exact: this.cameraSize.height };
      }
      return constraints;
    },
    updateCamera() {
      if (this.stream) {
        this.stream.getTracks().forEach(track => {
          track.stop();
        });
      }

      navigator.mediaDevices
        .getUserMedia({ video: this._getConstraints() })
        .then(mediaStream => {
          this.stream = mediaStream;
          this.video.srcObject = mediaStream;
        })
        .catch(error => {
          console.error(error);
          alert(error);
        });
    }
  }
};
</script>
