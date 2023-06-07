<template>
	<view style="height:100%">
		<!-- subpage_v3/pages/star_rate/change_record_list.wxml -->

		<view class="head_view">
			<view class="item head_title">每周一0点重置产星加速电池为初始状态，请知悉</view>
			<view class="item star_times_num">
				<view class="status" v-if="pageData.type==-1">已达下限</view>
				<view class="status" v-if="pageData.type==1">已达上限</view>
				<text class="num">{{pageData.speed}}</text>个
			</view>
			<view class="item star_text">当前产星加速电池
				<image class="icon"
					src="https://star-img.xingxiaoculture.com/search/topics/images/2021/4/19/FQsdABBNPy1618802867912.png"
					@tap="starSpeed" />
			</view>
		</view>
		<view class="m-title">
			<view>产星加速电池记录</view>
			<view class="desc">只记录一周内的记录</view>
		</view>
		<view class="change_record_list">
			<!-- parse <template v-if="pageData.is_open == 1" is="change_record_item" :data="list,speedDetail"/> -->
			<block name="change_record_item" v-if="pageData.is_open == 1">
				<view class="item" v-for="(item,index) in (list)" :key="index">



					<view class="item_line">
						<view class="item_col">
							<view class="text1 t-over">
								{{item.label}}
								<block v-if="item.style">
									<view class="tag tag3">{{item.style}}</view>
									<image v-if="item.is_rematch==1" class="question-icon"
										src="https://star-img.xingxiaoculture.com/search/topics/images/2021/5/11/aTGhk34ERz1620723306432.png"
										@tap="pkRestartDialog" />
								</block>
							</view>
							<view class="text2">
								<!-- <view class="pk-desc" wx:if="{{item.style}}">{{item.label2}}</view> -->
								<view>{{item.begindate}}</view>
							</view>
						</view>
						<view class="item_col ml-auto">
							<view class="times t-right">
								<view class="status" v-if="item.type == 1 && item.num == 0">已达上限</view>
								<view class="status" v-if="item.type == -1 && item.num == 0">已达下限</view>
								{{item.fuhao}}{{item.num}}个
							</view>
							<view class="text3 t-right">产星加速电池：{{item.speed_last}}个</view>
						</view>
					</view>






					<view class="item_line" v-if="item.label2">
						<view class="pk_desc_box">{{item.label2}}
							<block v-if="item.fuhao == '-' && item.num == 1">
								<text v-if="speedDetail.speed_protected_num == 0" @tap="toBatteryProtect" style="color:#FF6D"
									00';text-decoration:underline;'>点我开启电池保护</text>
								<text v-else>当时未开启电池保护</text>
							</block>
						</view>
						<!-- <rich-text bindtap="toBatteryProtect" class="pk_desc_box" nodes="<div>我防守失败，损失加速电池*1个，<span style='color:#FF6D'00';text-decoration:underline;'>点我开启电池保护</span></div>"></rich-text> -->
					</view>






					<view class="item_line" v-if="(item.type == -1 || item.type == -5) && item.is_win !== 1 && item.is_rematch == 0">
						<view class="user-btn" @tap="toDressRoom">去换装再重新跟ta比赛</view>
						<view class="user-btn" @tap="toFashionRematch" :data-opponent_id="item.cid" :data-speed_id="item._id.$id">
							直接重新跟ta比赛</view>
					</view>






					<view class="item_line" v-if="item.is_rematch == 1">
						<view class="user-btn" v-if="item.type == -5 || item.type == -1" @tap="toDressRoom">去换装再重新跟ta比赛</view>
						<!-- <view class="user-btn" wx:if="{{item.type == 1 }}" style="opacity: .5;">去换装再重新跟ta比赛</view> -->
						<view class="user-btn" v-if="item.type == -5 || item.type == -1" @tap="toFashionRematch"
							:data-opponent_id="item.cid" :data-speed_id="item._id.$id">继续重赛</view>
						<!-- <view class="user-btn" wx:if="{{item.type == 1}}" style="opacity: .5;"  >已重赛并胜利</view> -->
					</view>



				</view>
			</block>
			<!-- parse <template v-else is="change_record_item_last" :data="list"/> -->
			<block name="change_record_item_last" v-else>
				<view class="item" v-for="(item,index) in (list)" :key="index">



					<view class="item_line">
						<view class="item_col">
							<view class="text1 t-over">
								{{item.label}}
								<block v-if="item.style">
									<view class="tag tag3">{{item.style}}</view>
									<image v-if="item.is_rematch==1" class="question-icon"
										src="https://star-img.xingxiaoculture.com/search/topics/images/2021/5/11/aTGhk34ERz1620723306432.png"
										@tap="pkRestartDialog" />
								</block>
							</view>
							<view class="text2">
								<view class="pk-desc" v-if="item.style">{{item.label2}}</view>
								<view>{{item.begindate}}</view>
							</view>
						</view>
						<view class="item_col ml-auto">
							<view class="times t-right">
								<view class="status" v-if="item.type>0 && item.type!=3 && item.type!=4 && item.num==0">已达上限</view>
								<view class="status" v-if="item.type < 0 && item.type!=-4 && item.num==0">已达下限</view>
								{{item.fuhao}}{{item.num}}个
							</view>
							<view class="text3 t-right">产星加速电池：{{item.speed_last}}个</view>
						</view>
					</view>






					<view class="item_line" v-if="item.type == -1 && item.is_win !== 1 && item.is_rematch == 0">
						<view class="user-btn" @tap="toDressRoom">去换装再重新跟ta比赛</view>
						<view class="user-btn" @tap="toFashionRematch" :data-opponent_id="item.cid" :data-speed_id="item._id.$id">
							直接重新跟ta比赛</view>
					</view>






					<view class="item_line" v-if="item.is_rematch == 1">
						<view class="user-btn" v-if="item.type == -5 || item.type == -1" @tap="toDressRoom">去换装再重新跟ta比赛</view>
						<!-- <view class="user-btn" wx:if="{{item.type == 1}}" style="opacity: .5;">去换装再重新跟ta比赛</view> -->
						<view class="user-btn" v-if="item.type == -5 || item.type == -1" @tap="toFashionRematch"
							:data-opponent_id="item.cid" :data-speed_id="item._id.$id">继续重赛</view>
						<!-- <view class="user-btn" wx:if="{{item.type == 1}}" style="opacity: .5;"  >已重赛并胜利</view> -->
					</view>



				</view>
			</block>
		</view>
		<view class="user-btn fixed-btn" @tap="toFashionPk">去参与时装赛</view>


		<!-- 产星加速电池说明弹窗 -->
		<view class="dialog" v-if="dialog_star_speed_info.show">
			<view class="dialog-cover"></view>
			<view class="dialog-view">
				<!-- <image class="dialog-close" bindtap="dialog_close" data-name="dialog_exchange_confirm" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"/> -->
				<view class="dialog-main" style="padding-top: 30rpx;">
					<!-- <image class="act-title" style="width: 108rpx;height: 113rpx;" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/27/WRNHbGZrmn1616837580301.png"/> -->
					<scroll-view class="scroll-view" scroll-y>
						<view class="sub-title" style="color:#0A0A0A">产星加速电池说明</view>
						<view class="text">1.每位用户默认初始产星加速电池为1个，上限为50个；</view>
						<view class="text">2.穿上犇犇套装，即可增加9个的产星加速电池，同时上限也变为60个；</view>
						<view class="text">3.可通过参与时装赛进行加速电池的增加和减少；达到上限或下限后，不再增加或者减少加速电池；</view>
						<view class="text">4.每周一0点将会重置产星加速电池为初始状态；</view>
						<view class="btn-group">
							<view class="user-btn" @tap="dialogStarSpeedInfoConfirm">我知道了</view>
						</view>
					</scroll-view>
				</view>
			</view>
		</view>

		<!-- 重赛说明弹窗 -->
		<view class="dialog" v-if="dialog_pk_restart.show">
			<view class="dialog-cover"></view>
			<view class="dialog-view">
				<!-- <image class="dialog-close" bindtap="dialog_close" data-name="dialog_exchange_confirm" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"/> -->
				<view class="dialog-main" style="padding-top: 30rpx;">
					<!-- <image class="act-title" style="width: 108rpx;height: 113rpx;" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/27/WRNHbGZrmn1616837580301.png"/> -->
					<scroll-view class="scroll-view" scroll-y>
						<view class="sub-title" style="color:#0A0A0A">重赛说明</view>
						<view class="text">1.只有在别人进攻我的比赛记录里才可发起重赛；</view>
						<view class="text">2.别人在时装赛进攻我一次，我就可在该比赛记录里发起重赛；</view>
						<view class="text">3.当我发起重赛时，会随机分配一个舞台与进攻我的人进行比赛，我重赛失败后，可以一直与ta重赛，直到我重赛获得胜利；</view>
						<view class="btn-group">
							<view class="user-btn" @tap="dialogPkRestartConfirm">我知道了</view>
						</view>
					</scroll-view>
				</view>
			</view>
		</view>

		<!-- 时装赛正在结算弹窗 -->
		<view class="dialog" v-if="dialog_fashion_pk_pause.show">
			<view class="dialog-cover"></view>
			<view class="dialog-view">
				<!-- <image class="dialog-close" bindtap="dialog_close" data-name="dialog_exchange_confirm" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"/> -->
				<view class="dialog-main" style="padding-top: 30rpx;">
					<!-- <image class="act-title" style="width: 108rpx;height: 113rpx;" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/27/WRNHbGZrmn1616837580301.png"/> -->
					<scroll-view class="scroll-view" scroll-y>
						<view class="sub-title" style="color:#0A0A0A">时装赛正在结算</view>
						<view class="text">{{fashion_data.pause_msg}}</view>
						<view class="btn-group">
							<view class="user-btn" @tap="fashionPkPauseConfirm">我知道了</view>
						</view>
					</scroll-view>
				</view>
			</view>
		</view>

		<view v-if="isShowNoTimeDialog" class="dialog_view">
			<view class="dialog_main_view">
				<view class="dialog_main_title">比赛次数已用完</view>
				<view class="dialog_main_text">今日您的次数已用完，是否需要兑换比赛次数</view>
				<view class="dialog_main_btn" style="width: 360rpx;" @tap="dialogCloseClick">兑换比赛次数</view>
			</view>
		</view>



		<battery_protect_dialog v-if="bShowBatteryProtect" :dialogType="batteryProtectType"
			@closeClick="closeBatteryProtectClick"></battery_protect_dialog>

	</view>
</template>

<script>
	import battery_protect_dialog from "../../../component/battery_protect_dialog/battery_protect_dialog";
	// subpage_v3/pages/star_rate/change_record_list.js
	var net = require("../../../commonscript/common/netLoad.js");
	var IDLoginManager = require("../../../commonscript/common/IDLoginManager.js");
	var globalData = require("../../../commonscript/common/globalData.js");
	var app = getApp();
	export default {
		components: {
			battery_protect_dialog
		},
		data() {
			return {
				dialog_star_speed_info: {
					show: false
				},
				dialog_pk_restart: {
					show: false
				},
				dialog_fashion_pk_pause: {
					show: false
				},
				fashion_data: {
					pause_msg: ""
				},
				pageData: {
					type: "",
					speed: "",
					is_open: 0
				},
				speedDetail: {
					speed_protected_num: 0
				},
				list: [],
				page: 1,
				bShowBatteryProtect: false,
				batteryProtectType: 2,
				isShowNoTimeDialog: false
			};
		},
		/**
			* 生命周期函数--监听页面加载
			*/
		onLoad: function(options) {
			uni.hideShareMenu({
				withShareTicket: true
			});
			// this.getPageData()
		},
		/**
			* 生命周期函数--监听页面初次渲染完成
			*/
		onReady: function() {},
		/**
			* 生命周期函数--监听页面显示
			*/
		onShow: function() {
			this.setData({
				list: [],
				page: 1
			});
			this.getPageData();
			this.getSpeedDetail();
		},
		/**
			* 生命周期函数--监听页面隐藏
			*/
		onHide: function() {},
		/**
			* 生命周期函数--监听页面卸载
			*/
		onUnload: function() {},
		/**
			* 页面相关事件处理函数--监听用户下拉动作
			*/
		onPullDownRefresh: function() {
			this.setData({
				list: [],
				page: 1
			});
			this.getPageData();
			this.getSpeedDetail();
		},
		/**
			* 页面上拉触底事件的处理函数
			*/
		onReachBottom: function() {
			let {
				page
			} = this;
			this.setData({
				page: page + 1
			});
			this.getPageData();
			this.getSpeedDetail();
		},
		/**
			* 用户点击右上角分享
			*/
		onShareAppMessage: function() {},
		methods: {
			starSpeed() {
				this.setData({
					dialog_star_speed_info: {
						show: true
					}
				});
			},

			pkRestartDialog() {
				this.setData({
					dialog_pk_restart: {
						show: true
					}
				});
			},

			dialogStarSpeedInfoConfirm() {
				this.setData({
					dialog_star_speed_info: {
						show: false
					}
				});
			},

			dialogPkRestartConfirm() {
				this.setData({
					dialog_pk_restart: {
						show: false
					}
				});
			},

			fashionPkPauseConfirm() {
				this.setData({
					dialog_fashion_pk_pause: {
						show: false
					}
				});
			},

			getSpeedDetail: function() {
				var that = this;
				const url = 'https://data.idol001.com/api_speed.php?action=speed_detail';
				const params = {};
				net.fetchApi(url, params, 'GET').then(resp => {
					if (resp) {
						if (resp.data) {
							that.setData({
								speedDetail: resp.data
							});
							// oThis.setData({
							//   pageData:resp.data,
							//   imgList:resp.data.imgList
							// })
						} else {
							uni.showToast({
								title: resp.data.msg || resp.data.error_description,
								icon: 'none'
							});
						}
					}
				}, function() {
					uni.showToast({
						title: '网络似乎不太顺畅哦',
						icon: 'none'
					});
				});
			},

			getPageData: function() {
				var that = this;
				let {
					page,
					list
				} = this;
				const url = 'https://data.idol001.com/api_speed.php?action=speed_list';
				const params = {
					page
				};
				uni.showLoading({
					title: '加载中...',
					//提示的内容,
					mask: true //显示透明蒙层，防止触摸穿透,
				});

				net.fetchApi(url, params, 'GET').then(resp => {
					uni.hideLoading();
					uni.stopPullDownRefresh();
					if (resp && resp.data) {
						if (resp.data.ok == 1) {
							resp.data.data.forEach((item, index) => {
								console.log("item", item);
								if (item.style.indexOf("重赛") > -1) {
									item.is_rematch = 1;
								} else {
									item.is_rematch = 0;
								}
							});
							that.setData({
								pageData: resp.data,
								list: [...list, ...resp.data.data]
							});
						} else {
							uni.showToast({
								title: resp.data.msg || resp.data.error_description,
								icon: 'none'
							});
						}
					}
				}, function() {
					uni.showToast({
						title: '网络似乎不太顺畅哦',
						icon: 'none'
					});
				});
			},

			toFashionPk() {
				var oThis = this;
				oThis.getFashionTime(function() {
					var pages = getCurrentPages(); //获取加载的页面
					var currentPage = pages[pages.length - 2]; //获取上个页面的对象
					var url = currentPage.route; //当前上个页面页面url
					if (url == 'subpage_v3/pages/fashion_pk/fashion_detail') {
						uni.navigateBack({
							delta: 1
						});
					} else {
						uni.navigateTo({
							url: '/subpage_v3/pages/fashion_pk/fashion_detail'
						});
					}
				});
			},

			toDressRoom() {
				uni.navigateTo({
					url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(globalData
						.idolUserInfo)) + '&bShowAngelWindows=' + '0' + '&bShowLevelGiftbtnNum=' + '1'
				});
			},

			toFashionRematch(e) {
				var oThis = this;
				oThis.getFashionTime(function() {
					let {
						opponent_id,
						speed_id
					} = e.currentTarget.dataset;
					var url = 'https://data.idol001.com/api_idol_acc_game.php?action=compare';
					net.fetchApi(url, {
						'opponent_id': opponent_id,
						'speed_id': speed_id
					}, 'GET').then(resp => {
						if (resp.data && resp.data && resp.data.code == 0) {
							uni.navigateTo({
								url: '/subpage_v3/pages/fashion_pk/fashion_pk_page?pkResult=' + encodeURIComponent(JSON.stringify(resp
									.data))
							});
						} else if (resp.data && resp.data && resp.data.code == 2) {
							oThis.setData({
								isShowNoTimeDialog: true
							});
						} else {
							uni.showToast({
								title: resp.data.msg ? resp.data.msg : '网络异常，请稍后再试',
								icon: 'none'
							});
						}
					}, function() {
						uni.showToast({
							title: '网络似乎不太顺畅哦',
							icon: 'none'
						});
					});
				});
			},

			//时装赛上线时间
			getFashionTime: function(cb) {
				var that = this;
				var url = "https://data.idol001.com/api_idol_acc_game.php?action=get_time";
				var params = {};
				net.fetchApi(url, params, 'GET').then(resp => {
					if (resp && resp.data) {
						that.setData({
							fashion_data: resp.data
						});
						if (resp.data.is_pause == 1) {
							that.setData({
								dialog_fashion_pk_pause: {
									show: true
								}
							});
						} else {
							cb();
						}
					}
				});
			},

			toBatteryProtect() {
				this.setData({
					bShowBatteryProtect: true
				});
			},

			closeBatteryProtectClick() {
				this.setData({
					bShowBatteryProtect: false
				});
				this.getPageData();
			},

			dialogCloseClick() {
				this.setData({
					isShowNoTimeDialog: false
				});
				uni.navigateTo({
					url: '/pages/subpages/pages/game_prop/game_prop?type=acc_game_times'
				});
			}
		}
	};
</script>
<style>
	@import "./change_record_list.css";
	@import "@/..";
</style>