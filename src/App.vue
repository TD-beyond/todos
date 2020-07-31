<template>
  <div id="app">
    <!-- 顶部开始 -->
    <div class="app-header">
      <Header></Header>
    </div>
    <!-- 顶部结束 -->
    <!-- 中间开始 -->
    <div class="app-content">
      <Content></Content>
    </div>
    <!-- 中间结束 -->
    <!-- 底部开始 -->
    <div class="app-footer">
      <Footer></Footer>
    </div>
    <!-- 底部结束 -->
  </div>
</template>

<script>
import Header from "./components/Header"
import Content from "./components/Content"
import Footer from "./components/Footer"
export default {
  name: 'App',
  components: {
    Header,
    Content,
    Footer,
  }
}
</script>

<style>
html,body,p,span,div,section,header,footer,h1,h2,h3,h4,h5,h6{
  margin: 0;
  padding: 0;
}
#app{
  width: 400px;
  margin: 20px auto;
  padding: 0 10px 10px;
  border: 1px solid #000;
}
</style>
