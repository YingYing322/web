<template>
  <div
    :style="{ width: width + 'px', height: height + 'px' }"
    class="inventoryList-table"
    id="demo"
  >
    <table width="100%" align="center" border="0" cellspacing="0">
      <tbody
        id="demo1"
        style="width: 500px; height: 370px; display: block"
      >
        <tr
          v-for="(item) in inventoryData"
          :key="item.index"
          align="center"
          valign="center"
          style="height: 50px"
        >
          <td>{{ item.receiptOrderNo }}</td>
          <td>{{ item.receiptOrderType }}</td>
          <td>{{ item.supplierName }}</td>
          <td>{{ item.orderNo }}</td>
          <td>{{ item.receiptOrderStatus }}</td>
          <td>{{ item.createTime }}</td>
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
      default: 400,
    },
  },
  created() {
    this.getinventoryList();
  },
  mounted() {},
  data() {
    return {
      inventoryData: [],
    };
  },
  methods: {
    getinventoryList() {
      axios({
        url:'http://',
        method:'get',
      }).then(response => {
        this.inventoryData = response;
        this.getinventoryListScroll();
      })
    },
    getinventoryListScroll() {
      var demo = document.getElementById("demo");
      var speed = 60; //滚动速度值，值越大速度越慢
      function Marquee() {
        if (demo.scrollTop + demo.clientHeight >= demo.scrollHeight) {
          demo.scrollTop = 0;
        } else {
          demo.scrollTop++;
        }
      }
      var MyMar = setInterval(Marquee, speed); //设置定时器
      demo.onmouseover = function () {
        clearInterval(MyMar);
      }; //鼠标经过时清除定时器达到滚动停止的目的
      demo.onmouseout = function () {
        MyMar = setInterval(Marquee, speed);
      }; //鼠标移开时重设定时器
    },
  },
};
</script>

<style lang="scss">
.inventoryList-table {
  height: 370px;
  overflow: hidden;
  table {
    height: 350px;
    margin: 10px 0;
    td {
      width: 83px;
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