// 分页一共分为三个层次

// 第一个层次 ： 页数小于等于5
// 呈现的状态: < 1 2 3 4 5 >
// 这个层次的逻辑最简单，仅仅是改变页码的样式就可以，逻辑很清晰

// 第二个层次： 页数大于5小于等于9
// 这个层次会根据页码的切换会呈现两种不同的状态
// 第一种状态：  < 1 2 3 4 ··· 9 >
// 第二种状态    < 1 ··· 4 5 6 7 8 9 >


// 第三个层次: 页数大于9
// 这个层次的分页会呈现三种状态
// 第一种状态： < 1 2 3 4 ··· n >
// 第二种状态： < 1 ··· 8 9 10 11 12 ··· n>
// 第三种状态： < 1 ··· n-3 n-2 n-1 n >


<template>
  <div class="paginate" v-if="pageRange > 0">
    <div class="pages-container" :style="[pagesStyle]" @click="prePage">
      <div class="pages">
        <i class="iconfont icon-left"></i>
      </div>
    </div>

    <!-- 从1到5的页码开始 -->
    <div
      class="pages-container"
      :style="[pagesStyle]"
      v-for="(item, index) in pages"
      :key="index * random()"
    >
      <div
        class="pages"
        @click="turnToPage(index)"
        :style="[index == choseActiveIndex ? activeStyle : '' ]"
      >{{index + 1}}</div>
    </div>
    <!-- 从1到5的页码结束 -->

    <!-- 从1到9的页码开始 -->
    <div
      class="pages-container"
      :style="[pagesStyle]"
      v-for="(item) in leftPages"
      :key="item  * random()"
    >
      <div
        class="pages"
        @click="turnToPage(item)"
        :style="[choseActiveIndex == item ? activeStyle : '']"
      >{{item + 1}}</div>
    </div>

    <div class="pages-container" :style="[pagesStyle]" v-if="pageRange > 5 && pageRange <=9">
      <div class="pages">
        <span style="font-weight: bold; ">···</span>
      </div>
    </div>

    <div
      class="pages-container"
      :style="[pagesStyle]"
      v-for="(item) in rightPages"
      :key="item * random()"
    >
      <div
        class="pages"
        @click="turnToPage(item)"
        :style="[choseActiveIndex == item ? activeStyle : '']"
      >{{item + 1}}</div>
    </div>
    <!-- 从1到9的页码结束 -->

    <!-- 从1到9+的页码开始 -->
    <div
      class="pages-container"
      :style="[pagesStyle]"
      v-for="(item) in bigLeftPages"
      :key="item  * random()"
    >
      <div
        class="pages"
        @click="turnToPage(item)"
        :style="[choseActiveIndex == item ? activeStyle : '']"
      >{{item + 1}}</div>
    </div>
    <div class="pages-container" :style="[pagesStyle]" v-if="pageRange > 9">
      <div class="pages">
        <span style="font-weight: bold; ">···</span>
      </div>
    </div>

    <div
      class="pages-container"
      :style="[pagesStyle]"
      v-for="(item) in centerPages"
      :key="item * random()"
    >
      <div
        class="pages"
        @click="turnToPage(item)"
        :style="[choseActiveIndex == item ? activeStyle : '']"
      >{{item + 1}}</div>
    </div>

    <div class="pages-container" :style="[pagesStyle]" v-if="pageRange > 9 && soonShow">
      <div class="pages">
        <span style="font-weight: bold; ">···</span>
      </div>
    </div>

    <div
      class="pages-container"
      :style="[pagesStyle]"
      v-for="(item) in bigRightPages"
      :key="item * random()"
    >
      <div
        class="pages"
        @click="turnToPage(item)"
        :style="[choseActiveIndex == item ? activeStyle : '']"
      >{{item + 1}}</div>
    </div>
    <!-- 从1到9+的页码结束 -->

    <div class="pages-container" :style="[pagesStyle]" @click="nextPage">
      <div class="pages">
        <!-- <div class="right-triangle"></div> -->
        <i class="iconfont icon-left_arrow"></i>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "cjhcj-paginate",
  data() {
    return {
      pageRange: 0, //页数范围
      pages: [], //当 pageRange<= 5时，存储页码
      choseActiveIndex: 0, // 被选中的页码的下标
      activeStyle: {
        //被选中的页码的样式
        background: "#19be6b",
        color: "white"
      },
      pagesStyle: {
        //普通页码的样式
        background: "skyblue",
        color: "black"
      },
      leftPages: [], //当 5 < pageRange <=9 时，存储左侧的页码
      rightPages: [], //当 5 < pageRange <=9 时，存储右侧的页码
      centerPages: [], //当 pageRange > 9 时 存储中部的页码
      bigLeftPages: [], // pageRange > 9 时，存储左侧的页码
      bigRightPages: [], // pageRange > 9 时，存储右侧的页码
      soonShow: false, //当出现中间的页码，两边的省略号需要同时出现，这时隐藏的省略号就会出现
      cssList: ["pages", "active"] //设置样式白名单
    };
  },
  props: {
    pageSize: {
      type: Number
    },
    pageCount: {
      type: Number
    },
    preIcon: {
      type: [Boolean, String],
      default: false
    },
    pageCss: {
      type: Object
    }
  },
  watch: {
    //   检测页码的改变
    choseActiveIndex(val, oldVal) {
      this.$emit("getPage", val + 1);
      if (this.pageRange > 5 && this.pageRange <= 9) {
        if (this.leftPages.length == 1 && val <= 3) {
          this.setLeftPages();
        } else if (this.rightPages.length == 1 && val >= 3) {
          this.setRightPages();
        }
      } else if (this.pageRange > 9) {
        if (val <= 2 && this.bigLeftPages.length <= 1) {
          this.setBigLeftPages(); //设置左边分页
        } else if (
          val >= this.pageRange - 3 &&
          this.bigRightPages.length <= 1
        ) {
          this.setBigRightPages(); //设置右边分页
        } else if (val >= 3 && val <= this.pageRange - 4) {
          this.setBigCenterPages(val); //设置中部分页
        }
      }
    },
    pageSize(val, oldVal) {
      this.analyzePage();
    },
    pageCount(val, oldVal) {
      this.analyzePage();
    },
    // 检测样式的改变
    pageCss(val, oldVal) {
      //   设置分页的样式
      this.setPageCss();
    }
  },
  mounted() {
    this.$(() => {
      //   console.log(this.pageSize, this.pageCount);
      this.analyzePage();

      //   设置分页的样式
      this.setPageCss();
    });
  },
  methods: {
    init() {
      this.pageRange = 0; //页数范围
      this.pages = []; //当 pageRange<= 5时，存储页码
      this.choseActiveIndex = 0; // 被选中的页码的下标
      this.leftPages = []; //当 5 < pageRange <=9 时，存储左侧的页码
      this.rightPages = []; //当 5 < pageRange <=9 时，存储右侧的页码
      this.centerPages = []; //当 pageRange > 9 时 存储中部的页码
      this.bigLeftPages = []; // pageRange > 9 时，存储左侧的页码
      this.bigRightPages = []; // pageRange > 9 时，存储右侧的页码
      this.soonShow = false; //当出现中间的页码，两边的省略号需要同时出现，这时隐藏的省略号就会出现
    },
    //这里的分页分为两个层次，8页以内的不做任何处理，8页以上的进行划分
    analyzePage() {
      this.init();
      let pages = Math.ceil(this.pageCount / this.pageSize);
      this.pageRange = pages;
      if (pages <= 5) {
        //当页数小于等于5时，直接显示全部的页码
        for (let i = 0; i < pages; i++) {
          this.pages.push(i);
        }
      } else if (pages > 5 && pages <= 9) {
        //当页数大于5小于等于9时
        for (let i = 0; i < 4; i++) {
          this.leftPages.push(i);
        }
        this.rightPages.push(pages - 1);
      } else if (pages > 9) {
        //当页数大于9时
        for (let i = 0; i < 4; i++) {
          this.bigLeftPages.push(i);
        }
        this.bigRightPages.push(pages - 1);
      }
    },
    // 5 < pageRange <= 9设置左边的页码，让右边的页码仅剩余一个
    setLeftPages() {
      this.rightPages = [this.pageRange - 1];
      this.leftPages = [];
      for (let i = 0; i < 4; i++) {
        this.leftPages.push(i);
      }
    },
    // 5 < pageRange <= 9 设置右边的页码，让左边的页码仅剩余一个
    setRightPages() {
      this.leftPages = [0];
      this.rightPages = [];
      for (let i = 3; i < this.pageRange; i++) {
        this.rightPages.push(i);
      }
    },
    // pageRange > 9 设置左边的页码，让右边的页码仅剩余一个,让中部只剩一个省略号
    setBigLeftPages() {
      this.soonShow = false;
      this.bigLeftPages = [];
      this.bigRightPages = [this.pageRange - 1];
      this.centerPages = [];
      for (let i = 0; i < 4; i++) {
        this.bigLeftPages.push(i);
      }
    },
    // pageRange > 9 设置右边的页码，让左边的页码仅剩余一个,让中部只剩一个省略号
    setBigRightPages() {
      this.soonShow = false;
      this.bigLeftPages = [0];
      this.bigRightPages = [];
      this.centerPages = [];
      for (let i = this.pageRange - 1; i >= this.pageRange - 4; i--) {
        this.bigRightPages.unshift(i);
      }
    },
    // pageRange > 9 设置中部的页码，让左右两边的页码仅剩余一个,让中部两边都有省略号
    setBigCenterPages(index) {
      this.soonShow = true;
      this.bigLeftPages = [0];
      this.bigRightPages = [this.pageRange - 1];
      this.centerPages = [];
      for (let i = index - 2; i <= index + 2; i++) {
        this.centerPages.push(i);
      }
    },
    // 上一页
    prePage() {
      if (this.choseActiveIndex == 0) {
        return;
      }
      this.choseActiveIndex -= 1;
    },
    // 下一页
    nextPage() {
      if (this.choseActiveIndex >= this.pageRange - 1) {
        return;
      }
      this.choseActiveIndex += 1;
    },
    // 页数跳转
    turnToPage(index) {
      this.choseActiveIndex = index;
    },
    // 设置分页样式
    setPageCss() {
      for (let key in this.pageCss) {
        if (this.cssList.indexOf(key) != -1) {
          this[`${key}Style`] = this.pageCss[key];
        }
      }
    },
    // 随机数
    random() {
      return Math.random();
    }
  }
};
</script>
<style scoped>
@import "./cjhcjPaginate.css";
@import "../../static/icons/iconfont.css";
</style>