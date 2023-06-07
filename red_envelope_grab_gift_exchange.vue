<template>
    <view class="bgView">
        <view class="innerView">
            <image src="/static/images/common/big_close.png" class="close-img" @tap="redEnvelopeGrabKnownClick"></image>
            <text class="title" style="margin-top: 10rpx">兑换成功</text>
            <text class="content" style="margin-top: 10rpx" v-if="expiry_date">兑换使用成功有效期+{{ expiry_date }}天</text>
            <view class="seconed_choice" v-if="gift.user_get.gift.origin == 'drop_year'">
                <image class="seconed_choice_img" :src="/static/pages/subpages_v2/component/red_envelope_grab_gift_exchange/gift.user_get.gift.dress_img" mode="aspectFill"></image>
                <view class="seconed_choice_title">{{ gift.user_get.gift.name }}</view>
                <view style="font-weight: 600; font-size: 24rpx; width: 80%">{{ gift.user_get.gift.dress_img_desc }}</view>
            </view>
            <image class="discount_dress" v-else :src="dress_img" mode="aspectFill" :lazy-load="true"></image>
            <view class="bottom">
                <view class="btnconfirm" @tap="redEnvelopeGrabKnownClick">知道了</view>
            </view>
        </view>
    </view>
</template>

<script>
// component/red_envelope_grab_success/red_envelope_grab_success.js
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
        expiry_date: {
            type: String,
            default: ''
        },
        dress_img: {
            type: String,
            default: ''
        },
        gift: {
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
            console.log('red_envelope_grab_gift_exchange refresh view ready ');
            var that = this;
            uni.getSystemInfo({
                success: function (res) {
                    console.log('red_envelope_grab_gift_exchange ready res.windowWidth==' + res.windowWidth);
                    console.log('red_envelope_grab_gift_exchange ready res.windowHeight==' + res.windowHeight);
                    let windowWidth = res.windowWidth * (750 / res.windowWidth);
                    let windowHeight = res.windowHeight * (750 / res.windowWidth);
                    console.log('red_envelope_grab_gift_exchange ready rpx res.windowWidth==' + windowWidth);
                    console.log('red_envelope_grab_gift_exchange ready rpx res.windowHeight==' + windowHeight);
                    that.setData({
                        windowWidth: windowWidth,
                        windowHeight: windowHeight
                    });
                    if (res.model.indexOf('iPad') > -1) {
                        console.log('red_envelope_grab_gift_exchange ready iPad');
                        that.setData({
                            isIpad: true
                        });
                    }
                }
            });
        },

        redEnvelopeGrabKnownClick: function () {
            var that = this;
            that.$emit('redEnvelopeGrabKnownClick');
        }
    },
    created: function () {}
};
</script>
<style>
@import './red_envelope_grab_gift_exchange.css';
</style>
