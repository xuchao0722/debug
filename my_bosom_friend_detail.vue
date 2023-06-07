<template>
    <view style="height: 100%">
        <view class="main_view view_column_center">
            <view class="top_view_fiexd view_column_center">
                <view class="top_level_status_view view_column_center">
                    <view class="top_level_status_tab view_row_center">
                        <view class="top_level_status_title" @tap="levelDetailClick">密友等级详情：</view>
                        <image
                            class="top_level_status_icon"
                            @tap="levelDetailClick"
                            src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/12/wmYf8rZzhc1615536052516.png"
                        ></image>
                        <view style="flex: 1"></view>
                        <view v-if="miyouInfoObj.week_gift == 0" class="top_level_status_status view_row_center" @tap="showTipDialogClick">
                            <view class="top_level_status_text">本周未互送礼物</view>
                            <image class="top_level_status_icon" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/12/wmYf8rZzhc1615536052516.png"></image>
                        </view>
                    </view>
                    <view class="top_level_status_line"></view>
                    <view class="friend_user_status view_row_center">
                        <view class="friend_user_status_item view_column_center">
                            <view class="view_row_center" style="justify-content: center; margin-top: 30rpx">
                                <view class="friend_user_status_level">LV.{{ miyouInfoObj.level }}</view>
                                <view v-if="miyouInfoObj.level == 999" class="friend_user_status_level_large">已满级</view>
                            </view>
                            <view class="friend_user_status_level_text">当前等级</view>
                        </view>
                        <view class="friend_user_status view_column_center">
                            <text class="friend_user_status_level" style="margin-top: 30rpx">
                                {{ miyouInfoObj.level == 999 ? miyouInfoObj.level_next_intimacy : miyouInfoObj.intimacy }}/
                                <text class="friend_user_status_level_inner">{{ miyouInfoObj.level_next_intimacy }}</text>
                            </text>
                            <view class="friend_user_status_level_text">亲密度详情</view>
                        </view>
                        <view class="friend_user_status_item view_column_center">
                            <view v-if="miyouInfoObj.status == 1" class="friend_user_status_text" style="color: #ff7200">已收到礼物</view>
                            <view v-else-if="miyouInfoObj.status == 2" class="friend_user_status_text" style="color: #ff7200">我已送礼</view>
                            <view v-else-if="miyouInfoObj.status == 3" class="friend_user_status_text" style="color: #ff0a '00'">未互相送礼</view>
                            <view v-else-if="miyouInfoObj.status == 4" class="friend_user_status_text" style="color: #319d1c">已互相送礼</view>
                            <view class="friend_user_status_level_text" style="margin-top: 4rpx">今日送礼状态</view>
                        </view>
                    </view>
                    <view class="friend_user_process_view view_row_center">
                        <view class="friend_user_process_left" :style="'flex: ' + miyouInfoObj.present + ';'"></view>
                        <view :style="'flex: ' + (100 - miyouInfoObj.present) + ';'"></view>
                    </view>
                    <!-- <text wx:if="{{miyouInfoObj.level==999}}" bindtap="lookMoreLevelDetail">满级后每升级{{miyouInfoObj.lack_level_next_intimacy}}亲密度，获得{{miyouInfoObj.reward_notice}}奖励，<text class="friend_user_status_desc_more">查看升级奖励</text></text> -->
                    <text v-if="miyouInfoObj.level == 999" class="friend_user_status_desc" @tap="lookMoreLevelDetail">
                        满级后每升级{{ miyouInfoObj.lack_level_next_intimacy }}亲密度，获得{{ miyouInfoObj.reward_score }}积分，
                        <text class="friend_user_status_desc_more">查看升级奖励</text>
                    </text>
                    <!-- <text wx:else class="friend_user_status_desc" bindtap="lookMoreLevelDetail">还差{{miyouInfoObj.lack_level_next_intimacy}}亲密度升级到LV{{miyouInfoObj.level_next}}，获得{{miyouInfoObj.reward_notice}}，<text class="friend_user_status_desc_more">查看升级奖励</text></text> -->
                    <text v-else class="friend_user_status_desc" @tap="lookMoreLevelDetail">
                        还差{{ miyouInfoObj.lack_level_next_intimacy }}亲密度升级到LV{{ miyouInfoObj.level_next }}，获得{{ miyouInfoObj.reward_score }}积分，
                        <text class="friend_user_status_desc_more">查看升级奖励</text>
                    </text>
                </view>

                <view class="top_level_status_view view_column_center">
                    <view class="top_level_status_tab view_row_center">
                        <view class="top_level_status_title" style="margin: 30rpx 0rpx 30rpx 30rpx">密友等级历程：</view>
                        <view style="flex: 1"></view>
                        <view class="look_more_send_history" @tap="gotoSendHistory">查看送礼记录 ></view>
                    </view>
                    <view class="friend_user_level_view view_row_center">
                        <view class="friend_user_level_item">
                            <view class="friend_user_level_count">LV.{{ miyouInfoObj.level }}</view>
                            <view class="friend_user_level_text">当前等级</view>
                        </view>
                        <view class="friend_user_level_item">
                            <view class="friend_user_level_count">LV.{{ miyouInfoObj.level_max }}</view>
                            <view class="friend_user_level_text">历史最高等级</view>
                        </view>
                    </view>
                </view>
            </view>

            <view style="height: 590rpx"></view>

            <view class="bottom_list_view view_column_center">
                <view class="bottom_list_view_item" v-for="(item, index) in levelHistoryList" :key="index">
                    <view class="bottom_list_view_left view_column_center">
                        <view class="bottom_list_view_left_line" :style="'height: 8rpx;flex: none; ' + (index == 0 ? 'background: #ffffff;' : '')"></view>
                        <view class="bottom_list_view_left_circle"></view>
                        <view class="bottom_list_view_left_line"></view>
                    </view>

                    <view class="bottom_list_view_right">
                        <view class="bottom_list_view_right_time">{{ item.create_time }}</view>
                        <view class="bottom_list_view_right_level view_row_center" :style="item.level_last < item.level_before ? 'color: #ff'0000';' : 'color: #666666;'">
                            {{
                                item.level_last >= 999
                                    ? '密友等级已达到满级'
                                    : item.level_last >= item.level_before
                                    ? '密友等级从LV' + item.level_before + '升级到LV' + item.level_last
                                    : '密友等级降级到LV' + item.level_last
                            }}
                            <view v-if="item.level_now" class="bottom_list_view_right_level2">{{ item.level_now }}</view>
                            <view v-if="item.history_highest_level" class="bottom_list_view_right_level1">{{ item.history_highest_level }}</view>
                        </view>
                        <view v-if="item.reward_notice" class="bottom_list_view_right_dece" :data-item="item" @tap="gotoGiftDetail">
                            {{ item.reward_notice }}
                            <text v-if="item.is_prize == 1" style="font-size: 24rpx; color: #ff6278; text-decoration: underline">查看奖励></text>
                        </view>
                    </view>
                </view>
            </view>

            <view
                v-if="miyouInfoObj.level_max > miyouInfoObj.level"
                class="main_view_bottom_btn"
                @tap="showBuyDialogClick"
                :style="miyouInfoObj.level_max > miyouInfoObj.level ? '' : 'background: linear-gradient(90deg,#ffb5a4 5%, #ffa4c7);'"
            >
                一键恢复到历史最高等级（LV{{ miyouInfoObj.level_max }}）
            </view>
        </view>

        <!-- 一键恢复密友等级 -->
        <view v-if="isShowBuyDialog" class="dialog_view">
            <view class="dialog_main_view">
                <view class="dialog_main_title">一键恢复密友等级</view>
                <view class="dialog_main_level_view">
                    <view class="dialog_main_level_item view_column_center">
                        <view class="dialog_main_level_left">LV.{{ miyouInfoObj.level }}</view>
                        <view class="dialog_main_level_text">当前等级</view>
                    </view>
                    <view class="dialog_main_level_tip">恢复到</view>
                    <view class="dialog_main_level_item view_column_center">
                        <view class="dialog_main_level_right">LV.{{ miyouInfoObj.level_max }}</view>
                        <view class="dialog_main_level_text">历史最高等级</view>
                    </view>
                </view>
                <view class="dialog_main_text">
                    是否花费
                    <text style="color: #ff6278">{{ miyouInfoObj.amount }}公会币</text>
                    ，将密友等级升级到
                    <text style="color: #ff6278">LV{{ miyouInfoObj.level_max }}</text>
                </view>
                <view class="dialog_main_btn" @tap="restoreHighestLeve">立即升级（{{ miyouInfoObj.amount }}公会币）</view>
                <view class="dialog_main_btn_close" @tap="closeBuyDialogClick">我再想想</view>
                <image src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/12/XmGrKXFn4Y1615541728076.png" class="close_img" @tap="closeBuyDialogClick"></image>
            </view>
        </view>

        <!-- 每周未互送礼物提醒 -->
        <view v-if="isShowTipDialog" class="dialog_view">
            <view class="dialog_main_view">
                <view class="dialog_main_title" style="margin-bottom: 30rpx">每周未互送礼物提醒</view>
                <view class="dialog_main_text_tip">如果你和密友一周都未完成互送礼物，下一周将会降低亲密度，降低亲密度的数值为：</view>
                <view class="dialog_main_down_count">下降的亲密度 = 当前亲密度*10%</view>
                <view class="dialog_main_text_tip">所以为了更好的维护密友等级，获取密友等级升级奖励，请务必每周完成互送礼物吧~</view>
                <view class="dialog_main_btn" @tap="closeTipDialogClick" style="width: 360rpx; margin: 36rpx 0rpx 40rpx">我知道了</view>
                <image src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/12/XmGrKXFn4Y1615541728076.png" class="close_img" @tap="closeTipDialogClick"></image>
            </view>
        </view>

        <view v-if="isShowSupportActWindows" class="dialog_view">
            <view class="dialog_main_view">
                <image class="coinImage" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/13/y5pHj5dbQf1615631640867.png"></image>
                <text class="coinTitle">公会币不足</text>
                <text class="coinDetail">当前剩余{{ ownSupportCoinCount }}公会币</text>
                <view class="support_btn" style="width: 65%; margin-bottom: 40rpx; font-size: 32rpx" @tap="gotoChargeNotEnougth">获取公会币</view>
                <image
                    src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/12/XmGrKXFn4Y1615541728076.png"
                    class="close_img"
                    @tap="closeSupportActWindows"
                ></image>
            </view>
        </view>

        <!-- 兑换公会币 -->
        <idol_chat_dialog :show="bShowidolChatDialog" :pay_state="pay_state" :myIsGrouper="myIsGrouper" :type="4"></idol_chat_dialog>
    </view>
</template>

<script>
import idol_chat_dialog from '../../../component/idol_chat_dialog/idol_chat_dialog';
// subpage_v3/pages/bosom_friend/my_bosom_friend_detail.js

var net = require('../../../commonscript/common/netLoad.js');
export default {
    components: {
        idol_chat_dialog
    },
    //生命周期函数--监听页面加载
    data() {
        return {
            userid_friend: '',
            miyouInfoObj: {
                week_gift: 0,
                level: '',
                level_next_intimacy: '',
                status: 0,
                present: '',
                level_max: '',
                amount: ''
            },
            levelHistoryList: [],
            ownSupportCoinCount: 0,
            isShowSupportActWindows: false,
            bShowidolChatDialog: false,
            pay_state: 0,
            myIsGrouper: 0,
            isShowBuyDialog: false,
            isShowTipDialog: false,
            page: 1,
            netRequest: false
        };
    },
    onLoad: function (options) {
        uni.hideShareMenu({});
        this.setData(
            {
                userid_friend: options.userid_friend ? options.userid_friend : ''
            },
            function () {
                this.initMiyouInfo();
                this.initLevelHistory();
            }
        );
    },
    //生命周期函数--监听页面初次渲染完成
    onReady: function () {
        this.getpayStates();
        this.requestOpp();
    },
    //生命周期函数--监听页面显示
    onShow: function () {},
    //生命周期函数--监听页面隐藏
    onHide: function () {},
    //生命周期函数--监听页面卸载
    onUnload: function () {},
    //页面相关事件处理函数--监听用户下拉动作
    onPullDownRefresh: function () {},
    //页面上拉触底事件的处理函数
    onReachBottom: function () {
        if (this.netRequest) {
            return;
        }
        this.page = this.page + 1;
        this.initLevelHistory();
    },
    //用户点击右上角分享
    onShareAppMessage: function () {},
    methods: {
        levelDetailClick: function () {
            uni.navigateTo({
                url: '/subpage_v3/pages/bosom_friend/my_friend_level_rule'
            });
        },

        gotoSendHistory: function () {
            uni.navigateTo({
                url: '/subpage_v3/pages/present/present_record_someone?friend_id=' + this.userid_friend
            });
        },

        lookMoreLevelDetail: function () {
            uni.navigateTo({
                url: '/subpage_v3/pages/bosom_friend/my_bosom_level'
            });
        },

        showBuyDialogClick: function () {
            if (this.miyouInfoObj.level_max && this.miyouInfoObj.level_max > this.miyouInfoObj.level) {
                this.setData({
                    isShowBuyDialog: true
                });
            }
        },

        closeBuyDialogClick: function () {
            this.setData({
                isShowBuyDialog: false
            });
        },

        showTipDialogClick: function () {
            this.setData({
                isShowTipDialog: true
            });
        },

        closeTipDialogClick: function () {
            this.setData({
                isShowTipDialog: false
            });
        },

        initMiyouInfo: function (refresh) {
            var that = this;
            var url = 'https://data.idol001.com/api_intimacy.php?action=miyou_info';
            var params = {
                userid_friend: that.userid_friend
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    resp.data['present'] = Number.parseInt((resp.data.intimacy * 100) / resp.data.level_next_intimacy);
                    that.setData({
                        miyouInfoObj: resp.data
                    });
                    if (refresh) {
                        that.refreshMiyouItem(resp.data);
                    }
                }
            });
        },

        initLevelHistory: function () {
            var that = this;
            that.netRequest = true;
            var url = 'https://data.idol001.com/api_intimacy.php?action=level_history';
            var params = {
                userid_friend: that.userid_friend,
                page: that.page
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        var list = that.levelHistoryList;
                        if (that.page == 1) {
                            list = resp.data;
                        } else {
                            list = list.concat(resp.data);
                            that.page = resp.data.length == 0 ? that.page - 1 : that.page;
                        }
                        that.setData({
                            levelHistoryList: list
                        });
                    } else {
                        uni.showToast({
                            title: resp.data.msg ? resp.data.msg : '网络异常，请稍后再试',
                            icon: 'none'
                        });
                        that.page = that.page - 1;
                    }
                    that.netRequest = false;
                },
                function (resp) {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                    that.netRequest = false;
                    that.page = that.page - 1;
                }
            );
        },

        restoreHighestLeve: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_intimacy.php?action=restore_highest_level';
            var params = {
                userid_friend: that.userid_friend
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.ok == 1) {
                        that.setData({
                            isShowBuyDialog: false
                        });
                        that.initMiyouInfo(true);
                        that.page = 1;
                        that.initLevelHistory();
                    } else if (resp && resp.data && resp.data.ok == 200) {
                        that.setData({
                            isShowBuyDialog: false
                        });
                        that.requestOwnSupportCoin();
                    } else {
                        uni.showToast({
                            title: resp.data.msg ? resp.data.msg : '网络异常，请稍后再试',
                            icon: 'none'
                        });
                    }
                },
                function (resp) {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
        },

        closeSupportActWindows: function () {
            this.setData({
                isShowSupportActWindows: false
            });
        },

        taskCenter() {
            // 8.9任务中心
            uni.navigateTo({
                url: '/pages/subpages/pages/task_center/task_center'
            });
        },

        gotoChargeNotEnougth: function () {
            this.closeSupportActWindows();
            if (this.pay_state == 1 || (this.pay_state == 2 && this.myIsGrouper == 1)) {
                this.setData({
                    bShowidolChatDialog: true
                });
            } else {
                uni.navigateTo({
                    url: '/pages/subpages/pages/task_center/task_center'
                });
            }
        },

        requestOwnSupportCoin: function () {
            var url = 'https://data.idol001.com/api_moblie_idol.php?action=get_user_diamond_prop';
            var that = this;
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data && resp.data.diamond >= 0) {
                    that.setData({
                        ownSupportCoinCount: resp.data.diamond,
                        isShowSupportActWindows: true,
                        isShowBuyDialog: false
                    });
                }
            });
        },

        getpayStates() {
            var value = 6;
            var url = 'https://' + (value > 5 ? 'sw.id' : 'he.ido') + 'ol001.com/ope' + (9 > value ? 'n_manu_w' : '') + 'xapp' + '.php';
            var oThis = this;
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data && !resp.data.error) {
                    oThis.setData({
                        pay_state: resp.data.pay
                    });
                }
            });
        },

        requestOpp: function () {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_opp';
            const params = {};
            const that = this;
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data && resp.data.ok == 1) {
                    that.setData({
                        myIsGrouper: 1
                    });
                } else {
                    that.setData({
                        myIsGrouper: 0
                    });
                }
            });
        },

        refreshMiyouItem: function (miyouObj) {
            var pages = getCurrentPages(); //获取加载的页面
            var currentPage = pages[pages.length - 2]; //获取上个页面的对象
            var url = currentPage.route; //当前上个页面页面url
            if (url == 'subpage_v3/pages/bosom_friend/my_bosom_friend') {
                currentPage.refreshMiyouItem(miyouObj);
            }
        },

        gotoGiftDetail: function (e) {
            var item = e.currentTarget.dataset.item;
            if (item.is_prize == 1) {
                uni.navigateTo({
                    url:
                        '/subpage_v3/pages/my_gift/gift_detail?uniqid=' +
                        item.uniqid +
                        '&userid=' +
                        this.miyouInfoObj.userid +
                        '&useridTo=' +
                        this.miyouInfoObj.userid.user_friend +
                        '&level_start=' +
                        item.level_before +
                        '&level_end=' +
                        item.level_last +
                        '&showShortText=true'
                });
            }
        }
    }
};
</script>
<style>
@import './my_bosom_friend_detail.css';
</style>
