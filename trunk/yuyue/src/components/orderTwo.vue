<template>
	<div id="orderTwo">
		<div class="parking">
			<p>{{name}}</p>
		</div>
		<div class="parkingInformation">
			<!-- <p style="position: relative;"> -->
				<!-- <span>
					<span>
						<img class="imgs" src="../../static/image/wz.png"/>
					</span><span class="cLeft">停车位置：</span>
				</span>
				<span @click="posinB()">{{alias.alias}}&nbsp;<img style="width: 0.3rem;" src="../../static/image/jt.png"/></span> -->
				<!--<p v-for="po in position">{{po.alias}}</p>-->
				<!-- <ul v-if="posin==1" class="brandnameList">
					<li @click="posinN(index)" v-for="(posin,index) in position">{{posin.alias}}</li>
				</ul> -->
			<!-- </p> -->
			<p style="position: relative;">
				<span>
					<span>
						<img class="imgs" src="../../static/image/chepai.png"/>
					</span><span class="cLeft">选择车牌：</span>
				</span>
				<span @click="brandB()">{{brandname}}&nbsp;<img style="width: 0.3rem;" src="../../static/image/jt.png"/></span>
				<ul v-if="brand==1" class="brandnameList">
					<li @click="brandN(index)" v-for="(brand,index) in brandnameList" :key="index">{{brand.brandname}}</li>
					<li><router-link class='addCard' to='/card'>+</router-link></li>
				</ul>
			</p>
			<p><span><span><img class="imgs" src="../../static/image/sf.png"/></span><span class="cLeft">收费单价：</span></span><span>{{unitPrice}}元/时</span></p>
			<p class="ktime">
				<span><span><img class="imgs" src="../../static/image/kssj.png"/></span><span class="cLeft">开始时间：</span></span>
				<!--<span class="ktimeP" @click="timeClick('k')">-->
				<span class="ktimeP">
					{{ktime[0]}}年{{ktime[1]}}月{{ktime[2]}}日{{ktime[3]}}时{{ktime[4]}}分
				</span>
			</p>
			<!-- <p class="mtime">
				<span><span><img class="imgs" src="../../static/image/jssj.png"/></span><span class="cLeft">结束时间：</span></span>
				<span class="ktimeP" @click="timeClick('m')">
					{{mtime[0]}}年{{mtime[1]}}月{{mtime[2]}}日{{mtime[3]}}时{{mtime[4]}}分
				</span>
			</p> -->
			<!-- <p><span><span><img class="imgs" src="../../static/image/yjsf.png"/></span><span class="cLeft">预计收费：</span></span><span>{{totalPrice}}元</span></p> -->
		</div>
		<div class="notice">注：结束时间仅作参考，实际收费以最终结算时间为准</div>
		<div class="setUp" @click="submission()"><div>确认预约</div></div>





		<div v-if="code==0||code==1||code==2||code==3||code==4||code==5||code==6" id="popup">
			<div v-if="code==0" class="popupBox">
				<p>恭喜您！预约成功</p>
				<p><img src="../../static/image/dg.png"/></p>
				<p @click="nextOk()">确定</p>
			</div>
			<div v-if="code==1" class="popupBox">
				<p>{{mag}}</p>
				<p><img src="../../static/image/dx.png"/></p>
				<p @click="nextRefresh()">确定</p>
			</div>
			<div v-if="code==500" class="popupBox">
				<p>您未绑定手机号请先绑定后，在预约</p>
				<p><img src="../../static/image/dx.png"/></p>
				<p @click="nextRefresh()">确定</p>
			</div>
			<div v-if="code==2" class="popupBox">
				<p>预约失败！你还有未完成订单</p>
				<p><img src="../../static/image/dx.png"/></p>
				<p @click="next()">确定</p>
			</div>
			<div v-if="code==3" class="popupBox">
				<p>目标时间不能早于开始时间</p>
				<p><img src="../../static/image/dx.png"/></p>
				<p @click="next()">确定</p>
			</div>
			<div v-if="code==4" class="popupBox">
				<p>开始时间不要早于当前时间</p>
				<p><img src="../../static/image/dx.png"/></p>
				<p @click="next()">确定</p>
			</div>
			<div v-if="code==5" class="popupBox">
				<p>请先添加车牌</p>
				<p><img src="../../static/image/dx.png"/></p>
				<p @click="nextCard()">确定</p>
			</div>
			<div v-if="code==6" class="popupLod">
				<img src="../../static/image/loading.gif"/>
			</div>
		</div>
	</div>
</template>

<script>
	import Qs from 'qs'
	import axios from 'axios'
	import Picker from 'better-picker'
	export default {
		name: 'orderTwo',
		data() {
			return {
				// position:[{alias:''}],
				alias:{},
				mag:'',
				brandname:'',
				brandnameList:[{brandname:''}],
				unitPrice:'5.00',
				name:'',
				id:'',
				timeout:'',
				dtime:['','','','',''],
				ktime:['','','','',''],
				mtime:['','','','',''],
				totalPrice:'2.00',
				yearList:[
					{text: '2018',value: 0},
					{text: '2019',value: 1}
				],
				selectors:0,
				parameter:'',
				canshu:'0',
				code:7,
				orderNumber:'',
				brand:0,
				posin:0
			}
		},
		methods: {
			money(){
				let that = this;
				let time = new Date(that.mtime[0]+'/'+that.mtime[1]+'/'+that.mtime[2]+' '+that.mtime[3]+':'+that.mtime[4]).getTime()-new Date(that.ktime[0]+'/'+that.ktime[1]+'/'+that.ktime[2]+' '+that.ktime[3]+':'+that.ktime[4]).getTime()
				that.totalPrice = (((time/1000)/60)/60*that.unitPrice).toFixed(0);
			},
			brandB(){
				if(this.brand == 0){
					this.brand = 1;
					this.posin = 0;
				}else{
					this.brand = 0;
				}
			},
			brandN(index){
				console.log(index)
				this.brandname = this.brandnameList[index].brandname;
				this.brand = 0;
			},
			// posinN(index){
			// 	console.log(this.position[index])
			// 	this.alias = this.position[index];
			// 	this.posin = 0;
			// },
			posinB(){
				if(this.posin == 0){
					this.posin = 1;
					this.brand = 0;
				}else{
					this.posin = 0;
				}
			},
			submission(){
				this.ktime = [String(new Date().getFullYear()),String((new Date().getMonth()+1)<=9?'0'+(new Date().getMonth()+1):(new Date().getMonth()+1)),String(new Date().getDate()<=9?'0'+(new Date().getDate()):(new Date().getDate())),String(new Date().getHours()<=9?'0'+(new Date().getHours()):(new Date().getHours())),String(new Date().getMinutes()<=9?'0'+(new Date().getMinutes()):(new Date().getMinutes()))];
				let d = new Date(this.dtime[0]+'/'+this.dtime[1]+'/'+this.dtime[2]+' '+this.dtime[3]+':'+this.dtime[4]).getTime();
				let k = new Date(this.ktime[0]+'/'+this.ktime[1]+'/'+this.ktime[2]+' '+this.ktime[3]+':'+this.ktime[4]).getTime();
				let m = new Date(this.mtime[0]+'/'+this.mtime[1]+'/'+this.mtime[2]+' '+this.mtime[3]+':'+this.mtime[4]).getTime();
				let that = this;
					if( 
						// localStorage.getItem('openid')&& 
						// this.unitPrice&&
						this.brandname&&
						// this.position[0].id&&
						this.name
					){
						// console.log('this.alias.id')
					   let data={
					   	// openid: "oOKex1Ow5nB_qymzrzj4L7Gm7snU",
						openid:localStorage.getItem('openid'),
						price:this.unitPrice,
						carNumber:this.brandname,
						startTime:this.ktime[0]+'-'+this.ktime[1]+'-'+this.ktime[2]+' '+this.ktime[3]+':'+this.ktime[4]+':00',
						// endTime:this.mtime[0]+'-'+this.mtime[1]+'-'+this.mtime[2]+' '+this.mtime[3]+':'+this.mtime[4]+':00',
						parkingId:this.id,
						parkingName:this.name,
						phone:localStorage.getItem('phone'),
						timeout:this.timeout
					   }
					   console.log(data);
						this.$http({
							method: 'post',
							url: this.httpUrl+'app/reserve/orderSheet',
							data: Qs.stringify(data)
						}).then(function(message){
							console.log(message.data)
							if(message.data.resCode== '0'){
								that.code=0;
								that.orderNumber=message.data.orderId;
								localStorage.setItem('orderDevId',message.data.devid);
								localStorage.setItem('orderNo',message.data.orderId);
								console.log(that.orderNumber)
							}else if(message.data.resCode== '500'){
								that.code = 1
								that.mag=message.data.msg
							}
						})
					}else{
						that.code = 1;
						that.mag='请添加车牌号码'
					}
			},
			nextOk(){
				console.log(this.orderNumber)
				this.$router.push({path:'/detailed',query:{orderid:this.orderNumber}})
			},
			next(){
				this.code=7;
			},
			nextRefresh(){
				this.code=7;
			},
			nextCard(){
				this.$router.push({path:'/card'})
			}
		},
		mounted: function() {
			let that=this;
			// localStorage.getItem('openid');
			console.log(this.$route.query);
			this.unitPrice=this.$route.query.price;
			this.name = this.$route.query.name;
			this.id = this.$route.query.id
			this.timeout=this.$route.query.timeout
			// this.$http({
			// 	method: 'post',
			// 	url: this.httpUrl+'parkinglot/canrent',
			// 	data: Qs.stringify({
			// 		id:this.$route.query.id
			// 	})
			// }).then(function(message){
			// 	/*console.log(message.data)*/
			// 	that.position = message.data.result;
			// 	that.alias = message.data.result[0];
			// })
			this.$http({
				method: 'post',
				url:  this.httpUrl+'app/brand/list',
				data: Qs.stringify({
					// openid: that.testOpenid
					// openid: "oOKex1Ow5nB_qymzrzj4L7Gm7snU"
					openid:localStorage.getItem('openid')
					// localStorage.getItem('openid')
				})
			}).then(function(message){
				console.log(message.data.result)
				if(message.data.result=='未添加车牌'){
					that.code=5;
				}else{
					that.brandnameList = message.data.result;
					let len=that.brandnameList.length;
					for (var i=0;i<len;i++) {
						if(that.brandnameList[i].priority==1){
							that.brandname = message.data.result[i].brandname;
						}
					}
					
				}
			})
			document.title=this.$route.meta.title;
			this.dtime[0] = String(new Date().getFullYear());
			this.dtime[1] = String((new Date().getMonth()+1)<=9?'0'+(new Date().getMonth()+1):(new Date().getMonth()+1));
			this.dtime[2] = String(new Date().getDate()<=9?'0'+(new Date().getDate()):(new Date().getDate()));
			this.dtime[3] = String(new Date().getHours()<=9?'0'+(new Date().getHours()):(new Date().getHours()));
			this.dtime[4] = String(new Date().getMinutes()<=9?'0'+(new Date().getMinutes()):(new Date().getMinutes()));
			this.mtime = [String(new Date().getFullYear()),String((new Date().getMonth()+1)<=9?'0'+(new Date().getMonth()+1):(new Date().getMonth()+1)),String(new Date().getDate()<=9?'0'+(new Date().getDate()):(new Date().getDate())),String((new Date().getHours()+2)<=9?'0'+(new Date().getHours()+2):(new Date().getHours()+2)),String(new Date().getMinutes()<=9?'0'+(new Date().getMinutes()):(new Date().getMinutes()))];
			this.ktime = [String(new Date().getFullYear()),String((new Date().getMonth()+1)<=9?'0'+(new Date().getMonth()+1):(new Date().getMonth()+1)),String(new Date().getDate()<=9?'0'+(new Date().getDate()):(new Date().getDate())),String(new Date().getHours()<=9?'0'+(new Date().getHours()):(new Date().getHours())),String(new Date().getMinutes()<=9?'0'+(new Date().getMinutes()):(new Date().getMinutes()))];
			this.money();
			/*console.log(new Date(this.dtime[0]+'/'+this.dtime[1]+'/'+this.dtime[2]+' '+this.dtime[3]+':'+this.dtime[4]).getTime())*/
			/*let time = new Date(that.mtime[0]+'/'+that.mtime[1]+'/'+that.mtime[2]+' '+that.mtime[3]+':'+that.mtime[4]).getTime()-new Date(that.ktime[0]+'/'+that.ktime[1]+'/'+that.ktime[2]+' '+that.ktime[3]+':'+that.ktime[4]).getTime()
			that.totalPrice = ((time/1000)/60)/60*that.unitPrice;*/
		}
	}
</script>

<style scoped="scoped" lang="scss">
	@import '../style/mixin';
	#orderTwo{height: 100%;}
	.parking{top: 0;z-index: 9;background-color: #FFFFFF;width: 100%;}
	.parking p{font-size: 0.32rem;text-align: center;height:1.22rem; line-height: 1.22rem;}
	#allmap{width: 100%;height: 100%;}
	.parkingInformation{background-color: #FFFFFF;}
	.parkingInformation p{font-size: 0.26rem; border-top: 1px solid #D8D8D8;height: 1.22rem; line-height: 1.22rem;display: flex;justify-content: space-between;padding: 0 5%;}
	.parkingInformation p span:nth-of-type(1){width: 40%;}
	.parkingInformation p span:nth-of-type(2){text-align: right;color: #787878;}
	.cLeft {width: 100%;color: #333333!important;font-size: 0.26rem;margin-left: 0.25rem;}
	.parkingInformation p:nth-last-of-type(1){font-size: 0.24rem;}
	.timeSelector{ border-top: 1px solid #F68B1B; background-color:#FFFFFF ;position: absolute;bottom: 0;width: 100%;}
	.xianDA{width: 10%;border-bottom:1px solid #D8D8D8;height: 1.38rem;position: absolute;top: 0;left: 45%;}
	.xianDB{width: 10%;border-top:1px solid #D8D8D8;position: absolute;top: 2.2rem;height: 1.38rem;left: 45%;}
	.xianXA{width: 100%;position: absolute;height: 1px; top: 1.38rem;background-color: #D8D8D8;}
	.xianXB{width: 100%;position: absolute;height: 1px; top: 2.2rem; background-color: #D8D8D8;}
	.year,.month,.day,.hour,.minute{width: 100%;text-align: center;height: 0.8rem;overflow: scroll;padding: 1.4rem 0;}
	.timeSelector span{font-size: 0.32rem;display: block;width: 20%; height: 0.68rem;line-height: 0.72rem;margin: 0 auto;}
	.ktime,.mtime{display: flex;}
	.ktime .ktimeP span{border: 1px solid #D8D8D8;padding: 0 5px;}
	.mtime .ktimeP span{border: 1px solid #D8D8D8;padding: 0 5px;}
	.notice{text-align: center;margin: 0.44rem 0;color: #787878;}
	.setUp{z-index: 9;width: 100%;height: 0.92rem;text-align: center;}
	.setUp div{border: 1px solid #F68B1B;width: 90%;margin:7px auto;height: 0.88rem;line-height: 0.88rem;font-size: 0.34rem;background-color: #F68B1B;border-radius:8px;color: #FFFFFF;}
	.imgs{width: 0.44rem;height: 0.44rem; position: relative;top: 5px;}
	#popup{width: 100%;height: 100%;background-color: rgba(0,0,0,0.5);position: absolute;left: 0;top: 0;}
	.popupBox{width: 70%;background-color: #FFFFFF;text-align: center;margin: 50% auto;border-radius:5px ;}
	.popupBox p{height: 0.6rem;line-height: 0.6rem;font-size: 0.28rem;color: #F68B1B;}
	.popupBox p:nth-of-type(1){padding-top: 5px;}
	.popupBox p:nth-of-type(2){margin: 9px 0;}
	.popupBox p:nth-of-type(3){border-top:1px solid #D8D8D8;height: 0.8rem;line-height: 0.8rem;}
	.popupBox img{width: 0.6rem;}
	.popupBox p:nth-of-type(3):active{background-color: #F68B1B;color: #FFFFFF;}
	.brandnameList{position: absolute;width: 2rem;background-color: #FFFFFF;z-index: 999;right: 0;top: 1.22rem;text-align: center;border:1px solid #D8D8D8 ;overflow: scroll;max-height: 3.7rem;}
	.brandnameList li{line-height: 0.72rem;border-bottom: 1px solid #D8D8D8;}
	.brandnameList li:nth-last-of-type(1){border:none;}
	.addCard{border: 1px solid #787878;width: 0.4rem;height: 0.4rem;display: block;line-height: 0.4rem;border-radius:50% ;margin: 0.16rem 0.8rem;color: #787878;}
	.popupLod{width: 70%;text-align: center;margin: 50% auto;}
	.popupLod img{width: 50%;}
</style>