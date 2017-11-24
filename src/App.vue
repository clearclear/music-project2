<template>
  <div id="app">
    <head-top></head-top>
    <nav-tab></nav-tab>
    <keep-alive>
      <router-view/>
    </keep-alive>

  </div>
</template>

<script>
import HeadTop from '@/components/head'
import NavTab from '@/components/navTab'
export default {
  components: {
    HeadTop,
    NavTab
  }
}
</script>
<style>
#app {

}
</style>
