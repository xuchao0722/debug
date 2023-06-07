<template>
    <view style="height: 100%">
        <!-- pages/subpages/component/game_prop_modal/gift_prop_modal.wxml -->

        <view class="dialog" v-if="compData.dialog_gift.show">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <image
                    class="dialog-close"
                    @tap="dialog_close"
                    data-name="dialog_gift"
                    src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"
                />
                <view class="dialog-main">
                    <scroll-view class="scroll-view" scroll-y>
                        <view class="gift-box">
                            <view class="discount" v-if="compData.dialog_gift.data.discount !== 1">{{ compData.dialog_gift.data.discount * 10 }}折</view>
                            <image class="img" :src="compData.dialog_gift.data.img" />
                            <view class="info">
                                <view class="title t-over">{{ compData.dialog_gift.data.name }}</view>
                                <view class="desc t-over3">{{ compData.dialog_gift.data.long_desc }}</view>
                            </view>
                        </view>
                        <view class="sub-title2">选择数量</view>
                        <view class="price-box">
                            <view
                                :class="'item ' + (index == compData.dialog_gift.select_index ? 'on' : '')"
                                :data-item="item"
                                :data-index="index"
                                @tap="priceTap"
                                v-for="(item, index) in compData.dialog_gift.data.list"
                                :key="index"
                            >
                                <view class="title">{{ item.num_str }}</view>

                                <view class="price">
                                    <image class="pay_icon_coin" src="/static/pages/subpages/images/listSupport/supportCoin.png" />
                                    {{ tools.priceFormatter(item.now_coin) }}
                                    <text class="origin-coin" v-if="compData.dialog_gift.data.discount !== 1">{{ tools.priceFormatter(item.origin_coin) }}</text>
                                </view>
                            </view>
                        </view>
                        <view class="input-box" v-if="false">
                            <view class="label">自定义兑换数量</view>
                            <input
                                class="input"
                                type="number"
                                placeholder="输入数量"
                                @input="buyInput"
                                :value="compData.dialog_gift.select_index !== null ? '' : compData.dialog_gift.buy_num"
                            />
                        </view>
                        <view class="input-box" v-if="compData.dialog_gift.vote_times > 0">
                            <view class="label">赠送投票券</view>
                            <input
                                class="input"
                                type="text"
                                placeholder="输入投票券"
                                :value="(compData.dialog_gift.vote_times > 0 ? compData.dialog_gift.vote_times : 0) + '张'"
                                disabled
                            />
                        </view>
                        <view class="coupon_view">
                            <view class="coupon_title">使用优惠券</view>
                            <view class="coupon_tip">已自动选择最优惠方案</view>
                            <view v-if="!couponObj._id" class="coupon_tip_right">无可用优惠劵</view>
                            <view v-else class="coupon_choose_view">
                                <view class="coupon_choose_count">-{{ couponObj.deduction_fee }}</view>
                                <image class="coupon_choose_icon_coin" src="/static/pages/subpages/images/listSupport/supportCoin.png" />
                                <view class="coupon_choose_tip">{{ couponObj.title }}</view>
                            </view>
                        </view>
                        <view class="pay_view">
                            <view class="pay_count">{{ compData.dialog_gift.total_coin }}</view>
                            <image class="pay_icon_coin" src="/static/pages/subpages/images/listSupport/supportCoin.png" />
                            <view class="pay_tip">实际支付：</view>
                        </view>
                        <view class="user-btn" style="margin-bottom: 20rpx" @tap="buy">立即兑换</view>
                        <view class="txt2 t-center">兑换后可在“道具-我的背包”中查看</view>
                    </scroll-view>
                </view>
            </view>
        </view>
        <view class="dialog" v-if="compData.dialog_gift_success.show">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <image
                    class="dialog-close"
                    @tap="dialog_close"
                    data-name="dialog_gift_success"
                    src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"
                />
                <view class="dialog-main">
                    <!-- <image class="act-title" style="width: 108rpx;height: 113rpx;" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/2/20/t4wey3YG5K1613813135677.png"/> -->
                    <scroll-view class="scroll-view" scroll-y>
                        <view class="sub-title" style="color: #0a0a0a">兑换成功</view>
                        <view class="hint" v-if="compData.dialog_gift_success.gifts.length">投票券只在本次活动中赠送，活动结束清零</view>
                        <view class="gift-list">
                            <view class="item">
                                <image class="img" :src="compData.dialog_gift.data.img" />
                                <view class="name">{{ compData.dialog_gift.data.name }}</view>
                                <view class="num">
                                    {{ compData.dialog_gift.buy_num }}
                                    {{ compData.dialog_gift.data.measure_word }}
                                </view>
                                <view class="user-btn" @tap="sendPrize">去赠送礼物</view>
                            </view>
                            <view class="item" v-if="compData.dialog_gift_success.gifts.length">
                                <image class="img" :src="/static/pages/subpages/component/game_prop_modal/compData.dialog_gift_success.gifts[0].icon" />
                                <view class="name">{{ compData.dialog_gift_success.gifts[0].name }}</view>
                                <view class="num">
                                    {{ compData.dialog_gift_success.gifts[0].gift_count }}
                                    {{ compData.dialog_gift_success.gifts[0].measure_word }}
                                </view>
                                <view class="user-btn" @tap="sendVoteTime">去赠送投票券</view>
                            </view>
                        </view>
                        <view class="txt1 t-center t-line" @tap="showMyGiftProp">查看我的礼物数量</view>
                    </scroll-view>
                </view>
            </view>
        </view>
        <!-- <view class="dialog" wx:if="{{compData.dialog_gift_success.show}}">
  <view class="dialog-cover"></view>
  <view class="dialog-view">
    <image class="dialog-close" bindtap="dialog_close" data-name="dialog_gift_success" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"/>
    <view class="dialog-main" style="margin-top:20rpx;">
      <image class="act-title" style="width: 108rpx;height: 113rpx;" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/3/12/nxc83QxAK51615537286473.png"/>
      <scroll-view class="scroll-view" scroll-y>
        <view class="sub-title" style="color:#0A0A0A">兑换成功</view>
        <view class="prize-text" wx:if="{{compData.dialog_gift_success.gifts.length}}">{{compData.dialog_gift_success.gifts[0].name}} x {{compData.dialog_gift_success.gifts[0].gift_count}}{{compData.dialog_gift_success.gifts[0].measure_word}}</view>
        <view class="user-btn" bindtap="sendPrize">去赠送礼物</view>
        <view class="txt1 t-center" bindtap="showMyGiftProp">查看我的礼物数量</view>
      </scroll-view>
    </view>
  </view>
</view> -->
        <protectModal
            v-if="compData.showProtectActivityModal"
            :bJumpFrom="bJumpFrom"
            :prop_obj="compData.prop_obj"
            :onactivity="compData.prop_obj.onactivity"
            @joinProtectActivity="joinProtectActivity"
            @closeProtectClick="closeProtectActivity"
        ></protectModal>
        <user_coin_add_fail
            v-if="compData.showRedEnvelopeAddFail"
            :red_envelope_title="compData.envelopeAddFailTitle"
            :pay_state="compData.pay_state"
            :is_opp="compData.is_opp"
            @addFailClose="addFailClose"
            @addFailGenerate="addFailGenerate"
            @addFailGainCoin="addFailGainCoin"
            :red_envelope_coin="compData.envelopeCoininput"
            :user_coin="compData.ownSupportCoinCount"
        ></user_coin_add_fail>
        <idol_tip_dialog v-if="compData.dialog_tip.show" title="送礼物功能还未开放，敬请期待\n去送投票券给您喜欢的吧" btnText="知道了" @sure="giftDialogSure"></idol_tip_dialog>
    </view>
</template>
<script module="tools" lang="wxs" src="@/pages/subpages/component/game_prop_modal/buy_prop_wxs.wxs"></script>
<script>
import protectModal from './game_prop_modal_protect';
import idol_tip_dialog from '../../../../component/idol_tip_dialog/idol_tip_dialog';
import user_coin_add_fail from '../../../../component/user_coin_add_fail/user_coin_add_fail';
// pages/subpages/component/game_prop_modal/gift_prop_modal.js
const JumpManager = require('../../../../commonscript/common/JumpManager.js');
var net = require('../../../../commonscript/common/netLoad.js');
export default {
    components: {
        protectModal,
        idol_tip_dialog,
        user_coin_add_fail
    },
    data() {
        return {
            //优惠券
            couponObj: {
                _id: '',
                deduction_fee: '',
                title: ''
            },

            bJumpFrom: ''
        };
    },
    /**
     * 组件的属性列表
     */
    props: {
        compData: {
            type: Object,
            default: () => ({})
        }
    },
    /**
     * 组件的方法列表
     */
    methods: {
        ready() {
            var oThis = this;
            let { dialog_gift } = oThis.getPrePage().data.gift_prop_modal;
            oThis.requestOfficialGroup();
        },

        dialog_close(e) {
            let { name } = e.currentTarget.dataset;
            this.getPrePage().setData({
                [`gift_prop_modal.${name}.show`]: false
            });
            this.compData.dialog_gift.select_index = 0;
        },

        diamondNotEnoughGotoCharge: function () {
            var oThis = this.getPrePage().data.gift_prop_modal;
            this.addFailClose();
            /**
             * 判断支付跳转
             */
            JumpManager.junmpToPayment();
        },

        // 判断是否官方公会
        requestOfficialGroup: function () {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_view_history';
            const params = {};
            const that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.official_group == 1) {
                        that.getPrePage().setData({
                            [`gift_prop_modal.is_official_group`]: true
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
                        that.getPrePage().setData({
                            [`gift_prop_modal.is_opp`]: true
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
                        that.getPrePage().setData({
                            [`gift_prop_modal.pay_state`]: resp.data.pay
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
                        that.getPrePage().setData({
                            [`gift_prop_modal.ownSupportCoinCount`]: resp.data.diamond
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
            that.getPrePage().setData({
                [`gift_prop_modal.showRedEnvelopeAddFail`]: false
            });
        },

        addFailGainCoin: function () {
            var that = this;
            that.diamondNotEnoughGotoCharge();
        },

        priceTap(e) {
            var oThis = this;
            let { item, index } = e.currentTarget.dataset;
            let { dialog_gift } = oThis.getPrePage().data.gift_prop_modal;
            oThis.setPriceTap(
                index,
                dialog_gift.data.origin_coin * dialog_gift.data.discount,
                dialog_gift.data.list[index].num,
                dialog_gift.data.list[index].origin_coin / dialog_gift.data.coin_vote_num_radio
            );
            oThis.getCouponMsgFun(dialog_gift.data.type);
        },

        setPriceTap(index, now_coin, buy_num, vote_times) {
            var oThis = this;
            oThis.getPrePage().setData({
                [`gift_prop_modal.dialog_gift.select_index`]: index,
                [`gift_prop_modal.dialog_gift.buy_num`]: buy_num,
                [`gift_prop_modal.dialog_gift.vote_times`]: vote_times,
                [`gift_prop_modal.dialog_gift.total_coin`]: Math.floor(now_coin * buy_num)
            });
        },

        buyInput(e) {
            var oThis = this;
            let { value } = e.detail;
            let { dialog_gift } = oThis.getPrePage().data.gift_prop_modal;
            oThis.getPrePage().setData({
                [`gift_prop_modal.dialog_gift.buy_num`]: value
            });
            oThis.setPriceTap(null, dialog_gift.data.origin_coin * dialog_gift.data.discount, value, (value * dialog_gift.data.origin_coin) / dialog_gift.data.coin_vote_num_radio);
        },

        buy(e) {
            // console.log("兑换成功")
            var oThis = this;
            let { dialog_gift } = oThis.getPrePage().data.gift_prop_modal;
            if (!dialog_gift.buy_num || dialog_gift.buy_num == 0) {
                uni.showToast({
                    title: '请输入兑换数量',
                    //提示的内容,
                    icon: 'none',
                    duration: 2000,
                    //延迟时间,
                    mask: true,
                    //显示透明蒙层，防止触摸穿透,
                    success: (res) => {}
                });
                return;
            }
            const url = 'https://data.idol001.com/api_guard_angle.php?action=buy_gift_tool';
            const params = {
                giftid: dialog_gift.data.giftid,
                num: dialog_gift.buy_num,
                coupon_id: this.couponObj._id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    if (resp.data.error_code == 10206) {
                        oThis.getPrePage().setData({
                            [`gift_prop_modal.dialog_gift.show`]: false,
                            [`gift_prop_modal.envelopeCoininput`]: dialog_gift.total_coin,
                            [`gift_prop_modal.showRedEnvelopeAddFail`]: true
                        });
                        return;
                    }
                    if (resp.data.ok == 1) {
                        oThis.getPrePage().setData({
                            [`gift_prop_modal.dialog_gift.show`]: false,
                            [`gift_prop_modal.dialog_gift_success.show`]: true,
                            [`gift_prop_modal.dialog_gift_success.gifts`]: resp.data.gifts
                        });
                        oThis.getPrePage().requestOwnSupportCoin();
                    } else {
                        uni.showToast({
                            title: resp.data.msg,
                            //提示的内容,
                            icon: 'none',
                            duration: 2000,
                            //延迟时间,
                            mask: true,
                            //显示透明蒙层，防止触摸穿透,
                            success: (res) => {}
                        });
                    }
                } else {
                }
            });
        },

        sendPrize() {
            var oThis = this;
            // oThis.getPrePage().setData({
            //   // [`gift_prop_modal.dialog_gift_success.show`]:false,
            //   [`gift_prop_modal.dialog_tip.show`]:true
            // })
            uni.navigateTo({
                url: '/subpage_v3/pages/bosom_friend/my_bosom_friend'
            });
        },

        sendVoteTime() {
            var oThis = this;
            oThis.getPrePage().setData({
                [`gift_prop_modal.dialog_gift_success.show`]: false
            });
            uni.navigateTo({
                url: '/subpage_v3/pages/act_vote_search/act_vote_search'
            });
        },

        showMyGiftProp() {
            var oThis = this;
            oThis.getPrePage().setData({
                [`gift_prop_modal.dialog_gift_success.show`]: false
            });
            uni.navigateTo({
                url: '/pages/subpages/pages/game_prop/my_prop_parcel'
            });
        },

        giftDialogSure() {
            var oThis = this;
            oThis.getPrePage().setData({
                [`gift_prop_modal.dialog_tip.show`]: false
            });
        },

        joinProtectActivity: function () {
            this.closeProtectActivity();
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/juneActivity/juneActivity'
            });
        },

        closeProtectActivity: function () {
            var that = this;
            that.getPrePage().setData({
                [`gift_prop_modal.showProtectActivityModal`]: false
            });
            this.$emit('closeGamePropModal');
        },

        getPrePage() {
            let pages = getCurrentPages();
            return pages[pages.length - 1];
        },

        //获取最佳优惠券
        getCouponMsgFun(target_id) {
            var oThis = this;
            var url = 'https://data.idol001.com/api_idol_7years.php?action=get_one_coupons';
            var params = {
                level: this.compData.dialog_gift.select_index + 1,
                target_id: target_id
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        oThis.setData({
                            couponObj: resp.data
                        });
                        oThis.getPrePage().setData({
                            [`gift_prop_modal.dialog_gift.total_coin`]: oThis.compData.dialog_gift.total_coin - resp.data.deduction_fee
                        });
                    } else {
                        oThis.setData({
                            couponObj: {}
                        });
                    }
                    //this.initDisCountLimit()
                },
                function (resp) {
                    oThis.setData({
                        couponObj: {}
                    });
                }
            );
        },

        addFailGenerate() {
            console.log('占位：函数 addFailGenerate 未声明');
        }
    },
    mounted() {
        // 处理小程序 ready 生命周期
        this.$nextTick(() => this.ready());
    },
    created: function () {},
    watch: {
        compData: {
            handler: function (newVal, oldVal) {},
            immediate: true,
            deep: true
        }
    }
};
</script>
<style>
@import './gift_prop_modal.css';
</style>
