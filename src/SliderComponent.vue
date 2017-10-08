<script>
  export default {
    props: {
      delay: {
        type: Number,
        default: 10000
      },
      opacity: {
        type: Number,
        default: 1
      },
      parallax: {
        type: Boolean,
        default: false
      }
    },
    data () {
      return {
        index: 0,
        slides: this.$children,
        direction: null,
        timer: null
      }
    },
    computed: {
      slidesCount () {
        return this.slides.length
      },
      parallaxEffect () {
        return {
          backgroundAttachment: this.parallax ? 'fixed' : 'inherit'
        }
      }
    },
    methods: {
      startRotation () {
        this.timer = setInterval(this.next, this.delay)
      },
      stopRotation () {
        clearTimeout(this.timer);
        this.timer = null;
      },
      prev () {
        this.index--
        this.direction = 'left'
        if (this.index < 0) {
          this.index = this.slidesCount - 1
        }
      },
      next () {
        this.index++
        this.direction = 'right'
        if (this.index >= this.slidesCount) {
          this.index = 0
        }
      },
      to_url (path) {
        return path.split(":").pop()
      },
      backgroundImg (path) {
        return {
          backgroundImage: 'url(' + this.to_url(path) + ')'
        }
      },
      goTo (index) {
        this.direction = index > this.index ? 'right' : 'left'
        this.index = index
      }
    },
    mounted () {
      this.startRotation()
    }
  }
</script>

<template>
  <div class="slider">
    <slot></slot>
    <div class="slider__pagination">
      <button class="slider__nav.slider__prev" @click.prevent="prev" title="Précédent")>Précédent</button>
      <button v-for="n in slidesCount" @click="goTo(n-1)" :class="{active: index == n-1 }"></button>
      <button class="slider__nav.slider__next" @click.prevent="next" title="Suivant")>Suivant</button>
    </div>
  </div>
</template>

<style scoped>

  @buttonColor: fade(#fff, 50%);
  @buttonActiveColor: #fff;

  .slider {
    position: relative;
    background-color: #000;
  }

  .slider__nav {
    display: inline-block;
    color: rgba(255, 255, 255, 0.5);
    transition: color .3s;
  }

  .slider__nav:hover {
    color: #fff;
    cursor: pointer;
  }

  .slider__nav.slider__prev {
    left: 10px;
  }

  .slider__nav.slider__next {
    right: 10px;
  }

  .slide {
    width: 100%;
    height: 600px;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: 50% 0%;
  }

  .slider__pagination {
    z-index: 2;
    position: absolute;
    bottom: 110px;
    left: 0;
    right: 0;
    text-align: center;
  }

  .slider__pagination button {
    display: inline-block;
    padding: 0;
    width: 10px;
    height: 10px;
    background-color: rgba(255, 255, 255, 0.5);
    border: none;
    border-radius: 10px;
    transition: backgroundColor .3s;
    margin: 3px;
  }

  .slider__pagination button:hover {
    background-color: #fff;
  }

  .slider__pagination button:focus {
    outline: none;
  }

  .slider__pagination button.active {
    background-color: #fff;
  }
</style>
