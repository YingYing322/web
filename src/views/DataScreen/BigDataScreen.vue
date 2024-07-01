<template>
  <div class="echartsBox" ref="echartsBox">
    <el-row class="DataScreenTitle">
      <el-col>
        <div class="grid-screen screenTitle">
          <span>江西赣越电缆仓库管理可视化大屏</span>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :xs="7" :sm="7" ::lg="7">
        <div class="grid-screen InventoryList">
          <div class="InventoryListTitle">
            <span>入库数据清单</span>
          </div>
          <div class="InventoryListThead">
            <ul>
              <li>入库单号</li>
              <li>入库类型</li>
              <li>供应商</li>
              <li>订单号</li>
              <li>入库状态</li>
              <li>创建时间</li>
            </ul>
          </div>
          <InventoryList :width="500" :height="360"></InventoryList>
          <div class="InventoryList-footer"></div>
        </div>
      </el-col>
      <el-col :xs="10" :sm="10" ::lg="10">
        <div class="grid-screen TotalOutputValue">
          <TotalOutputValue></TotalOutputValue>
        </div>
      </el-col>
      <el-col :xs="7" :sm="7" ::lg="7">
        <div class="grid-screen TypeProportion">
          <div class="TypeProportionTitle">
            <span>物料类型占比</span>
          </div>
          <TypeProportion :width="500" :height="400"></TypeProportion>
          <div class="TypeProportion-footer"></div>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :xs="7" :sm="7" ::lg="7">
        <div class="grid-screen OutBoundList">
          <div class="OutBoundListTitle">
            <span>出库数据清单</span>
          </div>
          <div class="OutBoundListThead">
            <ul>
              <li>出库单号</li>
              <li>出库类型</li>
              <li>供应商</li>
              <li>订单号</li>
              <li>出库状态</li>
            </ul>
          </div>
          <OutBoundList :width="500" :height="420"></OutBoundList>
          <div class="OutBoundList-footer"></div>
        </div>
      </el-col>
      <el-col :xs="10" :sm="10" ::lg="10">
        <div class="grid-screen MaterialRanking">
          <div class="MaterialRankingTitle">
            <span>物料排行</span>
          </div>
          <MaterialRanking :width="800" :height="450"></MaterialRanking>
          <div class="MaterialRanking-footer"></div>
        </div>
      </el-col>
      <el-col :xs="7" :sm="7" ::lg="7">
        <div class="grid-screen RegisterInvenOut">
          <div class="RegisterInvenOutTitle">
            <span>近一周出入库数量</span>
          </div>
          <RegisterInvenOut :width="500" :height="440"></RegisterInvenOut>
          <div class="RegisterInvenOut-footer"></div>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import InventoryList from "./echarts/InventoryList.vue";
import OutBoundList from "./echarts/OutBoundList.vue";
import MaterialRanking from "./echarts/MaterialRanking.vue";
import RegisterInvenOut from "./echarts/RegisterInvenOut.vue";
import TotalOutputValue from "./echarts/TotalOutputValue.vue";
import TypeProportion from "./echarts/TypeProportion.vue";
export default {
  components: {
    InventoryList,
    OutBoundList,
    MaterialRanking,
    RegisterInvenOut,
    TotalOutputValue,
    TypeProportion,
  },
  name: "App",
  mounted() {
    this.adaptToDevice();
    window.addEventListener("resize", this.adaptToDevice);
  },
  methods: {
    adaptToDevice() {
      // 设计稿：1920 * 1080
      let targetWidth = 1920;
      let targetHeight = 1080;
      // 获取真实视口尺寸
      let currentWidth =
        document.documentElement.clientWidth || document.body.clientWidth;
      let currentHeight =
        document.documentElement.clientHeight || document.body.clientHeight;
      // 计算缩放比例
      let scaleWidth = currentWidth / targetWidth;
      let scaleHeight = currentHeight / targetHeight;
      this.$refs.echartsBox.style = `transform: scale(${scaleWidth},${scaleHeight})`;
    },
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.adaptToDevice);
  },
};
</script>

<style lang="scss" scoped>
.echartsBox {
  width: 1920px;
  height: 1080px;
  position: relative;
  overflow: hidden;
  background: url("../../assets/screen_images/bg.png") no-repeat !important;
  background-size: 100%;
  /* 设置缩放原点 */
  transform-origin: left top;
  box-sizing: border-box;
}

.el-row {
  /* &:last-child {
    margin-bottom: 0;
  } */
  margin: 0 0 20px 0 !important;
}

.el-col {
  border-radius: 4px;
}

.DataScreenTitle {
  margin-bottom: 10px !important;
  background: url("../../assets/screen_images/top.png") no-repeat top center;
  margin-left:-30px !important;
}

.grid-screen {
  border-radius: 10px;
}

.grid-screen.screenTitle {
  min-height: 80px;
}

.screenTitle {
  display: flex;
  align-items: center;
  justify-content: center;

  span {
    margin-left:30px;
    color: rgb(233, 231, 231);
    font-weight: 800;
    font-size: 2.1rem;
    letter-spacing: 4px;
  }
}

//入库清单
.InventoryList {
  background-color: rgba(255, 255, 255, 0.05);
  min-height: 440px;
  display: flex;
  flex-direction: column;
  align-items: center;

  .InventoryListTitle {
    width: 100%;
    height: 30px;
    background: url("../../assets/screen_images/title.png") no-repeat center;
    background-size: 45%;
    color: #dbdada;
    text-align: center;
    font-size: 22px;
    position: relative;
    left: 0;
    bottom: 0;
    span {
      vertical-align: middle;
      color: #e6f0f1ee;
      letter-spacing: 2px;
    }
  }
  .InventoryListTitle::before {
    position: absolute;
    bottom: 10px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .InventoryListTitle::after {
    position: absolute;
    bottom: 10px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
  .InventoryListThead {
    ul {
      width: 500px;
      height: 20px;
      display: flex;
      justify-content: space-around;
      list-style-type: none;
      padding: 0;
      margin: 10px 0;
    }

    li {
      color: #e4e4e4fe;
      font-size: 16px;
      letter-spacing: 1px;
    }
  }

  .InventoryList-footer {
    position: relative;
    left: 0;
    bottom: 0;
    width: 100%;
  }

  .InventoryList-footer::before {
    position: absolute;
    bottom: -10px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .InventoryList-footer::after {
    position: absolute;
    bottom: -10px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
}

//中心
.TotalOutputValue {
  min-height: 450px;
  // background-color: rgba(255, 255, 255, 0.2);
}

// 物料类型
.TypeProportion {
  background-color: rgba(255, 255, 255, 0.05);
  min-height: 440px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  .TypeProportionTitle {
    width: 100%;
    height: 30px;
    background: url("../../assets/screen_images/title.png") no-repeat center;
    background-size: 48%;
    color: #dbdada;
    text-align: center;
    font-size: 22px;
    position: relative;
    left: 0;
    bottom: 0;
    span {
      vertical-align: middle;
      color: #e6f0f1ee;
      letter-spacing: 2px;
    }
  }
  .TypeProportionTitle::before {
    position: absolute;
    bottom: 15px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .TypeProportionTitle::after {
    position: absolute;
    bottom: 15px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
  .TypeProportion-footer {
    position: relative;
    left: 0;
    bottom: 0;
    width: 100%;
  }

  .TypeProportion-footer::before {
    position: absolute;
    bottom: -5px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .TypeProportion-footer::after {
    position: absolute;
    bottom: -5px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
}

// 出库清单
.OutBoundList {
  background-color: rgba(255, 255, 255, 0.05);
  min-height: 500px;
  display: flex;
  flex-direction: column;
  align-items: center;

  .OutBoundListTitle {
    width: 100%;
    height: 20px;
    margin-top: 10px;
    background: url("../../assets/screen_images/title.png") no-repeat center;
    background-size: 45%;
    color: #dbdada;
    text-align: center;
    font-size: 22px;
    position: relative;
    left: 0;
    bottom: 0;
    span {
      display: block;
      margin-top: -6px;
      color: #e6f0f1ee;
      letter-spacing: 2px;
    }
  }
  .OutBoundListTitle::before {
    position: absolute;
    bottom: 10px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .OutBoundListTitle::after {
    position: absolute;
    bottom: 10px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
  .OutBoundListThead {
    ul {
      width: 500px;
      height: 20px;
      display: flex;
      justify-content: space-around;
      list-style-type: none;
      padding: 0;
      margin: 10px 0;
    }

    li {
      color: #e4e4e4fe;
      font-size: 16px;
      letter-spacing: 1px;
    }
  }

  .OutBoundList-footer {
    position: relative;
    left: 0;
    bottom: 0;
    width: 100%;
  }

  .OutBoundList-footer::before {
    position: absolute;
    bottom: -10px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .OutBoundList-footer::after {
    position: absolute;
    bottom: -10px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
}

//物料排行
.MaterialRanking {
  min-height: 500px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.05);

  .MaterialRankingTitle {
    width: 100%;
    height: 30px;
    background: url("../../assets/screen_images/title.png") no-repeat center;
    background-size: 45%;
    background-position-y: top;
    margin-top: 10px;
    color: #dbdada;
    text-align: center;
    font-size: 23px;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    position: relative;
    left: 0;
    bottom: 0;
    span {
      margin-top: -8px;
      color: #f8f8f8;
      letter-spacing: 2px;
    }
  }
  .MaterialRankingTitle::before {
    position: absolute;
    bottom: 19px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .MaterialRankingTitle::after {
    position: absolute;
    bottom: 19px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
  .MaterialRanking-footer {
    position: relative;
    left: 0;
    bottom: 0;
    width: 100%;
  }

  .MaterialRanking-footer::before {
    position: absolute;
    bottom: -10px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .MaterialRanking-footer::after {
    position: absolute;
    bottom: -10px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
}

//出入库数量记录
.RegisterInvenOut {
  background-color: rgba(255, 255, 255, 0.05);
  min-height: 500px;
  display: flex;
  flex-direction: column;
  align-items: center;

  .RegisterInvenOutTitle {
    width: 100%;
    height: 20px;
    margin-top: 10px;
    background: url("../../assets/screen_images/title.png") no-repeat center;
    background-size: 48%;
    color: #dbdada;
    text-align: center;
    display: table;
    font-size: 22px;
    position: relative;
    left: 0;
    bottom: 0;
    span {
      display: block;
      margin-top: -3px;
      color: #e6f0f1ee;
      letter-spacing: 1px;
    }
  }
  .RegisterInvenOutTitle::before {
    position: absolute;
    bottom: 15px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .RegisterInvenOutTitle::after {
    position: absolute;
    bottom: 15px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-top: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
  .RegisterInvenOut-footer {
    position: relative;
    left: 0;
    bottom: 0;
    width: 100%;
  }

  .RegisterInvenOut-footer::before {
    position: absolute;
    bottom: -23.5px;
    left: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-left: 2px solid #a6f9ffcb;
  }

  .RegisterInvenOut-footer::after {
    position: absolute;
    bottom: -23.5px;
    right: 0;
    content: "";
    width: 20px;
    height: 20px;
    border-bottom: 2px solid #a6f9ffcb;
    border-right: 2px solid #a6f9ffcb;
  }
}
</style>