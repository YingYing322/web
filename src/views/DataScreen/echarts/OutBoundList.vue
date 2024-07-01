<template>
  <div
    class="OutBoundList-table"
    :style="{ width: width + 'px', height: height + 'px' }"
    id="demo3"
  >
    <table width="100%" align="center" border="0" cellspacing="0">
      <tbody id="demo4" style="width: 500px; height: 410px; display: block">
        <tr
          v-for="(item) in OutBountData"
          :key="item.index"
          align="center"
          valign="center"
          style="height: 50px"
        >
          <td style="width: 100px;">{{ item.shipmentOrderNo }}</td>
          <td style="width: 120px;">{{ item.shipmentOrderType }}</td>
          <td style="width: 90px;">{{ item.customerName }}</td>
          <td style="width: 90px;">{{ item.orderNo }}</td>
          <td style="width: 110px;">{{ item.shipmentOrderStatus }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  props: {
    width: {
      type: Number,
      default: 400,
    },
    height: {
      type: Number,
      default: 500,
    },
  },
  created() {
    this.getOutBoundList();
  },
  mounted() {},
  data() {
    return {
      OutBountData: [],
    };
  },
  methods: {
    getOutBoundList() {
      axios({
        url:'http://',
        method:'get',
      }).then(response => {
        this.OutBountData = response;
        this.getOutBoundListScroll();
      });
    },
    getOutBoundListScroll() {
      var demo3 = document.getElementById("demo3");
      var speed1 = 60; //滚动速度值，值越大速度越慢
      function Marquee1() {
        if (demo3.scrollTop + demo3.clientHeight >= demo3.scrollHeight) {
          demo3.scrollTop = 0;
        } else {
          demo3.scrollTop++;
        }
      }
      var MyMar1 = setInterval(Marquee1, speed1); //设置定时器
      demo3.onmouseover = function () {
        clearInterval(MyMar1);
      }; //鼠标经过时清除定时器达到滚动停止的目的
      demo3.onmouseout = function () {
        MyMar1 = setInterval(Marquee1, speed1);
      }; //鼠标移开时重设定时器
    },
  },
};
</script>

<style lang="scss">
.OutBoundList-table {
  height: 410px;
  overflow: hidden;
  table {
    height: 390px;
    margin-top: 20px;
    td {
      font-size: 14px;
      color: #ffffffee;
    }
  }
}
*::-webkit-scrollbar {
  display: none;
}
* {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}
</style>