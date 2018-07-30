<template lang="html">
  <div id="bPage">
    <TopHeader :message="msg" class="TopHeader"></TopHeader>

    <div class="content" :style="{height: contentH + 'px'}" ref="wrapper" @scroll="scrollEvent()">
      <p>
       测试输入: <input type="text"/>
      </p>
      <Loadmore class="LoadMore" :top-method="loadTop" :bottom-method="loadBottom" ref="loadmore">
        <div class="item" v-for="(index, elem) in dataArray" @click="itemClick(index, elem)">
          <p class="itemP">item_{{index}}</p>
        </div>
      </Loadmore>
    </div>

  </div>
</template>

<script>
import TopHeader from '@/components/TopHeader';
import { Loadmore } from 'mint-ui';

let isFromC = false;

export default {
  data() {
    return {
      msg: "BPage",
      dataArray: [0, 1, 2, 3, 4, 5],
      contentH: 0
    }
  },
  beforeRouteEnter(to, from, next) {

    if (from.name != 'C') {
      isFromC = false;
    } else {
      isFromC = true;
    }

    next();
  },
  components: {
    TopHeader,
    Loadmore
  },
  mounted() {
    console.log("mounttttttttttttttttttttt")
    // mint-ui loadmore组件需要包裹，且内容高度要高去包裹才可加载更多，所以给它一个 指定的高度
    this.contentH = document.documentElement.clientHeight - this.$refs.wrapper.getBoundingClientRect().top;
  },
  beforeCreate () {
        console.log('beforeCreate 创建前状态===============》');
        //  console.log("%c%s", "color:red" , "el     : " + this.$el); //undefined
        //  console.log("%c%s", "color:red","data   : " + this.$data); //undefined 
        //  console.log("%c%s", "color:red","message: " + this.message)  
  },
  created: function () {
      console.group('created 创建完毕状态===============》');
      console.log("%c%s", "color:red","el     : " + this.$el); //undefined
          console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化 
          console.log("%c%s", "color:red","message: " + this.message); //已被初始化
  },
  beforeMount: function () {
      console.group('beforeMount 挂载前状态===============》');
      console.log("%c%s", "color:red","el     : " + (this.$el)); //已被初始化
      console.log(this.$el);
          console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化  
          console.log("%c%s", "color:red","message: " + this.message); //已被初始化  
  },
  
  beforeUpdate: function () {
      console.group('beforeUpdate 更新前状态===============》');
      console.log("%c%s", "color:red","el     : " + this.$el);
      console.log(this.$el);   
          console.log("%c%s", "color:red","data   : " + this.$data); 
          console.log("%c%s", "color:red","message: " + this.message); 
  },
  updated: function () {
      console.group('updated 更新完成状态===============》');
      console.log("%c%s", "color:red","el     : " + this.$el);
      console.log(this.$el); 
          console.log("%c%s", "color:red","data   : " + this.$data); 
          console.log("%c%s", "color:red","message: " + this.message); 
  },
  beforeDestroy: function () {
      console.group('beforeDestroy 销毁前状态===============》');
      console.log("%c%s", "color:red","el     : " + this.$el);
      console.log(this.$el);    
      console.log("%c%s", "color:red","data   : " + this.$data); 
      console.log("%c%s", "color:red","message: " + this.message); 
  },
  destroyed: function () {
      console.group('destroyed 销毁完成状态===============》');
      console.log("%c%s", "color:red","el     : " + this.$el);
      console.log(this.$el);  
      console.log("%c%s", "color:red","data   : " + this.$data); 
      console.log("%c%s", "color:red","message: " + this.message)
  },
  activated() {
    console.log("activated===================")
    if (isFromC) { // 来自C页面
      this.$refs.wrapper.scrollTop = this.$store.state.pageYOffset;

    } else { // 滚动到最顶，数据初始化
      this.$refs.wrapper.scrollTop = 0;

      this.dataArray = [0, 1, 2, 3, 4, 5,6,7,8,8,9,10];
    }
  },
  deactivated() {
    console.log("deactivated-----------------")
  },
  methods: {
    // 下拉刷新
    loadTop() {
      let that = this;
      setTimeout(function() {
        that.dataArray = [0, 1, 2, 3, 4, 5];
        that.$refs.loadmore.onTopLoaded();
      }, 1500);
    },
    // 上拉加载更多
    loadBottom() {
      let that = this;
      setTimeout(function() {

        let tempArray = [];
        let lastItem = that.dataArray[that.dataArray.length - 1];
        for (let i = 0; i < 6; i ++) {
          that.dataArray.push(i + lastItem + 1);
        }
        that.$refs.loadmore.onBottomLoaded();
      }, 1500);
    },
    itemClick(item, index) { // 进入C页面
      // 保存滚动条位置
      this.$store.commit('setPageYOffset', this.$refs.wrapper.scrollTop);
      this.$router.push({
        name: 'C',
        params: {
          item: item,
          index: index
        }
      });
    },
    scrollEvent() {
      console.log('scrollEvent===');
    }
  }
}
</script>

<style lang="css" scoped="bVue">

.itemP {
  height: 90px;
  line-height: 40px;
  background-color: gray;
  color: white;
  padding-left: 12px;
  margin: 12px 12px 12px 12px;
}

.TopHeader {
  width: 100%;
  position: fixed;
  top: 0;
  z-index: 10;
}

.content {
  margin-top: 49px;
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch;
}

</style>
