<template>
  <div class="hello">
    <h2>
      <img src="../common/images/default.png" alt="">
    </h2>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
.hello
  h2
    width:64px
    height:64px
    margin:0 auto
    img
      display:block
      width:100%
</style>
