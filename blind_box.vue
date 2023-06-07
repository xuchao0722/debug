<template>
    <view style="height: 100%">
        <!-- pages/subpages_v2/component/blind_box/blind_box.wxml -->
        <!-- 盲盒菜单 -->
        <view v-if="false" class="blind_box">
            <view class="tabs">
                <view :class="'item ' + (tabIndex == 0 ? 'on' : '')" @tap="tabChange" data-index="0">我的盲盒</view>
                <view :class="'item ' + (tabIndex == 1 ? 'on' : '')" @tap="tabChange" data-index="1">兑换盲盒</view>
            </view>
            <view class="panel">
                <view class="panel_item">
                    <image class="box_img" mode="widthFix" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/26/bKYFbNzZ7h1616726484551.png" />
                    <view class="box_text">我的盲盒： {{ user_mystery_boxes_info.mystery_boxes_num }}个盲盒</view>
                </view>
                <view class="panel_item" v-if="tabIndex == 0">
                    <view class="user-btn" @tap="openBlindBox">开启盲盒</view>
                    <view class="user-btn" @tap="exchangeBlindBox">兑换盲盒</view>
                    <view class="btn-group">
                        <view class="btn" @tap="record">盲盒开启记录</view>
                        <view class="btn" @tap="intro">盲盒介绍</view>
                    </view>
                </view>
                <view class="panel_item" v-if="tabIndex == 1">
                    <view class="exchange_title">天使装扮盲盒</view>
                    <view class="exchange_coin_text">{{ user_mystery_boxes_info.one_mystery_boxes_coin }}公会币/个</view>
                    <view class="num_box">
                        <image class="btn" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/27/hDBXMk3hjR1616832483109.png" @tap="boxNumCut" />
                        <input class="input" type="number" :value="box_num" @input="boxNumInput" />
                        <image class="btn" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/27/bNX4BYZ62y1616832520692.png" @tap="boxNumplus" />
                    </view>
                    <view class="user-btn" @tap="exchangeBlindBoxStart">立即兑换</view>
                </view>
            </view>
        </view>

        <view class="dialog_main_view" :style="'height: ' + (isAct ? '460' : '420') + 'rpx;'">
            <view class="dialog_main_top">
                <view class="dialog_main_top_left" @tap="intro">盲盒介绍</view>
                <view class="dialog_main_top_right" @tap="record">盲盒开启记录</view>
            </view>
            <view v-if="isAct" class="act_item_view">
                <view class="act_item_tip">{{ actDetail.box_end_desc }}</view>
            </view>
            <scroll-view class="scroll_view" :scroll-x="true">
                <view style="margin: 0rpx 15rpx; display: flex; flex-direction: row; align-items: center">
                    <view class="scroll_view_item" :data-item="item" @tap="openBlindBox" v-for="(item, index) in boxList" :key="index">
                        <view class="scroll_view_item_inner">
                            <image class="scroll_item_icon" :src="item.icon"></image>
                            <view class="scroll_item_title">{{ item.title }}</view>
                            <view class="scroll_item_text">{{ item.sub_title }}</view>
                            <view class="scroll_view_price_view" :style="item.is_open ? 'opacity:1' : 'opacity:0.4'">
                                <image class="scroll_view_price_icon" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/5/22/FtH5jM7xys1621680690675.png"></image>
                                <view class="scroll_view_price_text">{{ item.price }}</view>
                            </view>
                        </view>

                        <view class="scroll_item_count">
                            当前拥有：
                            <text style="color: #ff6d '00'">{{ item.my_box_num }}</text>
                            个
                        </view>
                    </view>
                </view>
            </scroll-view>
        </view>

        <!-- 盲盒确定兑换弹窗 -->
        <view class="dialog" v-if="dialog_exchange_confirm.show">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <view class="dialog-main" style="padding-top: 30rpx">
                    <scroll-view class="scroll-view" scroll-y>
                        <view class="sub-title" style="color: #0a0a0a">确定兑换盲盒？</view>
                        <view class="sub-hint">{{ dialog_exchange_confirm.data.gifts[1] ? '当前您将要兑换盲盒和活动期间赠送投票券如下' : '当前您将要兑换盲盒如下' }}</view>
                        <view class="gift-list">
                            <view class="item">
                                <image
                                    class="img"
                                    style="width: 157rpx; height: 118rpx"
                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/26/bKYFbNzZ7h1616726484551.png"
                                />
                                <view class="num">{{ box_num }}个</view>
                                <view class="name">天使装扮盲盒</view>
                            </view>
                            <view v-if="dialog_exchange_confirm.data.gifts[1]" class="item">
                                <image class="img" mode="aspectFit" :src="/static/pages/subpages_v2/component/blind_box/dialog_exchange_confirm.data.gifts[1].icon" />
                                <view class="num">{{ dialog_exchange_confirm.data.gifts[1].gift_count }}{{ dialog_exchange_confirm.data.gifts[1].measure_word }}</view>
                                <view class="name">{{ dialog_exchange_confirm.data.gifts[1].desc }}</view>
                            </view>
                        </view>
                        <view class="need-coin-text">消耗：{{ user_mystery_boxes_info.one_mystery_boxes_coin * box_num }}公会币</view>
                        <view class="btn-group">
                            <view class="user-btn s2" style="margin-right: 20rpx" @tap="closeBlindBoxExchange">取消兑换</view>
                            <view class="user-btn" @tap="confirmBlindBoxExchange">确定兑换</view>
                        </view>
                    </scroll-view>
                </view>
            </view>
        </view>

        <!-- 成功兑换X个盲盒弹窗 -->
        <!-- <view class="dialog" wx:if="{{dialog_shop_success.show}}">
  <view class="dialog-cover"></view>
  <view class="dialog-view">
    <image class="dialog-close" bindtap="dialog_close" data-name="dialog_exchange" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"/>
    <view class="dialog-main" style="padding-top: 30rpx;">
      <scroll-view class="scroll-view" scroll-y>
        <view class="sub-title" style="color:#0A0A0A">成功兑换x个盲盒</view>
        <view class="hint2">您当前拥有N个盲盒</view>
        <view class="text" style="margin-top:48rpx;">继续开启N个盲盒</view>
        <view class="btn-group">
          <view class="user-btn" style="margin-right:20rpx;" bindtap="closeBlindBoxExchange">立即开启</view>
        </view>
      </scroll-view>
    </view>
  </view>
</view> -->

        <!-- 盲盒开启弹窗 -->
        <view class="dialog" v-if="dialog_open.show">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <image
                    class="dialog-close"
                    @tap="dialog_close"
                    data-name="dialog_open"
                    src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"
                />
                <view class="dialog-main" style="padding-top: 30rpx">
                    <scroll-view class="scroll-view t-center" scroll-y>
                        <view class="sub-title" style="color: #0a0a0a">选择您想开的{{ selectItem.title }}数量</view>
                        <view class="hint">我的{{ selectItem.title }}：{{ selectItem.my_box_num }}个盲盒</view>
                        <view class="btn-group blind_box_open">
                            <view class="user-btn" @tap="openBlindBoxStart" data-num="1">开1个</view>
                            <view class="user-btn" @tap="openBlindBoxStart" data-num="10">开10个</view>
                            <view class="user-btn" @tap="openBlindBoxStart" data-num="50">开50个</view>
                            <view class="user-btn" @tap="openBlindBoxStart" data-num="100">开100个</view>
                        </view>
                        <!-- <view class="user-btn blind_box_start_all_btn" bindtap="openBlindBoxAll">开启全部盲盒</view> -->
                        <block v-if="selectItem.box_type == 0">
                            <view class="blind_box_open_nowday_text t-center">
                                今日还可开启{{ user_mystery_boxes_info.max_mystery_boxes_use_per_day_limit - user_mystery_boxes_info.today_use_mystery_boxes_count }}个普通盲盒
                            </view>
                            <view class="txt1 t-center">每日限制开启盲盒数量为{{ user_mystery_boxes_info.max_mystery_boxes_use_per_day_limit }}个</view>
                        </block>
                        <view v-else class="txt1 t-center" style="margin-top: 30rpx; margin-bottom: 20rpx">限定装扮盲盒不限开启数量</view>
                    </scroll-view>
                </view>
            </view>
        </view>

        <!-- 确定开启全部盲盒弹窗 -->
        <view class="dialog" v-if="dialog_open_all_confirm.show">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <view class="dialog-main" style="padding-top: 30rpx">
                    <scroll-view class="scroll-view" scroll-y>
                        <view class="sub-title" style="color: #0a0a0a">确定开启您的全部盲盒吗？</view>
                        <view class="gift-list"></view>
                        <view class="btn-group">
                            <view class="user-btn" style="margin-right: 20rpx" @tap="closeBlindBoxOpen">我再想想</view>
                            <view class="user-btn" @tap="openBlindBoxStart" :data-num="selectItem.my_box_num">确定开启</view>
                        </view>
                    </scroll-view>
                </view>
            </view>
        </view>

        <!-- 今日开启盲盒数量限制弹窗 -->
        <view class="dialog" v-if="dialog_open_nowday_tips.show">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <view class="dialog-main" style="padding-top: 30rpx">
                    <scroll-view class="scroll-view" scroll-y>
                        <view class="sub-title" style="color: #0a0a0a">{{ dialog_open_nowday_tips.data.msg }}</view>
                        <view class="blind_box_open_nowday_text t-center" v-if="dialog_open_nowday_tips.data.ok == 7">
                            今日还可开启{{ user_mystery_boxes_info.max_mystery_boxes_use_per_day_limit - user_mystery_boxes_info.today_use_mystery_boxes_count }}个盲盒
                        </view>
                        <view style="font-size: 26rpx; margin: 20rpx 0">
                            每人每日开启天使装扮盲盒数量为{{ user_mystery_boxes_info.max_mystery_boxes_use_per_day_limit }}个，您今日已达到上限，明日再来开启吧~
                        </view>
                        <view class="btn-group">
                            <view
                                v-if="dialog_open_nowday_tips.data.ok == 7"
                                class="user-btn"
                                @tap="openBlindBoxStart"
                                :data-num="user_mystery_boxes_info.max_mystery_boxes_use_per_day_limit - user_mystery_boxes_info.today_use_mystery_boxes_count"
                            >
                                继续开启{{ user_mystery_boxes_info.max_mystery_boxes_use_per_day_limit - user_mystery_boxes_info.today_use_mystery_boxes_count }}个盲盒
                            </view>
                            <view v-else class="user-btn" @tap="closeBlindOpenNowdayTips">我知道了</view>
                        </view>
                        <view class="txt1 t-pink t-center" @tap="closeBlindOpenNowdayTips" v-if="dialog_open_nowday_tips.data.ok == 7">明日再开</view>
                    </scroll-view>
                </view>
            </view>
        </view>

        <!-- 盲盒开启数量不足弹窗 -->
        <view class="dialog" v-if="dialog_open_not_enough.show">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <image
                    class="dialog-close"
                    @tap="dialog_close"
                    data-name="dialog_open_not_enough"
                    src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"
                />
                <view class="dialog-main" style="padding-top: 30rpx">
                    <scroll-view class="scroll-view" scroll-y>
                        <view class="sub-title" style="color: #0a0a0a">{{ selectItem.title }}数量不足</view>
                        <view class="hint2">
                            您当前拥有{{ selectItem.my_box_num }}个{{ selectItem.title }}，还差{{ dialog_open_not_enough.box_num - selectItem.my_box_num }}个{{
                                selectItem.title
                            }}，是否需要兑换？
                        </view>
                        <view class="gift-list">
                            <view class="item">
                                <image class="img" style="width: 180rpx; height: 180rpx" :src="selectItem.icon" />
                                <view class="name">兑换{{ dialog_open_not_enough.box_num - selectItem.my_box_num }}个{{ selectItem.title }}</view>
                                <view class="coin_text">{{ 50 * (dialog_open_not_enough.box_num - selectItem.my_box_num) }}公会币</view>
                            </view>
                        </view>
                        <view class="btn-group">
                            <view class="user-btn s2" style="margin-right: 20rpx" @tap="closeBlindBoxNotEnough">下次再买</view>
                            <view
                                class="user-btn"
                                @tap="confirmBlindBoxExchange"
                                :data-box_num="dialog_open_not_enough.box_num - selectItem.my_box_num"
                                :data-need_num="dialog_open_not_enough.box_num"
                            >
                                立即兑换
                            </view>
                        </view>
                    </scroll-view>
                </view>
            </view>
        </view>

        <!-- 盲盒兑换弹窗 -->
        <view class="dialog" v-if="dialog_exchange.show">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <image
                    class="dialog-close"
                    @tap="dialog_close"
                    data-name="dialog_exchange"
                    src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"
                />
                <view class="dialog-main" style="padding-top: 30rpx">
                    <image
                        class="act-title"
                        style="width: 108rpx; height: 113rpx"
                        src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/27/WRNHbGZrmn1616837580301.png"
                    />
                    <scroll-view class="scroll-view" scroll-y>
                        <view class="sub-title" style="color: #0a0a0a">兑换成功</view>
                        <!-- <view class="hint">投票券只在本次活动中赠送，活动结束清零</view> -->
                        <view class="gift-list">
                            <view class="item">
                                <image
                                    class="img"
                                    style="width: 157rpx; height: 118rpx"
                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/26/bKYFbNzZ7h1616726484551.png"
                                />
                                <view class="num">{{ dialog_exchange.data.gifts[0].gift_count }}个</view>
                                <view class="name">天使装扮盲盒</view>
                                <view class="user-btn" @tap="openBlindBoxStart" v-if="dialog_exchange.fast_exchange" :data-num="dialog_exchange.need_num">
                                    {{ '继续开启' + dialog_exchange.need_num + '个盲盒' }}
                                </view>
                                <view class="user-btn" @tap="openBlindBox" v-else>去开启盲盒</view>
                            </view>
                            <view v-if="dialog_exchange.data.gifts[1]" class="item">
                                <image class="img" mode="aspectFit" :src="/static/pages/subpages_v2/component/blind_box/dialog_exchange.data.gifts[1].icon" />
                                <view class="num">{{ dialog_exchange.data.gifts[1].gift_count }}{{ dialog_exchange.data.gifts[1].measure_word }}</view>
                                <view class="name">{{ dialog_exchange.data.gifts[1].desc }}</view>
                                <view :class="'user-btn ' + (dialog_exchange.data.gifts[1].can_use_now == 1 ? '' : 'disabled')" @tap="sendQuanClick">
                                    {{ dialog_exchange.data.gifts[1].can_use_now == 1 ? '去赠送' + dialog_exchange.data.gifts[1].name : '未到赠送时间' }}
                                </view>
                            </view>
                        </view>
                        <view class="txt1 t-center t-line">您当前拥有{{ user_mystery_boxes_info.mystery_boxes_num }}个盲盒</view>
                    </scroll-view>
                </view>
            </view>
        </view>

        <view v-if="bShowBuySuccess" class="dialog_main_end_view">
            <view class="dialog_main_end_view_inner">
                <image class="dialog-close" @tap="closeBuySuccessClick" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png" />
                <view class="buy_success_text1">成功兑换{{ dialog_exchange.data.gifts[0].gift_count }}个{{ selectItem.title }}</view>
                <view class="buy_success_text2">您当前拥有{{ selectItem.my_box_num }}个{{ selectItem.title }}</view>
                <view class="buy_success_text3">继续开启{{ dialog_exchange.need_num }}个{{ selectItem.title }}</view>
                <view class="buy_success_btn" @tap="openBlindBoxStart" :data-num="dialog_exchange.need_num">立即开启</view>
            </view>
        </view>

        <!-- 获得天使装扮弹窗 -->
        <view class="dialog" v-if="dialog_get_spirit_clothing.show">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <image
                    class="dialog-close"
                    @tap="dialog_close"
                    data-name="dialog_get_spirit_clothing"
                    src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"
                />
                <view class="dialog-main" style="padding-top: 30rpx">
                    <image class="box-icon" :src="selectItem.icon" />
                    <scroll-view class="scroll-view" scroll-y>
                        <view class="sub-title" style="color: #0a0a0a">恭喜您获得天使装扮</view>
                        <view class="hint2" style="margin: 0 auto 18rpx">
                            本次开启{{ selectItem.title }}{{ dialog_get_spirit_clothing.data.use_mystery_boxes_num }}个，获得以下天使装扮，稍后发放到您账户上，请查收
                        </view>
                        <scroll-view class="prize-list" scroll-y>
                            <view class="item" v-for="(item, index) in dialog_get_spirit_clothing.data.list" :key="index">
                                <image class="img" :src="item.acc_info.cover" />

                                <view class="coin">
                                    <image class="icon" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/4/19/AiADRnineR1618815429572.png" />
                                    {{ item.acc_info.diamond }}
                                </view>

                                <view class="day">{{ item.acc_count + '天' }}</view>
                            </view>
                        </scroll-view>
                        <view class="btn-group">
                            <view class="user-btn" style="margin-right: 20rpx" @tap="closeDialogGetSpiritClothing">下次再开</view>
                            <view class="user-btn" @tap="closeDialogGetSpiritClothing">继续开盲盒</view>
                        </view>
                        <block v-if="selectItem.box_type == 0">
                            <view class="blind_box_open_nowday_text t-center">
                                今日还可开启{{ user_mystery_boxes_info.max_mystery_boxes_use_per_day_limit - user_mystery_boxes_info.today_use_mystery_boxes_count }}个{{
                                    selectItem.title
                                }}
                            </view>
                            <view class="txt1 t-center">每日限制开启{{ selectItem.title }}数量为{{ user_mystery_boxes_info.max_mystery_boxes_use_per_day_limit }}个</view>
                        </block>
                    </scroll-view>
                </view>
            </view>
        </view>

        <user_coin_add_fail
            v-if="showRedEnvelopeAddFail"
            :red_envelope_title="envelopeAddFailTitle"
            :pay_state="pay_state"
            :is_opp="is_opp"
            @addFailClose="addFailClose"
            @addFailGenerate="addFailGenerate"
            @addFailGainCoin="addFailGainCoin"
            :red_envelope_coin="envelopeCoininput"
            :user_coin="ownSupportCoinCount"
        ></user_coin_add_fail>
        <idol_tip_dialog
            v-if="dialog_no_open_tips.show"
            title="天使装扮盲盒开启功能还未开放，敬请期待\n去送投票券给您喜欢的吧"
            btnText="知道了"
            @sure="noOpenDialogSure"
        ></idol_tip_dialog>

        <view v-if="bShowActEndDialog" class="dialog_main_end_view">
            <view class="dialog_main_end_view_inner">
                <view class="dialog_main_end_tip">{{ selectItem.box_end_dialog_txt }}</view>
                <view class="dialog_main_end_btn" @tap="closeActEndDialog">我知道了</view>
            </view>
        </view>
    </view>
</template>

<script>
import idol_tip_dialog from '../../../../component/idol_tip_dialog/idol_tip_dialog';
import user_coin_add_fail from '../../../../component/user_coin_add_fail/user_coin_add_fail';
// pages/subpages_v2/component/blind_box/blind_box.js
const WXNotificationCenter = require('../../../../commonscript/common/WxNotificationCenter.js');
var net = require('../../../../commonscript/common/netLoad.js');
var IDLoginManager = require('../../../../commonscript/common/IDLoginManager.js');
var globalData = require('../../../../commonscript/common/globalData.js');
export default {
    components: {
        idol_tip_dialog,
        user_coin_add_fail
    },
    data() {
        return {
            tabIndex: 0,
            box_num: 1,
            act_time: {},

            user_mystery_boxes_info: {
                mystery_boxes_num: '',
                one_mystery_boxes_coin: '',
                max_mystery_boxes_use_per_day_limit: 0,
                today_use_mystery_boxes_count: 0
            },

            dialog_exchange: {
                show: false,
                box_num: 0,

                data: {
                    gifts: ''
                },

                fast_exchange: '',
                need_num: ''
            },

            dialog_exchange_confirm: {
                show: false,

                data: {
                    gifts: ''
                }
            },

            dialog_no_open_tips: {
                show: false
            },

            dialog_open: {
                show: false
            },

            dialog_open_not_enough: {
                show: false,
                box_num: 0
            },

            dialog_open_all_confirm: {
                show: false
            },

            dialog_shop_success: {
                show: false
            },

            dialog_get_spirit_clothing: {
                show: false,

                data: {
                    use_mystery_boxes_num: '',
                    list: []
                }
            },

            dialog_open_nowday_tips: {
                show: false,

                data: {
                    msg: '',
                    ok: 0
                }
            },

            envelopeAddFailTitle: '兑换道具失败',
            ownSupportCoinCount: '',
            showRedEnvelopeAddFail: false,

            //是否显示福袋分享失败弹窗
            envelopeCoininput: 0,

            //公会币不足的数量
            is_official_group: false,

            //是否官方公会
            pay_state: 0,

            //支付开关
            is_opp: false,

            selectItem: {
                title: '',
                my_box_num: '',
                box_type: 0,
                icon: '',
                box_end_dialog_txt: ''
            },

            boxList: [],
            bShowActEndDialog: false,
            bShowBuySuccess: false,
            isAct: false,

            actDetail: {
                box_end_desc: ''
            },

            gift_count: '',
            measure_word: '',
            desc: '',
            can_use_now: 0,
            name: ''
        };
    },
    /**
     * 组件的属性列表
     */
    props: {},
    /**
     * 组件的方法列表
     */
    methods: {
        ready() {
            var oThis = this;
            oThis.getBlindBoxData();
            oThis.requestOfficialGroup();
            oThis.getActTime();
            var url = 'https://data.idol001.com/api_mystery_boxes.php?action=get_limit_box_time';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    oThis.setData({
                        isAct: resp.data.open ? true : false,
                        actDetail: resp.data
                    });
                }
            });
        },

        getBoxList: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mystery_boxes.php?action=get_box_list';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    var item = that.selectItem;
                    if (item.title) {
                        for (var i = 0; i < resp.data.length; i++) {
                            if (item.box_type == resp.data[i].box_type) {
                                item = resp.data[i];
                            }
                        }
                    }
                    that.setData({
                        boxList: resp.data,
                        selectItem: item
                    });
                }
            });
        },

        closeActEndDialog: function () {
            this.setData({
                bShowActEndDialog: false
            });
        },

        closeBuySuccessClick: function () {
            this.setData({
                bShowBuySuccess: false
            });
        },

        // tab切换
        tabChange(e) {
            let { index } = e.currentTarget.dataset;
            this.setData({
                tabIndex: index
            });
        },

        // 盲盒开启记录
        record() {
            let { now_time, begin_time } = this.act_time;
            if (now_time < begin_time) {
                this.setData({
                    dialog_no_open_tips: {
                        show: true
                    }
                });
                return;
            }
            uni.navigateTo({
                url: '/subpage_v3/pages/blind_box/record_list'
            });
        },

        // 盲盒介绍
        intro() {
            uni.navigateTo({
                url: '/subpage_v3/pages/april_month_s/april_month_box'
            });
        },

        // 开启盲盒
        openBlindBox(e, box_type) {
            let { now_time, begin_time } = this.act_time;
            if (now_time < begin_time) {
                this.setData({
                    dialog_no_open_tips: {
                        show: true
                    }
                });
                return;
            }
            this.setData({
                selectItem:
                    e && e.currentTarget && e.currentTarget.dataset.item
                        ? e.currentTarget.dataset.item
                        : box_type
                        ? box_type
                        : this.selectItem.title
                        ? this.selectItem
                        : this.boxList[0],
                dialog_open: {
                    show: true
                },
                dialog_exchange: {
                    show: false
                }
            });
        },

        openBlindBoxStart(e) {
            var oThis = this;
            let { num } = e.currentTarget.dataset;
            if (num == 0) {
                uni.showToast({
                    title: '目前没有盲盒啦~',
                    //提示的内容,
                    icon: 'none'
                });
                return;
            }
            if (this.selectItem.box_type == 0) {
            } else {
            }
            const params = {
                num: num,
                box_type: this.selectItem.box_type
            };
            uni.showLoading({
                title: '加载中...',
                //提示的内容,
                mask: true //显示透明蒙层，防止触摸穿透,
            });

            net.fetchApi(url, params, this.selectItem.box_type == 0 ? 'POST' : 'GET').then(
                (resp) => {
                    uni.hideLoading();
                    uni.stopPullDownRefresh();
                    if (resp && resp.data) {
                        if (resp.data.ok == 1) {
                            console.log('兑换盲盒数量', num);
                            // 关闭确认开启全部盲盒弹窗
                            oThis.setData({
                                ['dialog_open_all_confirm.show']: false,
                                ['dialog_exchange.show']: false,
                                ['dialog_open_nowday_tips.show']: false,
                                bShowBuySuccess: false,
                                ['dialog_exchange.box_num']: num,
                                dialog_get_spirit_clothing: {
                                    show: true,
                                    data: resp.data
                                }
                            });
                            oThis.getBlindBoxData();
                        } else if (resp.data.ok == 7) {
                            oThis.setData({
                                dialog_open_nowday_tips: {
                                    show: true,
                                    data: resp.data
                                }
                            });
                        } else if (resp.data.ok == 8) {
                            oThis.setData({
                                dialog_open_nowday_tips: {
                                    show: true,
                                    data: resp.data
                                }
                            });
                        } else if (resp.data.ok == 9) {
                            if (oThis.selectItem.box_type != 0 && !resp.data.open) {
                                this.setData({
                                    bShowActEndDialog: true
                                });
                                return;
                            }
                            oThis.setData({
                                dialog_open_not_enough: {
                                    show: true,
                                    box_num: num
                                }
                            });
                        } else {
                            uni.showToast({
                                title: resp.data.msg || resp.data.error_description,
                                icon: 'none'
                            });
                        }
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
        },

        // 兑换盲盒
        exchangeBlindBox() {
            console.log('兑换盲盒');
            this.setData({
                tabIndex: 1
            });
        },

        // 取消兑换盲盒
        closeBlindBoxExchange() {
            this.setData({
                ['dialog_exchange_confirm.show']: false
            });
        },

        closeBlindBoxOpen() {
            this.setData({
                ['dialog_open_all_confirm.show']: false
            });
        },

        closeBlindBoxNotEnough() {
            this.setData({
                ['dialog_open_not_enough.show']: false
            });
        },

        openBlindBoxAll() {
            this.setData({
                ['dialog_open_all_confirm.show']: true
            });
        },

        closeBlindOpenNowdayTips() {
            this.setData({
                ['dialog_open_nowday_tips.show']: false
            });
        },

        closeDialogGetSpiritClothing() {
            this.setData({
                ['dialog_get_spirit_clothing.show']: false
            });
        },

        // 确认兑换盲盒
        confirmBlindBoxExchange(e) {
            if (parseInt(this.box_num) <= 0 || !this.box_num) {
                uni.showToast({
                    title: '请输入兑换数量',
                    icon: 'none'
                });
                return;
            }
            var oThis = this;
            let { box_num, need_num } = e.currentTarget.dataset;
            const url = 'https://data.idol001.com/api_mystery_boxes.php?action=buy_mystery_boxes';
            const params = {
                num: box_num || this.box_num,
                box_type: this.selectItem.box_type
            };
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    if (resp && resp.data) {
                        if (resp.data.ok == 1) {
                            this.setData({
                                dialog_exchange: {
                                    show: this.selectItem.box_type == 0 ? true : false,
                                    data: resp.data,
                                    fast_exchange: !!box_num,
                                    need_num
                                },
                                ['dialog_open_not_enough.show']: false,
                                ['dialog_exchange_confirm.show']: false,
                                bShowBuySuccess: this.selectItem.box_type != 0 ? true : false
                            });
                            oThis.getBlindBoxData();
                        } else if (resp.data.error_code == 10206) {
                            oThis.requestOwnSupportCoin();
                            oThis.setData({
                                [`showRedEnvelopeAddFail`]: true,
                                [`envelopeCoininput`]: oThis.selectItem.price * params.num
                            });
                        } else {
                            uni.showToast({
                                title: resp.data.msg || resp.data.error_description,
                                icon: 'none'
                            });
                        }
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
        },

        // 获取盲盒数据
        getBlindBoxData() {
            this.getBoxList();
            var oThis = this;
            const url = 'https://data.idol001.com/api_mystery_boxes.php?action=user_mystery_boxes_info';
            const params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        this.setData(
                            {
                                user_mystery_boxes_info: resp.data
                            },
                            function () {
                                WXNotificationCenter.postNotificationName('refreshBoxCount', oThis.user_mystery_boxes_info.mystery_boxes_num);
                            }
                        );
                    } else {
                        uni.showToast({
                            title: resp.data.msg || resp.data.error_description,
                            icon: 'none'
                        });
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
        },

        // 立即兑换盲盒
        exchangeBlindBoxStart() {
            if (parseInt(this.box_num) <= 0 || !this.box_num) {
                uni.showToast({
                    title: '请输入兑换数量',
                    icon: 'none'
                });
                return;
            }
            var oThis = this;
            const url = 'https://data.idol001.com/api_mystery_boxes.php?action=pre_exchange_mystery_boxes';
            const params = {
                num: this.box_num
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        if (resp.data.gifts) {
                            this.setData({
                                dialog_exchange_confirm: {
                                    show: true,
                                    data: resp.data
                                }
                            });
                            // oThis.getBlindBoxData();
                        } else {
                            uni.showToast({
                                title: resp.data.msg || resp.data.error_description,
                                icon: 'none'
                            });
                        }
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
        },

        getActTime() {
            var oThis = this;
            const url = 'https://data.idol001.com/api_idol_acc_game.php?action=get_time';
            const params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('getActTime', resp.data);
                oThis.setData({
                    act_time: resp.data
                });
            });
        },

        // 兑换盲盒数量(增加)
        boxNumplus() {
            let { box_num } = this;
            box_num = box_num ? box_num : 0;
            this.setData({
                box_num: parseInt(box_num) + 1
            });
        },

        // 兑换盲盒数量(减少)
        boxNumCut() {
            let { box_num } = this;
            box_num = box_num ? box_num : 1;
            this.setData({
                box_num: parseInt(box_num) > 1 ? parseInt(box_num) - 1 : 1
            });
        },

        boxNumInput(e) {
            console.log(e);
            let { value } = e.detail;
            var num = value.replace('.', '');
            num = parseInt(num) == 0 ? 1 : num;
            this.setData({
                box_num: num
            });
        },

        dialog_close(e) {
            let { name } = e.currentTarget.dataset;
            this.setData({
                [`${name}.show`]: false
            });
        },

        // 打开盲盒没有开启弹窗
        noOpenDialogSure() {
            this.setData({
                dialog_no_open_tips: {
                    show: false
                }
            });
        },

        diamondNotEnoughGotoCharge: function () {
            var that = this;
            uni.navigateTo({
                url: '/pages/subpages/pages/task_center/task_center'
            });
            this.addFailClose();
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
                            [`is_official_group`]: true
                        });
                    }
                    // 获取is_opp
                    that.requestOpp();
                },
                function () {
                    // 获取is_opp
                    that.requestOpp();
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
                            [`is_opp`]: true
                        });
                    }
                    // 获取pay_state
                    that.requestPayStates();
                },
                function () {
                    // 获取pay_state
                    that.requestPayStates();
                }
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
                            [`pay_state`]: resp.data.pay
                        });
                    }
                    // 获取公会币数量
                    that.requestOwnSupportCoin();
                },
                function () {
                    // 获取公会币数量
                    that.requestOwnSupportCoin();
                }
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
                            [`ownSupportCoinCount`]: resp.data.diamond
                        });
                    }
                    // 获取福袋头图与活动剩余时间
                    // oThis.getLunbo();
                },
                function () {
                    // 获取福袋头图与活动剩余时间
                    // oThis.getLunbo();
                }
            );
        },

        addFailClose: function () {
            var that = this;
            that.requestOwnSupportCoin();
            that.setData({
                [`showRedEnvelopeAddFail`]: false
            });
        },

        addFailGainCoin: function () {
            var that = this;
            that.diamondNotEnoughGotoCharge();
        },

        sendQuanClick: function () {
            if (this.dialog_exchange.data.gifts[1].can_use_now == 1) {
                uni.navigateTo({
                    url: '/subpage_v3/pages/act_vote_search/act_vote_search'
                });
            }
        },

        addFailGenerate() {
            console.log('占位：函数 addFailGenerate 未声明');
        }
    },
    mounted() {
        // 处理小程序 ready 生命周期
        this.$nextTick(() => this.ready());
    },
    created: function () {}
};
</script>
<style>
@import './blind_box.css';
</style>
