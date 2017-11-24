<template>
  <div class="nav">
    <router-link　class="tab_item" tag="div" to="/recommend"><span class="tab_link">推荐</span></router-link>
    <router-link　class="tab_item" tag="div" to="/singer"><span class="tab_link">歌手</span></router-link>
    <router-link　class="tab_item" tag="div" to="/music-list"><span class="tab_link">排行</span></router-link>
    <router-link　class="tab_item" tag="div" to="/search"><span class="tab_link">搜索</span></router-link>
　</div>
</template>
<script>
  export default{

  }
</script>
<style lang="stylus" scoped>
  .nav
    padding:0 2.5%
    display:flex
    justify-content:space-between
    .tab_item
        width:25%
        text-align:center
        cursor:pointer
        .tab_link
          display:inline-block
          padding:10px 0
        &.router-link-active
          .tab_link
            color:#fff
            border-bottom:2px solid #fff
</style>