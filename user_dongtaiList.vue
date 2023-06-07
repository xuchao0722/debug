<template>
    <view style="height: 100%">
        <!-- pages/subpages/pages/persenal_mainPage/user_dongtaiList.wxml -->

        <template name="loading_holder">
            <view class="loading_holder_cell">
                <view class="loading_holder_cell_img"></view>
                <view class="loading_holder_cell_desc"></view>
            </view>
        </template>

        <view class="mainHomePageView">
            <image class="toMainPage" src="/static/pages/subpages/images/personalMainPage/push_mianPage.png" v-if="isShareType" @tap="pushMainPageClick">
                <view class="toMainPage_text">前往ta的主页</view>
            </image>

            <view class="contentView">
                <!-- 请求出错的处理 -->
                <view class="list_error_view_frame" :style="'display:' + (errorDic && errorDic.needShowErrorView ? 'block' : 'none') + ';'">
                    <error_view :error-dic="errorDic" @clickRetryEvent="retryGetData"></error_view>
                </view>
                <!-- cell -->
                <block v-for="(item, index) in dynamicItemLists" :key="index">
                    <!-- parse <template is="dynamicCellList" :data="item:dynamicItemLists[index],windowWidth:windowWidth,dynamicData:dynamicItemLists[index].data_quanzi,isDisplayTotalCount:true,cellIndex:index,angelLevel:userAngelModel.level,showFrom:0"></template> -->
                    <block name="dynamicCellList" v-if="false">
                        <view class="mainCellView" :style="'width:' + (windowWidth - 8 * 2) + 'px;'" @tap="dynamicCellItemClick" :data-cellindex="index">
                            <image
                                class="my_honor_bgImg"
                                :data-cellindex="index"
                                v-if="dynamicItemLists[index].data_quanzi.userinfo.honor"
                                @tap.stop.prevent="userhonorClick"
                                :src="dynamicItemLists[index].data_quanzi.userinfo.honor.background"
                            ></image>
                            <!-- 头部icon那些 -->
                            <view class="topIconView">
                                <image
                                    class="topIconImg"
                                    :src="dynamicItemLists[index].data_quanzi.userinfo.image.thumbnail_pic"
                                    @tap.stop.prevent="gotoHomePageAction"
                                    :hover-stop-propagation="true"
                                    :data-cellindex="index"
                                ></image>
                                <image
                                    class="topIconImg_honor"
                                    v-if="dynamicItemLists[index].data_quanzi.userinfo.honor && dynamicItemLists[index].data_quanzi.userinfo.honor.icon"
                                    :src="dynamicItemLists[index].data_quanzi.userinfo.honor.icon"
                                ></image>
                                <view class="middleView" :style="'width:' + (windowWidth - 8 * 4 - 48 - 20 - 8 * 2) + 'px;'">
                                    <view class="nickNameLayout">
                                        <text class="nickNameText" :style="'color:#' + (dynamicItemLists[index].data_quanzi.userinfo.is_vip == 1 ? 'ff872d' : '262626')">
                                            {{ dynamicItemLists[index].data_quanzi.userinfo.nickname }}
                                        </text>
                                        <image
                                            v-if="dynamicItemLists[index].data_quanzi.userinfo.is_vip == 1"
                                            class="nickNameText_vip_icon"
                                            src="https://star-img.xingxiaoculture.com/search/topics/images/2020/8/4/ecKCCpRzeR1596530582740.png"
                                        ></image>
                                        <image
                                            v-if="dynamicItemLists[index].data_quanzi.userinfo.medal"
                                            class="nickNameIcon"
                                            :src="dynamicItemLists[index].data_quanzi.userinfo.medal.img"
                                        ></image>
                                        <view class="level_text" v-if="dynamicItemLists[index].data_quanzi.angleinfo.level > 0">
                                            Lv.{{ dynamicItemLists[index].data_quanzi.angleinfo.level }}
                                        </view>
                                    </view>
                                    <view>
                                        <text class="timeText">{{ dynamicItemLists[index].data_quanzi.public_time }}</text>
                                        <text class="fromText" v-if="dynamicItemLists[index].data_quanzi.is_ai_xiao_dou == 0">
                                            <text class="fromTextPre" v-if="dynamicItemLists[index].data_quanzi.group_name">来自</text>
                                            {{ item.data_quanzi.group_name ? item.data_quanzi.group_name : '' }}
                                        </text>
                                    </view>
                                </view>
                                <image
                                    class="deleteImg"
                                    src="https://star-img.xingxiaoculture.com/2019/08/09/e7effbbda02eeff5d808a07f244c14d8.png"
                                    :style="dynamicItemLists[index].data_quanzi.userinfo.honor ? 'margin-top:14px;' : ''"
                                    :data-cellindex="index"
                                    @tap.stop.prevent="deleteDynamicCellClick"
                                    :hover-stop-propagation="true"
                                    v-if="dynamicItemLists[index].data_quanzi.isShowDeleteBtn"
                                ></image>
                            </view>
                            <!-- 内容区域 -->
                            <view class="contentView">
                                <!-- 文本描述 -->
                                <text class="detailTextView" :style="'width:' + (windowWidth - 8 * 3) + 'px;'">
                                    {{ dynamicItemLists[index].data_quanzi.text }}
                                </text>
                                <!-- 九宫格 -->
                                <view class="imgs_content" v-if="true">
                                    <block v-for="(dynamicItemLists[index],index) in (dynamicItemLists[index].data_quanzi.images)" :key="index">
                                        <view style="float: left; position: relative">
                                            <view class="single_img_right_default" v-if="dynamicItemLists[index].img_url.thumbnail_pic == '' && index <= 2"></view>
                                            <image
                                                :data-imgindex="index"
                                                :data-cellindex="index"
                                                class="single_img_right"
                                                :src="dynamicItemLists[index].img_url.thumbnail_pic"
                                                :hover-stop-propagation="true"
                                                @tap.stop.prevent="showTripImages"
                                                mode="aspectFill"
                                                :lazy-load="true"
                                                v-if="dynamicItemLists[index].img_url.thumbnail_pic && index <= 2"
                                            >
                                                <view class="rightCordiausView" v-if="true && index == 2 && dynamicItemLists[index].data_quanzi.images.length > 3">
                                                    <image
                                                        class="moreImgIcon"
                                                        src="https://star-img.xingxiaoculture.com/2019/08/09/976302dac7756ef5d02f5e7d8ea6d17c.png"
                                                        :lazy-load="true"
                                                    ></image>
                                                    <text class="morePhotoCountText">{{ dynamicItemLists[index].data_quanzi.images.length }}</text>
                                                </view>
                                            </image>
                                        </view>
                                    </block>
                                </view>
                                <view class="imgs_content" v-else>
                                    <block v-for="(dynamicItemLists[index],index) in (dynamicItemLists[index].data_quanzi.images)" :key="index">
                                        <image
                                            v-if="dynamicItemLists[index].img_url.thumbnail_pic"
                                            :input-data="index"
                                            :data-imgindex="index"
                                            class="single_img_right"
                                            :src="dynamicItemLists[index].img_url.thumbnail_pic"
                                            mode="aspectFill"
                                            :lazy-load="true"
                                            @tap="showTripImages"
                                        ></image>
                                    </block>
                                </view>
                                <!-- 底部点赞相关 -->
                                <view class="bottomDisplayView" v-if="!isHiddenBottomBtn">
                                    <view class="attitudeView" :data-cellindex="index" @tap.stop.prevent="attitudeDynamicCellClick" :hover-stop-propagation="true">
                                        <image
                                            class="bottomIconImg"
                                            :src="
                                                dynamicItemLists[index].data_quanzi.isattituded
                                                    ? 'https://star-img.xingxiaoculture.com/2019/08/09/2c710b2db76e2709b069160426d61da2.png'
                                                    : 'https://star-img.xingxiaoculture.com/2019/08/09/06a976507fd7533779f906b9f89fac6e.png'
                                            "
                                            :lazy-load="true"
                                        ></image>
                                        <text class="shareTitle" :style="'color: ' + (dynamicItemLists[index].data_quanzi.isattituded ? '#FF5981' : '#999999') + ';'">
                                            {{ dynamicItemLists[index].data_quanzi.attitude > 0 ? dynamicItemLists[index].data_quanzi.attitude : '点赞' }}
                                        </text>
                                    </view>
                                    <view class="shareOutsideView">
                                        <button @tap.stop.prevent="shareBtnClick" class="shareView" :hover-stop-propagation="true" open-type="share" :data-cellindex="index">
                                            <image
                                                class="shareView_icon"
                                                src="https://star-img.xingxiaoculture.com/2019/08/09/8a1efc8870b4fdbced7c2d66ae5e1adc.png"
                                                :lazy-load="true"
                                            ></image>
                                            分享
                                        </button>
                                    </view>
                                    <view class="commentView">
                                        <image
                                            class="bottomIconImg"
                                            src="https://star-img.xingxiaoculture.com/2019/08/09/a63aa40523ba8e8ebb7073bee69617d0.png"
                                            :lazy-load="true"
                                        ></image>
                                        <text class="shareTitle">
                                            {{ dynamicItemLists[index].data_quanzi.comment_num > 0 ? dynamicItemLists[index].data_quanzi.comment_num : '评论' }}
                                        </text>
                                    </view>
                                </view>
                            </view>
                        </view>
                    </block>
                </block>
                <view class="noDataView" v-if="dynamicItemLists.length == 0">
                    <image class="noDataImage" src="https://star-img.xingxiaoculture.com/2019/08/09/ff14edff58933b0e22cf147978e040a2.png"></image>
                    <text class="noDataText">{{ isMyself ? '快发布你的动态，吸引小伙伴们来围观吧~' : 'ta暂时还没有发布动态哦～' }}</text>
                </view>
                <view class="bottom_desc" :style="'margin-bottom:10px;margin-top: 10px;display:' + (!showBottomRefresh && dynamicItemLists.length > 0 ? 'block' : 'none') + ';'">
                    没有更多动态了~
                </view>
                <!-- 底部刷新 -->
                <view class="bottom_refresh" :style="'display:' + (showBottomRefresh ? 'block' : 'none') + ';'">
                    <refresh_view :is-animation="bottomAnimation"></refresh_view>
                </view>
            </view>

            <image
                class="bottom_publish_button"
                src="https://star-img.xingxiaoculture.com/2019/08/09/3df269598d405ff588c5b9b0c33ab62c.png"
                v-if="isMyself"
                @tap="gotoPublishPage"
            ></image>
        </view>

        <!-- 关注 取关 -->
        <!-- 删除 -->
        <idol_toastTip_view
            v-if="isDisplayToast"
            :tipType="tipType"
            :toastIcon="tipToastIcon"
            :toastTitle="tipToastTitle"
            :alertTitle="alertTitle"
            :alertLeftBtnTitle="alertLeftTitle"
            :alertLeftBtnTitleColor="alertLeftBtnColor"
            :alertRightBtnTitle="alertRightTitle"
            :alertRightBtnColor="alertRightBtnColor"
            @leftBtnClick="myAlertLeftBtnClick"
            @rightBtnClick="myAlertRightBtnClick"
        ></idol_toastTip_view>

        <!-- 删除后 -->
        <idol_toastTip_view
            v-if="isDisplayOnlyIKnowTip"
            tipType="alert"
            alertTitle="该条动态已被删除!"
            alertLeftBtnTitle="知道了"
            alertLeftBtnTitleColor="#262626"
            @oneBtnClick="myAlertLeftIKnowBtnClick"
        ></idol_toastTip_view>

        <login_panel v-if="bShowLoginPanel" @loginCallback="loginCallback" @closeViewEvent="closeLoginView"></login_panel>
    </view>
</template>

<script>
import error_view from '../../../../component/error_view/error_view';
import login_panel from '../../../../component/login_panel/login_panel';
import refresh_view from '../../../../component/refresh_view/refresh_view';
import idc_image from '../../../../component/idc_image/idc_image';
import share_panel from '../../../../component/share_panel/share_panel';
import idol_toastTip_view from '../../../../component/idol_toastTip_view/idol_toastTip_view';
// pages/subpages/pages/persenal_mainPage/user_dongtaiList.js
var net = require('../../../../commonscript/common/netLoad.js');
var IDLoginManager = require('../../../../commonscript/common/IDLoginManager.js');
var globalData = require('../../../../commonscript/common/globalData.js');
var wxNotificationCenter = require('../../../../commonscript/common/WxNotificationCenter.js');
var time = require('../../utils/util.js');
const app = getApp();
export default {
    components: {
        error_view,
        login_panel,
        refresh_view,
        idc_image,
        share_panel,
        idol_toastTip_view
    },
    data() {
        return {
            dynamicItemLists: [],
            userID: '',
            windowWidth: 0,
            windowHeight: 0,
            showBottomRefresh: false,
            bottomAnimation: true,
            myGroupID: '5c6f68783a1fd4bb618b4613',

            errorDic: {
                needShowErrorView: false,
                needShowRetryBtn: false,
                isNothing: false,
                isNoSupportData: false,
                isSomethingError: false,
                errorText: '没有数据哦，请点击重试'
            },

            userInfo: {},

            //传过来的用户信息
            user_info: globalData.idolUserInfo,

            //用户自己信息
            isShowBottomBtn: false,

            isDisplayToast: false,

            //toast alert 提示
            tipType: '',

            //提示类型
            tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/b368234f27e2635c0ca4a8247077c86c.png',

            // toast图片
            tipToastTitle: '删除成功',

            //toast 文案
            alertTitle: '',

            alertLeftTitle: '',
            alertLeftBtnColor: '',
            alertRightTitle: '',
            alertRightBtnColor: '',
            bShowLoginPanel: false,
            starIDStr: '',
            myCurrentDeleteDynamicCellIndex: 0,
            isDisplayOnlyIKnowTip: false,

            //我知道了 那种alert提示的控制,
            isLoginStatus: false,

            isShareType: false,
            isMyself: false,
            userAngelModel: '',
            isClickPublishRefresh: false,
            myPage: 1,

            //分页
            isloading: false,

            loginLogicStar: false,
            supportRankTotalList: [],

            data_quanzi: {
                userinfo: {
                    honor: {
                        background: '',
                        icon: ''
                    },

                    image: {
                        thumbnail_pic: ''
                    },

                    is_vip: 0,
                    nickname: '',

                    medal: {
                        img: ''
                    }
                },

                angleinfo: {
                    level: 0
                },

                public_time: '',
                is_ai_xiao_dou: 0,
                group_name: '',
                isShowDeleteBtn: '',
                text: '',
                images: [],
                isattituded: false,
                attitude: 0,
                comment_num: 0
            },

            img_url: {
                thumbnail_pic: ''
            },

            isHiddenBottomBtn: ''
        };
    },
    /**
     * 生命周期函数--监听页面加载
     */
    onLoad: function (options) {
        var that = this;
        IDLoginManager.stepLoginState();
        this.loginLogicStarFun();
        this.setData({
            windowWidth: uni.getSystemInfoSync().windowWidth,
            windowHeight: uni.getSystemInfoSync().windowHeight * (750 / uni.getSystemInfoSync().windowWidth),
            userID: options.userID,
            isShareType: options.isShareType,
            user_info: globalData.idolUserInfo
        });
        that.requestUserInfo();
        that.requestAngelDetail();
        uni.showShareMenu({
            withShareTicket: true
        });
    },
    /**
     * 页面相关事件处理函数--监听用户下拉动作
     */
    onPullDownRefresh: function () {
        this.requestDynamicDataList(1);
    },
    /**
     * 页面上拉触底事件的处理函数
     */
    onReachBottom: function () {
        console.log('onReachBottom', this.isloading, this.showBottomRefresh);
        var that = this;
        if (!this.isloading && this.showBottomRefresh) {
            this.setData(
                {
                    bottomAnimation: true
                },
                function () {
                    that.requestDynamicDataList(that.page + 1);
                }
            );
        }
    },
    /**
     * 生命周期函数--监听页面初次渲染完成
     */
    onReady: function () {},
    /**
     * 生命周期函数--监听页面显示
     */
    onShow: function () {
        if (this.isClickPublishRefresh) {
            this.requestDynamicDataList(1);
            this.setData({
                isClickPublishRefresh: false
            });
        }
    },
    /**
     * 生命周期函数--监听页面隐藏
     */
    onHide: function () {},
    /**
     * 生命周期函数--监听页面卸载
     */
    onUnload: function () {},
    /**
     * 用户点击右上角分享
     */
    onShareAppMessage: function (options) {
        var that = this;
        var path = '/pages/index/index?action=gopage&page=dynamic_page&userID=' + that.userID + '&isShareType=' + '1';
        var p = {
            title: that.userInfo.nickname + '的动态',
            path: path,
            imageUrl: 'https://star-img.xingxiaoculture.com/2019/05/24/39901b1c18c6bbe55fc2dcabe08b4a39.png'
        };
        if (that.isLoginStatus) {
            // 来自页面内的按钮的转发
            if (options.from == 'button') {
                var index = options.target.dataset.cellindex;
                var obj = that.dynamicItemLists[index].data_quanzi;
                p = {
                    title: that.user_info.nickname + '分享了新动态，快去看看吧~',
                    path: '/pages/index/index?action=gopage&page=user_feed_detail&qz_id=' + obj.qzid + '&message_id=' + obj._id + '&is_share=1' + '&getofficial=1',
                    imageUrl: 'https://star-img.xingxiaoculture.com/2023/02/21/0efb4d0b1604dfd4a7054df2e43f61d4.png'
                };
                if (obj.images.length > 0) {
                    var imgModel = obj.images[0];
                    p.imageUrl = imgModel.img_url.origin_pic;
                }
            }
        } else {
            // 来自页面内的按钮的转发
            if (options.from == 'button') {
                var index = options.target.dataset.cellindex;
                var obj = that.dynamicItemLists[index].data_quanzi;
                p = {
                    title: obj.text,
                    path: '/pages/index/index?action=gopage&page=user_feed_detail&qz_id=' + obj.qzid + '&message_id=' + obj._id + '&is_share=1' + '&getofficial=1',
                    imageUrl: 'https://star-img.xingxiaoculture.com/2023/02/21/0efb4d0b1604dfd4a7054df2e43f61d4.png'
                };
                if (obj.images.length > 0) {
                    var imgModel = obj.images[0];
                    p.imageUrl = imgModel.img_url.origin_pic;
                }
            }
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
        closeLoginView: function () {
            this.setData({
                bShowLoginPanel: false
            });
        },

        loginCallback: function (e) {
            var that = this;
            if (e.detail.success) {
                that.setData({
                    bShowLoginPanel: false,
                    user_info: globalData.idolUserInfo,
                    loginLogicStar: true
                });

                //更新登陆状态
                app.globalData.isShouldCheckInLoginStatus = true;

                //比较一下是不是自己的主页
                if (that.userInfo._id == globalData.idolUserInfo._id) {
                } else {
                }
                that.setData({
                    isMyself: isMe ? true : false
                });
                that.requestDynamicDataList(1);
            }
        },

        /**
         * 天使详情
         */
        requestAngelDetail: function () {
            var url = 'https://data.idol001.com/api_guard_angle.php?action=get_guard_angle';
            var that = this;
            net.fetchApi(
                url,
                {
                    user_id: that.userID
                },
                'GET'
            ).then(
                (resp) => {
                    if (resp.data) {
                        that.setData({
                            userAngelModel: resp.data
                        });
                    }
                },
                function () {}
            );
        },

        requestUserInfo: function () {
            var url = 'https://data.idol001.com/api_moblie_idol.php?action=get_userinfo_detail';
            var that = this;
            net.fetchApi(
                url,
                {
                    quanzhu: '1',
                    power: '1',
                    userid: that.userID
                },
                'GET'
            ).then(
                (resp) => {
                    if (resp.data && resp.data && resp.data._id && resp.data._id.length > 0) {
                        that.setData({
                            userInfo: resp.data
                        });

                        //比较一下是不是自己的主页
                        if (globalData.idolUserInfo._id == resp.data._id) {
                            let isMe = true;
                        } else {
                            let isMe = false;
                        }
                        this.setData({
                            isMyself: isMe ? true : false
                        });
                        this.requestDynamicDataList(1);
                    }
                },
                function () {}
            );
        },

        /**
         * 请求列表数据
         */
        requestDynamicDataList: function (page) {
            var url = 'https://data.idol001.com/api_moblie_idol_userdt.php?action=get_user_group_dongtai';
            var that = this;
            that.isloading = true;
            var params = {
                user_id: that.userID ? that.userID : '',
                page: page,
                count: '10'
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (page == 1) {
                        uni.stopPullDownRefresh(); //停止下拉刷新
                    }

                    if (resp && resp.data && resp.data.list) {
                        that.page = page;
                        var list = that.dynamicItemLists;
                        //处理resp.data.list的public_time
                        for (var i = 0; i < resp.data.list.length; i++) {
                            var obj = resp.data.list[i];
                            obj.data_quanzi.public_time = time.commentTimeHandle(obj.data_quanzi.public_time);
                            obj.data_quanzi.group_name = null;
                            //判断一下是否需要展示删除按钮
                            if (that.user_info._id == obj.data_quanzi.userinfo._id) {
                                obj.data_quanzi.isShowDeleteBtn = true;
                            }
                            resp.data.list[i] = obj;
                        }
                        if (page == 1) {
                            list = resp.data.list;
                        } else {
                            list = list.concat(resp.data.list);
                        }
                        var bShowBottomRefresh = false;
                        if (resp.data.list.length > 0 && resp.data.list.length == 10) {
                            bShowBottomRefresh = true;
                        }
                        if (page == 1) {
                            if (list.length > 0) {
                                that.setData(
                                    {
                                        dynamicItemLists: list,
                                        showBottomRefresh: bShowBottomRefresh,
                                        errorDic: {
                                            needShowErrorView: false
                                        }
                                    },
                                    function () {
                                        that.endRefresh();
                                    }
                                );
                            } else {
                                that.setData(
                                    {
                                        dynamicItemLists: [],
                                        showBottomRefresh: false,
                                        errorDic: {
                                            needShowErrorView: false,
                                            isNoFriendsJoinGroup: true,
                                            needShowRetryBtn: false,
                                            errorText: '暂无相关动态～'
                                        }
                                    },
                                    function () {
                                        that.endRefresh();
                                    }
                                );
                            }
                        } else {
                            that.setData(
                                {
                                    dynamicItemLists: list,
                                    showBottomRefresh: bShowBottomRefresh,
                                    errorDic: {
                                        needShowErrorView: false
                                    }
                                },
                                function () {
                                    that.endRefresh();
                                }
                            );
                        }
                    } else {
                        if (page == 1) {
                            that.setData(
                                {
                                    dynamicItemLists: [],
                                    supportRankTotalList: [],
                                    showBottomRefresh: false
                                },
                                function () {
                                    that.endRefresh();
                                }
                            );
                        } else {
                            that.endRefresh();
                        }
                    }
                },
                function () {
                    if (page == 1) {
                        uni.stopPullDownRefresh(); //停止下拉刷新
                        that.setData(
                            {
                                dynamicItemLists: [],
                                showBottomRefresh: false
                            },
                            function () {
                                that.endRefresh();
                            }
                        );
                    } else {
                        that.endRefresh();
                    }
                }
            );
        },

        /**
         * 结束刷新
         */
        endRefresh: function () {
            console.log('endrefresh');
            var that = this;
            this.setData(
                {
                    bottomAnimation: false
                },
                function () {
                    uni.stopPullDownRefresh();
                    that.isloading = false;
                }
            );
        },

        // 网络异常后 点击重试按钮
        retryGetData: function (e) {
            this.requestDynamicDataList(1);
        },

        /**
         * 获取登录状态
         */
        loginLogicStarFun: function () {
            var that = this;
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                IDLoginManager.checkIsLogin(function (v) {
                    if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
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

        /**
         * 动态cell点击
         */
        dynamicCellItemClick: function (e) {
            var that = this;
            var cellObj = that.dynamicItemLists[e.currentTarget.dataset.cellindex].data_quanzi;
            if (cellObj) {
                uni.navigateTo({
                    url: '../user_feed_detail/user_feed_detail?qz_id=' + cellObj.qzid + '&message_id=' + cellObj._id + '&bShowGroupName=0'
                });
            }
        },

        /**
         * 点赞的处理
         */
        attitudeDynamicCellClick: function (e) {
            var that = this;
            that.setData({
                myCurrentDeleteDynamicCellIndex: e.currentTarget.dataset.cellindex
            });
            var cellObj = that.dynamicItemLists[e.currentTarget.dataset.cellindex];
            if (cellObj) {
                that.requestAttitudeDynamic(!cellObj.data_quanzi.isattituded, cellObj.data_quanzi._id, e.currentTarget.dataset.cellindex);
            }
        },

        /**
         * 点赞的请求
         */
        requestAttitudeDynamic: function (attitude, objid, index) {
            var url = 'https://data.idol001.com/api_moblie_idol_userdt.php?action=commit_message_attitude';
            var that = this;
            var params = {
                attitude: attitude ? 1 : 0,
                objid: objid && objid.length > 0 ? objid : ''
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data && resp.data.error && resp.data.error_code == 10115) {
                        IDLoginManager.setLoginState(IDLoginManager.is_not_login);
                        that.setData({
                            bShowLoginPanel: true
                        });
                        return;
                    }
                    if (resp.data && resp.data && resp.data.ok == 1) {
                        var cellObj = that.dynamicItemLists[index];
                        var myData = that.dynamicItemLists;
                        var tipStr = '点赞成功';
                        if (attitude == 0) {
                            tipStr = '取消点赞成功';
                            cellObj.data_quanzi.isattituded = false;
                            cellObj.data_quanzi.attitude = parseInt(cellObj.data_quanzi.attitude) - 1;
                        } else {
                            cellObj.data_quanzi.isattituded = true;
                            cellObj.data_quanzi.attitude = parseInt(cellObj.data_quanzi.attitude) + 1;
                        }
                        uni.showToast({
                            title: tipStr,
                            duration: 2000
                        });
                        myData[index] = cellObj;
                        that.setData({
                            dynamicItemLists: myData
                        });
                    } else if (resp.data && resp.data && resp.data.error_code == 10120) {
                        that.setData({
                            isDisplayOnlyIKnowTip: true
                        });
                    } else {
                        uni.showToast({
                            title: (resp.data && resp.data.error_description) || '点赞失败',
                            icon: 'none',
                            duration: 2000
                        });
                    }
                },
                function () {}
            );
        },

        /**
         * 删除动态
         */
        deleteDynamicCellClick: function (e) {
            var that = this;
            that.setData({
                myCurrentDeleteDynamicCellIndex: e.currentTarget.dataset.cellindex,
                tipType: 'alert',
                isDisplayToast: true,
                alertTitle: '确定删除这条动态吗？',
                alertLeftTitle: '取消',
                alertLeftBtnColor: '#262626',
                alertRightTitle: '确定',
                alertRightBtnColor: '#262626'
            });
        },

        /**
         * 删除动态请求
         */
        requestDeleteDynamic: function (objid, index) {
            var url = 'https://data.idol001.com/api_moblie_idol_userdt.php?action=delete_message';
            var that = this;
            var params = {
                messageid: objid && objid.length > 0 ? objid : ''
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data && resp.data.error && resp.data.error_code == 10115) {
                        IDLoginManager.setLoginState(IDLoginManager.is_not_login);
                        that.setData({
                            bShowLoginPanel: true
                        });
                        return;
                    }
                    if (resp.data && resp.data && resp.data.ok == 1) {
                        //弹窗提示
                        that.setData({
                            tipType: 'toast',
                            isDisplayToast: true
                        });
                        setTimeout(function () {
                            that.setData({
                                isDisplayToast: false
                            }); //自定义方法，根据编辑需求
                        }, 2000);

                        //刷新列表
                        var dataArr = that.dynamicItemLists;
                        dataArr.splice(index, 1);
                        that.setData({
                            dynamicItemLists: dataArr
                        });
                        that.requestDynamicDataList(1);
                    } else if (resp.data && resp.data && resp.data.error_code == 10120) {
                        that.setData({
                            isDisplayOnlyIKnowTip: true
                        });
                    } else {
                    }
                },
                function () {}
            );
        },

        /**
         * 我知道了 按钮点击
         */
        myAlertLeftIKnowBtnClick: function () {
            var that = this;
            that.setData(
                {
                    isDisplayOnlyIKnowTip: false
                },
                function () {
                    //刷新列表
                    var dataArr = that.dynamicItemLists;
                    dataArr.splice(that.myCurrentDeleteDynamicCellIndex, 1);
                    that.setData({
                        dynamicItemLists: dataArr
                    });
                }
            );
        },

        /**
         * alert 左边按钮点击 取消 继续关注
         */
        myAlertLeftBtnClick: function () {
            var that = this;
            that.setData({
                isDisplayToast: false
            });
        },

        /**
         * alert 右边按钮点击 取消关注 或者删除
         */
        myAlertRightBtnClick: function () {
            var that = this;
            that.setData(
                {
                    isDisplayToast: false
                },
                function () {
                    //判断是取关 还是删除
                    if (that.alertTitle == '确定删除这条动态吗？') {
                        //删除
                        var cellObj = that.dynamicItemLists[that.myCurrentDeleteDynamicCellIndex];
                        if (cellObj) {
                            that.requestDeleteDynamic(cellObj.data_quanzi._id, that.myCurrentDeleteDynamicCellIndex);
                        }
                    } else {
                        //取关
                        this.requestCareOrUnCareUser(this.bShowCareView);
                    }
                }
            );
        },

        /**
         * 点击图片的方法
         */
        showTripImages: function (e) {
            var urls = [];
            var nowUrl = '';
            var i = 0;
            var cellObj = this.dynamicItemLists[e.currentTarget.dataset.cellindex];
            if (cellObj.data_quanzi.images.length > 0) {
                cellObj.data_quanzi.images.forEach(function (image) {
                    if (i == e.currentTarget.dataset.imgindex) {
                        nowUrl = image.img_url.origin_pic;
                    }
                    i++;
                    urls.push(image.img_url.origin_pic);
                });
                uni.previewImage({
                    current: nowUrl,
                    urls: urls
                });
            }
        },

        pushMainPageClick: function () {
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(this.userInfo)) + '&showBackHomeButton=' + '1'
            });
        },

        gotoHomePageAction: function () {
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(this.userInfo))
            });
        },

        shareBtnClick: function () {},

        gotoPublishPage: function () {
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            that.setData({
                isClickPublishRefresh: true
            });
            uni.navigateTo({
                url: '../../pages/group_spaceDynamic/publish_dynamicMessage?gid=' + (that.WeChatGroupGid && that.WeChatGroupGid.length > 0 ? that.WeChatGroupGid : '')
            });
        },

        userhonorClick() {
            console.log('占位：函数 userhonorClick 未声明');
        }
    }
};
</script>
<style>
@import './user_dongtaiList.css';
@import '@/templates/dynamicCellList/dynamicCellList.css';
</style>
