<template>
  <div>
    <div class="singerWrapper" ref="singerWrapper">
      <div class="singerCont" ref="singerContent" @touchstart="">
        <div class="singer" v-for="singer in singerData">
          <h2>{{singer.name}}</h2>
          <div class="singerItem" ref="singerItem">
            <p v-for="item in singer.cities" @click="getValue">{{item.name}}</p>
          </div>
        </div>
      </div>
    </div>
    <div class="singerNums" @touchstart="onTouchStart">
      <ul>
        <li v-for="(num,index) in singerData" :data-index="index" :class="{active : index == currIndex}">{{num.name.split('')[0]}}</li>
      </ul>
    </div>
  </div>
</template>
<script>
  import axios from 'axios'
  import singerData from './index-list.json'
  import BScroll from 'better-scroll'
  export default {
    data() {
      return {
        currIndex: 0,
        singerHeight: [],
        scrollY: 0
      }
    },
    created() {
      this._getData()
      this._getJsonpData()
    },
    mounted() {
      setTimeout(() => {
        this._initHeigt()
        this._initScroll()
        this._singerHeight()
      }, 20)
    },
    computed: {
    },
    methods: {
      _getJsonpData() {
        axios({
          method: 'get',
          url: '/apis/v8/fcg-bin/v8.fcg?g_tk=1928093487&inCharset=utf-8&outCharset=utf-8&notice=0&format=jsonp&channel=singer&page=list&key=all_all_all&pagesize=100&pagenum=1&hostUin=0&needNewCode=0&platform=yqq'
        }).then((response) => {
          // console.log(response.data.data.list)
          let list = response.data.data.list
          this.obj = {

          }
          for (var i = 0; i < list.length; i++) {
            if (!this.obj[list[i].Findex]) {
              this.obj[list[i].Findex] = {
                title: list[i].Findex,
                items: []
              }
            }
            this.obj[list[i].Findex].items.push({
              name: list[i].Fsinger_name,
              id: list[i].Fsinger_mid
            })
          }
          // console.log(this.obj)
          let hot = []
          let ret = []
          for (let o in this.obj) {
            if (!/^[a-zA-Z]/g.test(o)) {
              hot.push(this.obj[o])
            } else {
              ret.push(this.obj[o])
            }
          }
          ret.sort((a, b) => {
            return a.title.charCodeAt(0) - b.title.charCodeAt(0)
          })
          ret = hot.concat(ret)
          console.log(ret)
        }, (data) => {
          console.log('err')
        })
      },
      getValue(e) {
        console.log(e.target.innerHTML)
      },
      _scrollY() {
        // console.log(22)
        this.singerScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
          for (var i = 0; i < this.singerHeight.length; i++) {
            if (i < this.singerHeight.length - 1) {
              let h1 = this.singerHeight[i]
              let h2 = this.singerHeight[i + 1]
              if (this.scrollY > h1 && this.scrollY < h2) {
                this.currIndex = i
              }
            }
          }
        })
      },
      _singerHeight() {
        let height = 0
        let singerHeightItem = this.$refs.singerContent.children
        // console.log(singerHeightItem)
        for (var i = 0; i < singerHeightItem.length; i++) {
          height += singerHeightItem[i].clientHeight
          this.singerHeight.push(height - 152)
        }
        // console.log(this.singerHeight)
        // this.singerHeight
      },
      onTouchStart(e) {
        if (e.target) {
          this.currIndex = e.target.getAttribute('data-index')
        }
        this.singerScroll.scrollToElement(this.$refs.singerContent.children[this.currIndex], 0)
      },
      onTouchMove(e) {

      },
      _getData() {
        this.singerData = singerData
      },
      _initScroll() {
        // console.log(11)
        if (!this.singerScroll) {
          this.singerScroll = new BScroll(this.$refs.singerWrapper, {
            scrollY: true,
            click: true,
            probeType: 3
          })
        } else {
          this.singerScroll.refresh()
        }
        this._scrollY()
      },
      _initHeigt() {
        this.$refs.singerWrapper.style.height = document.documentElement.clientHeight - 108 + 'px'
      }
    }
  }
</script>
<style lang="stylus" scoped>
  .singerWrapper
    overflow:hidden
    position:relative
  .singerNums
    position:fixed
    top:50%
    right:5px
    transform:translateY(-50%)
    line-height:38px
  .active
    color:yellow
  .singerCont
    width:100%
    h2
      color:rgba(255,255,255,.5)
      background:#333
      line-height:44px
      padding-left:4%
    .singerItem
      p
        line-height:34px
        padding-left:8%
</style>
