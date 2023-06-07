<template>
    <view style="height: 100%">
        <!-- subpage_v3/pages/blind_box/record_list.wxml -->
        <view class="record_list" v-if="list.length > 0">
            <view class="item" @tap="detail" :data-id="item._id" v-for="(item, index) in list" :key="index">
                <view class="item_col">
                    <view class="name">开启了{{ item.use_mystery_boxes_num }}个{{ item.box_type.title }}</view>
                    <view class="time">{{ item.update_time }}</view>
                </view>

                <view class="item_col" style="margin-left: auto">
                    <view class="status" :style="item.has_send == 1 ? 'background: #ff6278;' : 'background: #ff6d'00';'">{{ item.has_send == 1 ? '已到账' : '发放中' }}</view>
                    <view class="btn">查看详情 ></view>
                </view>
            </view>
        </view>
        <view class="no-data" v-else>
            <view class="hint">您还没有开启过盲盒，快去开启吧~</view>
            <view class="user-btn" @tap="toOpenBlindBox">去开启盲盒吧~</view>
        </view>
    </view>
</template>

<script>
// subpage_v3/pages/blind_box/record_list.js
var net = require('../../../commonscript/common/netLoad.js');
var IDLoginManager = require('../../../commonscript/common/IDLoginManager.js');
var globalData = require('../../../commonscript/common/globalData.js');
const utils = require('../../../utils/util.js');
export default {
    data() {
        return {
            list: [],
            page: 1
        };
    }
    /**
     * 生命周期函数--监听页面加载
     */,
    onLoad: function (options) {
        this.getBlindBoxList();
    },
    /**
     * 生命周期函数--监听页面初次渲染完成
     */
    onReady: function () {},
    /**
     * 生命周期函数--监听页面显示
     */
    onShow: function () {},
    /**
     * 生命周期函数--监听页面隐藏
     */
    onHide: function () {},
    /**
     * 生命周期函数--监听页面卸载
     */
    onUnload: function () {},
    /**
     * 页面相关事件处理函数--监听用户下拉动作
     */
    onPullDownRefresh: function () {
        this.setData({
            list: [],
            page: 1
        });
        this.getBlindBoxList();
    },
    /**
     * 页面上拉触底事件的处理函数
     */
    onReachBottom: function () {
        let { page } = this;
        this.setData({
            page: page + 1
        });
        this.getBlindBoxList();
    },
    /**
     * 用户点击右上角分享
     */
    onShareAppMessage: function () {},
    methods: {
        getBlindBoxList: function () {
            var that = this;
            let { list, page } = this;
            const url = 'https://data.idol001.com/api_mystery_boxes.php?action=mystery_boxes_use_record_list';
            const params = {
                page,
                count: 50
            };
            uni.showLoading({
                title: '加载中...',
                //提示的内容,
                mask: true //显示透明蒙层，防止触摸穿透,
            });

            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    uni.hideLoading();
                    uni.stopPullDownRefresh();
                    if (resp && resp.data) {
                        if (resp.data) {
                            resp.data.forEach(function (item, index) {
                                item.update_time = utils.formatTimeTwo(item.update_time, 'Y.M.D h:m:s');
                            });
                            this.setData({
                                list: [...list, ...resp.data]
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

        detail(e) {
            let { id } = e.currentTarget.dataset;
            uni.navigateTo({
                url: `/subpage_v3/pages/blind_box/record_detail?id=${id}`
            });
        },

        toOpenBlindBox: function () {
            uni.navigateBack({
                delta: 1
            });
        }
    }
};
</script>
<style>
@import './record_list.css';
</style>
