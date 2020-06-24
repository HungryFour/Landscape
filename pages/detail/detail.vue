<template>
	<view class="container">
		<view class="video-container"><video class="video" src="https://cloud.video.taobao.com/play/u/2662389975/p/1/e/6/t/1/50278604002.mp4" object-fit="fill" autoplay loop muted="muted"></video></view>
		
		<view class="info-container">
			
			<view class="director-container">
				
				<image class="director-avatar" src="http://image.yy.com/yywebalbumbs2bucket/144152f8680f421599233c6ffcfcef49_1476265267104.jpeg" mode="aspectFill"></image>
				<view class="director-name">我是哈哈</view>
				
			</view>
			
			<view class="info">我是视频介绍，这个视频老牛逼了。 我们的拍摄手法是</view>

		</view>
		
		<button class="talk" type="primary" open-type="contact"> 交个朋友 </button>
		
	</view>
</template>

<script>
	const data = require('@../../testData.json');
	
export default {
	data() {
		return {
			list: []
		};
	},
	onLoad() {
		this.list = data.list;
	},
	methods: {}
};
</script>

<style>
.container {
	display: flex;
	flex-direction: column;
	position: relative;
}

.video-container {
	display: flex;
	width: 100%;
	height: 420rpx;
	position: fixed;
	top: 0rpx;
	z-index: 10;
	background: #FFFFFF;
}

.video {
	width: 100%;
	height: 400rpx;
}

.info-container {
	position: absolute;
	width: 100%;
	top: 420rpx;
	display: flex;
	flex-direction: column;
}

.director-container {
	display: flex;
	flex-direction: row;
	justify-content: flex-start;
	align-items: center;
}

.director-avatar {
	margin-left: 30rpx;
	width: 80rpx;
	height: 80rpx;
	border-radius: 50%;
	overflow: hidden;
	border:1rpx solid #007AFF
}

.director-name {
	margin-left: 30rpx;
}

.info {
	padding: 30rpx;
}

.talk {
	position: fixed;
	right: 40rpx;
	bottom: 80rpx;
	height: 60rpx;
	line-height: 60rpx;
	padding: 0 20rpx;
	text-align: center;
	font-size: 24rpx;
	border-radius: 10rpx;
	background-color: #ffffff;
	box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.08);
}

</style>
