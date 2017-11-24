<template>
  <div class="recommdenBorder" ref="scroll">
    <div class="recommend">
      <div class="sliderBorder" v-if="sliders.length">
        <Slider>
          <div class="slider-item" v-for="slider in sliders">
            <img :src="slider.picUrl" alt="">
          </div>
        </Slider>
      </div>
      <div class="listBorder">
        <div class="recommdenList">
          <h2>热门歌单</h2>
          <ul>
            <li class="listItem" v-for="list in recommdenList">
              <div class="list_img">
                  <img v-lazy="list.imgurl" alt="">
              </div>
              <div class="listDisc">
                <h4>{{list.creator.name}}</h4>
                <p>{{list.dissname}}</p>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import axios from 'axios'
  import Slider from '../../base/slider/slider'
  import BScroll from 'better-scroll'
  export default {
    components: {
      Slider
    },
    data() {
      return {
        sliders: [],
        recommdenList: []
      }
    },
    created() {
      axios({
        method: 'get',
        url: '/apis/musichall/fcgi-bin/fcg_yqqhomepagerecommend.fcg'
      }).then((response) => {
        this.sliders = response.data.data.slider
        // console.log('slider:' + this.sliders.length)
      }, (data) => {
        console.log('error axios')
      })
      axios.get('/api/getDiscList', {
        params: {
          uin: 0,
          g_tk: 1928093487,
          inCharset: 'utf-8',
          outCharset: 'utf-8',
          notice: 0,
          platform: 'yqq',
          hostUin: 0,
          sin: 0,
          ein: 29,
          sortId: 5,
          needNewCode: 0,
          categoryId: 10000000,
          rnd: Math.random(),
          format: 'json'
        }
      }).then((response) => {
        // console.log(response.data.data.list)
        this.recommdenList = response.data.data.list
      }, (data) => {
        console.log('err')
      })
    },
    mounted() {
      setTimeout(() => {
        this._initHeigt()
        this._initScroll()
      }, 20)
    },
    methods: {
      _initScroll() {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.scroll, {
            scrollY: true,
            click: true
          })
        } else {
          this.scroll.refresh()
        }
        console.log(this.scroll)
      },
      _initHeigt() {
        // console.log(document.documentElement.clientHeight)
        this.$refs.scroll.style.height = document.documentElement.clientHeight + 'px'
        // console.log(this.$refs.scroll.children[0].offsetHeight)
        this.$refs.scroll.children[0].style.height = this.$refs.scroll.children[0].offsetHeight + 200 + 'px'
        // this.contW = 1
      }
    }
  }
</script>
<style lang="stylus">
  .recommdenBorder
    overflow:hidden
    .recommend
      width:100%
      height:4820px
    .slider-item
      img
        display:block
        width:100%
    .recommdenList
      padding:0 5%
      h2
        text-align:center
        font-size:34px
        color:yellow
        padding:30px 0 0 0
    .listItem
      display:flex
      margin-top:30px
      .list_img
        width:120px
        height:120px
        img
          display:block
          width:100%
      .listDisc
        display:flex
        flex-direction:column
        align-item:center
        justify-content:center
        line-height:38px
        padding-left:5%
        h4
          font-size:28px
          color:#fff
</style>
