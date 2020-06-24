<template>
	<view
		class="tabs"
		:style="{
			'z-index': zIndex,
			'background-color': getBgColor,
			'transition-duration': getDuration + 's'
		}"
	>
		<scroll-view
			scroll-x
			class="tabs-scroll"
			:scroll-left="left"
			scroll-with-animation
			:style="{
				'z-index': Number(zIndex) + 1
			}"
		>
			<view class="tabs-scroll-box">
				<!-- 循环tabs -->
				<view
					class="tabs-scroll-item"
					:style="{
						height: getHeight + 'rpx',
						'line-height': getHeight + 'rpx',
						'min-width': getWidth + 'rpx',
						color: index === getCurrent ? getActiveColor : getDefaultColor,
						'font-weight': activeBold && index === getCurrent ? 'bold' : '',
						'font-size': index === getCurrent ? activeFontSize + 'rpx' : defaultFontSize + 'rpx',
						'transition-duration': getDuration + 's',
						'z-index': Number(zIndex) + 2
					}"
					v-for="(item, index) in getTabs"
					:key="index"
					@tap="emit(index)"
					:id="preId + index"
				>
					{{ item.name || item }}
				</view>
				<view
					class="boxStyle"
					:style="
						getLinezIndex +
							getDurationStyle +
							'width:' +
							animationLineWidth +
							'px;' +
							'background-color:' +
							(lineColor || getActiveColor) +
							';' +
							lineStyle +
							';' +
							'left:' +
							line2Dx +
							'px;' +
							'transform: translateY(' +
							lineOffsetY +
							'rpx);'
					"
				/>
			</view>
		</scroll-view>
	</view>
</template>

<script>
const { windowWidth } = uni.getSystemInfoSync();
const preId = 'TabsID_';
export default {
	props: {
		tabs: {
			//需循环的标签列表
			type: Array,
			default() {
				return [];
			}
		},
		current: {
			//当前所在滑块的 index
			type: Number,
			default: 0
		},
		height: {
			//tabs的高度和行高
			type: [String, Number],
			default: 100
		},
		minWidth: {
			//单个tab的最小宽度	//v1.4修改
			type: [String, Number],
			default: 250
		},
		activeFontSize: {
			//字体大小
			type: [String, Number],
			default: 28
		},
		defaultFontSize: {
			//字体大小
			type: [String, Number],
			default: 26
		},
		duration: {
			//过渡动画时长, 单位 s
			type: [String, Number],
			default: 0.5
		},
		activeColor: {
			//选中项的主题颜色
			type: String,
			default: '#000000'
		},
		defaultColor: {
			//未选中项的主题颜色
			type: String,
			default: '#666666'
		},
		activeLineColor: {
			//选中项的主题颜色
			type: String,
			default: '#FFC501'
		},
		animationLineWidth: {
			//动画线条的宽度
			type: [Number],
			default: 13
		},
		lineStyle: {
			//line2线条的样式
			type: String,
			default: 'height: 4rpx;border-radius: 2rpx;'
		},
		autoCenter: {
			//是否自动滚动至中心目标
			type: Boolean,
			default: true
		},
		autoCenterMode: {
			//滚动至中心目标类型
			type: String,
			default: 'component'
		},
		activeStyle: {
			type: String,
			default: 'bottom:0;left:50%;transform: translate(-50%,-100%);height: 4rpx;border-radius:2rpx;'
		},
		defaultStyle: {
			type: String,
			default: 'bottom:0;left:50%;transform: translate(-50%,-100%);height: 4rpx;border-radius:2rpx;'
		},
		backgroundColor: {
			//统一背景颜色
			type: String,
			default: 'rgba(255,255,255,0)'
		},
		hasItemBackground: {
			//是否开启背景追光
			type: Boolean,
			default: false
		},
		itemBackgroundColor: {
			//统一追光背景颜色
			type: String,
			default: 'rgba(255,255,255,0)'
		},
		itemBackgroundStyle: {
			//追光样式
			type: String,
			default: ''
		},
		zIndex: {
			//css的z-index属性值
			type: [String, Number],
			default: 99
		},
		space: {
			//tab间距
			type: [String, Number],
			default: '40'
		},
		activeBold: {
			//当前tab字体是否加粗
			type: Boolean,
			default: true
		},
		lineColor: {
			//line颜色
			type: String,
			default: '#FFC501'
		},
		lineOffsetY: {
			type: [String, Number],
			default: '-20'
		}
	},
	computed: {
		getCurrent() {
			const current = Number(this.current);
			if (current > this.getTabs.length - 1) {
				return this.getTabs.length - 1;
			}
			return current;
		},
		getTabs() {
			return this.tabs;
		},
		getHeight() {
			return Number(this.height);
		},
		getWidth() {
			return Number(this.minWidth);
		},
		getFontSize() {
			return this.fontSize;
		},
		getDuration() {
			return Number(this.duration);
		},
		getBgColor() {
			const defaultColor = this.backgroundColor || 'rgba(255,255,255,0)';
			if (this.getTabs[this.getCurrent] instanceof Object) {
				return this.getTabs[this.getCurrent].backgroundColor || defaultColor;
			} else {
				return defaultColor;
			}
		},
		getItemBackgroundColor() {
			const defaultColor = this.itemBackgroundColor || 'rgba(255,255,255,0)';
			if (this.getTabs[this.getCurrent] instanceof Object) {
				return this.getTabs[this.getCurrent].itemBackgroundColor || defaultColor;
			} else {
				return defaultColor;
			}
		},
		getDurationStyle() {
			return `transition-duration: ${this.getDuration}s;`;
		},
		getActiveColor() {
			let activeColor;
			if (this.getTabs[this.getCurrent] instanceof Object) {
				if (this.getTabs[this.getCurrent].activeColor) {
					activeColor = this.getTabs[this.getCurrent].activeColor;
				} else {
					activeColor = this.activeColor;
				}
			} else {
				activeColor = this.activeColor;
			}
			return activeColor;
		},
		getDefaultColor() {
			let defaultColor;
			if (this.getTabs[this.getCurrent] instanceof Object) {
				if (this.getTabs[this.getCurrent].defaultColor) {
					defaultColor = this.getTabs[this.getCurrent].defaultColor;
				} else {
					defaultColor = this.defaultColor;
				}
			} else {
				defaultColor = this.defaultColor;
			}
			return defaultColor;
		},
		getLinezIndexNum() {
			return Number(this.zIndex) + 2;
		},
		getLinezIndex() {
			return `z-index: ${this.getLinezIndexNum};`;
		}
	},
	watch: {
		current(n, o) {
			this.change(n);
		},
		tabs() {
			this.init();
		}
	},
	data() {
		return {
			left: 0,
			tabsInfo: [],
			setTimeoutFc: null,
			componentsWidth: 0,
			animationFinishCurrent: this.current,
			preId,
			line2Dx: 0
		};
	},
	// #ifndef H5
	onReady() {
		this.init();
	},
	// #endif
	// #ifdef H5
	mounted() {
		this.init();
	},
	// #endif
	methods: {
		init() {
			console.log('init');
			let view = uni.createSelectorQuery().in(this);
			for (let i = 0; i < this.tabs.length; i++) {
				view.select('#' + preId + i).boundingClientRect();
			}
			view.exec(res => {
				const arr = [];
				for (let i = 0; i < res.length; i++) {
					arr.push(res[i]);
				}
				this.tabsInfo = arr;
				this.countLine();
				let _this = this;
				_this.getQuery(() => {
					_this.countScrollX();
				});
			});
		},
		countLine() {
			const tab = this.tabsInfo[this.getCurrent];			
			if (tab) {
				this.line2Dx = tab.width*this.getCurrent + tab.width / 2 - this.animationLineWidth/2;
			};
		},
		emit(index) {
			this.$emit('change', index);
		},
		change() {
			this.countScrollX();
			if (this.setTimeoutFc) clearTimeout(this.setTimeoutFc);
			this.setTimeoutFc = setTimeout(() => {
			}, (this.getDuration * 1000 * 3) / 5);
			this.countLine();
		},
		getQuery(cb) {
			try {
				let view = uni
					.createSelectorQuery()
					.in(this)
					.select('.tabs');
				view.fields(
					{
						size: true
					},
					data => {
						if (data) {
							this.componentsWidth = data.width;
							if (cb && typeof cb === 'function') cb(data);
						} else {
							this.retryGetQuery(cb);
						}
					}
				).exec();
			} catch (e) {
				//TODO handle the exception
				this.componentsWidth = windowWidth;
			}
		},
		retryGetQuery(cb) {
			try {
				let view = uni.createSelectorQuery().select('.tabs');
				view.fields(
					{
						size: true
					},
					data => {
						if (data) {
							this.componentsWidth = data.width;
						} else {
							this.componentsWidth = windowWidth;
						}
						if (cb && typeof cb === 'function') cb(data);
					}
				).exec();
			} catch (e) {
				//TODO handle the exception
				this.componentsWidth = windowWidth;
			}
		},
		countScrollX() {
			if (this.autoCenter) {
				let tab = this.tabsInfo[this.getCurrent];
				if (tab) {
					let tabCenter = tab.left + tab.width / 2;
					let fatherWidth;
					if (this.autoCenterMode === 'window') {
						fatherWidth = windowWidth;
					} else {
						fatherWidth = this.componentsWidth;
					}
					this.left = tabCenter - fatherWidth / 2;
				}
			}
		}
	}
};
</script>

<style scoped>
view,
scroll-view {
	box-sizing: border-box;
}

.tabs {
	width: 100%;
	transition-property: background-color, color;
}

.tabs::-webkit-scrollbar {
	display: none;
	width: 0 !important;
	height: 0 !important;
	-webkit-appearance: none;
	background: transparent;
}

.tabs-scroll {
	width: 100%;
	white-space: nowrap;
	position: relative;
}

.tabs-scroll-box {
	position: relative;
	display: flex;
	white-space: nowrap !important;
	display: block !important;
}

.tabs-scroll-item {
	position: relative;
	display: inline-block;
	text-align: center;
	transition-property: background-color, color, font-weight;
}

.content {
	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
}

.boxStyle {
	pointer-events: none;
	position: absolute;
	bottom: 0;
	border-radius: 50%;
	transition-property: all;
}

.itemBackgroundBox {
	pointer-events: none;
	position: absolute;
	top: 0;
	transition-property: left, background-color;
	display: flex;
	flex-direction: row;
	justify-content: center;
	align-items: center;
}

.itemBackground {
	height: 100%;
	width: 100%;
	transition-property: all;
}
</style>
