<template>
    <view style="height: 100%">
        <view class="page position_relative">
            <scroll-view :style="'width:100%;height:' + windowHeightrpx + 'rpx'" :scroll-y="true" :scroll-top="scrollTop" @scrolltolower="onScrollLower" lower-threshold="100">
                <view class="pager_bg position_absolute">
                    <image class="pager_bg_img" :src="swiperDetail.bg_img" :lazy-load="true" mode="aspectFill"></image>
                </view>
                <view class="pager_inner">
                    <view class="time_remain_bg flex_row flex_center" style="margin-top: 400rpx">
                        <block v-if="swiperDetail && swiperDetail.is_over && swiperDetail.is_over == 2">
                            <view class="time_remain_box flex_row flex_center" style="width: 330rpx">
                                <view class="time_remain">{{ swiperStartTime }} ~ {{ swiperEndTime }}</view>
                            </view>
                        </block>
                        <block v-else>
                            <block v-if="remainTime > 0">
                                <view class="time_remain_box flex_row flex_center">
                                    <view class="time_remain">活动剩余时间：{{ day }}天{{ hour }}时{{ min }}分{{ sec }}秒</view>
                                </view>
                            </block>
                            <block v-else-if="remainTime == 0">
                                <view class="time_remain_box flex_row flex_center">
                                    <view class="time_remain">活动已结束</view>
                                </view>
                            </block>
                            <block v-else></block>
                        </block>
                    </view>

                    <view class="rules_bg flex_row flex_center" style="margin-top: 55rpx">
                        <view class="rules_box flex_row flex_center" @tap="go2Rules">
                            <view class="rules">点此查看活动规则</view>
                        </view>
                    </view>

                    <view class="desc_bg flex_row flex_center" style="margin-top: 35rpx">
                        <view class="desc_box">
                            <text class="desc">{{ filters.filter_N(swiperDetail.info_desc) }}</text>
                        </view>
                    </view>

                    <view class="gift_bg flex_row flex_center" style="margin-top: 40rpx">
                        <view class="gift_box flex_column">
                            <view class="gift_title_bg flex_row flex_center" style="margin-top: 30rpx">
                                <image
                                    class="gift_title"
                                    src="https://img.xingxiaoculture.com/origin/2020/03/03/910917fb4d1b8e306dcee52bc016e2f01583218504.png"
                                    :lazy-load="true"
                                    mode="aspectFill"
                                ></image>
                            </view>
                            <view class="gift_sub_img_bg flex_row flex_center">
                                <image class="gift_sub_img" :src="swiperDetail.dress_img" :lazy-load="true" mode="aspectFill"></image>
                            </view>
                        </view>
                    </view>

                    <view class="group_gift_bg flex_row flex_center" style="margin-top: 40rpx">
                        <view class="group_gift_box flex_column">
                            <view class="group_gift_title_bg flex_row flex_center" style="margin-top: 30rpx">
                                <image
                                    class="group_gift_title"
                                    src="https://img.xingxiaoculture.com/origin/2020/03/03/fa091c7a0826aa53ed12132368b44cfa1583218451.png"
                                    :lazy-load="true"
                                    mode="aspectFill"
                                ></image>
                            </view>
                            <block v-if="bShowGiftAll">
                                <block v-for="(item, index) in swiperDetail.gift_list" :key="item._id">
                                    <view class="group_gift_sub flex_column flex_center" style="margin-top: 60rpx">
                                        <view class="group_gift_sub_title_bg position_relative">
                                            <image
                                                class="group_gift_sub_title_bg_img"
                                                src="https://img.xingxiaoculture.com/origin/2020/03/03/4fbe750a831afabf480fa1b228b891231583218431.png"
                                                :lazy-load="true"
                                                mode="aspectFill"
                                            ></image>
                                            <view class="group_gift_sub_title">{{ item.title }}</view>
                                        </view>
                                        <image class="group_gift_sub_title_img" style="margin-top: 20rpx" :src="item.url" :lazy-load="true" mode="aspectFill"></image>
                                        <view class="group_gift_sub_title_txt" style="margin-top: 27rpx">{{ item.info }}</view>
                                    </view>
                                </block>
                                <view class="bottom_tip_wrapper_1 flex_row flex_center" style="margin-top: 30rpx">
                                    <view class="bottom_tip_1 flex_row flex_center">
                                        <image
                                            class="bottom_tip_1_img"
                                            style="margin-right: 20rpx"
                                            src="https://img.xingxiaoculture.com/origin/2020/03/13/1a10345df9c1bea26a5cdbdac0e063bf1584068928.png"
                                            :lazy-load="true"
                                            mode="aspectFill"
                                        ></image>
                                        <text class="bottom_tip_1_txt">{{ swiperDetail.tip_1 }}</text>
                                    </view>
                                </view>
                                <view class="bottom_tip_wrapper_2 flex_row flex_center" style="margin-top: 30rpx">
                                    <text class="bottom_tip_2">{{ swiperDetail.tip_2 }}</text>
                                </view>
                            </block>
                            <block v-else>
                                <view class="group_gift_sub flex_column flex_center" style="margin-top: 60rpx">
                                    <view class="group_gift_sub_title_bg position_relative">
                                        <image
                                            class="group_gift_sub_title_bg_img"
                                            src="https://img.xingxiaoculture.com/origin/2020/03/03/4fbe750a831afabf480fa1b228b891231583218431.png"
                                            :lazy-load="true"
                                            mode="aspectFill"
                                        ></image>
                                        <view class="group_gift_sub_title">{{ swiperDetail.gift_list[0].title }}</view>
                                    </view>
                                    <image
                                        class="group_gift_sub_title_img"
                                        style="margin-top: 20rpx"
                                        :src="/static/pages/subpages_v2/pages/gardener_rank_hd/swiperDetail.gift_list[0].url"
                                        :lazy-load="true"
                                        mode="aspectFill"
                                    ></image>
                                    <view class="group_gift_sub_title_txt" style="margin-top: 27rpx">{{ swiperDetail.gift_list[0].info }}</view>
                                </view>
                            </block>
                            <view class="group_gift_bottom_wrapper flex_row flex_center" style="margin-top: 50rpx">
                                <view class="group_gift_bottom flex_row flex_center" @tap="changeGiftState" v-if="bShowGiftAll">
                                    <text class="group_gift_bottom_txt">收起</text>
                                    <image
                                        class="group_gift_bottom_arrow"
                                        style="margin-left: 10rpx"
                                        src="https://img.xingxiaoculture.com/origin/2020/03/04/b2fb459443f64aa99a7a85a482b15f251583305922.png"
                                        :lazy-load="true"
                                        mode="aspectFill"
                                    ></image>
                                </view>
                                <view class="group_gift_bottom flex_row flex_center" @tap="changeGiftState" v-else>
                                    <text class="group_gift_bottom_txt">展开全部奖励</text>
                                    <image
                                        class="group_gift_bottom_arrow"
                                        style="margin-left: 10rpx"
                                        src="https://img.xingxiaoculture.com/origin/2020/03/04/5d66c5f1351268a741c8604787b1bdd01583306374.png"
                                        :lazy-load="true"
                                        mode="aspectFill"
                                    ></image>
                                </view>
                            </view>
                            <view style="height: 30rpx"></view>
                        </view>
                    </view>

                    <view class="group_rank_bg flex_row flex_center" style="margin-top: 40rpx">
                        <view class="group_rank_box flex_column">
                            <view class="group_rank_title_bg flex_row flex_center" style="margin-top: 30rpx">
                                <image
                                    class="group_rank_title"
                                    src="https://img.xingxiaoculture.com/origin/2020/03/03/38e0bdd48ec17900b355eb86bd3d76ba1583218485.png"
                                    :lazy-load="true"
                                    mode="aspectFill"
                                ></image>
                            </view>
                            <view class="group_rank_sub_title_bg flex_column flex_center" style="margin-top: 55rpx">
                                <text class="group_rank_sub_title">
                                    活动将以这一周（
                                    <text class="group_rank_sub_title">{{ swiperStartTime }}</text>
                                    ~
                                    <text class="group_rank_sub_title">{{ swiperEndTime }}</text>
                                    ）
                                </text>
                                <view class="group_rank_sub_title">公会树的累计生命力进行排名</view>
                                <view class="group_rank_sub_title_red" style="margin-top: 14rpx">(排名数据每3分钟更新)</view>
                            </view>
                        </view>
                    </view>

                    <view class="group_rank_list_bg flex_row flex_center">
                        <view class="group_rank_list_box flex_column">
                            <view v-if="errorDic.needShowErrorView">
                                <view class="netTipBg">
                                    <image
                                        class="netImg"
                                        src="https://img.xingxiaoculture.com/origin/2020/03/02/36d1af4c7e05b426da45ce4ff313ecd21583130480.png"
                                        :lazy-load="true"
                                        mode="aspectFill"
                                    ></image>
                                    <text class="netTip">本周还没有人参与培养哦~</text>
                                </view>
                            </view>
                            <view v-if="!errorDic.needShowErrorView">
                                <block v-for="(item, index) in allList" :key="item._id">
                                    <view class="item_wrapper flex_column" style="margin-left: 10rpx">
                                        <view class="item_content flex_row">
                                            <view class="flex_row" style="width: 65%">
                                                <view class="item_rank_wrapper flex_row flex_center">
                                                    <block v-if="index >= 3">
                                                        <view class="item_rank_num">
                                                            {{ item.rank ? item.rank : index + 1 }}
                                                        </view>
                                                    </block>
                                                    <block v-else>
                                                        <image
                                                            class="item_rank_img"
                                                            :src="
                                                                '/images/listSupport/' +
                                                                (index == 0 ? 'honor_one' : index == 1 ? 'honor_two' : 'honor_three') +
                                                                '/static/pages/subpages_v2/pages/gardener_rank_hd/.png'
                                                            "
                                                            :lazy-load="true"
                                                            mode="aspectFill"
                                                        ></image>
                                                    </block>
                                                </view>
                                                <image class="item_head" style="margin-left: 8rpx" :src="item.groupinfo.star_img" :lazy-load="true" mode="aspectFill"></image>
                                                <text class="item_name" style="margin-left: 12rpx">
                                                    {{ item.groupinfo.name.length > 8 ? filters.subStr(item.groupinfo.name) : item.groupinfo.name }}
                                                </text>
                                                <view class="item_gift_bg flex_row flex_center" @tap="showAward" :data-light="item.light" :data-award="item.award">
                                                    <block v-if="item.light && item.light == 1">
                                                        <image
                                                            class="item_gift"
                                                            src="https://img.xingxiaoculture.com/origin/2020/03/03/ed2b248c4bcfd11da2254699acd9c6901583218467.png"
                                                            :lazy-load="true"
                                                            mode="aspectFill"
                                                        ></image>
                                                    </block>
                                                    <block v-else>
                                                        <image
                                                            class="item_gift"
                                                            src="https://img.xingxiaoculture.com/origin/2020/03/03/43a5c6db3a954ca351aad0a7c706e0681583222127.png"
                                                            :lazy-load="true"
                                                            mode="aspectFill"
                                                        ></image>
                                                    </block>
                                                </view>
                                            </view>
                                            <view class="flex_row flex_center" style="width: 35%">
                                                <text class="item_score">贡献生命力{{ item.score }}</text>
                                            </view>
                                        </view>
                                    </view>
                                </block>
                                <view class="refreshBg" v-if="!isRequestAll">
                                    <refresh_view :is-animation="isLoading"></refresh_view>
                                </view>
                                <view class="item_bottom flex_row flex_center" v-else>
                                    <view class="bottom_desc">没有更多参与园丁啦~</view>
                                </view>
                                <view class="wrapper_bottom flex_row flex_center"></view>
                            </view>
                        </view>
                    </view>
                </view>
            </scroll-view>
            <view class="own_rank_bg flex_row flex_center" v-if="myGroupInfo && myGroupInfo.name">
                <view class="own_rank_box flex_column">
                    <view class="own_rank_line"></view>
                    <view class="own_rank_title_bg flex_row" style="margin-top: 10rpx">
                        <view class="own_rank_title position_absolute" style="left: 24rpx">我当前的公会排名</view>
                        <view class="own_rank_group_tree position_absolute" style="right: 24rpx" @tap="go2groupTree" v-if="isShare == 1">去培养公会树</view>
                    </view>
                    <view class="own_item_content flex_row flex_center">
                        <view class="flex_row flex_center position_absolute" style="left: 20rpx">
                            <view :class="(myScore > 100 ? 'own_item_rank_wrapper' : 'own_item_rank_wrapper_no_rank') + ' flex_row flex_center'">
                                <block v-if="myScore > 100 && myRank > 3">
                                    <view class="own_item_rank_num">{{ myRank }}</view>
                                </block>
                                <block v-else-if="myScore > 100">
                                    <image
                                        class="own_item_rank_img"
                                        :src="
                                            '/images/listSupport/' +
                                            (myRank == 1 ? 'honor_one' : myRank == 2 ? 'honor_two' : 'honor_three') +
                                            '/static/pages/subpages_v2/pages/gardener_rank_hd/.png'
                                        "
                                        :lazy-load="true"
                                        mode="aspectFill"
                                    ></image>
                                </block>
                                <block v-else>
                                    <view class="own_item_rank_num">未上排行</view>
                                </block>
                            </view>
                            <image class="own_item_head" style="margin-left: 8rpx" :src="myGroupInfo.star_img" :lazy-load="true" mode="aspectFill"></image>
                            <text class="own_item_name" style="margin-left: 12rpx">{{ myGroupInfo.name.length > 8 ? filters.subStr(myGroupInfo.name) : myGroupInfo.name }}</text>
                            <view class="own_item_gift_bg flex_row flex_center" @tap="showAward" :data-light="myLight" :data-award="myAward">
                                <block v-if="myLight && myLight == 1">
                                    <image
                                        class="own_item_gift"
                                        src="https://img.xingxiaoculture.com/origin/2020/03/03/ed2b248c4bcfd11da2254699acd9c6901583218467.png"
                                        :lazy-load="true"
                                        mode="aspectFill"
                                    ></image>
                                </block>
                                <block v-else>
                                    <image
                                        class="own_item_gift"
                                        src="https://img.xingxiaoculture.com/origin/2020/03/03/43a5c6db3a954ca351aad0a7c706e0681583222127.png"
                                        :lazy-load="true"
                                        mode="aspectFill"
                                    ></image>
                                </block>
                            </view>
                        </view>
                        <view class="flex_row flex_center position_absolute" style="height: 90rpx; right: 30rpx">
                            <text class="own_item_score">贡献生命力{{ myScore }}</text>
                        </view>
                    </view>
                </view>
            </view>
        </view>
        <login_panel v-if="bShowLoginPanel" @loginCallback="loginCallback" @closeViewEvent="closeLoginView" @webLogin="webLogin"></login_panel>
        <mainGroup_settingWindows
            v-if="isShowSetingMainGroupWindows"
            :windowsType="mainGroupSetingWindowsType"
            :endTime="endTimes"
            :groupInfo="groupInfo"
            :mainGroupInfo="mainGroupInfo"
            :starName="starName"
            @closeMainGroupWindowsClick="closeMainGroupWindowsClick"
            @singleSetingClick="singleSetingClick"
            @leftSetingOthersClick="leftSetingOthersClick"
            @rightSetingThisClick="rightSetingThisClick"
        ></mainGroup_settingWindows>
    </view>
</template>
<script module="filters" lang="wxs" src="@/pages/subpages_v2/pages/gardener_rank_hd/gardener_rank_hd.wxs"></script>
<script>
import error_view from '../../../../component/error_view/error_view';
import login_panel from '../../../../component/login_panel/login_panel';
import refresh_view from '../../../../component/refresh_view/refresh_view';
import mainGroup_settingWindows from '../../../../component/mainGroup_settingWindows/mainGroup_settingWindows';
var net = require('../../../../commonscript/common/netLoad.js');
var IDLoginManager = require('../../../../commonscript/common/IDLoginManager.js');
var IDVisibleManager = require('../../../../commonscript/common/IDVisibleManager.js');
const app = getApp();
const utils = require('../../../../utils/util.js');
const formatNumber = (n) => {
    n = n.toString();
    return n[1] ? n : '0' + n;
};
export default {
    components: {
        error_view,
        login_panel,
        refresh_view,
        mainGroup_settingWindows
    },
    data() {
        return {
            windowWidth: 0,
            windowHeight: 0,
            windowWidthrpx: 0,
            windowHeightrpx: 0,
            isLoginStatus: false,

            //登录状态
            isIpad: false,

            isIphoneX: false,
            isIOSDev: false,
            bShowPickPanel: false,

            //是否展示切换弹窗
            bShowLoginPanel: false,

            //是否展示登录弹窗
            isShowSetingMainGroupWindows: false,

            //是否展示排行弹窗
            mainGroupSetingWindowsType: 'gardenerRankHDFinish',

            //弹窗类型
            actDetailRequestDone: true,

            //是否请求完成
            pageRefresh: false,

            allcount: 0,
            allList: [],
            allListOri: [],
            page: 1,
            count: 10,
            day: '',
            hour: '',
            min: '',
            sec: '',
            remainTime: -1,
            isPreload: true,
            isLoading: false,
            isRequestAll: false,
            bShowGiftAll: false,

            //是否展示全部公会排名奖励
            errorDic: {
                needShowErrorView: false,
                errorText: '暂无排行记录哟~'
            },

            WeChatGroupGid: '',

            //公会id
            isShare: 0,

            //是否分享跳转当前页面
            swiperDetail: {
                bg_img: '',
                is_over: '',
                dress_img: '',
                gift_list: [],
                tip_1: '',
                tip_2: ''
            },

            //活动详细内容
            swiperStartTime: 0,

            //活动开始时间
            swiperEndTime: 0,

            //活动结束时间
            myGroupInfo: {
                name: '',
                star_img: ''
            },

            //当前公会信息
            myRank: 0,

            //当前公会排行
            myScore: 0,

            //当前公会贡献点
            myLight: 0,

            //当前公会是否点亮
            myAward: '',

            //当前公会奖励
            awardScoreMin: 0,

            //生命力达awardScoreMin时将获得7天子年限定套装
            rankList: [],

            //园丁排行排行数据
            activityTimeout: null,

            retryGetgroupgid: 0,
            WeChatGroupGid: '',
            WeChatGroupCode: '',
            WeChatGroupiv: '',
            WeChatGroupEncrypted: '',
            DEBUG: false,
            scrollTop: 0,
            title: '',
            info: '',
            endTimes: [],
            groupInfo: '',
            mainGroupInfo: '',
            starName: ''
        };
    },
    /**
     * 生命周期函数--监听页面
     */
    onLoad: function (option) {
        console.log('onLoad');
        var that = this;
        uni.showShareMenu({
            withShareTicket: true
        });
        that.setData({
            windowWidth: uni.getSystemInfoSync().windowWidth,
            windowHeight: uni.getSystemInfoSync().windowHeight
        });
        console.log('++++++++===onLoad==' + that.windowWidth);
        console.log('++++++++===onLoad==' + that.windowHeight);
        uni.getSystemInfo({
            success: function (res) {
                console.log('onLoad res.windowWidth==' + res.windowWidth);
                console.log('onLoad res.windowHeight==' + res.windowHeight);
                let windowWidthrpx = res.windowWidth * (750 / res.windowWidth);
                let windowHeightrpx = res.windowHeight * (750 / res.windowWidth);
                console.log('onLoad rpx res.windowWidthrpx==' + windowWidthrpx);
                console.log('onLoad rpx res.windowHeightrpx==' + windowHeightrpx);
                that.setData({
                    windowWidthrpx: windowWidthrpx,
                    windowHeightrpx: windowHeightrpx
                });
                if (res.model.indexOf('iPad') > -1) {
                    console.log('idol_sprite_flowers ready iPad');
                    that.setData({
                        isIpad: true
                    });
                }
                if (res.model && res.model.match('iPhone X')) {
                    that.setData({
                        isIphoneX: true
                    });
                }
                if (res.platform == 'ios' || res.platform == 'devtools') {
                    that.setData({
                        isIOSDev: true
                    });
                }
            }
        });
        var is_share = option.is_share || '';
        var gid = option.gid || '';
        console.log('onLoad is_share ==' + is_share);
        console.log('onLoad gid ==' + gid);
        that.setData({
            isShare: is_share,
            WeChatGroupGid: gid
        });
        if (that.DEBUG) {
            that.setData({
                WeChatGroupGid: 'AED23019D4D0461120514B7B6A64C1EA'
            });
        }
        IDLoginManager.stepLoginState();
        that.loginLogicStar();
        if (that.WeChatGroupGid) {
            console.log('onLoad WeChatGroupGid ==' + that.WeChatGroupGid);
        } else {
            console.log('onLoad WeChatGroupGid EMPTY==');
            that.initGid();
        }
    },
    /**
     * 生命周期函数--监听页面初次渲染完成
     */
    onReady: function () {
        console.log('onReady');
    },
    /**
     * 生命周期函数--监听页面显示
     */
    onShow: function () {
        console.log('onShow');
        var that = this;
        if (that.pageRefresh) {
            uni.getNetworkType({
                success: function (res) {
                    var networkType = res.networkType;
                    if (utils.equalsIgnoreCase('unknown', networkType) || utils.equalsIgnoreCase('none', networkType)) {
                        console.log('++++++++===onShow networkError==');
                    } else {
                        if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                            console.log('++++++++===onShow loginError==');
                            return;
                        }
                        that.initPage();
                    }
                }
            });
        }
        that.setData({
            pageRefresh: true
        });
    },
    /**
     * 生命周期函数--监听页面隐藏
     */
    onHide: function () {
        console.log('onHide');
    },
    /**
     * 生命周期函数
     */
    onUnload: function () {
        console.log('onUnload');
        var that = this;
        if (that.activityTimeout) {
            clearTimeout(that.activityTimeout);
        }
    },
    onShareAppMessage: function (options) {
        var that = this;
        var sharepath = '/pages/subpages_v2/pages/gardener_rank_hd/gardener_rank_hd?is_share=1';
        var p = {};
        if (options.from == 'menu') {
            p = {
                title: that.swiperDetail.title,
                imageUrl: that.swiperDetail.share_img
                    ? that.swiperDetail.share_img
                    : 'https://img.xingxiaoculture.com/origin/2020/03/04/b0c2f0e225c2db43474ef359f7a7bc4c1583314140.png',
                path: sharepath
            };
        } else if (options.from == 'button') {
        }

        // #if MP
        return p;
        // #else
        uni.miniapp.invokeMiniappNativeExtension({
            method: 'shareCustomAppMesssage',
            param: p,
            success: (res) => {
                console.log('shareCustomAppMesssage success', res);
            }
        });
        // #endif
    },
    methods: {
        /**
         * 检测登录态
         */
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
                        that.initPage();
                    }
                });
            } else {
                that.setData({
                    isLoginStatus: true
                });
                that.initPage();
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
         * 初始化gid
         */
        initGid: function () {
            var that = this;
            console.log('initGid app.globalData.shareTicket==' + app.globalData.shareTicket);
            if (app.globalData.shareTicket) {
                //标记 （保证两个api都请求完再请求gid）
                var target = 0;
                uni.getShareInfo({
                    shareTicket: app.globalData.shareTicket,
                    success: function (res) {
                        console.log(res.encryptedData);
                        target++;
                        that.setData({
                            WeChatGroupEncrypted: res.encryptedData,
                            WeChatGroupiv: res.iv
                        });
                        if (target == 2) {
                            that.getwechatGid();
                        }
                    }
                });
                uni.login({
                    success: function (obj) {
                        console.log(obj.code);
                        target++;
                        that.setData({
                            WeChatGroupCode: obj.code
                        });
                        if (target == 2) {
                            that.getwechatGid();
                        }
                    }
                });
            } else {
                that.initPage();
            }
        },

        // 获取微信gid
        getwechatGid: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=return_opengid';
            var params = {
                code: this.WeChatGroupCode,
                encrypted_data: this.WeChatGroupEncrypted,
                iv: this.WeChatGroupiv
            };
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    console.log('getwechatGid==', resp.data);
                    if (!resp.data || !resp.data.openGId || resp.data.openGId.length == 0) {
                        //第一次如果获取不到 重试获取
                        if (that.retryGetgroupgid == 0) {
                            var target = 0;
                            uni.getShareInfo({
                                shareTicket: app.globalData.shareTicket,
                                success: function (res) {
                                    console.log(res.encryptedData);
                                    target++;
                                    that.setData({
                                        WeChatGroupEncrypted: res.encryptedData,
                                        WeChatGroupiv: res.iv
                                    });
                                    if (target == 2) {
                                        that.getwechatGid();
                                    }
                                }
                            });
                            uni.login({
                                success: function (obj) {
                                    console.log(obj.code);
                                    target++;
                                    that.setData({
                                        WeChatGroupCode: obj.code
                                    });
                                    if (target == 2) {
                                        that.getwechatGid();
                                    }
                                }
                            });
                        }
                        that.setData({
                            retryGetgroupgid: 1
                        });
                        return;
                    }
                    app.globalData.groupGid = that.WeChatGroupGid;
                    that.setData({
                        WeChatGroupGid: resp.data.openGId
                    });
                    that.initPage();
                },
                function () {}
            );
        },

        initPage: function () {
            console.log('initPage');
            var that = this;
            that.getLunbo(1);
        },

        backHomeClick: function () {
            var url = '/pages/idol_sprite_guard/idol_sprite_guard';
            uni.switchTab({
                url: url
            });
        },

        go2Rules: function () {
            var that = this;
            if (that.swiperDetail && that.swiperDetail.rules) {
                var url = '/pages/web_page/web_page?url=' + encodeURIComponent(that.swiperDetail.rules);
                uni.navigateTo({
                    url: url
                });
            }
        },

        starCountDown: function () {
            var that = this;
            that.timer = setTimeout(function () {
                var time = that.countDownTime - (new Date() - that.nowTime) / 1000;
                that.setData({
                    day: formatNumber(Math.floor(time / 60 / 60 / 24)),
                    hour: formatNumber(Math.floor((time / 60 / 60) % 24)),
                    min: formatNumber(Math.floor((time / 60) % 60)),
                    sec: formatNumber(Math.floor(time % 60)),
                    remainTime: time < 0 ? 0 : time
                });
                if (that.remainTime <= 0) {
                    that.timerIsStarted = false;
                    that.getLunbo();
                    clearTimeout(that.activityTimeout);
                }
                if (that.timerIsStarted) {
                    that.starCountDown();
                }
            }, 1000);
        },

        /**
         * 获取活动头图与活动剩余时间
         */
        getLunbo: function (type) {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_idol_tree.php?action=get_tree_activity';
            var params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('获取活动头图与活动剩余时间 getLunbo resp.data==', resp.data);
                    that.setData({
                        swiperDetail: resp.data,
                        awardScoreMin: resp.data.award_score_min ? resp.data.award_score_min : 0,
                        swiperStartTime: resp.data.time_date.split('-')[0],
                        swiperEndTime: resp.data.time_date.split('-')[1]
                    });
                    if (resp.data.is_over && resp.data.is_over == -1) {
                        that.setData({
                            isShowSetingMainGroupWindows: true,
                            mainGroupSetingWindowsType: 'gardenerRankHDFinish'
                        });
                    } else {
                        that.setData({
                            isShowSetingMainGroupWindows: false
                        });
                    }
                    if (resp.data.title) {
                        uni.setNavigationBarTitle({
                            title: resp.data.title
                        });
                    }
                    that.nowTime = new Date();
                    that.countDownTime = resp.data.time || 0;
                    if (that.countDownTime > 0) {
                        that.timerIsStarted = true;
                        that.starCountDown();
                        clearTimeout(that.activityTimeout);
                        var cycleTime = 0;
                        var startTime = new Date().getTime();
                        var count = 0;
                        function fixed() {
                            count++;
                            var offset = new Date().getTime() - (startTime + count * 1000);
                            var nextTime = 1000 - offset;
                            if (nextTime < 0) {
                                nextTime = 0;
                            }
                            that.activityTimeout = setTimeout(fixed, nextTime);
                            cycleTime = cycleTime + 1;
                            if (cycleTime == 60) {
                                that.getLunbo();
                                clearTimeout(that.activityTimeout);
                            }
                        }
                        that.activityTimeout = setTimeout(fixed, 1000);
                    } else {
                        that.setData({
                            remainTime: 0
                        });
                    }
                    if (type == 1) {
                        that.requestRankList();
                    }
                },
                function (resp) {
                    console.log('获取活动头图与活动剩余时间 getLunbo resp.data===', JSON.stringify(resp.data));
                }
            );
        },

        /**
         * 获取排行数据
         */
        requestRankList: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_idol_tree.php?action=get_tree_activity_group_rank_list';
            var params = {
                gid: that.WeChatGroupGid,
                page: that.page,
                count: that.count
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('++++++++===获取排行数据 requestRankList resp==', resp);
                    if (resp.data != null && resp.data.list != null && resp.data.list.length > 0) {
                        var list = resp.data.list;
                        var tempArr = that.allListOri;
                        console.log(tempArr);
                        if (params.page == 1) {
                            tempArr = list;
                            uni.stopPullDownRefresh();
                        } else {
                            tempArr.push(...list);
                        }
                        that.setData({
                            allcount: resp.data.allcount,
                            allListOri: tempArr,
                            allList: tempArr
                        });
                        that.setData(
                            {
                                isRequestAll: tempArr.length < resp.data.allcount ? false : true,
                                isLoading: true,
                                isPreload: false,
                                errorDic: {
                                    needShowErrorView: false,
                                    errorText: resp.data && resp.data.error_description ? resp.data.error_description : '暂无排行记录哟~'
                                }
                            },
                            function () {
                                that.endRefresh();
                            }
                        );
                    } else {
                        console.log('++===++resp.data==');
                        console.log('++===++resp.data oThis.data.page==' + that.page);
                        if (that.page == 1) {
                            that.setData(
                                {
                                    isRequestAll: true,
                                    isLoading: false,
                                    isPreload: false,
                                    allList: [],
                                    allcount: 0,
                                    errorDic: {
                                        needShowErrorView: true,
                                        errorText: resp.data && resp.data.error_description ? resp.data.error_description : '暂无排行记录哟~'
                                    }
                                },
                                function () {
                                    that.endRefresh();
                                }
                            );
                        } else {
                            that.setData(
                                {
                                    isRequestAll: true,
                                    isLoading: false,
                                    isPreload: false,
                                    errorDic: {
                                        needShowErrorView: false,
                                        errorText: resp.data && resp.data.error_description ? resp.data.error_description : '暂无排行记录哟~'
                                    }
                                },
                                function () {
                                    that.endRefresh();
                                }
                            );
                        }
                    }
                    that.setData({
                        actDetailRequestDone: true,
                        myGroupInfo: resp.data.groupinfo ? resp.data.groupinfo : {},
                        myRank: resp.data.rank ? resp.data.rank : 0,
                        myScore: resp.data.score ? resp.data.score : 0,
                        myLight: resp.data.light ? resp.data.light : 0,
                        myAward: resp.data.award ? resp.data.award : ''
                    });
                },
                function (resp) {
                    console.log('+++error==++获取排行数据 requestRankList resp.data===', resp.data);
                    if (that.page == 1) {
                        that.setData(
                            {
                                isRequestAll: true,
                                isLoading: false,
                                isPreload: false,
                                allList: [],
                                allcount: 0,
                                errorDic: {
                                    needShowErrorView: true,
                                    errorText: resp.data && resp.data.error_description ? resp.data.error_description : '暂无排行记录哟~'
                                }
                            },
                            function () {
                                that.endRefresh();
                            }
                        );
                    } else {
                        that.setData(
                            {
                                isRequestAll: true,
                                isLoading: false,
                                isPreload: false,
                                errorDic: {
                                    needShowErrorView: false,
                                    errorText: resp.data && resp.data.error_description ? resp.data.error_description : '暂无排行记录哟~'
                                }
                            },
                            function () {
                                that.endRefresh();
                            }
                        );
                    }
                }
            );
        },

        endRefresh: function () {
            console.log('endRefresh');
            var that = this;
            console.log('endRefresh isRequestAll==' + that.isRequestAll);
            that.setData(
                {
                    isLoading: false
                },
                function () {
                    uni.stopPullDownRefresh();
                    uni.hideLoading();
                }
            );
        },

        onScrollLower: function (e) {
            console.log('onScrollLower');
            var that = this;
            if (that.isRequestAll) {
                return;
            }
            that.page++;
            that.requestRankList();
        },

        changeGiftState: function () {
            console.log('changeGiftState');
            var that = this;
            if (that.bShowGiftAll) {
                that.setData({
                    bShowGiftAll: false
                });
            } else {
                that.setData({
                    bShowGiftAll: true
                });
            }
        },

        go2groupTree: function () {
            console.log('go2groupTree');
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            console.log('go2groupTree gid==', that.WeChatGroupGid);
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/support_tree/support_tree?gid=' + (that.WeChatGroupGid && that.WeChatGroupGid.length > 0 ? that.WeChatGroupGid : '')
            });
        },

        singleSetingClick: function () {
            console.log('singleSetingClick');
            var that = this;
            if (that.mainGroupSetingWindowsType == 'gardenerRankHDFinish' && this.actDetailRequestDone) {
                that.setData({
                    actDetailRequestDone: false
                });
                that.initPage();
            }
        },

        showAward: function (e) {
            if (e.currentTarget.dataset) {
                console.log('showAward e light==' + JSON.stringify(e.currentTarget.dataset.light));
                console.log('showAward e award==' + JSON.stringify(e.currentTarget.dataset.award));
            }
            if (e.currentTarget.dataset && e.currentTarget.dataset.light && e.currentTarget.dataset.light == 1) {
                console.log('showAward 已点亮==');
                uni.showToast({
                    title: e.currentTarget.dataset.award,
                    icon: 'none'
                });
            } else {
                console.log('showAward 未点亮==');
                uni.showToast({
                    title: e.currentTarget.dataset.award,
                    icon: 'none'
                });
            }
        },

        webLogin() {
            console.log('占位：函数 webLogin 未声明');
        },

        closeMainGroupWindowsClick() {
            console.log('占位：函数 closeMainGroupWindowsClick 未声明');
        },

        leftSetingOthersClick() {
            console.log('占位：函数 leftSetingOthersClick 未声明');
        },

        rightSetingThisClick() {
            console.log('占位：函数 rightSetingThisClick 未声明');
        }
    }
};
</script>
<style>
@import './gardener_rank_hd.css';
</style>
