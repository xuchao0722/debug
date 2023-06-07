<template>
    <view class="bgView">
        <view class="innerView">
            <text class="title">抢到{{ coin_num }}个公会币</text>
            <text class="content">已存入我的公会币</text>
            <block v-if="discount < 10 && discount > 0">
                <text class="discount_desc">
                    获得
                    <text class="red">{{ discount }}</text>
                    折
                    <text class="red">限定套装</text>
                    兑换资格
                </text>
            </block>
            <block v-else>
                <text class="discount_desc">{{ desc }}</text>
            </block>
            <view class="seconed_choice" v-if="redPacketGrabDetail.user_get.gift.origin == 'drop_year'">
                <image
                    class="seconed_choice_img"
                    :src="/static/pages/subpages_v2/component/red_envelope_grab_gift_success/redPacketGrabDetail.user_get.gift.dress_img"
                    mode="aspectFill"
                ></image>
                <view class="seconed_choice_title">{{ redPacketGrabDetail.user_get.gift.name }}</view>
                <view style="font-weight: 600; font-size: 24rpx; width: 80%">{{ redPacketGrabDetail.user_get.gift.dress_img_desc }}</view>
            </view>
            <image class="discount_dress" v-else :src="dress_img" mode="aspectFill" :lazy-load="true"></image>
            <text class="discount_expiry_date" v-if="expiry_date">{{ expiry_date }}天有效期</text>
            <view class="flex_row flex_center" style="margin-top: 5rpx">
                <image class="coin_img" src="/static/images/common/coin.png" mode="aspectFill" :lazy-load="true" />
                <text class="coin_current">{{ coin_current }}</text>
                <text class="coin_ori">{{ coin_ori }}</text>
            </view>
            <view class="btn">
                <view class="btn_item">
                    <view class="btn_item_abandoned">
                        <view class="btn_enter" @tap="redEnvelopeGrabAbandoned">放弃兑换</view>
                    </view>
                </view>
                <view class="btn_item">
                    <view class="btn_item_exchange">
                        <view class="btn_enter" @tap="redEnvelopeGrabExchange">兑换</view>
                    </view>
                </view>
            </view>
            <view class="cover-img_bg flex_row flex_center">
                <image src="/static/images/common/coin.png" class="cover-img" :lazy-load="true" mode="aspectFill" />
            </view>
        </view>
    </view>
</template>

<script>
/**
 * 组件的对外属性，是属性名到属性设置的映射表，属性设置中可包含三个字段， type 表示属性类型、 value 表示属性初始值、 observer 表示属性值被更改时的响应函数
 */
export default {
    data() {
        return {
            windowWidth: 0,
            windowHeight: 0,
            isIpad: false
        };
    },
    /**
     * 组件的属性列表
     */
    props: {
        coin_num: {
            type: String,
            default: ''
        },
        discount: {
            type: String,
            default: ''
        },
        desc: {
            type: String,
            default: ''
        },
        dress_img: {
            type: String,
            default: ''
        },
        expiry_date: {
            type: String,
            default: ''
        },
        coin_current: {
            type: String,
            default: ''
        },
        coin_ori: {
            type: String,
            default: ''
        },
        redPacketGrabDetail: {
            type: Object,
            default: () => ({})
        }
    },
    mounted() {
        // 处理小程序 ready 生命周期
        this.$nextTick(() => this.ready());
    },
    /**
     * 组件的方法列表
     */
    methods: {
        ready: function () {
            console.log('red_envelope_grab_success refresh view ready ');
            var that = this;
            uni.getSystemInfo({
                success: function (res) {
                    console.log('red_envelope_grab_success ready res.windowWidth==' + res.windowWidth);
                    console.log('red_envelope_grab_success ready res.windowHeight==' + res.windowHeight);
                    let windowWidth = res.windowWidth * (750 / res.windowWidth);
                    let windowHeight = res.windowHeight * (750 / res.windowWidth);
                    console.log('red_envelope_grab_success ready rpx res.windowWidth==' + windowWidth);
                    console.log('red_envelope_grab_success ready rpx res.windowHeight==' + windowHeight);
                    that.setData({
                        windowWidth: windowWidth,
                        windowHeight: windowHeight
                    });
                    if (res.model.indexOf('iPad') > -1) {
                        console.log('red_envelope_grab_success ready iPad');
                        that.setData({
                            isIpad: true
                        });
                    }
                }
            });
        },

        redEnvelopeGrabAbandoned: function () {
            var that = this;
            that.$emit('redEnvelopeGrabAbandoned');
        },

        redEnvelopeGrabExchange: function () {
            var that = this;
            that.$emit('redEnvelopeGrabExchange');
        }
    },
    created: function () {}
};
</script>
<style>
@import './red_envelope_grab_gift_success.css';
</style>
