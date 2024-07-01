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
  name: "TypeProportion",
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
      type: [],
    };
  },
  created() {
    this.getTypeProportion();
  },
  // mounted() {
  //   this.$nextTick(() => {
  //     this.initChart();
  //   });
  // },
  methods: {
    getTypeProportion() {
      axios({
        url:'http://',
        method:'get',
      }).then(response => {
        this.type = response;
        this.initChart();
      });
    },
    initChart() {
      const chartContainer = this.$refs.chartContainer;
      if (chartContainer) {
        let myChart = echarts.init(chartContainer);
        const rawData = this.type;
        const styleMap = {
          中压电缆: { color: "#b4aeff", borderColor: "#000", borderWidth: 5 },
          低压电缆: { color: "#42b0d8", borderColor: "#000", borderWidth: 5 },
          原材料: { color: "#6f8ff8", borderColor: "#000", borderWidth: 5 },
        };
        const seriesData = rawData.map((item) => ({
          value: item.storage,
          name: item.identification,
          itemStyle: styleMap[item.identification],
        }));
        let option = {
          tooltip: {
            trigger: "item",
          },
          legend: {
            orient: "vertical",
            left: "right",
            top: "22%",
            textStyle: {
              color: "#fff",
            },
          },

          series: [
            {
              name: "物料类型",
              type: "pie",
              radius: ["32%", "62%"],
              left: "0%",
              data: seriesData,
              label: {
                show: true,
                fontSize: 17,
              },
              emphasis: {
                itemStyle: {
                  shadowBlur: 10,
                  shadowOffsetX: 0,
                  shadowColor: "rgba(0, 0, 0, 0.5)",
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