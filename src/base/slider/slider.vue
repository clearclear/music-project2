<template>
  <div class="slider">
    <div class="wrapper" ref="wrapper">
      <div class="content" ref="content">
        <slot></slot>
      </div>
    </div>
    <div class="nums">
      <div class="nums_cont">
        <p :class="{active : currentPage === index}" v-for="(n, index) in nums">{{index}}</p>
      </div>
    </div>
  </div>
</template>
<script>
  import BScroll from 'better-scroll'
  export default {
    props: {
      loop: {
        type: Boolean,
        default: true
      },
      autoPlay: {
        type: Boolean,
        default: true
      }
    },
    data() {
      return {
        nums: [],
        currentPage: 0
      }
    },
    created() {
    },
    mounted() {
      setTimeout(() => {
        this._initSliderW()
        this._initNums()
        this._initScroll()
        if (this.autoPlay) {
          this._play()
        }
      }, 20)
    },
    methods: {
      _initSliderW() {
        let wrapperW = this.$refs.wrapper.clientWidth
        this.children = this.$refs.content.children
        let width = 0
        for (let i = 0; i < this.children.length; i++) {
          this.children[i].style.width = wrapperW + 'px'
          width += wrapperW
        }
        if (this.loop) {
          width += 2 * wrapperW
        }
        this.$refs.content.style.width = width + 'px'
      },
      _initScroll() {
        this.slider = new BScroll(this.$refs.wrapper, {
          scrollX: true,
          scrollY: false,
          momentum: false,
          snap: {
            loop: this.loop,
            threshold: 0.3,
            speed: 400
          }
        })
        // getCurrentPage() scrollEnd事件 goToPage()方法
        this.slider.on('scrollEnd', () => {
          // console.log(this.slider.getCurrentPage().pageX)
          this.currentPage = this.slider.getCurrentPage().pageX
          if (this.loop) {
            this.currentPage = this.slider.getCurrentPage().pageX - 1
          }
          if (this.autoPlay) {
            this._play()
          }
        })
        // beforeScrollStart事件
        this.slider.on('beforeScrollStart', () => {
          if (this.autoPlay) {
            clearTimeout(this.timer)
          }
        })
      },
      _initNums() {
        this.nums = new Array(this.children.length)
      },
      _play() {
        let pageIndex = this.currentPage + 1
        if (this.loop) {
          pageIndex += 1
        }
        this.timer = setTimeout(() => {
          this.slider.goToPage(pageIndex, 0, 400)
        }, 2000)
      }
    }
  }
</script>
<style lang="stylus" scoped>
  .slider
    position:relative
    .content
      display: flex
  .nums
    width:100%
    position:absolute
    bottom:4px
    left:0
    .nums_cont
      display:flex
      justify-content:center
    p
      width:26px
      height:30px
      line-height:30px
      text-align:center
      background:#ccc
      margin: 0 4px
    .active
      color: red
</style>