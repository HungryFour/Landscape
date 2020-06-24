<template>
	<view class="container">
		<view class="video" v-if="currentTab == 0">
			<view class="tabContainer"><tab :current="currentTopTabIndex" :backgroundColor="'#ffffff'" :tabs="tabBarList" @change="chooseTab($event)"></tab></view>

			<view class="list-container">
				<view class="item" v-for="(item, index) in list" :key="index">
					<image class="img" v-if="currentVideoPlayIndex != index" mode="aspectFill" :src="item.image" :lazy-load="true" @click="playVideo(index, item.id)"></image>
					<view class="video-container" v-if="currentVideoPlayIndex == index">
						<video
							class="video"
							:id="'video'+index"
							src="https://cloud.video.taobao.com/play/u/2662389975/p/1/e/6/t/1/50278604002.mp4"
							object-fit="fill"
							autoplay="false"
							loop="false"
							webkit-playsinline
						></video>
					</view>
					<view class="text-container" @click="toDetail(item.id)">
						<view class="title">{{ item.content }}</view>
						<view class="director">我是价格</view>
					</view>
				</view>
			</view>
		</view>

		<view class="animation" v-if="currentTab == 1">
			<view class="list-container">
				<view class="item" v-for="(item, index) in list" :key="index" @click="toDetail(item.id)">
					<image class="img" mode="aspectFill" :src="item.image" :lazy-load="true" @click="playVideo(index, item.id)"></image>
					<view class="video-container">
						<video
							class="video"
							src="https://cloud.video.taobao.com/play/u/2662389975/p/1/e/6/t/1/50278604002.mp4"
							object-fit="fill"
							autoplay
							loop
							muted="muted"
						></video>
					</view>
					<view class="text-container">
						<view class="title">{{ item.content }}</view>
						<view class="director">我是价格</view>
					</view>
				</view>
			</view>
		</view>

		<view class="billboard" v-if="currentTab == 2">
			<view class="list-container">
				<view class="item" v-for="(item, index) in list" :key="index" @click="toDetail(item.id)">
					<image class="img" mode="aspectFill" :src="item.image" :lazy-load="true" @click="playVideo(index, item.id)"></image>
					<view class="text-container">
						<view class="title">{{ item.content }}</view>
						<view class="director">我是价格</view>
					</view>
				</view>
			</view>
		</view>

		<view class="tab-container">
			<view :class="[currentTab == 0 ? 'tab-item-selected tab-item' : 'tab-item-normal tab-item']" @click="tabDidSelected(0)">按钮按钮</view>
			<view :class="[currentTab == 1 ? 'tab-item-selected tab-item' : 'tab-item-normal tab-item']" @click="tabDidSelected(1)">按钮按钮按钮</view>
			<view :class="[currentTab == 2 ? 'tab-item-selected tab-item' : 'tab-item-normal tab-item']" @click="tabDidSelected(2)">按钮按钮</view>
		</view>
	</view>
</template>

<script>
const data = require('@../../testData.json');

import tab from '../../components/tab.vue';

export default {
	components: { tab },
	data() {
		return {
			page: 1,
			start: 0,
			end: 0,
			list: [], // 列表
			loading: true,
			currentTopTabIndex: 0,
			currentTab: 0,
			currentVideoPlayIndex: null,
			tabBarList: ['全部热门商品', '我能兑换', '超V专区', '全部商品', '我能兑换', '超V专区']
		};
	},
	onLoad() {
		this.getList();
	},
	onUnload() {
		console.log("=====onUnload======");
	},
	onHide() {
		let that = this;
		console.log("=====onHide======", that.currentVideoPlayIndex);
		if (that.currentVideoPlayIndex != null) {
			var videoContextPrev = uni.createVideoContext('video' + that.currentVideoPlayIndex, this);
			console.log("=====videoContextPrev======", videoContextPrev);
			videoContextPrev.pause();
		};
	},
	onReachBottom() {
		this.page++;
		this.loading = true;
		this.getList();
	},
	methods: {
		// 选中
		toDetail(id) {
			// item 返回选中 JSON 对象
			console.log('=======', id);
			uni.navigateTo({
				url: '../detail/detail'
			});
		},
		playVideo(index) {
			if (this.currentVideoPlayIndex == null) {
				// 没有播放时播放视频
				this.currentVideoPlayIndex = index;
				var videoContext = uni.createVideoContext('video' + index, this);
				videoContext.play();
			} else {
				//停止正在播放的视频
				var videoContextPrev = uni.createVideoContext('video' + this.currentVideoPlayIndex, this);
				videoContextPrev.stop();
				//将点击视频进行播放
				this.currentVideoPlayIndex = index;
				var videoContextCurrent = uni.createVideoContext('video' + index);
				videoContextCurrent.play();
			}
		},
		tabDidSelected(index) {
			this.currentTab = index;
			console.log('====index===', index);
		},
		chooseTab(index) {
			this.currentTopTabIndex = index;
			console.log('====index===', index);
		},
		// 模拟加载数据
		getList() {
			if (this.list.length < data.list.length) {
				setTimeout(() => {
					this.end = this.page * 10;
					this.list = this.list.concat(data.list.slice(this.start, this.end));
					this.start = this.end;
					// 延迟 120 毫秒隐藏加载动画，为了跟组件里面的 100 毫秒定位有个平缓过度
					setTimeout(() => {
						this.loading = false;
					}, 120);
				}, 1000);
			} else {
				this.loading = false;
			}
		}
	}
};
</script>

<style>
.container {
	position: relative;
	display: flex;
	flex-direction: column;
}

.tabContainer {
	margin-top: 10rpx;
	position: sticky;
	width: 100%;
	left: 0;
	right: 0;
	top: 0;
	z-index: 1;
}

.list-container {
	position: relative;
	padding: 20rpx 20rpx;
	display: flex;
	flex-direction: column;
}

.item {
	position: relative;
	margin-bottom: 30rpx;
	margin-top: 10rpx;
	width: 100%;
	background: #ffffff;
	display: flex;
	flex-direction: column;
	border-bottom: solid #f5f5f5 1rpx;
}

.img {
	width: 100%;
	height: 350rpx;
	border-radius: 28rpx;
	box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.08);
}

.video-container {
	display: flex;
	width: 100%;
	height: 350rpx;
}

.video {
	width: 100%;
	height: 100%;
}

.text-container {
	display: flex;
	flex-direction: column;
	margin-top: 20rpx;
	padding-left: 20rpx;
	padding-right: 20rpx;
}

.title {
	margin-top: 20rpx;
	font-size: 28rpx;
	color: #333333;
	font-weight: bold;
}

.director {
	margin-top: 10rpx;
	margin-bottom: 25rpx;
	font-size: 24rpx;
	color: #666666;
	font-weight: 400rpx;
	margin-top: 20rpx;
}

.tab-container {
	position: fixed;
	display: flex;
	height: 80rpx;
	width: 550rpx;
	padding-left: 100rpx;
	padding-right: 100rpx;
	bottom: 40rpx;
	justify-content: space-between;
}

.tab-item {
	height: 80rpx;
	line-height: 80rpx;
	padding: 0 20rpx;
	text-align: center;
	border-radius: 40rpx;
	background-color: #ffffff;
	box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.08);
}

.tab-item-selected {
	font-size: 28rpx;
	color: #333333;
	font-weight: bold;
}

.tab-item-normal {
	font-size: 24rpx;
	color: #666666;
	font-weight: 400rpx;
}
</style>
