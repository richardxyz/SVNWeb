<template>
  <div style="width:100%;position:fixed;top:0;left:0;"
       id="shengcheng"
       class="rating_page">
    <!-- <header style="width:100%;height:1.5rem;position: relative;">
      <img src="../../static/image/qiantu.png"
           alt=""
           style="width: 100%;height: 100%;">
      <div style="position: absolute;top:0.4rem;width: 40%;left:50%;margin-left: -20%;text-align: center;color: #fff;font-family: 'PingFang-SC-Regular';">
        <p style="font-size: 13px;">当前积分</p>
        <div style="font-size: 40px;padding-top:0.1rem!important;"
             class="lesfen"
             v-if="jifen==1">{{numji}}</div>
        <div style="font-size: 40px;padding-top:0.1rem!important;"
             class="lesfen"
             v-if="jifen==2">0</div>
      </div> -->
    <!-- </header> -->
    <!-- <section style="height: 0.15rem;">

    </section>
    <footer style="text-align: center;background: #fff;">
      <p style="font-size: 16px;color: rgba(48,48,48);padding-top:0.19rem;margin-bottom:0.27rem;">兑换优惠券</p>
      <p style="margin-bottom:0.34rem;">
        <span class="tong">您将消耗</span>
        <span class="nums">100</span>
        <span class="tong">积分兑换</span>
        <span class="nums laoi">1</span>
        <span class="tong">元优惠券</span>
      </p>
      <div style="display:flex;justify-content: space-between; margin-bottom: 0.35rem;"
           class="inldiv">
        <div>
          <p style="width: 25px;height: 25px;float: right"><img src="../../static/image/z (1).png"
                 alt=""
                 style="width: 100%;height: 100%;"
                 @click="jian"></p>
        </div>
        <div style="text-align:center;line-height:0.2rem; margin-top: -0.06rem;
    margin-right: 0.05rem;">
          <input type="text"
                 v-model="shenum"
                 class="num"
                 readonly="readonly"
                 style="font-size:0.15rem;
    font-family: 'PingFang-SC-Regular';
    color: rgba(48, 48, 48);text-align: center;border: none;outline: none;background: #fff;    width:100%;
    ">
        </div>
        <div>
          <p style="width: 25px;height: 25px;"><img src="../../static/image/z (2).png"
                 alt=""
                 style="width: 100%;height: 100%;"
                 @click="addf"></p>
        </div>
      </div>
      <p style="padding-bottom:0.18rem;">
        <span class="comp">温馨提示：优惠券</span>
        <span class="gouqi"
              style="font-size: 12px;color:rgba(255,59,59) ">{{this.getNowFormatDate().day}}</span>
        <span class="comp">日过期，请及时使用！</span>
      </p>
    </footer>
    <div class="paytootsl"
         @click="dunh">立即兑换</div> -->
    <div>
      <div v-for="item in numji"
           class="parkname">
        <div v-for="items in item.parkingName">{{items}}</div>
        <div>{{item.allMark}}</div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import Qs from 'qs'
import { MessageBox } from 'mint-ui';
export default {
  name: 'shengcheng',
  data () {
    return {
      numji: [],
      jifen: 1,
      shenum: 0,
      show: ''
    }
  },
  watch: {

  },
  methods: {
    // addf () {
    //   if (this.numji == '') {
    //     console.log(98)
    //   } else {
    //     this.shenum = this.shenum += 100
    //   }

    // },
    // jian () {
    //   if (this.shenum <= 0) {

    //   } else {
    //     this.shenum = this.shenum -= 100
    //   }
    // },
    // dunh () {
    //   let that = this;
    //   let data = {
    //     tel: localStorage.getItem('tel'),
    //     parking_id: 'PI1540957659034816320',
    //     mark: that.shenum,
    //     sub: (that.shenum) / 100,
    //     start_time: that.getNowFormatDates().day + ' ' + that.getNowFormatDates().time,
    //     end_time: that.getNowFormatDate().day + ' ' + that.getNowFormatDate().time,
    //     dealer: 123
    //   }
    //   if (data.mark > that.numji || data.mark == 0) {
    //     MessageBox('提示', '您的积分不足')
    //     return false
    //   }
    //   that.$http({
    //     method: 'post',
    //     url: `${that.httpUrl}/MarkRecords/exchangeMark`,
    //     data: Qs.stringify(data)
    //   }).then(res => {
    //     if (res.data.resCode == "0") {
    //       MessageBox('提示', '优惠券兑换成功').then(action => {
    //         that.$router.go(-1)
    //         sessionStorage.setItem('ticknumbar3', res.data.ticketId)
    //       })
    //     } else {
    //       MessageBox('提示', '兑换失败');
    //     }
    //   }).catch(error => {
    //     MessageBox('提示', '兑换失败');
    //   })
    // },
    // getNowFormatDate () {
    //   let date = new Date();
    //   let seperator1 = "-";
    //   let seperator2 = ":";
    //   let month = date.getMonth() + 2;
    //   let strDate = date.getDate();
    //   let Hours = date.getHours() > 9 ? date.getHours() : '0' + date.getHours();
    //   let Minutes = date.getMinutes() > 9 ? date.getMinutes() : '0' + date.getMinutes();
    //   let Seconds = date.getSeconds() > 9 ? date.getSeconds() : '0' + date.getSeconds();
    //   if (month >= 1 && month <= 9) {
    //     month = "0" + month;
    //   }
    //   if (strDate >= 0 && strDate <= 9) {
    //     strDate = "0" + strDate;
    //   }
    //   let data = {
    //     day: date.getFullYear() + seperator1 + month + seperator1 + strDate,
    //     time: Hours + seperator2 + Minutes + seperator2 + Seconds
    //   };
    //   return data;
    // },
    // getNowFormatDates () {
    //   let date = new Date();
    //   let seperator1 = "-";
    //   let seperator2 = ":";
    //   let month = date.getMonth() + 1;
    //   let strDate = date.getDate();
    //   let Hours = date.getHours() > 9 ? date.getHours() : '0' + date.getHours();
    //   let Minutes = date.getMinutes() > 9 ? date.getMinutes() : '0' + date.getMinutes();
    //   let Seconds = date.getSeconds() > 9 ? date.getSeconds() : '0' + date.getSeconds();
    //   if (month >= 1 && month <= 9) {
    //     month = "0" + month;
    //   }
    //   if (strDate >= 0 && strDate <= 9) {
    //     strDate = "0" + strDate;
    //   }
    //   let data = {
    //     day: date.getFullYear() + seperator1 + month + seperator1 + strDate,
    //     time: Hours + seperator2 + Minutes + seperator2 + Seconds
    //   };
    //   return data;
    // }
  },
  mounted () {
    // console.log(this.getNowFormatDate())
    // document.title=this.$route.meta.title;
    let that = this;
    that.$http({
      method: 'post',
      url: `${that.httpUrl}/MarkRecords/findMark`,
      data: Qs.stringify({
        tel: localStorage.getItem('tel'),
        dealer: '1561430951496'
      })
    }).then(res => {
      if (res.data.resCode == 0) {
        that.numji = res.data.result
        that.jifen = 1
      } else {
        that.jifen = 2
      }
    })
  }
}
</script>
<style scoped>
.rating_page {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #efefef;
  z-index: 204;
}
.inldiv div {
  width: 30%;
  text-align: center;
}
.tong {
  font-size: 13px;
  font-family: 'PingFang-SC-Regular';
  color: rgba(48, 48, 48);
}
.nums {
  font-size: 15px;
  font-family: 'PingFang-SC-Regular';
  color: rgba(255, 59, 59);
}
.laoi {
  color: #f68b1b;
}
.comp {
  font-family: 'PingFang-SC-Regular';
  color: rgba(48, 48, 48);
  font-size: 12px;
}
.paytootsl {
  width: 80%;
  margin: 0 auto;
  height: 0.5rem;
  line-height: 0.5rem;
  text-align: center;
  font-size: 16px;
  border-radius: 0.5rem;
  background: #f68b1b;
  color: #fff;
  cursor: pointer;
  margin-top: 0.2rem;
}
.parkname {
  /* display: flex; */
  /* justify-content: space-between; */
}
.parkname div {
  line-height: 0.2rem;
  font-size: 15px;
}
</style>


