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
		<view class="jifen">
			<view class="bifen">
				<view class="jia" @click="dom" ref="jia">
					<view class="bi_btn">
						<img src="../../static/jian.png"  alt="">
					</view>
					<view class="bifen-zi">
						{{bifen.jia}}
					</view>
					<view class="bi_btn">
						<img src="../../static/jia.png"  alt="">
					</view>
				</view>
				<view class="vs">
					:
				</view>
				<view class="yi">
					<view class="bi_btn">
						<img src="../../static/jian.png"  alt="">
					</view>
					<view class="bifen-zi">
						{{bifen.yi}}
					</view>
					<view class="bi_btn">
						<img src="../../static/jia.png"  alt="">
					</view>
				</view>
			</view>
			
			<view class="count_bifen">
				<view class="btn">
					<img src="../../static/jian.png"  alt="">
				</view>
				<view class="">
					<span style="font-size: 2rem;">总比分</span> <br/>
					5:1
				</view>
				<view class="btn">
					<img src="../../static/jia.png"  alt="">
				</view>
				
			</view>
			<view class="reload">
				<img src="../../static/reload.png" alt="">
			</view>
			<view class="bottom">
				
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bifen:{
					jia:5,
					yi:1
				}
			}
		},
		methods: {
			dom(){
				var top_width = this.$refs.jia.getBoundingClientRect().top;
				 // var leftLength = dom.getBoundingClientRect().top;
				
				            //当前鼠标点击距离左边屏幕的位置
				            var clickWidth=event.clientY;
				            //本元素所占的宽多少
				            var this_width = this.$refs.jia.clientWidth;
				
				            //点击位置-本div距离左边距离 = 点击在本div中的位置
				            var x = clickWidth - leftLength;
				console.log(x,clickWidth,this_width)
				
			}
		}
	}
</script>

<style lang="scss">
	*{
		margin: 0;
		padding: 0;
	}
	.conta{
		background-color: rgb(243, 244, 246);
	}
.jifen{
	
	min-height: calc(100vh - 38vh - 60px);
	background-color: rgb(243, 244, 246);
	.reload{
		width: 12rem;
		height: 12rem;
		border-radius: 12rem;
		margin: 0 auto;
		background-color: #fff;
		line-height: 17rem;
		box-shadow: 10px 10px 59px 19px rgba(0,0,0,0.1),-5px 10px 18px -3px rgba(0,0,0,0.1);
		text-align: center;
		img{
			width: 6rem;
			height: 6rem;
			line-height: 12rem;
		}
	}
	.bi_btn{
		width: 100%;
		text-align: center;
		img{
			width: 3rem;
			height: 3rem;
		}
	}
	.bifen-zi{
		line-height: 15rem;
		text-align: center;
		width: 100%;
		font-size: 10rem;
	}
	.vs{
		line-height: 20rem;
		text-align: center;
		width: 100%;
		font-size: 10rem;
	}
	
	.btn{
		line-height: 15rem;
		// margin-right: 2rem;
		text-align: center;
		width: 6rem;
		img{
			width: 4rem;
			height: 4rem;
		}
	}
	.count_bifen{
		display: flex;
		justify-content:space-around;
		height: 12rem;
		width: 85%;
		margin: 3rem auto;
		font-size: 4rem;
		text-align: center;
		// background-color: #cccccc;
		border-radius: 2rem;
		box-shadow: 10px 10px 59px 19px rgba(0,0,0,0.1),-5px 10px 18px -3px rgba(0,0,0,0.1);
	}
	.bifen{
		display: flex;
		justify-content: center;
		
		.jia{
			height: 22rem;
			width: 18rem;
			// background-color: #cccccc;
			border-radius: 2rem;
			box-shadow: 10px 10px 59px 19px rgba(0,0,0,0.1),-5px 10px 18px -3px rgba(0,0,0,0.1);
			
			
		
		}
		@keyframes m {
		  0%  {background-position-x:-200%, -100%, 0%, 100%}
		  100%{background-position-x:  0%, 100%, 200%, 300%}
		}
		.vs{
			width: 10rem;
			height: 5rem;
			// background-color: #fff;
			background-color: rgba(243, 244, 246,0);
		}
		.yi{
			height: 22rem;
			width: 18rem;
			// background-color: #cccccc;
			border-radius: 2rem;
			box-shadow: 10px 10px 59px 19px rgba(0,0,0,0.1),-5px 10px 18px -3px rgba(0,0,0,0.1);
		}
	}
	
}
.bottom{
		width: 100%;
		height: 8rem;
		position: fixed;
		bottom: 60px;
		--c: #2196F3;
		  --w1: radial-gradient(100% 57% at top ,#0000 100%,var(--c) 100.5%) no-repeat;
		  --w2: radial-gradient(100% 57% at bottom, var(--c) 100%,#0000 100.5%) no-repeat;
		  background: var(--w1),var(--w2),var(--w1),var(--w2);
		  background-position-x: 100%, 100%, 0%, 100%;
		  background-position-y: 50%;
		  background-size: 50.5% 100%;
		  animation: m 1s infinite linear;
	}
</style>
