<template>
  <div
    :style="{ width: width + 'px', height: height + 'px' }"
    ref="chartContainer"
  ></div>
</template>

<script>
import * as echarts from 'echarts';
import axios from 'axios';
export default {
  name: "RegisterInvenOut",
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
      data: [],
      receipts: [],
      shipments: [],
      ratios: [],
    };
  },
  created() {
    this.getRegisterInvenOut();
  },
  // mounted() {
  //   this.$nextTick(() => {
  //   });
  // },
  methods: {
    getRegisterInvenOut() {
      axios({
        url:'http://',
        method:'get',
      }).then(response => {
        response.forEach((item) => {
          this.receipts.push(item.receipt);
          this.shipments.push(item.shipment);
          this.ratios.push(item.ratio);
        });
        this.initChart();
      });
    },
    initChart() {
      const chartContainer = this.$refs.chartContainer;
      if (chartContainer) {
        let myChart = echarts.init(chartContainer);
        let option = {
          grid: {
            left: "0%",
            right: "0%",
            top: "20%",
            bottom: "1%",
            containLabel: true,
          },
          tooltip: {
            trigger: "axis",
            axisPointer: {
              type: "cross",
              crossStyle: {
                color: "#999",
              },
            },
          },
          toolbox: {
            feature: {
              dataView: {
                show: true,
                readOnly: false,
              },
              magicType: {
                show: true,
                type: ["line", "bar"],
              },
              restore: {
                show: true,
              },
              saveAsImage: {
                show: true,
              },
            },
          },
          legend: {
            data: ["入库数量", "出库数量", "出库/入库"],
            top: "8%",
            left: "20%",
            textStyle: {
              color: "#",
              fontSize: 15,
              // fontStyle: 'normal', // 'normal' 或 'italic'
              // fontWeight: 'normal', // 'normal'、'bold'、'bolder'、'lighter' 或 100-900 的数字
            },
          },
          xAxis: [
            {
              type: "category",
              data: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"],
              axisPointer: {
                type: "shadow",
              },
              axisLabel: {
                textStyle: {
                  color: "#fafafa",
                },
              },
              axisLine: {
                lineStyle: {
                  color: "#989898",
                },
              },
              axisTick: {
                lineStyle: {
                  color: "#fafafa",
                },
              },
            },
          ],
          yAxis: [
            {
              type: "value",
              name: "出库数量",
              min: 0,
              max: 1000,
              interval: 150,
              axisLabel: {
                formatter: "{value} 件",
                textStyle: {
                  color: "#dddddd",
                },
              },
              axisLine: {
                lineStyle: {
                  color: "#888888",
                },
              },
              axisTick: {
                lineStyle: {
                  color: "#dddddd",
                },
              },
            },
            {
              type: "value",
              name: "入库数量",
              min: 0,
              max: 1000,
              interval:150,
              axisLabel: {
                formatter: "{value} 件",
                textStyle: {
                  color: "#dddddd",
                },
              },
              axisLine: {
                lineStyle: {
                  color: "#777777",
                },
              },
              axisTick: {
                lineStyle: {
                  color: "#dddddd",
                },
              },
            },
          ],
          series: [
            {
              name: "出库数量",
              type: "bar",
              itemStyle: {
                color: "#ece69d",
              },
              tooltip: {
                valueFormatter: function (value) {
                  return value;
                },
              },
              data: this.shipments,
            },
            {
              name: "入库数量",
              type: "bar",
              itemStyle: {
                color: "#7dd08b",
              },
              tooltip: {
                valueFormatter: function (value) {
                  return value;
                },
              },
              data: this.receipts,
            },
            {
              name: "出库/入库",
              type: "line",
              itemStyle: {
                color: "#7dbafe",
              },
              lineStyle: {
                width: 2.5,
              },
              yAxisIndex: 1,
              tooltip: {
                valueFormatter: function (value) {
                  return value;
                },
              },
              data: this.ratios,
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