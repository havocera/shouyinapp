<template>
	<view class="show_content">
		<view class="conta">
		
			<u-row customStyle="margin-bottom: 10px;">
				<u-col span="6">
					<view class="zhuohao">
						<view class="zhuohao-title">
							桌号
						</view></br>
						<view class="zhuohao-num" >1</view>
					</view>
				</u-col>
				<u-col span="6" class="fuwu">
					<view class="fuwuhujiao"><iframe width="400" scrolling="no" height="100" frameborder="0" allowtransparency="true" src="https://i.tianqi.com?c=code&id=35&icon=1&py=baotou&site=34"></iframe></view>
					<view class="fuwu2">
						
					</view>
					<view class="circle">
						
					</view>
					<view class="circle2">
						
					</view>
				</u-col>
			</u-row>
		
		</view>
		<view class="wifi">
			<view class="">
				
			</view>
		</view>
		<view class="qrcode">
			<view class="qr-item" v-if="setting.qrcode1">
				<img :src="setting.qrcode1" width="200" height="200" alt="">
			</view>
			<view class="qr-item" v-if="setting.qrcode2">
				<img :src="setting.qrcode2" width="200" height="200"  alt="">
			</view>
			<view class="qr-item" v-if="setting.qrcode3">
				<img :src="setting.qrcode3" width="200" height="200"  alt="">
			</view>
		</view>
	</view>
</template>

<script>
	import request from '../../api/index.js'
	export default {
		data() {
			return {
				setting:{}
			}
		},
		onShow() {
			this.getsetting()
		},
		methods: {
			getsetting() {
				let that =this
				request("getsetting",
					'post'
				).then((res) => {
					if (res.code) {
						// data.select_qiutai=[]
			
						this.setting = res.data;
			
					} else {
			
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.show_content{
		height: calc(100vh - 70px);
		background-image: url("../../static/bg.png");
		background-size: cover;
	}
	.wifi{
		display: flex;
		justify-content: center;
	}
	.qrcode{
		display: flex;
		justify-content: center;
		.qr-item{
			margin: 0 10upx;
		}
	}

</style>
