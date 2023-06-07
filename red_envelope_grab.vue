<template>
    <view style="height: 100%">
        <view class="page" v-if="hasInit">
            <view class="page_view_frame" :style="'display:' + (errorDic && !errorDic.needShowErrorView ? '' : 'none') + ';'">
                <view
                    v-if="
                        (isLoginStatus && redPacketDetail && redPacketDetail.has_get == 1) ||
                        (redPacketDetail && redPacketDetail.has_get_count == redPacketDetail.all_count) ||
                        (redPacketDetail && redPacketDetail.has_get_count < redPacketDetail.all_count && redPacketDetail.has_end == 1)
                    "
                    class="page"
                >
                    <view class="main_view_top">
                        <image
                            class="fuli_img_top"
                            src="http://img.xingxiaoculture.com/origin/2020/01/06/5693420e9ff2952f6bc380b9abab7ce41578279704.png"
                            mode="aspectFill"
                            :lazy-load="true"
                        />
                        <view class="main_view_top_detail" style="padding-top: 70rpx">
                            <text class="fuli_txt_top" @tap="gotoRedEnvelopeRecord">福袋记录></text>
                            <view class="user-style">
                                <image class="img-style" :src="userInfo.image.thumbnail_pic" mode="aspectFill" :lazy-load="true"></image>
                                <text class="txt-style">{{ userInfo.nickname }}的福袋</text>
                            </view>
                            <view class="fuli_center" style="margin-top: 50rpx">
                                <text class="fuli_blessings">{{ redPacketDetail.desc }}</text>
                            </view>
                            <view class="fuli_center" style="margin-top: 87rpx" v-if="redPacketDetail && redPacketDetail.has_get == 1">
                                <view class="flex_column">
                                    <text class="red_envelope_num">{{ redPacketDetail.user_get.coin }}</text>
                                    <text class="red_envelope_num_tip">个公会币</text>
                                    <text class="red_envelope_num_tip_bottom">已存入我的公会币</text>
                                </view>
                            </view>
                            <view class="fuli_center" style="margin-top: 132rpx" v-else-if="redPacketDetail && redPacketDetail.has_get_count == redPacketDetail.all_count">
                                <text class="fuli_status">来晚了，福袋已被领完~</text>
                            </view>
                            <view
                                class="fuli_center"
                                style="margin-top: 132rpx"
                                v-else-if="redPacketDetail && redPacketDetail.has_get_count < redPacketDetail.all_count && redPacketDetail.has_end == 1"
                            >
                                <text class="fuli_status">福袋超过24小时，已过期~</text>
                            </view>
                        </view>
                    </view>
                    <view class="main_view_bottom">
                        <view class="red_envelope">
                            <text v-if="redPacketDetail && redPacketDetail.has_get_count == redPacketDetail.all_count" class="red_envelope_status">
                                已领取{{ redPacketDetail.has_get_count }}/{{ redPacketDetail.all_count }}个，共{{ redPacketDetail.has_get_coin }}/{{
                                    redPacketDetail.all_coin
                                }}个公会币，福袋领完了
                            </text>
                            <text
                                v-else-if="redPacketDetail && redPacketDetail.has_get_count < redPacketDetail.all_count && redPacketDetail.has_end == 1"
                                class="red_envelope_status"
                            >
                                该福袋已过期。已领取{{ redPacketDetail.has_get_count }}/{{ redPacketDetail.all_count }}个，共{{ redPacketDetail.has_get_coin }}/{{
                                    redPacketDetail.all_coin
                                }}个公会币
                            </text>
                            <text v-else-if="redPacketDetail && redPacketDetail.has_get == 1" class="red_envelope_status">
                                已领取{{ redPacketDetail.has_get_count }}/{{ redPacketDetail.all_count }}个，共{{ redPacketDetail.has_get_coin }}/{{
                                    redPacketDetail.all_coin
                                }}个公会币
                            </text>
                            <view class="flex_row flex_center" @tap="showRedEnvelopeRules">
                                <image class="red_envelope_rules_img" src="/static/images/common/icon_tip.png" mode="aspectFill" :lazy-load="true" />
                                <text class="red_envelope_rules" style="margin-left: 7rpx">规则</text>
                            </view>
                        </view>
                        <view class="red_envelope_dress flex_column flex_center">
                            <block v-if="redPacketLotteryListStr">
                                <view class="red_envelope_dress_banner" style="text-align: left">
                                    恭喜{{ redPacketLotteryList && redPacketLotteryList.length > 3 ? redPacketLotteryListStr + '等' : redPacketLotteryListStr }}获得稀有道具兑换资格
                                </view>
                                <view class="red_envelope_dress_detail flex_row_reverse" @tap="gotoRedEnvelopeH5">去培养公会树 ></view>
                            </block>
                            <block v-else>
                                <block v-if="swiperDetail && swiperDetail.time > 0">
                                    <view class="red_envelope_dress_banner" style="text-align: center">当前暂无用户抽中公会公会树培养材料兑换资格</view>
                                    <view class="red_envelope_dress_detail flex_row_reverse" @tap="gotoRedEnvelopeH5">去培养公会树 ></view>
                                </block>
                            </block>
                        </view>
                        <view class="envelope_list">
                            <block v-for="(item, index) in allList" :key="index">
                                <view class="item flex_row" style="padding-top: 15rpx; padding-bottom: 15rpx">
                                    <view class="flex_row" style="padding-left: 20rpx">
                                        <view class="item_img_view flex_row flex_center">
                                            <image class="item_img" :src="item.userinfo.image.thumbnail_pic" mode="aspectFill" :lazy-load="true" />
                                            <image
                                                v-if="item.the_max && item.the_max == 1"
                                                class="item_img_best"
                                                src="https://img.xingxiaoculture.com/origin/2020/01/04/8b14339473179df485292e61e1b50bc81578123711.png"
                                                mode="aspectFill"
                                                :lazy-load="true"
                                            />
                                        </view>
                                        <view class="flex_column flex_start" style="margin-left: 20rpx">
                                            <text class="item_name">{{ common.subStrCut(item.userinfo.nickname, 10) }}</text>
                                            <text class="item_time" style="margin-top: 7rpx">{{ common.dateFormat(item.update_time * 1000, 'yyyy-MM-dd hh:mm') }}</text>
                                        </view>
                                    </view>
                                    <view class="flex_column flex_end" style="height: 108rpx; position: absolute; right: 20rpx">
                                        <text class="item_coin" style="width: 350rpx; text-align: right">{{ item.coin }}公会币</text>
                                        <view
                                            v-if="item && item.gift"
                                            style="width: 400rpx; text-align: right; margin-top: 7rpx"
                                            @tap="showRedEnvelopeGiftDetail"
                                            :data-gift="item.gift"
                                        >
                                            <image
                                                class="item_detail_img"
                                                :src="/static/pages/subpages_v2/pages/red_envelope_grab/item.gift.icon"
                                                :lazy-load="true"
                                                mode="aspectFill"
                                            />
                                            <text class="item_detail" style="margin-left: 8rpx">
                                                获
                                                <text class="red">{{ item.gift.measure_word }}</text>
                                                {{ item.gift.measure_word_desc }}
                                            </text>
                                        </view>
                                        <view style="width: 350rpx; height: 25rpx; margin-top: 7rpx" v-else />
                                    </view>
                                </view>
                            </block>
                        </view>
                        <view class="bottom_refresh" :style="'display:' + (!isRequestAll ? 'block' : 'none') + ';'">
                            <refresh_view :is-animation="bottomAnimation"></refresh_view>
                        </view>
                        <view class="bottom_padding" />
                    </view>
                    <view class="main_view_bottom_add">
                        <view class="red_envelope_add" @tap="gotoRedEnvelopeAdd">
                            <text class="red_envelope_add_txt">包福袋</text>
                        </view>
                    </view>
                </view>
                <view v-else style="width: 100vw; height: 100vh; display: flex; flex-direction: column">
                    <view class="main_view_top_bgcolor-grab" style="background: #90090b"></view>
                    <image
                        class="main_view_top_img-grab"
                        src="http://img.xingxiaoculture.com/origin/2020/01/04/c7c18383862d36f58c306948276021e01578137931.png"
                        mode="aspectFill"
                        :lazy-load="true"
                    ></image>
                    <view class="main_view_top-grab" style="padding-top: 120rpx">
                        <view class="user-style">
                            <image class="img-style-grab" :src="userInfo.image.thumbnail_pic" mode="aspectFill" :lazy-load="true"></image>
                            <text class="txt-style-grab">{{ userInfo.nickname }}的福袋</text>
                        </view>
                        <view class="fuli_center" style="margin-top: 80rpx">
                            <text class="fuli_blessings-grab">{{ redPacketDetail.desc }}</text>
                        </view>
                    </view>
                    <view class="flex_column flex_center" style="width: 100%">
                        <image
                            class="grab_img"
                            src="https://img.xingxiaoculture.com/origin/2020/01/04/0e61168eca575f8f66b43aa7278258ad1578123846.png"
                            mode="aspectFill"
                            :lazy-load="true"
                        />
                        <view class="grab" @tap="grabRedEnvelope">抢福袋</view>
                    </view>
                </view>
            </view>
            <view class="error_view_frame flex_column flex_center" :style="'display:' + (errorDic && errorDic.needShowErrorView ? '' : 'none') + ';'">
                <error_view :error-dic="errorDic" @clickRetryEvent="retryGetData"></error_view>
            </view>
        </view>
        <view class="backhome_btn_top" @tap="backHomeClick" v-if="isShare == 1">
            <text class="backhome_txt_top">回首页</text>
        </view>
        <login_panel v-if="bShowLoginPanel" @loginCallback="loginCallback" @closeViewEvent="closeLoginView" @webLogin="webLogin"></login_panel>

        <red_envelope_rules_tip v-if="showRedEnvelopeRulesTip" :rules="redEnvelopeTipRules" @redEnvelopeTipKnownClick="redEnvelopeTipKnownClick"></red_envelope_rules_tip>

        <red_envelope_grab_gift_success
            v-if="showRedEnvelopeGrabGiftSuccessTip"
            :redPacketGrabDetail="redPacketGrabDetail"
            :coin_num="redPacketGrabDetail.user_get.coin"
            :discount="redPacketGrabDetail.user_get.gift.discount"
            :desc="redPacketGrabDetail.user_get.gift.desc"
            :dress_img="redPacketGrabDetail.user_get.gift.dress_img"
            :expiry_date="redPacketGrabDetail.user_get.gift.expiry_date"
            :coin_current="redPacketGrabDetail.user_get.gift.coin_current"
            :coin_ori="redPacketGrabDetail.user_get.gift.coin_ori"
            @redEnvelopeGrabAbandoned="redEnvelopeGrabSuccessAbandoned"
            @redEnvelopeGrabExchange="redEnvelopeGrabSuccessExchange"
        ></red_envelope_grab_gift_success>

        <red_envelope_grab_gift_abandoned
            v-if="showRedEnvelopeGrabAbandonedTip"
            @redEnvelopeGrabAbandoned="redEnvelopeGrabAbandoned"
            @redEnvelopeGrabExchange="redEnvelopeGrabAbandonedExchange"
        ></red_envelope_grab_gift_abandoned>

        <red_envelope_grab_gift_coin_not_enough
            v-if="showRedEnvelopeGrabCoinNotEnoughTip"
            :pay_state="pay_state"
            :is_opp="is_opp"
            :needCoinCount="redPacketGrabDetail.user_get.gift.coin_current"
            :ownSupportCoinCount="ownSupportCoinCount"
            @diamondNotEnoughClose="diamondNotEnoughClose"
            @gotoContact="gotoContact"
            @diamondNotEnoughGotoCharge="diamondNotEnoughGotoCharge"
        ></red_envelope_grab_gift_coin_not_enough>

        <red_envelope_grab_gift_fail
            v-if="swiperDetail && swiperDetail.time > 0 && showRedEnvelopeGrabGiftFailTip"
            :coin_num="redPacketGrabDetail.user_get.coin"
            :img_url="swiperDetail.mini_img"
            :discount="swiperDetail.discount"
            @redEnvelopeGrabGiftClose="redEnvelopeGrabGiftFailClose"
            @gotoActivityDetail="gotoActivityDetail"
            @gotoProp="gotoProp"
            @gotoDress="gotoDress"
        ></red_envelope_grab_gift_fail>

        <red_envelope_grab_gift_exchange
            :gift="redPacketGrabDetail"
            v-if="showRedEnvelopeGrabGiftExchangeTip"
            :expiry_date="redPacketGrabDressDetail.gift.expiry_date"
            :dress_img="redPacketGrabDressDetail.gift.dress_img"
            @redEnvelopeGrabKnownClick="redEnvelopeGrabGiftExchangeClose"
        ></red_envelope_grab_gift_exchange>

        <red_envelope_grab_gift_detail
            v-if="showRedEnvelopeGrabGiftDetailTip"
            :gift="redPacketGiftDetail"
            @redEnvelopeDetailClose="redEnvelopeGiftDetailClose"
        ></red_envelope_grab_gift_detail>

        <!-- 领取福袋额度上限提醒 -->
        <view v-if="isShowErrorDialog" class="dialog_view">
            <view class="dialog_main_view">
                <image src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/12/XmGrKXFn4Y1615541728076.png" class="dialog_close" @tap="errorDialogClose"></image>
                <text class="dialog_main_title">今天领取福袋额度\n已达上限</text>
                <view class="dialog_main_text">{{ showDialogErrorMsg }}</view>
                <view class="dialog_main_btn" @tap="errorDialogClose">我知道了</view>
            </view>
        </view>
    </view>
</template>
<script module="common" lang="wxs" src="@/pages/subpages_v2/pages/red_envelope_grab/red_envelope_grab.wxs"></script>
<script>
import idc_image from '../../../../component/idc_image/idc_image';
import refresh_view from '../../../../component/refresh_view/refresh_view';
import error_view from '../../../../component/error_view/error_view';
import login_panel from '../../../../component/login_panel/login_panel';
import red_envelope_rules_tip from '../../component/red_envelope_rules_tip/red_envelope_rules_tip';
import red_envelope_grab_gift_success from '../../component/red_envelope_grab_gift_success/red_envelope_grab_gift_success';
import red_envelope_grab_gift_fail from '../../component/red_envelope_grab_gift_fail/red_envelope_grab_gift_fail';
import red_envelope_grab_gift_coin_not_enough from '../../component/red_envelope_grab_gift_coin_not_enough/red_envelope_grab_gift_coin_not_enough';
import red_envelope_grab_gift_exchange from '../../component/red_envelope_grab_gift_exchange/red_envelope_grab_gift_exchange';
import red_envelope_grab_gift_abandoned from '../../component/red_envelope_grab_gift_abandoned/red_envelope_grab_gift_abandoned';
import red_envelope_grab_gift_detail from '../../component/red_envelope_grab_gift_detail/red_envelope_grab_gift_detail';
var net = require('../../../../commonscript/common/netLoad.js');
var IDLoginManager = require('../../../../commonscript/common/IDLoginManager.js');
var IDVisibleManager = require('../../../../commonscript/common/IDVisibleManager.js');
var globalData = require('../../../../commonscript/common/globalData.js');
const JumpManager = require('../../../../commonscript/common/JumpManager.js');
const app = getApp();
export default {
    components: {
        idc_image,
        refresh_view,
        error_view,
        login_panel,
        red_envelope_rules_tip,
        red_envelope_grab_gift_success,
        red_envelope_grab_gift_fail,
        red_envelope_grab_gift_coin_not_enough,
        red_envelope_grab_gift_exchange,
        red_envelope_grab_gift_abandoned,
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
            showRedEnvelopeRulesTip: false,
            //是否显示规则弹窗
            showRedEnvelopeGrabGiftSuccessTip: false,
            //是否显示抢福袋获得限定套装弹窗
            showRedEnvelopeGrabAbandonedTip: false,
            //是否显示放弃领取限定套装提示弹窗
            showRedEnvelopeGrabCoinNotEnoughTip: false,
            //是否显示公会币不足弹窗
            showRedEnvelopeGrabGiftFailTip: false,
            //是否显示抢福袋结果弹窗
            showRedEnvelopeGrabGiftExchangeTip: false,
            //是否显示抢福袋兑换结果弹窗
            showRedEnvelopeGrabGiftDetailTip: false,
            //是否显示福袋限定套装详情弹窗

            bGotoDiamondNotEnoughCharge: false,
            //是否跳转兑换公会币

            redPacketDetail: {
                has_get: 0,
                has_get_count: '',
                all_count: '',
                has_end: 0,
                desc: '',

                user_get: {
                    coin: ''
                },

                has_get_coin: '',
                all_coin: ''
            },
            //福袋详情
            redPacketGrabDetail: {
                user_get: {
                    coin: '',

                    gift: {
                        discount: '',
                        desc: '',
                        dress_img: '',
                        expiry_date: '',
                        coin_current: '',
                        coin_ori: ''
                    }
                }
            },
            //抢福袋详情
            redPacketGrabDressDetail: {
                gift: {
                    expiry_date: '',
                    dress_img: ''
                }
            },
            //福袋装扮详情
            swiperDetail: {
                time: 0,
                mini_img: '',
                discount: ''
            },
            //福袋头图与活动剩余时间
            redPacketLotteryList: {},
            //获取某次福袋的有抽到兑换资格并已兑换人员列表
            redPacketLotteryListStr: '',
            //获取某次福袋的有抽到兑换资格并已兑换人员列表-str
            redPacketGiftDetail: {},
            //福袋限定套装详情
            redEnvelopeTipRules: '',
            //福袋规则

            isShare: 0,
            //是否分享跳转当前页面
            isShareBack: 0,
            //是否分享后回到当前页面
            isFromRecord: 0,
            //是否从福袋记录跳转
            userInfo: {
                image: {
                    thumbnail_pic: ''
                },

                nickname: ''
            },
            //福袋用户信息
            user_id: '',
            //福袋用户id
            red_packet_id: '',
            //福袋id
            page: 1,
            //福袋领取列表page
            count: 10,
            //福袋领取列表count
            isRequestAll: false,
            //福袋领取列表是否加载完毕
            isLoading: false,
            //福袋领取列表是否正在加载
            isPreload: false,
            //福袋领取列表是否未执行加载
            bottomAnimation: true,
            //福袋领取列表底部加载状态是否显示动画
            allcount: 0,
            //福袋领取列表数量
            allList: {},
            //福袋领取列表
            allListOri: [],
            //福袋领取列表
            hasInit: false,
            //是否已加载页面
            is_official_group: false,
            //是否官方公会
            pay_state: 0,
            //支付开关
            is_opp: false,
            ownSupportCoinCount: '',
            //当前剩余公会币

            retryGetgroupgid: 0,
            WeChatGroupGid: '',
            WeChatGroupCode: '',
            WeChatGroupiv: '',
            WeChatGroupEncrypted: '',
            DEBUG: false,
            isShowErrorDialog: false,
            showDialogErrorMsg: ''
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
        var is_share = option.is_share || '';
        var is_from_record = option.is_from_record || '';
        var user_id = option.user_id || '';
        var red_packet_id = option.red_packet_id || '';
        console.log('is_share ==' + is_share);
        console.log('is_from_record ==' + is_from_record);
        console.log('user_id ==' + user_id);
        console.log('red_packet_id ==' + red_packet_id);
        that.setData({
            isShare: is_share,
            isFromRecord: is_from_record,
            user_id: user_id,
            red_packet_id: red_packet_id
        });
        if (that.DEBUG) {
            that.setData({
                user_id: '5b0f84c265a8752b248b4572',
                red_packet_id: '1583225879sk1WG9ahnc'
            });
        }
        IDLoginManager.stepLoginState();
        that.loginLogicStar();
        that.initGid();
    },
    onShow: function () {
        var that = this;
        if (that.hasInit) {
            if (that.isShareBack == 1) {
                that.setData({
                    isShareBack: 0
                });
            }
        }
        if (that.bGotoDiamondNotEnoughCharge) {
            that.showRedEnvelopeGrabGiftSuccess();
        }
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
    methods: {
        backHomeClick: function () {
            var url = '/pages/idol_sprite_guard/idol_sprite_guard';
            uni.switchTab({
                url: url
            });
        },

        gotoRedEnvelopeH5: function () {
            uni.reLaunch({
                url: '/pages/idol_home/idol_home'
            });
        },

        /**
         * 显示规则弹窗
         */
        showRedEnvelopeRules: function () {
            var that = this;
            that.setData({
                showRedEnvelopeRulesTip: true
            });
        },

        /**
         * 关闭规则弹窗
         */
        redEnvelopeTipKnownClick: function () {
            var that = this;
            that.setData({
                showRedEnvelopeRulesTip: false
            });
        },

        /**
         * 显示领取限定套装弹窗
         */
        showRedEnvelopeGrabGiftSuccess: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabGiftSuccessTip: true
            });
        },

        /**
         * 放弃领取限定套装
         */
        redEnvelopeGrabSuccessAbandoned: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabGiftSuccessTip: false
            });
            that.showRedEnvelopeGrabAbandoned();
        },

        /**
         * 兑换领取限定套装
         */
        redEnvelopeGrabSuccessExchange: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabGiftSuccessTip: false
            });
            that.requestBuyRedEnvelopeDress();
        },

        /**
         * 显示放弃领取限定套装提示弹窗
         */
        showRedEnvelopeGrabAbandoned: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabAbandonedTip: true
            });
        },

        /**
         * 确认放弃领取限定套装
         */
        redEnvelopeGrabAbandoned: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabAbandonedTip: false
            });
            // 放弃某福袋对用户产生的抽奖奖品兑换资格
            that.requestRedEnvelopeAbandoned();
        },

        /**
         * 继续领取限定套装
         */
        redEnvelopeGrabAbandonedExchange: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabAbandonedTip: false
            });
            that.showRedEnvelopeGrabGiftSuccess();
        },

        /**
         * 显示公会币不足弹窗
         */
        showDiamondNotEnough: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabCoinNotEnoughTip: true
            });
        },

        /**
         * 关闭公会币不足弹窗
         */
        diamondNotEnoughClose: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabCoinNotEnoughTip: false
            });
        },

        /**
         * 公会币不足弹窗-支付
         */
        diamondNotEnoughGotoCharge: function () {
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                return;
            }
            /**
             * 判断支付跳转
             */
            JumpManager.junmpToPayment();
            oThis.diamondNotEnoughClose();
        },

        /**
         * 公会币不足弹窗-支付
         */
        gotoContact: function () {
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                return;
            }
            var that = this;
            uni.getSystemInfo({
                success: function (res) {
                    console.log('gotoContact res.platform==' + res.platform);
                    that.setData({
                        bGotoDiamondNotEnoughCharge: true
                    });
                }
            });
            that.diamondNotEnoughClose();
        },

        /**
         * 显示抢福袋结果弹窗
         */
        showRedEnvelopeGrabGiftFail: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabGiftFailTip: true
            });
        },

        /**
         * 关闭抢福袋结果弹窗
         */
        redEnvelopeGrabGiftFailClose: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabGiftFailTip: false
            });
        },

        /**
         * 抢福袋结果弹窗 - 跳转活动详情
         */
        gotoActivityDetail: function () {
            var that = this;
            that.redEnvelopeGrabGiftFailClose();
            that.gotoRedEnvelopeH5();
        },

        /**
         * 抢福袋结果弹窗 - 跳转道具
         */
        gotoProp: function () {
            var that = this;
            that.redEnvelopeGrabGiftFailClose();
            uni.navigateTo({
                url: '/pages/subpages/pages/game_prop/game_prop'
            });
        },

        /**
         * 抢福袋结果弹窗 - 跳转装扮
         */
        gotoDress: function () {
            var that = this;
            that.redEnvelopeGrabGiftFailClose();
            uni.navigateTo({
                url:
                    '/pages/subpages_v2/pages/dress_room/dress_room?user=' +
                    encodeURIComponent(JSON.stringify(globalData.idolUserInfo)) +
                    '&bShowAngelWindows=' +
                    '0' +
                    '&bShowLevelGiftbtnNum=' +
                    '1'
            });
        },

        /**
         * 显示抢福袋兑换结果弹窗
         */
        showRedEnvelopeGrabGiftExchange: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabGiftExchangeTip: true
            });
        },

        /**
         * 关闭抢福袋兑换结果弹窗
         */
        redEnvelopeGrabGiftExchangeClose: function () {
            var that = this;
            that.setData({
                showRedEnvelopeGrabGiftExchangeTip: false
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
         * 跳转公会币记录
         */
        gotoRedEnvelopeRecord: function () {
            var that = this;
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                that.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var url = '/pages/subpages_v2/pages/red_envelope_record/red_envelope_record?user_id=' + globalData.idolUserInfo._id;
            console.log(getCurrentPages().length);
            if (getCurrentPages() && getCurrentPages().length > 1) {
                uni.navigateBack({
                    delta: 1
                });
            } else {
                uni.navigateTo({
                    url: url
                });
            }
        },

        /**
         * 跳转发公会币
         */
        gotoRedEnvelopeAdd: function () {
            var that = this;
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                that.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var url = '/pages/subpages_v2/pages/red_envelope_add/red_envelope_add';
            uni.redirectTo({
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
                that.initGid();
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

        /**
         * 初始化页面
         */
        initPage: function () {
            var that = this;
            that.getUserInfoDetail(that.user_id);
            that.requestOpp();
            that.requestOfficialGroup();
            that.requestPayStates();
            that.requestOwnSupportCoin();
            that.getLunbo();
            that.grabRedEnvelopeLotteryList();
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
                        that.initRedEnvelopeStatus();
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

        // 获取is_opp
        requestOpp: function () {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_opp';
            const params = {};
            const that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.ok == 1) {
                        that.setData({
                            is_opp: true
                        });
                    }
                },
                function () {}
            );
        },

        // 判断是否官方公会
        requestOfficialGroup: function () {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_view_history';
            const params = {};
            const that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.official_group == 1) {
                        that.setData({
                            is_official_group: true
                        });
                    }
                },
                function () {}
            );
        },

        // 获取pay_state
        requestPayStates: function () {
            var value = 6;
            var url = 'https://' + (value > 5 ? 'sw.id' : 'he.ido') + 'ol001.com/ope' + (9 > value ? 'n_manu_w' : '') + 'xapp' + '.php';
            var that = this;
            var params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getpayStates resp==' + JSON.stringify(resp));
                    if (resp && resp.data && !resp.data.error) {
                        that.setData({
                            pay_state: resp.data.pay
                        });
                    }
                },
                function () {}
            );
        },

        // 获取公会币数量
        requestOwnSupportCoin: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_moblie_idol.php?action=get_user_diamond_prop';
            var params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.diamond >= 0) {
                        that.setData({
                            ownSupportCoinCount: resp.data.diamond
                        });
                    }
                },
                function () {}
            );
        },

        /**
         * 读福袋状态
         */
        initRedEnvelopeStatus: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_red_packet.php?action=read_red_packet';
            var params = {
                red_packet_id: that.red_packet_id
            };
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    console.log('读福袋状态 initRedEnvelopeStatus resp.data==', resp.data);
                    console.log('读福袋状态 initRedEnvelopeStatus typeof resp.data.all_coin==', typeof resp.data.all_coin);
                    console.log('读福袋状态 initRedEnvelopeStatus typeof resp.data.all_count==', typeof resp.data.all_count);
                    console.log('读福袋状态 initRedEnvelopeStatus typeof.has_get_coin=', typeof resp.data.has_get_coin);
                    console.log('读福袋状态 initRedEnvelopeStatus typeof resp.data.has_get_count==', typeof resp.data.has_get_count);
                    console.log('读福袋状态 initRedEnvelopeStatus typeof resp.data.has_end==', typeof resp.data.has_end);
                    console.log('读福袋状态 initRedEnvelopeStatus typeof resp.data.has_get==', typeof resp.data.has_get);
                    if (typeof resp.data.all_coin == 'string') {
                        resp.data.all_coin = parseInt(resp.data.all_coin);
                    }
                    if (typeof resp.data.all_count == 'string') {
                        resp.data.all_count = parseInt(resp.data.all_count);
                    }
                    if (typeof resp.data.has_get_coin == 'string') {
                        resp.data.has_get_coin = parseInt(resp.data.has_get_coin);
                    }
                    if (typeof resp.data.has_get_count == 'string') {
                        resp.data.has_get_count = parseInt(resp.data.has_get_count);
                    }
                    if (typeof resp.data.has_end == 'string') {
                        resp.data.has_end = parseInt(resp.data.has_end);
                    }
                    if (typeof resp.data.has_get == 'string') {
                        resp.data.has_get = parseInt(resp.data.has_get);
                    }
                    that.setData({
                        redPacketDetail: resp.data,
                        hasInit: true
                    });
                    if (that.isFromRecord && that.isFromRecord == 1) {
                        console.log('读福袋状态 initRedEnvelopeStatus isFromRecord==' + that.isFromRecord);
                    } else {
                        console.log('读福袋状态 initRedEnvelopeStatus isFromRecord==' + that.isFromRecord + '->checkStatus');
                        that.checkStatus();
                    }
                    if ((resp.data && resp.data.has_get && resp.data.has_get == 1) || (resp.data && resp.data.has_end && resp.data.has_end == 1)) {
                        console.log('读福袋状态 requestRedEnvelopeList');
                        that.requestRedEnvelopeList(1);
                    }
                },
                function (resp) {
                    console.log('读福袋状态 initRedEnvelopeStatus resp.data===', JSON.stringify(resp.data));
                    that.showError(resp);
                }
            );
        },

        checkStatus: function () {
            var that = this;
            console.log('checkStatus==');
            /**
             * 已领取
             */
            if (that.isLoginStatus && that.redPacketDetail && that.redPacketDetail.has_get == 1) {
                console.log('checkStatus 已领取');
            } else if (that.isLoginStatus && that.redPacketDetail && that.redPacketDetail.has_get_count == that.redPacketDetail.all_count) {
                console.log('checkStatus 已领完');
                /**
                 * 已领完
                 */
                uni.showToast({
                    title: '抢福袋失败',
                    icon: 'none',
                    duration: 2000
                });
            } else if (that.isLoginStatus && that.redPacketDetail && that.redPacketDetail.has_get_count < that.redPacketDetail.all_count && that.redPacketDetail.has_end == 1) {
                console.log('checkStatus 已过期');
                /**
                 * 已过期
                 */
                uni.showToast({
                    title: '抢福袋失败',
                    icon: 'none',
                    duration: 2000
                });
            }
        },

        /**
         * 获取福袋的获取人员列表
         */
        requestRedEnvelopeList: function (currentPage) {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_red_packet.php?action=read_red_packet_coin_list';
            var params = {
                red_packet_id: that.red_packet_id,
                page: currentPage,
                count: that.count
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('获取福袋的获取人员列表 requestRedEnvelopeList resp.data==', resp.data);
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
                            page: params.page,
                            allcount: resp.data && resp.data.allcount ? resp.data.allcount : 0,
                            allListOri: tempArr
                        });
                        that.setData(
                            {
                                isRequestAll: list && list.length < that.count ? true : false,
                                isLoading: false,
                                isPreload: false
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
                                isPreload: false
                            });
                        }
                    }
                },
                function (resp) {
                    console.log('获取福袋的获取人员列表 requestRedEnvelopeList resp.data===', JSON.stringify(resp.data));
                    if (params.page == 1) {
                        uni.stopPullDownRefresh();
                        that.setData(
                            {
                                allList: [],
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
         * 放弃某福袋对用户产生的抽奖奖品兑换资格
         */
        requestRedEnvelopeAbandoned: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_red_packet.php?action=give_up_red_packet_gift';
            var params = {
                red_packet_id: that.red_packet_id
            };
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    console.log('放弃某福袋对用户产生的抽奖奖品兑换资格 initRedEnvelopeStatus resp.data==', resp.data);
                },
                function (resp) {
                    console.log('放弃某福袋对用户产生的抽奖奖品兑换资格 initRedEnvelopeStatus resp.data===', JSON.stringify(resp.data));
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
        },

        /**
         * 获取福袋头图与活动剩余时间
         */
        getLunbo: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_red_packet.php?action=red_packet_active_top';
            var params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('获取福袋头图与活动剩余时间 getLunbo resp.data==', resp.data);
                    that.setData({
                        swiperDetail: resp.data,
                        redEnvelopeTipRules: resp.data.info_desc
                    });
                },
                function (resp) {
                    console.log('获取福袋头图与活动剩余时间 getLunbo resp.data===', JSON.stringify(resp.data));
                }
            );
        },

        /**
         * 获取某次福袋的有抽到兑换资格并已兑换人员列表
         */
        grabRedEnvelopeLotteryList: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_red_packet.php?action=read_red_packet_lottery_list';
            var params = {
                red_packet_id: that.red_packet_id
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('获取某次福袋的有抽到兑换资格并已兑换人员列表 grabRedEnvelopeLotteryList==', resp.data);
                    var lotteryStr = '';
                    if (resp && resp.data && resp.data.list && resp.data.list.length > 0) {
                        if (resp.data.list.length > 3) {
                            var indexup = 3;
                        } else {
                            var indexup = resp.data.list.length;
                        }
                        for (let index = 0; index < indexup; index++) {
                            const lottery = resp.data.list[index];
                            lotteryStr = lotteryStr + lottery.nickname;
                            if (index < indexup - 1) {
                                lotteryStr = lotteryStr + '、';
                            }
                        }
                    }
                    that.setData({
                        redPacketLotteryList: resp.data.list,
                        redPacketLotteryListStr: lotteryStr
                    });
                    console.log('获取某次福袋的有抽到兑换资格并已兑换人员列表 redPacketLotteryListStr==', lotteryStr);
                },
                function (resp) {
                    console.log('获取某次福袋的有抽到兑换资格并已兑换人员列表 grabRedEnvelopeLotteryList error==', resp.data);
                    that.setData({
                        redPacketLotteryListStr: ''
                    });
                }
            );
        },

        /**
         * 获取福袋
         */
        grabRedEnvelope: function () {
            var that = this;
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                that.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var url = 'https://data.idol001.com/api_mobile_red_packet.php?action=gain_red_packet';
            var params = {
                gid: that.WeChatGroupGid,
                red_packet_id: that.red_packet_id
            };
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    console.log('获取福袋 grabRedEnvelope==', resp.data);
                    if (resp && resp.data && resp.data.error_code == 12010) {
                        that.setData({
                            isShowErrorDialog: true,
                            showDialogErrorMsg: resp.data.error_description
                        });
                        return;
                    }
                    that.setData({
                        redPacketGrabDetail: resp.data
                    });
                    that.initPage();
                    if (resp.data.has_get && resp.data.has_get == 1) {
                        if (that.swiperDetail && that.swiperDetail.time > 0) {
                            if (resp.data.user_get && resp.data.user_get.gift) {
                                that.showRedEnvelopeGrabGiftSuccess();
                            } else {
                                // oThis.showRedEnvelopeGrabGiftFail();
                                uni.showToast({
                                    title: '成功抢到' + resp.data.user_get.coin + '公会币',
                                    icon: 'none',
                                    duration: 2000
                                });
                            }
                        } else {
                            if (resp.data.user_get && resp.data.user_get.gift) {
                                that.showRedEnvelopeGrabGiftSuccess();
                            } else {
                                uni.showToast({
                                    title: '成功抢到' + resp.data.user_get.coin + '公会币',
                                    icon: 'none',
                                    duration: 2000
                                });
                            }
                        }
                    }
                },
                function (resp) {
                    console.log('获取福袋 grabRedEnvelope error==', resp.data);
                }
            );
        },

        /**
         * 兑换奖励
         */
        requestBuyRedEnvelopeDress: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_red_packet.php?action=buy_red_packet_gift';
            var params = {
                red_packet_id: that.red_packet_id
            };
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    console.log('兑换奖励 requestBuyRedEnvelopeDress==', resp.data);
                    if (resp.data.error_code && resp.data.error_code > 0) {
                        if (resp.data.error_code == 10206) {
                            that.showDiamondNotEnough();
                        } else {
                            if (resp.data.error_description) {
                                uni.showToast({
                                    title: resp.data.error_description,
                                    icon: 'none',
                                    duration: 2000
                                });
                            }
                        }
                    } else {
                        that.setData({
                            redPacketGrabDressDetail: resp.data
                        });
                        if (resp && resp.data && resp.data.ok == 1) {
                            that.showRedEnvelopeGrabGiftExchange();
                        }
                    }
                },
                function (resp) {
                    console.log('兑换奖励 requestBuyRedEnvelopeDress error==', resp.data);
                }
            );
        },

        errorDialogClose: function () {
            this.setData({
                isShowErrorDialog: false
            });
        },

        webLogin() {
            console.log('占位：函数 webLogin 未声明');
        }
    }
};
</script>
<style>
@import './red_envelope_grab.css';
</style>
