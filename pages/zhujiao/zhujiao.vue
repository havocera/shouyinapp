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
					<view class="fuwuhujiao"><iframe width="400"  scrolling="no" height="100" frameborder="0" allowtransparency="true" src="https://i.tianqi.com?c=code&id=35&icon=1&py=baotou&site=34"></iframe></view>
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
			<u-modal :show="show" :title="title" :closeOnClickOverlay="true" @close="show=false" @cancel="show=false" :showCancelButton="true" cancelText="取消选择" confirmText="确认选择" @confirm="queren" :content='content'></u-modal>
			<scroll-view :scroll-y=true>
				<view class="shop_list">
					<el-row :gutter="10">
						<el-col :span="6" v-for="(item,i) in rowdata" :key="i" class="shop_item">
							<img v-if="item.img" :src="item.img" @click="isstart(item)" class="item_img" alt="">
							<!-- <u--image :showLoading="true" :src="item.img" @click="" width="160upx" radius="10px"></u--image> -->
							<view class="shop_item_t" @click="isstart(item)"
								style="background-color: rgba(0, 0, 0, 0.3);display: flex;justify-content: space-between;">
								<view class="" style="margin-left: 10upx;">
									{{item.name}}
								</view>
								<view class="status" :class="item.start_time==null?'noac':'ac'" style="">
									<span class="noac" v-if="!item.start_time" style="">点击选择助教</span> <span class="ac"
										v-else>点击可预约助教</span>
								</view>
							</view>
						</el-col>
					</el-row>
				</view>
			</scroll-view>
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
				current: {},
				src: 'https://cdn.uviewui.com/uview/album/1.jpg',
				rowdata: [],
				show: false,
				title: '提示',
				content: '确定下单该助教吗？',

			}
		},
		onShow() {
			this.getRowData()
		},
		methods: {
			getRowData() {
				request("getzhujiaoView", "post").then((res) => {
					if (res.code) {
						this.rowdata = res.data;


					} else {

					}
				})

			},
			
			isstart(item) {
				this.current = item
				if (item.start_time == null) {
					this.content="确定下单该助教吗？"
					this.show = true
				} else {
					this.content = `确认预约该助教吗？还需等待${item.yuyue}位。`
					this.show = true
				}
			},
			tostartZhujiao() {
				request("startzhujiao", "post", {
					id: this.current.id,
					qiutai_id: 1
				}).then((res) => {
					if (res.code) {

						getRowData()

					} else {

					}
				})
			},
			toyuyue() {
				request( "yuyueStartzhujiao","post", this.current,).then((res) => {
					if (res.code) {
						
						getRowData()
						
					} else {
						
					}
				})
			},queren(){
				if (this.current.start_time == null) {
					this.show = false
					this.tostartZhujiao()
				} else {
					
					this.show = false
					this.toyuyue()
				}
			},

		}
	}
</script>

<style lang="scss">
	.shop_list {
		margin-top: 20upx;
		margin-left: 10upx;
		margin-right: 10upx;

		.shop_item {
			position: relative;
			margin-bottom: 10upx;
			border-radius: 10upx;
		}

		.shop_item_t {
			position: absolute;
			bottom: 0;
			width: 180upx;
			line-height: 48upx;
			color: #fff;
			border-radius: 10upx;
			// padding-left: 5upx;
		}
	}
	

	.item_img {
		width: 180upx;
		height: 250upx;
		border-radius: 10upx;
	}

	.status {
		// margin-right: 10upx;
		border-radius: 5upx;
		font-size: 15upx;
		min-width: 40upx;
		padding: 0 10upx;


	}

	.noac {
		color: #fff;
		background-color: #19be6b;
	}

	.ac {

		color: #fff;
		background-color: #fa3534;
	}
</style>