<template>
    <view class="bg_view" @touchmove.stop.prevent="grayBgTouch">
        <view class="mainView">
            <block>
                <image src="https://star-img.xingxiaoculture.com/2020/08/12/fedee19b9901a5d18e66033ff49de080.png" class="title_img"></image>
                <text class="title" style="margin-top: 40rpx">在七夕活动中贡献{{ axdNoticeInfo.life }}生命力\n获得礼盒</text>
            </block>
            <image src="/static/images/common/big_close.png" class="close_img" @tap="closeAugustGiftClick"></image>
            <block>
                <image :src="/static/component/idol_sprite_guard_august_gift/axdNoticeInfo.giftbox[0].img" class="flower_img" style="margin-top: 30rpx"></image>
                <text class="flower" style="margin-top: 10rpx">{{ axdNoticeInfo.giftbox[0].content }}</text>
            </block>
            <view class="flex_center flex_row" style="margin-top: 30rpx">
                <view class="flex_center flex_column" style="width: 250rpx">
                    <image :src="/static/component/idol_sprite_guard_august_gift/axdNoticeInfo.giftbox[1].img" class="glove_img"></image>
                    <text class="glove" style="margin-top: 10rpx">{{ axdNoticeInfo.giftbox[1].content }}</text>
                </view>
                <view class="flex_center flex_column" style="width: 250rpx">
                    <image :src="/static/component/idol_sprite_guard_august_gift/axdNoticeInfo.giftbox[2].img" class="protect_img"></image>
                    <text class="protect" style="margin-top: 10rpx">{{ axdNoticeInfo.giftbox[2].content }}</text>
                </view>
            </view>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {};
    },
    /**
     * 组件的属性列表
     */
    props: {
        axdNoticeInfo: {
            type: Object,
            default: null
        }
    },
    mounted() {
        // 处理小程序 attached 生命周期
        this.attached();
    },
    /**
     * 组件的方法列表
     */
    methods: {
        /**
         * attached
         */
        attached: function () {},

        closeAugustGiftClick: function () {
            console.log('idol_sprite_guard_august_gift closeAugustGiftClick');
            var that = this;
            that.$emit('closeAugustGiftClick');
        },

        grayBgTouch() {
            console.log('占位：函数 grayBgTouch 未声明');
        }
    },
    created: function () {}
};
</script>
<style>
@import './idol_sprite_guard_august_gift.css';
</style>
