<template>
  <div
    :style="{ width: width + 'px', height: height + 'px' }"
    ref="chartContainer"
  ></div>
</template>

<script>
import * as echarts from 'echarts';
import axios from 'axios';
function contains(arr, dst) {
  var i = arr.length;
  while ((i -= 1)) {
    if (arr[i] == dst) {
      return i;
    }
  }
  return false;
} 
function attackSourcesDataFmt(sData) {
  var sss = [];
  sData.forEach(function (item, i) {
    let itemStyle = {
      color: i > 3 ? attackSourcesColor[3] : attackSourcesColor[i],
    };
    sss.push({
      value: item,
      itemStyle: itemStyle,
    });
  });
  return sss;
}
var attackSourcesColor = [
  new echarts.graphic.LinearGradient(0, 1, 1, 1, [
    { offset: 0, color: "#95565d" },
    { offset: 1, color: "#b69f87" },
  ]),
  new echarts.graphic.LinearGradient(0, 1, 1, 1, [
    { offset: 0, color: "#b35d23" },
    { offset: 1, color: "#c7a23b" },
  ]),
  new echarts.graphic.LinearGradient(0, 1, 1, 1, [
    { offset: 0, color: "#8b9118" },
    { offset: 1, color: "#ccc48e" },
  ]),
  new echarts.graphic.LinearGradient(0, 1, 1, 1, [
    { offset: 0, color: "#576b9c" },
    { offset: 1, color: "#1f8b8f" },
  ]),
];
export default {
  name: "MaterialRanking",
  props: {
    width: {
      type: Number,
      default: 400,
    },
    height: {
      type: Number,
      default: 400,
    },
  },
  data() {
    return {
      rank: [],
    };
  },
  created() {
    this.getMaterialRanking();
  },
  // mounted() {  
  //   this.$nextTick(() => {
  //   });
  // },
  methods: {
    getMaterialRanking() {
      axios({
        url:'http://',
        method:'get',
      }).then(response => {
        this.rank = response;
        this.initChart();
      })
    },
    initChart() {
      const chartContainer = this.$refs.chartContainer;
      if (chartContainer) {
        let myChart = echarts.init(chartContainer);
        var data = this.rank;
        var attackSourcesColor1 = [
          "#b5696e",
          "#c28c63",
          "#9eba61",
          "#808ec8",
          "#5d86a6",
          "#af6a65",
          "#75a1aa",
          "#996c68",
          "#628eaa",
          "#1a936f",
        ];
        var attaData = [];
        var attaName = [];
        var topName = [];
        data.forEach((it, index) => {
          attaData[index] = it.storage;
          var reversedIndex = data.length - 1 - index;
          attaName[reversedIndex] = it.itemName;
          topName[reversedIndex] = `${it.itemName} `;
        });
        var salvProMax = []; //背景按最大值
        for (let i = 0; i < attaData.length; i++) {
          salvProMax.push(attaData[0]);
        }
        let option = {
          // title: {
          //   show: true,
          //   text: '物料排行',
          //   textStyle: {
          //     color: '#454545',
          //     fontStyle: 'normal',
          //     fontWeight: 'bold',
          //     fontFamily: 'sans-serif',
          //     fontSize: 25,
          //     lineHeight: 25,
          //   },
          //   textAlign: 'auto',
          //   textVerticalAlign: 'auto',
          //   left: '20%',
          //   right: '55%',
          //   top: '2.5%',
          // },
          tooltip: {
            show: false,
            backgroundColor: "rgb(128, 128, 128, 0.1)", //背景颜色（此时为默认色）
            textStyle: {
              fontSize: 16,
            },
          },
          color: "#F7B731",
          legend: {
            pageIconSize: [12, 12],
            itemWidth: 20,
            itemHeight: 10,
            textStyle: {
              //图例文字的样式
              fontSize: 10,
              color: "#4F4F4F",
            },
            selectedMode: false,
            data: ["院校数量排行榜"],
          },
          grid: {
            width: "127%",
            height:"102%",
            left: "-22%",
            top: "0%",
            containLabel: true
          },
          xAxis: {
            type: "value",
            splitLine: {
              show: false,
            },
            axisLabel: {
              show: false,
            },
            axisTick: {
              show: false,
            },
            axisLine: {
              show: false,
            },
          },
          yAxis: [
            {
              type: "category",
              inverse: true,
              axisLine: {
                show: false,
              },
              axisTick: {
                show: false,
              },
              axisPointer: {
                label: {
                  show: true,
                  // margin: 30
                },
              },
              padding: [5, 0, 0, 0],
              postion: "right",
              data: attaName,
              axisLabel: {
                margin: 35,
                fontSize: 10,
                align: "left",
                padding: [2, 0, 0, 0],
                color: "#363636",
                rich: {
                  nt1: {
                    color: "#fff",
                    backgroundColor: attackSourcesColor1[0],
                    width: 18,
                    height: 18,
                    fontSize: 12,
                    fontWeight: "bold",
                    align: "center",
                    borderRadius: 100,
                    lineHeight: "5",
                    padding: [0, 1, 2, 1],
                  },
                  nt2: {
                    color: "#fff",
                    backgroundColor: attackSourcesColor1[1], //圆形标签
                    width: 18,
                    height: 18,
                    fontSize: 12,
                    fontWeight: "bold",
                    align: "center",
                    borderRadius: 100,
                    padding: [0, 1, 2, 1],
                  },
                  nt3: {
                    color: "#fff",
                    backgroundColor: attackSourcesColor1[2],
                    width: 18,
                    height: 18,
                    fontSize: 12,
                    fontWeight: "bold",
                    align: "center",
                    borderRadius: 100,
                    padding: [0, 1, 2, 1],
                  },
                  nt: {
                    color: "#fff",
                    backgroundColor: attackSourcesColor1[3],
                    width: 18,
                    height: 18,
                    fontSize: 12,
                    fontWeight: "bold",
                    align: "center",
                    // lineHeight: 3,
                    borderRadius: 100,
                    padding: [0, 1, 2, 1],
                    lineHeight: 5,
                  },
                },
                formatter: function (value, index) {
                  index = contains(attaName, value) + 1;
                  if (index - 1 < 3) {
                    return ["{nt" + index + "|" + index + "}"].join("\n");
                  } else {
                    return ["{nt|" + index + "}"].join("\n");
                  }
                },
              },
            },
            // {
            //   type: "category",
            //   inverse: true,
            //   axisTick: "none",
            //   axisLine: "none",
            //   show: true,
            //   axisLabel: {
            //     textStyle: {
            //       color: "#262626",
            //       fontSize: "10",
            //     },
            //   },
            //   data: attackSourcesDataFmt(attaName),
            // },
            {
              //名称
              type: "category",
              offset: -10,
              position: "left",
              // axisLabel: {
              //   color: "#363636",
              //   fontSize: 10
              // },
              axisLine: {
                show: false,
              },
              inverse: false,
              axisTick: {
                show: false,
              },
              axisLabel: {
                interval: 0,
                color: ["#cacaca"],
                align: "left",
                verticalAlign: "bottom",
                lineHeight: 30,
                fontSize: 14,
              },
              data: topName,
            },
          ],
          series: [
            {
              zlevel: 1,
              name: "个人所得(亿元)",
              type: "bar",
              barWidth: "15px",
              animationDuration: 1500,
              data: attackSourcesDataFmt(attaData),
              align: "center",
              itemStyle: {
                normal: {
                  barBorderRadius: 10,
                },
              },
              label: {
                show: true,
                fontSize: 13,
                color: "#fff",
                textBorderWidth: 3,
                padding: [2, 0, 0, 0],
              },
            },
            {
              name: "个人所得(亿元)",
              type: "bar",
              barWidth: 15,
              barGap: "-100%",
              margin: "20",
              data: salvProMax,
              textStyle: {
                //图例文字的样式
                fontSize: 10,
                color: "#363636",
              },
              itemStyle: {
                normal: {
                  color: "#45525f", //柱形底色
                  //width:"100%",
                  fontSize: 10,
                  barBorderRadius: 30,
                },
              },
            },
          ],
        };
        myChart.setOption(option);
      } else {
        console.error("无法找到图表容器");
      }
    },
  },
};
</script>

<style>
</style>