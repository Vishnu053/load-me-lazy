<template>
  <div
    :id="containerId"
    :class="infiniteClass"
    style="overflow-y: auto;"
    :style="`max-height:${maxHeight||'calc(100vh - 13vh - 42px)'}`"
  >
    <slot></slot>
  </div>
  <!-- usage -->
  <!-- <vLazy maxHeight="200px" @triggered="callbackMethod">
    <div v-for="i in 100" :key="i">
      item {{i}}
    </div>
    </vLazy> -->
</template>
<script>
export default {
  name:"vLazy",
  props: {
    infiniteId: [String, Number],
    infiniteClass: [String],
    infiniteStyle: [String],
    maxHeight: [String],
  },
  data: () => ({
    containerId: "lazy-load-id",
    scrollParent: new Object(),
  }),
  mounted() {
    this.initialiseLazyLoad().then(res=>{
        this.setLazyLoad();

    })
  },
  beforeDestroy() {
    if (this.scrollParent) {
      this.scrollParent.removeEventListener("scroll", () => {});
    }
  },
  methods: {
    initialiseLazyLoad() {
        return new Promise((resolve, reject) => {
           resolve( this.infiniteId
          ? (this.containerId = this.infiniteId)
          : (this.containerId = btoa(new Date().getTime() + Math.random())))
        })
    },
    setLazyLoad() {
      this.scrollParent = document.querySelector("#" + this.containerId);
      this.scrollParent.addEventListener("scroll", () => {
        if (
          this.scrollParent.scrollTop + this.scrollParent.clientHeight >=
          this.scrollParent.scrollHeight
        ) {
          this.$emit("triggered")
        }
      });
    },
  },
};
</script>
