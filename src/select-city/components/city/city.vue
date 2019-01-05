<template lang="html">
  <transition name="slide">
    <div class="xin-widget-citys animated">
      <!-- 搜索框 -->
      <SearchBox class="search" @query="query" :placeholder="placeholder"></SearchBox>
      <!-- 当前定位城市 -->
      <div class="currentCity" v-if="queryCity===''">
        <ul>
          <div class="box">
            <h2>定位城市</h2>
            <i class="line"></i>
          </div>
          <li class="positionCity">深圳市</li>
        </ul>
      </div>
      <!-- 热门城市 -->
      <div class="hotCity">
        <div class="title">热门城市</div>
        <div class="contentList">
          <ul>
            <li v-for="(item,index) in hotCityList" :key="index" class="hotCityItem" @click="selectSearchItem(item)">{{item.name}}</li>
          </ul>
        </div>
      </div>
      <!-- 城市选择 -->
      <Scroll :data="searchList" class="searchlist" v-if="queryCity !== ''" :class="{'bg':searchList.length === 0}">
        <div>
          <ul v-if="searchList.length!==0">
            <li class="bdb" v-for="item in searchList" @click="selectSearchItem(item)">{{item.name}}</li>
          </ul>
            <img v-else src="../../common/img/404.png" class="nomatch"/>
        </div>
      </Scroll>
      <CityList class="city" v-if="queryCity===''" @selectItem="selectItem"></CityList>
    </div>
  </transition>
</template>

<script>
import Scroll from "base/m-scroll/scroll";
import SearchBox from "base/m-search-box/search-box";
import { citylist } from "common/js/city";
import CityList from "base/city-list/city-list";
import { changeTitle } from "common/js/dom.js";
import { mapMutations } from "vuex";
export default {
  components: {
    SearchBox,
    CityList,
    Scroll
  },
  data() {
    return {
      city: [],
      letterList: [],
      searchList: [], //搜索结果
      queryCity: "",
      placeholder: "城市/拼音/首字母",
      hotCityList: [
        {
          name:'北京市',
          zip: "010",
        },
        {
          name:'上海市',
          zip: "021",
        },
        {
          name:'深圳市',
          zip: "0755",
        },
        {
          name:'广州市',
          zip: "020",
        },
        {
          name:'重庆市',
          zip: "023",
        },
        {
          name:'哈尔滨市',
          zip: "0451",
        },
        {
          name:'沈阳市',
          zip: "024",
        },
        {
          name:'成都市',
          zip: "028",
        },
        {
          name:'郑州市',
          zip: "0371",
        },
        {
          name:'苏州',
          zip: "0512",
        },
        {
          name:'厦门市',
          zip: "0592",
        },
        {
          name:'南京市',
          zip: "025",
        },
        {
          name:'武汉市',
          zip: "027",
        },
        {
          name:'杭州',
          zip: "0571",
        },
        {
          name:'株洲市',
          zip: "0731",
        },
        {
          name:'长沙市',
          zip: "0731",
        },
      ]
    };
  },

  methods: {
    handleScroll() {},
    hide() {
      this.$router.back();
    },
    query(newVal) {
      this._search(newVal);
      this.queryCity = newVal;
    },
    // 搜索
    _search(newVal) {
      var reg = new RegExp(newVal == "" ? "xxyy" : newVal, "ig");
      var _arr = [];
      for (var i in this.letterList) {
        for (var j = 0; j < this.letterList[i].length; j++) {
          if (
            reg.test(this.letterList[i][j]["name"]) ||
            reg.test(this.letterList[i][j]["firstLetter"])
          ) {
            _arr.push(this.letterList[i][j]);
          }
        }
      }
      
      this.searchList = _arr;
    },
    // 序列化数组
    _formatCityList(arr) {
      var letterArr = {};
      for (var i = 0; i < arr.length; i++) {
        if (!(arr[i]["initial"] in letterArr)) {
          letterArr[arr[i]["initial"]] = [];
          for (var j = 0; j < arr[i].list.length; j++) {
            letterArr[arr[i]["initial"]].push(arr[i].list[j]);
          }
        } else {
          for (var j = 0; j < arr[i].list.length; j++) {
            letterArr[arr[i]["initial"]].push(arr[i].list[j]);
          }
        }
      }
      this.letterList = letterArr;
    },
    // 给数组添首字母
    _addFirstLetter(citylist) {
      for (var i = 0; i < citylist.length; i++) {
        for (var j = 0; j < citylist[i].list.length; j++) {
          citylist[i].list[j]["firstLetter"] = citylist[i].initial;
        }
      }
      this._formatCityList(citylist);
    },
    selectItem(item) {
      this.afterSelect(item);
    },
    selectSearchItem(item) {
      this.afterSelect(item);
      // console.log(item);
      
    },
    // 选择之后的操作
    afterSelect(item) {
      this.$router.back();
      this.setCity(item.name);
      this.setCityId(item.zip);
    },
    ...mapMutations({
      setCity: "SET_CITY",
      setCityId: "SET_CITYID"
    })
  },
  mounted() {
    this._addFirstLetter(citylist);
    changeTitle("选择城市");
  }
};
</script>

<style lang="less" scoped>
.xin-widget-citys {
  background: #f5f5f9;
  width: 100%;
  .currentCity {
    margin-top: 1.3rem;
    padding: 0 0.2rem;
    margin-bottom: 0.3rem;
    ul {
      .box {
        // background-color: pink;
        height: 0.7rem;
        line-height: 0.7rem;
        display: flex;
        align-items: center;
        margin-bottom: 0.1rem;
        h2 {
          font-size: 0.3rem;
          color: #999;
          margin-bottom: 0.22rem;
          margin-top: 0.32rem;
          margin-right: 0.3rem;
          font-family: PingFangSC-Regular;
        }
        .line {
          display: block;
          width: 7rem;
          height: 0.01rem;
          background-color: #999990;
        }
      }

      .positionCity {
        text-align: center;
        width: 20%;
        padding: 0.15rem 0;
        font-size: 0.28rem;
        color: #666666;
        letter-spacing: 0;
        border: 0.025rem solid #e3e3e3;
        border-radius: 8px;
        background-color: #fff;
        &.active{
          color: #fc5e55;
          border: 0.025rem solid #fc5e55;
        }
      }
    }
  }
  // 热门城市选择
  .hotCity {
    padding: 0 0.2rem;
    .title {
      font-size: 0.3rem;
      color: #999;
      font-family: PingFangSC-Regular;
      margin-bottom: 0.22rem;
    }
    .contentList {
      ul {
        display: flex;
        flex-wrap: wrap;
        .hotCityItem {
          width: 20%;
          line-height: 0.8rem;
          text-align: center;
          font-family: PingFangSC-Regular;
          font-size: 0.3rem;
          color: #666666;
          letter-spacing: 0;
          border: 0.025rem solid #e3e3e3;
          border-radius: 0.1rem;
          background-color: #fff;
          margin-right: 0.25rem;
          margin-bottom: 0.25rem;
          &.active {
            color: #fc5e55;
            border: 0.025rem solid #fc5e55;
          }
        }
      }
    }
  }
  .searchlist {
    position: fixed;
    top: 1.3rem;
    width: 100%;
    bottom: 0;
    overflow: hidden;
    background: #fff;
    ul {
      li {
        font-size: 0.28rem;
        color: #323232;
        width: 6.56rem;
        margin: 0 auto;
        height: 0.82rem;
        line-height: 0.82rem;
        border-bottom: 1px solid #f6f4f4;
      }
    }
    .nomatch {
      display: block;
      margin: 0 auto;
      margin-top: 2.04rem;
      width: 2.09rem;
      height: 1.95rem;
    }
  }
  .bg {
    background: #f6f4f4;
  }
}
.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s;
}
.slide-enter,
.slide-leave-to {
  transform: translate3d(100%, 0, 0);
}
</style>
