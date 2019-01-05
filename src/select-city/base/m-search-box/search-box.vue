<template>
  <div class="search_component"> 
     <div class="search-box">
      <i class="icon-search"></i>
      <input ref="query"  class="input_box" :placeholder="placeholder" v-model="query"/>
      <i class="close" @click="cancelEvent"></i>
      <button class="cancel" @click="cancelEvent">取消</button>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
export default {
  props: {
    placeholder: {
      type: String,
      default: "城市/拼音/首字母"
    }
  },
  data() {
    return {
      query: ""
    };
  },
  methods: {
    clear() {
      this.query = "";
    },
    setQuery(query) {
      this.query = query;
    },
    cancelEvent() {
      this.query = "";
    }
  },
  // 把用户输入的内容以watch的形式暴露出去
  // query变化就去派发一个事件
  created() {
    this.$watch("query", newQuery => {
      this.$emit("query", newQuery);
    });
  }
};
</script>

<style scoped lang="less">
@import "../../common/less/mixin.less";
.search_component {
  position: fixed;
  height: 1.3rem;
  top:0;
  .search-box {
    width: 100%;
    padding: 0 0.2rem;
    height: 1.3rem;
    font-size: 0.28rem;
    color: #999;
    margin: 0 auto;
    border-radius: 0.04rem;
    position: relative;
    background: #fff;
    display: flex;
    align-items: center;
    .icon-search {
      margin-left: 0.18rem;
      height: 0.4rem;
      width: 0.4rem;
      .bgimg("../../common/img/icon_search.png");
      display: inline-block;
      position: absolute;
      left: 0.2rem;
      top: 50%;
      transform: translateY(-0.2rem);
    }
    .input_box {
      background: #eee;
      font-size: 0.32rem;
      height: 0.8rem;
      display: inline-block;
      border: 0;
      border-radius: 0.1rem;
      color: #999;
      width: 7.4rem;
      // vertical-align: 0.05rem;
      padding-left: 0.8rem;
      margin-right: 0.2rem;
      font-family: PingFangSC-Regular;
      letter-spacing: 0;
    }
    .cancel {
      height: 0.8rem;
      background: #eee;
      border: 0;
      font-size: 0.35rem;
      vertical-align: baseline;
      width: 1rem;
      color: #666666;
    }
    .close {
      width: 0.5rem;
      height: 0.5rem;
      border-radius: 50%;
      background-color: #999;
      position: absolute;
      left: 7.6rem;
      position: absolute;
    }
    .close::after,
    .close::before {
      width: 0.4rem;
      position: absolute;
      height: 0.03rem;
      background-color: #fff;
      content: "";
      top: 0.22rem;
      left: 0.06rem;
      transform: rotate(45deg);
    }
    .close::after {
      width: 0.03rem;
      height: 0.4rem;
      left: 0.23rem;
      top: 0.03rem;
    }
  }
}
</style>
