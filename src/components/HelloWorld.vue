<template>
  <div class="hello">
    <h1>跑马灯</h1>
    <div class="test-content">
      <marquee-bar value="Jenny测试"></marquee-bar>
      <marquee-bar value="测试跑马灯的效果，在超出限定内容框时，是否能滚动起来"></marquee-bar>
      <marquee-bar value="内容滚动一轮后，停顿片刻继续滚动"></marquee-bar>
      <marquee-bar value="就这样吧"></marquee-bar>
    </div>
  </div>
</template>

<script>
import MarqueeBar from './MarqueeBar'
export default {
  name: 'HelloWorld',
  components: {
    MarqueeBar
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
.test-content {
  width: 200px;
  margin: 20px auto;
  padding: 5px;
  background-color: aquamarine;
}
</style>
