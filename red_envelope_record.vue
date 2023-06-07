<template>
    <view style="height: 100%">
        <view class="page" v-if="hasInit">
            <view class="page_view_frame" :style="'display:' + (errorDic && !errorDic.needShowErrorView ? '' : 'none') + ';'">
                <view class="main_view_top flex_column">
                    <image
                        class="fuli_img_top"
                        src="http://img.xingxiaoculture.com/origin/2020/01/06/5693420e9ff2952f6bc380b9abab7ce41578279704.png"
                        mode="aspectFill"
                        :lazy-load="true"
                    />
                    <view class="red_envelope_tabbar flex_row flex_center" v-if="recordStatus == 1">
                        <view class="red_envelope_tabbar_receive_on flex_row flex_center">收到的福袋</view>
                        <view class="red_envelope_tabbar_send_off flex_row flex_center" @tap="gotoSend">发出的福袋</view>
                    </view>
                    <view class="red_envelope_tabbar flex_row flex_center" v-else>
                        <view class="red_envelope_tabbar_receive_off flex_row flex_center" @tap="gotoReceive">收到的福袋</view>
                        <view class="red_envelope_tabbar_send_on flex_row flex_center">发出的福袋</view>
                    </view>
                    <view class="user-style flex_column flex_center">
                        <image class="img-style" :src="userInfo.image.thumbnail_pic" mode="aspectFill" :lazy-load="true"></image>
                        <text class="txt-style">{{ recordStatus == 1 ? '我收到的福袋' : '我发出的福袋' }}</text>
                    </view>
                </view>
                <view class="main_view_bottom">
                    <view :class="hasInit && allList && allList.length >= 1 ? 'envelope_list' : 'envelope_list_empty'">
                        <block v-if="allList && allList.length >= 1">
                            <block v-for="(item, index) in allList" :key="index">
                                <view class="item flex_row" style="padding-top: 10rpx; padding-bottom: 10rpx" :data-item="item" @tap="gotoRedEnveleGrab">
                                    <view class="flex_row" style="height: 108rpx; padding-left: 10rpx">
                                        <view class="flex_column flex_start" style="margin-left: 20rpx">
                                            <text class="item_name">{{ recordStatus == 1 ? userInfo.nickname : '拼手气福袋' }}</text>
                                            <text class="item_time" style="margin-top: 7rpx">{{ common.dateFormat(item.update_time * 1000, 'yyyy-MM-dd') }}</text>
                                        </view>
                                    </view>
                                    <view class="flex_column flex_end" style="height: 108rpx; position: absolute; right: 20rpx">
                                        <text class="item_coin" style="width: 350rpx; text-align: right">{{ recordStatus == 1 ? item.coin : item.all_coin }}公会币</text>
                                        <view
                                            v-if="recordStatus == 1 && item && item.gift"
                                            style="width: 400rpx; height: 25rpx; text-align: right; margin-top: 7rpx"
                                            @tap.stop.prevent="showRedEnvelopeGiftDetail"
                                            :data-gift="item.gift"
                                        >
                                            <image
                                                class="item_detail_img"
                                                :src="/static/pages/subpages_v2/pages/red_envelope_record/item.gift.icon"
                                                :lazy-load="true"
                                                mode="aspectFill"
                                            />
                                            <text class="item_detail" style="margin-left: 8rpx">
                                                获
                                                <text class="red">{{ item.gift.measure_word }}</text>
                                                {{ item.gift.measure_word_desc }}
                                            </text>
                                        </view>
                                        <view v-else-if="recordStatus == 1" style="width: 400rpx; height: 25rpx; text-align: right; margin-top: 7rpx" />
                                        <view
                                            v-else-if="recordStatus == 2 && item && item.has_get_count == item.all_count"
                                            style="width: 400rpx; text-align: right; margin-top: 7rpx"
                                        >
                                            <text class="item_detail_red" style="margin-left: 8rpx">已领完{{ item.has_get_count }}/{{ item.all_count }}个</text>
                                        </view>
                                        <view
                                            v-else-if="recordStatus == 2 && item && item.has_get_count < item.all_count && item.has_end == 1"
                                            style="width: 400rpx; text-align: right; margin-top: 7rpx"
                                        >
                                            <text class="item_detail_red" style="margin-left: 8rpx">已过期{{ item.has_get_count }}/{{ item.all_count }}个</text>
                                        </view>
                                        <view v-else-if="recordStatus == 2 && item" style="width: 400rpx; text-align: right; margin-top: 7rpx">
                                            <text class="item_detail_red" style="text-align: right; margin-left: 8rpx">{{ item.has_get_count }}/{{ item.all_count }}个</text>
                                        </view>
                                    </view>
                                </view>
                            </block>
                        </block>
                        <block v-else>
                            <text v-if="recordStatus == 1" class="red_envelope_empty">暂无收到的福袋</text>
                            <text v-if="recordStatus == 2" class="red_envelope_empty">暂无发出的福袋</text>
                        </block>
                    </view>
                    <view class="bottom_refresh" :style="'display:' + (hasInit && allList && allList.length >= 1 && !isRequestAll ? 'block' : 'none') + ';'">
                        <refresh_view :is-animation="bottomAnimation"></refresh_view>
                    </view>
                </view>
            </view>
            <view class="error_view_frame flex_column flex_center" :style="'display:' + (errorDic && errorDic.needShowErrorView ? '' : 'none') + ';'">
                <error_view :error-dic="errorDic" @clickRetryEvent="retryGetData"></error_view>
            </view>
        </view>
        <red_envelope_grab_gift_detail
            v-if="showRedEnvelopeGrabGiftDetailTip"
            :gift="redPacketGiftDetail"
            @redEnvelopeDetailClose="redEnvelopeGiftDetailClose"
        ></red_envelope_grab_gift_detail>
    </view>
</template>
<script module="common" lang="wxs" src="@/pages/subpages_v2/pages/red_envelope_record/red_envelope_record.wxs"></script>
<script>
import idc_image from '../../../../component/idc_image/idc_image';
import refresh_view from '../../../../component/refresh_view/refresh_view';
import error_view from '../../../../component/error_view/error_view';
import login_panel from '../../../../component/login_panel/login_panel';
import red_envelope_grab_gift_detail from '../../component/red_envelope_grab_gift_detail/red_envelope_grab_gift_detail';
var net = require('../../../../commonscript/common/netLoad.js');
var IDLoginManager = require('../../../../commonscript/common/IDLoginManager.js');
const app = getApp();
export default {
    components: {
        idc_image,
        refresh_view,
        error_view,
        login_panel,
        red_envelope_grab_gift_detail
    },
    data() {
        return {
            windowWidth: 0,
            windowHeight: 0,
            bShowLoginPanel: false,
            //是否展示登录弹窗
            isLoginStatus: false,
            //登录状态
            errorDic: {
                needShowErrorView: false,
                needShowRetryBtn: false,
                isNothing: false,
                isSomethingError: false,
                errorText: '没有数据哦，请点击重试'
            },
            user_id: '',
            //福袋用户id
            userInfo: {
                image: {
                    thumbnail_pic: ''
                },

                nickname: ''
            },
            page: 1,
            //福袋列表page
            count: 10,
            //福袋列表count
            isRequestAll: false,
            //福袋列表是否加载完毕
            isLoading: false,
            //福袋列表是否正在加载
            isPreload: false,
            //福袋列表是否未执行加载
            bottomAnimation: true,
            //福袋列表底部加载状态是否显示动画
            allcount: 0,
            //福袋列表数量
            allList: {},
            //福袋列表
            allListOri: [],
            //福袋列表
            hasInit: false,
            //是否已加载页面
            recordStatus: 1,
            //1 显示收到的福袋；2 显示发出的福袋
            showRedEnvelopeGrabGiftDetailTip: false,
            //是否显示福袋限定套装详情弹窗
            redPacketGiftDetail: {} //福袋限定套装详情
        };
    },
    onLoad: function (option) {
        console.log('onLoad');
        var that = this;
        uni.hideShareMenu();
        // 获取屏幕宽高
        uni.getSystemInfo({
            success: function (res) {
                that.setData({
                    windowWidth: res.windowWidth,
                    windowHeight: res.windowHeight
                });
            }
        });
        var user_id = option.user_id || '';
        that.setData({
            user_id: user_id
        });
        IDLoginManager.stepLoginState();
        that.loginLogicStar();
    },
    onShow: function () {
        this.initPage();
    },
    onHide: function () {},
    onUnload: function () {},
    /**
     * 页面上拉触底事件的处理函数
     */
    onReachBottom: function () {
        var that = this;
        console.log('onReachBottom', that.isLoading, that.isRequestAll);
        if (!that.isLoading && !that.isRequestAll) {
            console.log('onReachBottom');
            that.setData(
                {
                    bottomAnimation: true
                },
                function () {
                    that.requestRedEnvelopeList(that.page + 1);
                }
            );
        }
    },
    onShareAppMessage: function (options) {},
    methods: {
        gotoReceive: function () {
            var that = this;
            that.setData({
                recordStatus: 1
            });
            uni.pageScrollTo({
                scrollTop: 0
            });
            that.requestRedEnvelopeList(1);
        },

        gotoSend: function () {
            var that = this;
            that.setData({
                recordStatus: 2
            });
            uni.pageScrollTo({
                scrollTop: 0
            });
            that.requestRedEnvelopeList(1);
        },

        gotoRedEnveleGrab: function (e) {
            console.log('gotoRedEnveleGrab e==' + JSON.stringify(e));
            var item = e.currentTarget.dataset.item;
            var url = '/pages/subpages_v2/pages/red_envelope_grab/red_envelope_grab?red_packet_id=' + item.red_packet_id + '&user_id=' + this.userInfo._id + '&is_from_record=1';
            uni.navigateTo({
                url: url
            });
        },

        loginLogicStar: function () {
            console.log('loginLogicStar');
            var that = this;
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                IDLoginManager.checkIsLogin(function (v) {
                    if (v == IDLoginManager.is_not_login) {
                        that.setData({
                            isLoginStatus: false
                        });
                    } else {
                        that.setData({
                            isLoginStatus: true
                        });
                    }
                });
            } else {
                that.setData({
                    isLoginStatus: true
                });
            }
        },

        loginCallback: function (e) {
            console.log('loginCallback');
            var that = this;
            if (e.detail.success) {
                that.setData({
                    bShowLoginPanel: false
                });
                that.setData({
                    isLoginStatus: true
                });
                app.globalData.isShouldCheckInLoginStatus = true;
                that.initPage();
            }
        },

        closeLoginView: function () {
            console.log('closeLoginView');
            this.setData({
                bShowLoginPanel: false
            });
        },

        /**
         * 显示福袋限定套装详情弹窗
         */
        showRedEnvelopeGiftDetail: function (e) {
            var that = this;
            console.log('showRedEnvelopeGiftDetail e==' + JSON.stringify(e));
            that.setData({
                redPacketGiftDetail: e.currentTarget.dataset.gift
            });
            that.setData({
                showRedEnvelopeGrabGiftDetailTip: true
            });
        },

        /**
         * 关闭福袋限定套装详情弹窗
         */
        redEnvelopeGiftDetailClose: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabGiftDetailTip: false
            });
        },

        /**
         * 初始化页面
         */
        initPage: function () {
            var that = this;
            that.getUserInfoDetail(that.user_id);
        },

        /**
         * 获取用户详细信息
         */
        getUserInfoDetail: function (userid) {
            var that = this;
            var url = 'https://data.idol001.com/api_moblie_idol.php?action=get_userinfo_detail';
            var params = {
                quanzhu: 1,
                power: 1,
                userid: userid
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('获取用户详细信息 getUserInfoDetail resp.data==', JSON.stringify(resp.data));
                    if (resp.data) {
                        that.setData({
                            userInfo: resp.data
                        });
                        that.requestRedEnvelopeList(1);
                    } else {
                        that.showError(resp);
                    }
                },
                function (resp) {
                    console.log('获取用户详细信息 getUserInfoDetail resp.data===', JSON.stringify(resp.data));
                    that.showError(resp);
                }
            );
        },

        /**
         * 获取福袋列表
         */
        requestRedEnvelopeList: function (currentPage) {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_red_packet.php?action=get_receive_red_packet';
            if (that.recordStatus == 2) {
                url = 'https://data.idol001.com/api_mobile_red_packet.php?action=get_send_red_packet';
            }
            var params = {
                page: currentPage,
                count: that.count
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('获取福袋列表 requestRedEnvelopeList resp.data==', resp.data);
                    console.log('获取福袋列表 requestRedEnvelopeList params.page==', params.page);
                    if (params.page == 1) {
                        that.setData({
                            allListOri: {}
                        });
                        uni.stopPullDownRefresh();
                    }
                    if (resp.data != null && resp.data.list != null && resp.data.list.length > 0) {
                        var list = resp.data.list;
                        var tempArr = that.allListOri;
                        if (params.page == 1) {
                            tempArr = list;
                        } else {
                            tempArr.push(...list);
                        }
                        that.setData({
                            page: currentPage,
                            allcount: resp.data.allcount,
                            allListOri: tempArr
                        });
                        that.setData(
                            {
                                isRequestAll: list && list.length < that.count ? true : false,
                                isLoading: false,
                                isPreload: false,
                                hasInit: true
                            },
                            function () {
                                that.setData({
                                    allList: tempArr
                                });
                            }
                        );
                    } else {
                        if (params.page == 1) {
                            that.setData(
                                {
                                    isRequestAll: true,
                                    isLoading: false,
                                    isPreload: false,
                                    hasInit: true,
                                    allList: [],
                                    allcount: 0
                                },
                                function () {
                                    that.endRefresh();
                                }
                            );
                        } else {
                            that.setData({
                                isRequestAll: true,
                                isLoading: false,
                                isPreload: false,
                                hasInit: true
                            });
                        }
                    }
                },
                function (resp) {
                    console.log('获取福袋列表 requestRedEnvelopeList resp.data===', JSON.stringify(resp.data));
                    if (params.page == 1) {
                        uni.stopPullDownRefresh();
                        that.setData(
                            {
                                allList: [],
                                hasInit: true,
                                isRequestAll: false
                            },
                            function () {
                                that.endRefresh();
                            }
                        );
                    } else {
                        uni.showToast({
                            title: '网络似乎不太顺畅哦',
                            icon: 'none',
                            duration: 2000
                        });
                        that.setData(
                            {
                                hasInit: true,
                                isRequestAll: false
                            },
                            function () {
                                that.endRefresh();
                            }
                        );
                    }
                }
            );
        },

        /**
         * 显示初始载入失败页面
         */
        showError: function (resp) {
            this.setData({
                hasInit: true,
                errorDic: {
                    needShowErrorView: true,
                    isSomethingError: true,
                    needShowRetryBtn: true,
                    errorText: resp.data && resp.data.error_description ? resp.data.error_description : '网络似乎不太顺畅哦'
                }
            });
        },

        /**
         * 结束刷新
         */
        endRefresh: function () {
            console.log('endrefresh');
            var that = this;
            that.setData(
                {
                    bottomAnimation: false
                },
                function () {
                    uni.stopPullDownRefresh();
                    that.setData({
                        isLoading: false
                    });
                }
            );
        },

        /**
         * 点击重试
         */
        retryGetData: function (e) {
            console.log('retryGetData');
            this.initPage();
        }
    }
};
</script>
<style>
@import './red_envelope_record.css';
</style>
