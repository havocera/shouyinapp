<template>
	<view class="content">
		<u-toast ref="uToast"></u-toast>
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
					<view class="fuwuhujiao"><iframe class="tianqi" :align="center" width="400"  scrolling="no" height="100" frameborder="0" allowtransparency="true" src="https://i.tianqi.com?c=code&id=35&icon=1&py=baotou&site=34"></iframe></view>
					<view class="fuwu2">
						
					</view>
					<view class="circle">
						
					</view>
					<view class="circle2">
						
					</view>
				</u-col>
			</u-row>

		</view>
		<view class="">
			<u-subsection :list="list"  :fontSize="25"   style="height: 3rem;" mode="subsection" :current="current" @change="sectionChange"></u-subsection>
			<scroll-view :scroll-y=true>
				<view class="shop_list" >
					<el-row :gutter="20" v-for="(item,index) in goodsList" :key="index" v-show="current==index">
						<el-col :span="6" v-for="(child,i) in item" :key="i" @click="buysku(child)" >
							<view  class="shop_item">
								<img v-if="child.product_img"  :src="child.product_img" @click="buysku(child)"
									class="item_img" alt="">
								<img v-else src="/static/yinliao.png" class="item_img" @click="buysku(child)" alt="">
								<!-- <u-image :src="child.product_img" width="160upx" height="250upx"   radius="10px"></u-image> -->
								<view class="shop_item_t" style="background-color: rgba(0, 0, 0, 0.4);">
									<span>{{child.product_name}}</span>
								
								</view>
							</view>
							
						</el-col>
					</el-row>

				</view>
			</scroll-view>

		</view>
		<!-- sku -->
		<u-popup :show="showsku" :round="10" :closeable="true" @close="showsku=false">
			<view class="" style="min-height: 65vh;">
				<view style="display: flex;margin-left: 20upx;margin-top: 15upx;margin-bottom: 20upx;">
					<view>
						<u--image :showLoading="true" :src="current_goods.product_img" width="80px"
							height="120px"></u--image>
					</view>
					<view class="product_de">
						<view class="">
							{{current_goods.product_name}}
						</view>
						<view class="" style="color:  #e45656;">
							￥<span v-if="minprice == maxprice">{{ minprice }}</span>
							<span v-else>{{ minprice }} - {{ maxprice }}</span>
						</view>
					</view>
				</view>

				<el-row type="flex" v-for="(attr, index) in process_attribute" :key="index" class="sku-info"
					justify="center" style="margin-left: 20upx;">
					<el-col>
						<div style="margin-bottom: 10px;">
							<h3>{{ attr.name }}</h3>
						</div>
						<!-- <el-button-group> -->
						<el-button v-for="(item, index2) in attr.item" style="width: 7rem;height: 3rem;" :key="index2"
							:type="item.actived ? 'danger' : ''" :disabled="item.disabled"
							@click="skuClick(index, index2)" round>{{ item.name }}</el-button>
						<!-- </el-button-group> -->

					</el-col>

				</el-row>
				<div style="margin-left: 20upx;margin-top: 20upx;">
					<div style="margin-bottom: 10px;">
						<h3>数量</h3>
					</div>
					<el-input-number style="width: 10rem;height: 3rem;" v-model="num" :min="1" :max="stock"
						@change="handleChange" />
				</div>
				<view class="bot_gouwu">
					<view class="num">
						<view class="" style="position: relative">
							<u-badge numberType="overflow" max="99" :offset="[26,5]" :absolute="true" :value="badge">

							</u-badge>
							<img src="/static/waimai.png" class="waimai" alt="">

						</view>
						<view class="" style="color: #e45656;font-size: 20px;">
							￥ <span style="font-size: 30px;">{{count_price}}</span>
						</view>
					</view>
					<view class="" style="">
						<view class="" style="display: inline-block;margin-right: 20upx;">
							<u-button type="primary" shape="circle" @click="submit" text="加入购物车"></u-button>
						</view>
						<view class="" style="display: inline-block;margin-right: 20upx;">
							<u-button type="primary" shape="circle" @click="xiadanFunction" text="立即下单"></u-button>
						</view>
					</view>
				</view>
			</view>

		</u-popup>
		<!-- sku----------------- -->
		<!-- 购物车 -->
		<u-popup :show="showgouwu" :round="10" :closeable="true" @close="showgouwu=false">
			<view style="min-height: 400upx;">
				<div class="cart">
					<el-card class="box-card" :body-style="{padding:0}">
						<template #header>
							<div class="card-header">

								<span> <el-icon>
										<ShoppingCartFull />
									</el-icon> 购物车</span>
								<!-- <el-button class="button" text>Operation button</el-button> -->
							</div>
						</template>
						<div>
							<div class="goods">
								<div v-if="select_sku.length">
									<div v-for="(sku_item, sku_index) in select_sku" :key="sku_index">


										<div style="display: flex;">

											<div style="justify-content: start;flex: 4;">{{sku_item.goods.product_name}}
											</div>
											<div style="justify-content: end;flex: 1;">￥：{{sku_item.price}}</div>
										</div>
										<div style="display: flex;width: 100%;">
											<div style="justify-content: start;flex: 4;">{{sku_item.sku.sku}}</div>
											<div style="justify-content: end;flex: 1;">数量：X{{sku_item.num}}</div>
											<div @click="delcart(sku_item)" style="color: red;cursor: pointer; ">删除
											</div>
											<!-- <el-row>
		                									<el-col :span="6">{{sku_item.sku.sku}}</el-col>
		                									<el-col :span="6">{{sku_item.num}}</el-col>
		                								</el-row> -->
										</div>
										<el-divider border-style="dashed" />
									</div>
								</div>
								<el-empty v-else description="您还没选择商品!" />
							</div>

							<view class="bot_gouwu">
								<view class="num">
									<view class="" style="position: relative">
										<u-badge numberType="overflow" max="99" :offset="[26,5]" :absolute="true"
											:value="badge">

										</u-badge>
										<img src="/static/waimai.png" class="waimai" alt="">

									</view>
									<view class="" style="color: #e45656;font-size: 20px;">
										￥ <span style="font-size: 30px;">{{count_price}}</span>
									</view>
								</view>
								<view class="" style="">
									<view class="" style="display: inline-block;margin-right: 20upx;">
										<u-button type="primary" shape="circle" @click="xiadanFunction" text="立即下单"></u-button>
									</view>

								</view>
							</view>
						</div>

					</el-card>
				</div>
			</view>
		</u-popup>
		<view class="bot_gouwu" v-if="badge">
			<view class="num">
				<view class="" style="position: relative" @click="showgouwu=true">
					<u-badge numberType="overflow" max="99" :offset="[26,5]" :absolute="true" :value="badge">

					</u-badge>
					<img src="/static/waimai.png" class="waimai" alt="">

				</view>
				<view class="" style="color: #e45656;font-size: 20px;">
					￥ <span style="font-size: 30px;">{{count_price}}</span>
				</view>
			</view>
			<view class="" style="">
				<view class="" style="display: inline-block;margin-right: 20upx;">
					<u-button type="primary" shape="circle" @click="xiadanFunction" text="立即下单"></u-button>
				</view>

			</view>
		</view>
	</view>
</template>

<script>
	import request from '../../api/index.js'
	export default {
		data() {
			return {
				list: ['咖啡&茶', '鲜榨', '饮料', '餐食', '烟酒', '球具'],
				// 或者如下，也可以配置keyName参数修改对象键名
				// list: [{name: '未付款'}, {name: '待评价'}, {name: '已付款'}],
				current: 0,
				src: 'https://cdn.uviewui.com/uview/album/1.jpg',
				goodsList: [],
				showgouwu: false,
				select_sku: [],
				count_price: 0,
				showsku: false,
				attribute: [],
				process_attribute: [],
				process_sku: [],
				sku: [],
				process_attribute: [],
				process_sku: [],
				stock: '',
				num: 1,
				badge: 0,
				minprice: '',
				maxprice: '',
				separator: ';',
				current_goods: {},
				count_price: 0,
				current_qiutai: "",
				qiutai_id:1,
			}
		},
		onLoad() {

		},
		onShow() {
			this.getDiancan()
		},
		methods: {
			sectionChange(index) {
				console.log(index)
				this.current = index;
			},
			getDiancan() {
				request("getdiancan", "post").then((res) => {
					this.goodsList = res.data
				})
			},
			buysku(item) {
				this.current_goods = item
				request("getSkudiancan",
					"post",
					item,
				).then((res) => {
					if (res.code) {
						this.sku = res.data.sku;
						this.attribute = res.data.attribute
						this.process_attribute = []
						this.process_sku = []
						this.stock = ''
						this.init()
						console.log(this.process_attribute)
						this.showsku = true

					} else {
						ElNotification({
							title: '错误',
							message: res.message,
							type: 'error',
						})
					}
				})
			},
			delcart(row) {
				let index = this.select_sku.findIndex(item => {
					if (item.sku.name = row.sku.name) {
						return true
					}
				})
				this.count_price = this.count_price - row.price
				this.select_sku.splice(index, 1)
				this.badge = this.select_sku.length
			},
			submit() {
				let sku = []
				let isSelectSKU = this.process_attribute.every(attr => {
					let filter = attr.item.filter(v => v.actived)
					if (filter.length != 0) {
						sku.push(filter[0].name)
					}
					return filter.length != 0
				})
				if (isSelectSKU) {
					let sku_detail = {}
					this.sku.forEach((item) => {

						if (item.sku == sku.join(this.separator)) {
							sku_detail = item
						}
					})
					this.count_price = this.count_price + sku_detail.price * this.num
					this.select_sku.push({
						goods: this.current_goods,
						sku: sku_detail,
						num: this.num,
						price: sku_detail.price * this.num
					})
					console.log(this.select_sku)
					this.showsku = false
					this.badge = this.select_sku.length

					this.showToast({
						type: 'success',
						message: "已加入购物车",
						iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/success.png'
					})
				} else {
					ElNotification({
						title: '错误',
						message: res.message,
						type: 'error',
					})

				}
			},
			showToast(params) {
				this.$refs.uToast.show({
					...params,
					complete() {
						params.url && uni.navigateTo({
							url: params.url
						})
					}
				})
			},
			xiadanFunction() {
				request("addFoodzhangdan",
					"post", {
						qiutai_id: this.qiutai_id,
						zhangdan: this.select_sku
					}).then((res) => {
					if (res.code) {
						this.select_sku = []
						this.current_qiutai = ""
						this.count_price = 0
						this.num = 0
						this.badge = 0
						// xiadanqiutaidialog.value = false
						this.showToast({
							type: 'success',
							message: "下单成功",
							iconUrl: 'https://cdn.uviewui.com/uview/demo/toast/success.png'
						})



					} else {
						ElNotification({
							title: '错误',
							message: res.message,
							type: 'error',
						})
					}
				})
			},
			// -------------------------------------------------------------
			init() {
				// 对 attribute 数据进行加工，并存入 process_attribute 中
				this.attribute.map(attr => {
					let temp = {
						name: attr.name
					}
					temp.item = attr.item.map(item => {
						return {
							name: item,
							actived: false,
							disabled: false
						}
					})
					this.process_attribute.push(temp)
				})
				// 对 sku 数据进行加工，并存入 process_sku 中
				this.sku.map(v => {
					var combArr = this.arrayCombine(v.sku.split(this.separator))
					for (var j = 0; j < combArr.length; j++) {
						var key = combArr[j].join(this.separator)
						if (this.process_sku[key]) {
							// 库存累加，价格添加进数组
							this.process_sku[key].stock += v.stock
							this.process_sku[key].prices.push(v.price)
						} else {
							this.process_sku[key] = {
								stock: v.stock,
								prices: [v.price]
							}
						}
					}
				})
				// 更新数据视图
				this.process_sku = Object.assign({}, this.process_sku)
				this.skuCheck()
			},
			arrayCombine(targetArr) {
				var resultArr = []
				for (var n = 0; n <= targetArr.length; n++) {
					var flagArrs = this.getFlagArrs(targetArr.length, n)
					while (flagArrs.length) {
						var flagArr = flagArrs.shift()
						var combArr = Array(targetArr.length)
						for (var i = 0; i < targetArr.length; i++) {
							if (flagArr[i]) {
								combArr[i] = targetArr[i]
							}
						}
						resultArr.push(combArr)
					}
				}
				return resultArr
			},
			getFlagArrs(m, n) {
				var flagArrs = [],
					flagArr = [],
					isEnd = false
				for (var i = 0; i < m; i++) {
					flagArr[i] = i < n ? 1 : 0
				}
				flagArrs.push(flagArr.concat())
				// 当n不等于0并且m大于n的时候进入
				if (n && m > n) {
					while (!isEnd) {
						var leftCnt = 0
						for (var i = 0; i < m - 1; i++) {
							if (flagArr[i] == 1 && flagArr[i + 1] == 0) {
								for (var j = 0; j < i; j++) {
									flagArr[j] = j < leftCnt ? 1 : 0
								}
								flagArr[i] = 0
								flagArr[i + 1] = 1
								var aTmp = flagArr.concat()
								flagArrs.push(aTmp)
								if (aTmp.slice(-n).join('').indexOf('0') == -1) {
									isEnd = true
								}
								break
							}
							flagArr[i] == 1 && leftCnt++
						}
					}
				}
				return flagArrs
			},
			skuClick(key1, key2) {
				if (!this.process_attribute[key1].item[key2].disabled) {
					this.process_attribute[key1].item.map((item, index) => {
						item.actived = index == key2 ? !item.actived : false
					})
					this.skuCheck()
					this.getStockPrice()
				}
			},
			skuCheck() {
				let sku = []
				this.process_attribute.map(attr => {
					let name = ''
					attr.item.map(item => {
						if (item.actived) {
							name = item.name
						}
					})
					sku.push(name)
				})
				this.stock = this.process_sku[sku.join(this.separator)].stock
				this.minprice = Math.min.apply(Math, this.process_sku[sku.join(this.separator)].prices)
				this.maxprice = Math.max.apply(Math, this.process_sku[sku.join(this.separator)].prices)
			},
			getStockPrice() {
				this.process_attribute.map(attr => {
					attr.item.map(item => {
						item.disabled = false
					})
				})
				let count = 0
				let i = 0
				this.process_attribute.map((attr, index) => {
					let flag = false
					attr.item.map(item => {
						if (item.actived) {
							flag = true
						}
					})
					if (!flag) {
						count += 1
						i = index
					}
				})
				// 当只有一组规格没选时
				if (count == 1) {
					this.process_attribute[i].item.map(item => {
						let sku = []
						let text = item.name
						this.process_attribute.map((attr, index) => {
							if (index != i) {
								attr.item.map(item2 => {
									if (item2.actived) {
										sku.push(item2.name)
									}
								})
							} else {
								sku.push(text)
							}
						})
						if (this.process_sku[sku.join(this.separator)].stock == 0) {
							item.disabled = true
						}
					})
				}
				// 当所有规格都有选时
				if (count == 0) {
					this.process_attribute.map((attr, index) => {
						let i = index
						this.process_attribute[index].item.map(item => {
							if (!item.actived) {
								let sku = []
								let text = item.name
								this.process_attribute.map((list, index) => {
									if (index != i) {
										list.item.map(item2 => {
											if (item2.actived) {
												sku.push(item2.name)
											}
										})
									} else {
										sku.push(text)
									}
								})
								if (this.process_sku[sku.join(this.separator)].stock ==
									0) {
									item.disabled = true
								}
							}
						})
					})
				}
			}
		}
	}
</script>

<style lang="scss">
	.content {
		height: 95vh;
		// background-color: #606266;
	}

	.conta {
		// position: fixed;
		// top:0;
	}

	.bot_gouwu {
		display: flex;
		background-color: #f4f4f5;
		justify-content: space-between;
		position: absolute;
		bottom: 0;
		width: 100%;
		height: 60upx;
		line-height: 60upx;
		font-size: 25upx;

		.num {
			display: flex;
			justify-content: start;
		}

		.waimai {
			height: 60upx;
		}
	}
.u-subsection__item{
	
}
.u-subsection__item{
	border-color: #000!important;
}
	.u-subsection{
		border-color: #fff!important;
		box-shadow: 10px 10px 59px 19px rgba(0,0,0,0.1),-5px 10px 18px -3px rgba(0,0,0,0.1);
	}

	.shop_list {
		margin-top: 20upx;
		margin-left: 10upx;
		margin-right: 10upx;

		.shop_item {
			position: relative;
			border-radius: 2rem;
			overflow: hidden;
			box-shadow: 10px 10px 72px 19px rgba(0,0,0,0.1),-5px 10px 18px -3px rgba(0,0,0,0.1);
		}

		.item_img {
			width: 180upx;
			height: 250upx;
		}

		.shop_item_t {
			position: absolute;
			bottom: 0;
			width: 180upx;
			line-height: 40upx;
			color: #fff;

			span {
				margin-left: 20upx;
			}

		}
	}

	

	.product_de {
		line-height: 40upx;
		font-size: 27upx;
		margin-left: 15upx;
	}

	.cart {
		flex: 1;
		justify-content: flex-end;
		min-width: 30rem;
		height: 100%;
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
			min-height: 65vh;
		}

		.goods {
			overflow: auto;
			max-height: calc(85vh - 44px);
			padding: 20px;
		}

		.heji {
			position: absolute;
			bottom: 5px;
			width: 80%;
			margin: 0 auto;
			border-top: 1px dashed rgb(238, 238, 238);
			background-color: rgb(255, 255, 255);
			display: flex;
			flex-direction: space-between;
			width: 90%;
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