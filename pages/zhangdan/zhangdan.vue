<template>
	<view>
		<view class="conta">
		
			<u-row customStyle="margin-bottom: 10px;">
				<u-col span="6">
					<view class="zhuohao">
						<view class="zhuohao-title">
							桌号
						</view></br>
						<view class="zhuohao-num">1</view>
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
		<view class="cart">
			<el-card class="box-card">
				<template #header>
					<view class="card-header">

						<span> <el-icon>
								<ShoppingCartFull />
							</el-icon>购物账单</span>
						<!-- <el-button class="button" text>Operation button</el-button> -->
					</view>
				</template>
				<view>

					<view class="goods">
						<view v-for="(qiutaiitem, qiutaiindex) in zhangdan" :key="qiutaiindex">


							<view style="display: flex;">

								<view style="justify-content: start;flex: 4;">{{qiutaiitem.goods}}</view>
								<view style="justify-content: end;flex: 1;">￥：{{qiutaiitem.price}}</view>
							</view>
							<view style="display: flex;width: 100%;">
								<view style="justify-content: start;flex: 4;">{{qiutaiitem.sku}}</view>
								<view style="justify-content: end;flex: 1;">数量：X{{qiutaiitem.num}}</view>
								<view v-if="qiutaiitem.is_app" style="color: red;cursor: pointer; ">球台点单
								</view>
								<!-- <el-row>
													<el-col :span="6">{{sku_item.sku.sku}}</el-col>
													<el-col :span="6">{{sku_item.num}}</el-col>
												</el-row> -->
							</view>
							<el-divider border-style="dashed" />
						</view>
						<el-empty v-if="zhangdan.lenght==0" description="您还没选择商品!" />
					</view>

					<view class="heji">
						<view class="jine">
							总计金额：￥{{food_count}}
						</view>
						<!-- <view class="xiadan">下单</view> -->
					</view>
				</view>

			</el-card>
		</view>
	</view>
</template>

<script>
	import request from '../../api/index.js'
	export default {
		data() {
			return {
				zhangdan: [],
				food_count: 0
			}
		},
		onShow() {
			this.getzhangdan()
		},
		methods: {
			heji() {
				this.food_count = 0
			
				this.zhangdan.forEach((item) => {
					this.food_count += parseFloat(item.price)
				})
				console.log(this.food_count)
				// data.sum_count = data.qiutai_count + data.food_count + data.zhujiao_count
				// data.balance.price = data.sum_count
			},
			getzhangdan() {
				let that =this
				request("getzhangdanAll",
					"post", {
						id: 1
					}
				).then((res) => {
					if (res.code) {
						// data.select_qiutai=[]

						this.zhangdan = res.data;

						that.heji()
					} else {

					}
				})
			}
		},
		
	}
</script>

<style lang="scss">
	
	.cart {
		flex: 1;
		justify-content: flex-end;
		min-width: 30rem;
		height: 100%;
		min-height: 30vh;
		position: relative;

		// padding-top: 5vh;
		.card-header {
			font-size: 1.3rem;


			.el-icon {
				height: 1.5rem;
				width: 1.5rem;
				vertical-align: middle;
			}
		}

		.box-card {
			height: 100%;
			min-height: calc(60vh - 400upx);
			box-shadow: 10px 10px 59px 19px rgba(0,0,0,0.1),-5px 10px 18px -3px rgba(0,0,0,0.1);
			width: 90%;
			margin: 0 auto;
			border-radius: 2rem;
			overflow: hidden;
		}

		.goods {
			overflow: auto;
			max-height: calc(85vh - 44px);
			padding: 20px;
		}

		.heji {
			// position: absolute;
			bottom: 5px;
			width: 80%;
			margin: 0 auto;
			border-top: 1px dashed rgb(238, 238, 238);
			background-color: rgb(255, 255, 255);
			display: flex;
			flex-direction: space-between;
			width: 80%;
			height: 44px;
			bottom: 1px;

			.jine {
				flex: 1 1 0%;
				line-height: 44px;
				font-weight: normal;
			}

			.xiadan {
				line-height: 44px;
				background-color: rgb(70, 132, 226);
				color: rgb(255, 255, 255);
				padding: 0px 16px;
				font-weight: bold;
				cursor: pointer;
			}
		}
	}
</style>