<template>
  <div id="app">
    <Paginate @getPage="getPage" :pageCss="pageCss" :pageCount="pageCount" :pageSize="pageSize"></Paginate>
  </div>
</template>

<script>
import Paginate from "cjhcj-paginate";
export default {
  name: "app",
  data() {
    return {
      pageCount: 200,
      pageSize: 10,
      pageCss: {
        pages: {
          width: "20px",
          height: "20px",
          background: "#999",
          color: "red",
          'font-size': '10px',
          'border-radius': '50%'
        },
        active: {
          background: "green",
          color: "white",
          'border-radius': '50%'
        }
      }
    };
  },
  methods: {
    // 页码跳换
    getPage(index) {
      // console.log(index);
    }
  },
  components: {
    Paginate
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
@font-face {
    font-family: 'iconfont';  /* project id 1328347 */
    src: url('../static/icons/iconfont.eot');
    src: url('../static/icons/iconfont.eot?#iefix') format('embedded-opentype'),
    url('../static/icons/iconfont.woff2') format('woff2'),
    url('../static/icons/iconfont.woff') format('woff'),
    url('../static/icons/iconfont.ttf') format('truetype'),
    url('../static/icons/iconfont.svg#iconfont') format('svg');
}
</style>
