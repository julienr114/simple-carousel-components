<script>
  export default {
    props: {
      index: Number,
      opacity: {
        type: Number,
        default: 1
      }
    },
    computed:  {
      transition () {
        if (this.$parent.direction) {
          return 'slide-' + this.$parent.direction
        }
      },
      visible () {
        return this.index === this.$parent.index
      }
    }
  }
</script>

<template>
  <transition :name="transition">
    <div v-show="visible" :style="{opacity: opacity}">
      <slot></slot>
    </div>
  </transition>
</template>

<style>
  .slide-right-enter-active {
    animation: slideRightIn 1s;
  }

  .slide-right-leave-active {
    animation: slideRightOut 1s;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
  }

  @keyframes slideRightIn {
    from { transform: translateX(100%); }
    to { transform: translateX(0); }
  }

  @keyframes slideRightOut {
    from { transform: translateX(0); }
    to { transform: translateX(-100%); }
  }

  .slide-left-enter-active {
    animation: slideLeftIn 1s ease;
  }

  .slide-left-leave-active {
    animation: slideLeftOut 1s ease;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
  }

  @keyframes slideLeftIn {
    from { transform: translateX(-100%); }
    to { transform: translateX(0); }
  }

  @keyframes slideLeftOut {
    from { transform: translateX(0); }
    to { transform: translateX(100%); }
  }
</style>
