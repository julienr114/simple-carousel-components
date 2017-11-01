<template>
  <div class="carousel">
    <div class="carousel_header">

    </div>
    <slot name="slide"></slot>
    <button class="carousel_control control_prev" @click.prevent="prev">
      <svg fill="#000000" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg">
        <path d="M15.41 16.09l-4.58-4.59 4.58-4.59L14 5.5l-6 6 6 6z"/>
        <path d="M0-.5h24v24H0z" fill="none"/>
      </svg>
    </button>
    <button class="carousel_control control_next" @click.prevent="next">
      <svg fill="#000000" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg">
        <path d="M8.59 16.34l4.58-4.59-4.58-4.59L10 5.75l6 6-6 6z"/>
        <path d="M0-.25h24v24H0z" fill="none"/>
      </svg>
    </button>
    <div class="carousel__pagination">
      <button v-for="n in slidesCount" @click="goTo(n-1)" class="" :class="{active: currentIndex == n-1 }"></button>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      auto: {
        type: Boolean,
        default: true
      },
      delay: {
        type: Number,
        default: 5000
      }
    },

    data () {
      return {
        currentIndex: 0,
        slidesCount: 0,
        direction: null,
        timer: null
      }
    },

    methods: {
      next () {
        this.inTransition()
        this.currentIndex++
        this.direction = 'right'
        if (this.currentIndex >= this.slidesCount) {
          this.currentIndex = 0
        }
      },
      prev () {
        this.inTransition()
        this.currentIndex--
        this.direction = 'left'
        if (this.currentIndex < 0) {
          this.currentIndex = this.slidesCount - 1
        }
      },
      goTo (index) {
        this.direction = index > this.index ? 'right' : 'left'
        this.currentIndex = index
      },
      startRotation () {
        this.timer = setInterval(this.next, this.delay)
      },
      stopRotation () {
        clearTimeout(this.timer)
        this.timer = null
      },
      inTransition () {
        this.stopRotation()
        setTimeout(() => {
          this.startRotation()
        }, 1000)
      }
    },

    mounted () {
      this.slidesCount = this.$children.length
      if (this.delay) {
        this.startRotation()
      }
    }
  }
</script>

<style scoped>
  .carousel {
    position: relative;
  }

  .carousel_control {
    position: absolute;
    top: 50%;
    background: transparent;
  }

  .carousel_control.control_prev {
    left: 15px;
  }

  .carousel_control.control_next {
    right: 15px;
  } 
</style>
