<template>
    <view style="height: 100%">
        <view v-if="isPreload">
            <!-- parse <template is="preload"/> -->
            <block name="preload" v-if="false">
                <view>
                    <view style="display: flex; flex-direction: space-around; justify-content: center; margin-top: 120rpx">
                        <view style="width: 100%">
                            <view style="height: 200rpx; padding: 30rpx" v-for="(item, index) in [1, 2]" :key="index">
                                <view style="height: 160rpx; width: 100%; background-color: #fafafa; border-radius: 20rpx; display: inline-block"></view>

                                <view style="vertical-align: top; display: inline-block; height: 100%; width: 100%">
                                    <view style="height: 20rpx; margin-right: 33rpx; margin-top: 15rpx; background-color: #fafafa; border-radius: 20rpx"></view>
                                </view>
                            </view>
                        </view>
                    </view>
                </view>
            </block>
        </view>
        <view v-if="!isPreload">
            <!-- <view wx:if="{{official_group ==0}}">
        <view class="netTipBg">
            <image class="netImg" src="../../images/groupSpace/commonTipError.png"></image>
            <text class="netTip">对不起，您的页面走丢了~</text>
        </view>
    </view> -->

            <view v-if="isshare == 1 && official_group == 1">
                <view
                    style="
                        display: flex;
                        flex-direction: row;
                        align-items: center;
                        justify-content: left;
                        padding-top: 10px;
                        padding-bottom: 10px;
                        background-color: #f5f5f5;
                        padding-left: 14px;
                    "
                    @tap="backHome"
                >
                    <image style="width: 8px; height: 10px" :lazy-load="true" src="/static/pages/subpages/images/common/left_arrow.png" />
                    <image style="width: 21px; height: 20px; margin-left: 4px" :lazy-load="true" src="/static/pages/subpages/images/groupSpace/home.png" />
                    <text style="color: #f7941c; font-size: 14px; text-align: center; margin-left: 2px">回到公会</text>
                </view>
            </view>
            <view v-if="isshare == 1 && official_group == 0 && messageStatus == 1">
                <view
                    style="
                        display: flex;
                        flex-direction: row;
                        align-items: center;
                        justify-content: left;
                        padding-top: 10px;
                        padding-bottom: 10px;
                        background-color: #f5f5f5;
                        padding-left: 14px;
                    "
                    @tap="backHome"
                >
                    <image style="width: 8px; height: 10px" :lazy-load="true" src="/static/pages/subpages/images/common/left_arrow.png" />
                    <image style="width: 21px; height: 20px; margin-left: 4px" :lazy-load="true" src="/static/pages/subpages/images/groupSpace/home.png" />
                    <text style="color: #f7941c; font-size: 14px; text-align: center; margin-left: 2px">回到ta的主页</text>
                </view>
            </view>
            <view v-if="isshare == 1 && official_group == 0 && messageStatus == 0">
                <view
                    style="
                        display: flex;
                        flex-direction: row;
                        align-items: center;
                        justify-content: left;
                        padding-top: 10px;
                        padding-bottom: 10px;
                        background-color: #f5f5f5;
                        padding-left: 14px;
                    "
                    @tap="backHome"
                >
                    <image style="width: 8px; height: 10px" :lazy-load="true" src="/static/pages/subpages/images/common/left_arrow.png" />
                    <image style="width: 21px; height: 20px; margin-left: 4px" :lazy-load="true" src="/static/pages/subpages/images/groupSpace/home.png" />
                    <text style="color: #f7941c; font-size: 14px; text-align: center; margin-left: 2px">回到首页</text>
                </view>
            </view>

            <view v-if="errorDic.needShowErrorView">
                <view v-if="errorDic.needShowDelErrorView">
                    <view class="netTipBg">
                        <image class="netImg" src="https://star-img.xingxiaoculture.com/2019/08/09/ff14edff58933b0e22cf147978e040a2.png"></image>
                        <text class="netTip">暂无内容~</text>
                    </view>
                </view>
                <view v-if="!errorDic.needShowDelErrorView">
                    <view class="netTipBg">
                        <image class="netImg" src="/static/pages/subpages/images/groupSpace/commonTipError.png"></image>
                        <text class="netTip">对不起，您的页面走丢了~</text>
                    </view>
                </view>
            </view>
            <view v-if="!errorDic.needShowErrorView" @touchstart="pageTouchStart" @touchend="pageTouchEnd" @touchcancel="pageTouchCancel" @touchmove="pageTouchMove">
                <view style="display: flex; flex-direction: column; flex-wrap: nowrap; width: 100%; background: #ffffff">
                    <view class="container">
                        <!-- parse <template is="dynamicCellList" :data="item:message_single,windowWidth:windowWidth,dynamicData:message_single.data_quanzi,isDisplayTotalCount:false,isHiddenBottomBtn:true,angelLevel:userAngelModel.level"></template> -->
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
                                    <view class="bottomDisplayView" v-if="!true">
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

                        <view style="display: flex; flex-direction: column; align-items: left; background-color: #ffffff; width: 100%">
                            <view style="display: flex; flex-direction: row; align-items: center; padding-bottom: 12rpx; background-color: #ffffff; margin-left: 10px">
                                <image
                                    style="width: 16px; height: 15px; white-space: nowrap; justify-content: left; text-align: left; flex-shrink: 0"
                                    :lazy-load="true"
                                    src="/static/pages/subpages/images/common/common_reply.png"
                                    mode="aspectFill"
                                />
                                <view
                                    style="
                                        height: 100%;
                                        color: #ff5981;
                                        font-size: 12px;
                                        text-align: center;
                                        white-space: nowrap;
                                        justify-content: left;
                                        align-items: left;
                                        flex-shrink: 0;
                                        margin-left: 2px;
                                    "
                                >
                                    评论{{ allcount }}
                                </view>
                            </view>
                            <view style="display: flex; flex-direction: row; align-items: center; background-color: #f5f5f5; width: 100%; height: 1rpx" />
                            <view style="display: flex; flex-direction: row; align-items: center; background-color: #ffffff; width: 100%; height: 18rpx" />
                        </view>

                        <view :style="'padding-left:10px;padding-right:10px;width:' + (windowWidth - 20) + 'px'" v-if="allList.length > 0">
                            <block :wx:item="item" v-for="(item, index) in allList" :key="item._id">
                                <view
                                    :data-commentid="item._id"
                                    :data-commentuserid="item.userinfo._id"
                                    :data-commentusername="item.userinfo.nickname"
                                    :data-recommentid="item._id"
                                    :data-recommentuserid="item.userinfo._id"
                                    :data-recommentusername="item.userinfo.nickname"
                                    @tap="updateCommentConfirm"
                                >
                                    <view :style="'display:flex;flex-direction:row;align-items: center;padding-bottom:40rpx;width:' + (windowWidth - 20) + 'px'" :data-id="item.id">
                                        <view :style="'display:flex;flex-direction:column;justify-content:center;width:' + (windowWidth - 20) + 'px'">
                                            <view :style="'display:flex;flex-direction:row;flex:1;justify-content:center;align-items: center;width:' + (windowWidth - 20) + 'px'">
                                                <view
                                                    style="
                                                        display: flex;
                                                        flex-direction: row;
                                                        flex: 1;
                                                        height: 100%;
                                                        color: #999999;
                                                        font-size: 10px;
                                                        text-align: center;
                                                        white-space: nowrap;
                                                        justify-content: left;
                                                        align-items: center;
                                                        flex-shrink: 0;
                                                        width: 70%;
                                                    "
                                                >
                                                    <image
                                                        style="width: 24px; height: 24px; border-radius: 50%"
                                                        :lazy-load="true"
                                                        :src="item.userinfo.image.thumbnail_pic"
                                                        :data-userinfo="item.userinfo"
                                                        @tap.stop.prevent="dynamicCellCommentItemClick"
                                                        :hover-stop-propagation="true"
                                                    />
                                                    <view style="margin-left: 5px; font-size: 11px; color: #666666">{{ myRankObject.userinfo.nickname }}</view>
                                                </view>
                                                <image
                                                    style="width: 26px; height: 26px; white-space: nowrap; justify-content: center; text-align: center; flex-shrink: 0"
                                                    :lazy-load="true"
                                                    src="/static/pages/subpages/images/common/common_del.png"
                                                    mode="aspectFill"
                                                    v-if="common.iscurrentUser(item.userinfo._id, uid)"
                                                    :data-commentid="item._id"
                                                    :data-commentuserid="item.userinfo._id"
                                                    :data-commentusername="item.userinfo.nickname"
                                                    :data-recommentid="item._id"
                                                    :data-recommentuserid="item.userinfo._id"
                                                    :data-recommentusername="item.userinfo.nickname"
                                                    @tap.stop.prevent="delCommentConfirm"
                                                    :hover-stop-propagation="true"
                                                />
                                            </view>

                                            <view style="display: flex; flex-direction: row; flex: 1; width: 100%; margin-top: 4px">
                                                <text style="display: flex; flex-direction: row" :decode="true" :space="true">&nbsp;&nbsp;&nbsp;&nbsp;</text>
                                                <view style="display: flex; flex-direction: row; width: 95%">
                                                    <view
                                                        style="
                                                            font-size: 13px;
                                                            flex-grow: 1;
                                                            color: #262626;
                                                            flex-shrink: 0;
                                                            display: flex;
                                                            overflow: hidden;
                                                            justify-content: left;
                                                            width: 92%;
                                                        "
                                                    >
                                                        {{ item.text }}{{ item.images != null && item.images.length > 0 ? '[图片]' : '' }}
                                                    </view>
                                                </view>
                                            </view>

                                            <view style="display: flex; flex-direction: row; flex: 1; width: 100%; margin-top: 10px" v-if="item.recomment_list.list.length > 0">
                                                <text style="display: flex; flex-direction: row" :decode="true" :space="true">&nbsp;&nbsp;&nbsp;&nbsp;</text>
                                                <view style="display: flex; flex-direction: row; width: 95%">
                                                    <view
                                                        style="
                                                            border-bottom-left-radius: 6px;
                                                            border-top-left-radius: 6px;
                                                            border-bottom-right-radius: 6px;
                                                            border-top-right-radius: 6px;
                                                            padding-left: 6px;
                                                            padding-top: 6px;
                                                            padding-bottom: 6px;
                                                            padding-right: 6px;
                                                            background: #f5f5f5;
                                                            width: 92%;
                                                        "
                                                    >
                                                        <block v-for="(recomment, index1) in item.recomment_list.list" :key="index1">
                                                            <view>
                                                                <text
                                                                    style="font-size: 11px; color: #ff5981"
                                                                    :data-commentid="item._id"
                                                                    :data-commentuserid="item.userinfo._id"
                                                                    :data-commentusername="item.userinfo.nickname"
                                                                    :data-recommentid="recomment._id"
                                                                    :data-recommentuserid="recomment.userinfo._id"
                                                                    :data-recommentusername="recomment.userinfo.nickname"
                                                                    @tap.stop.prevent="updateCommentConfirm"
                                                                    :hover-stop-propagation="true"
                                                                >
                                                                    {{ recomment.userinfo.nickname }}
                                                                </text>
                                                                <text
                                                                    v-if="!common.recommentUser(recomment)"
                                                                    style="font-size: 11px; color: #ff5981"
                                                                    :data-commentid="item._id"
                                                                    :data-commentuserid="item.userinfo._id"
                                                                    :data-commentusername="item.userinfo.nickname"
                                                                    :data-recommentid="recomment._id"
                                                                    :data-recommentuserid="recomment.userinfo._id"
                                                                    :data-recommentusername="recomment.userinfo.nickname"
                                                                    @tap.stop.prevent="updateCommentConfirm"
                                                                    :hover-stop-propagation="true"
                                                                >
                                                                    :
                                                                </text>
                                                                <text
                                                                    v-if="common.recommentUser(recomment)"
                                                                    style="font-size: 11px; color: #505050"
                                                                    :data-commentid="item._id"
                                                                    :data-commentuserid="item.userinfo._id"
                                                                    :data-commentusername="item.userinfo.nickname"
                                                                    :data-recommentid="recomment._id"
                                                                    :data-recommentuserid="recomment.userinfo._id"
                                                                    :data-recommentusername="recomment.userinfo.nickname"
                                                                    @tap.stop.prevent="updateCommentConfirm"
                                                                    :hover-stop-propagation="true"
                                                                >
                                                                    回复
                                                                </text>
                                                                <text
                                                                    v-if="common.recommentUser(recomment)"
                                                                    style="font-size: 11px; color: #ff5981"
                                                                    :data-commentid="item._id"
                                                                    :data-commentuserid="item.userinfo._id"
                                                                    :data-commentusername="item.userinfo.nickname"
                                                                    :data-recommentid="recomment._id"
                                                                    :data-recommentuserid="recomment.userinfo._id"
                                                                    :data-recommentusername="recomment.userinfo.nickname"
                                                                    @tap.stop.prevent="updateCommentConfirm"
                                                                    :hover-stop-propagation="true"
                                                                >
                                                                    {{ recomment.recomment.userinfo.nickname }} :
                                                                </text>
                                                                <text
                                                                    style="font-size: 11px; color: #505050"
                                                                    :data-commentid="item._id"
                                                                    :data-commentuserid="item.userinfo._id"
                                                                    :data-commentusername="item.userinfo.nickname"
                                                                    :data-recommentid="recomment._id"
                                                                    :data-recommentuserid="recomment.userinfo._id"
                                                                    :data-recommentusername="recomment.userinfo.nickname"
                                                                    @tap.stop.prevent="updateCommentConfirm"
                                                                    :hover-stop-propagation="true"
                                                                >
                                                                    {{ recomment.text }}
                                                                </text>
                                                            </view>
                                                        </block>
                                                        <view
                                                            v-if="item.recomment_list.allcount > 2"
                                                            style="color: #ff5981; font-size: 11px"
                                                            :data-commentid="item._id"
                                                            @tap.stop.prevent="jumpCommentDetail"
                                                            :hover-stop-propagation="true"
                                                        >
                                                            查看全部{{ item.recomment_list.allcount }}条回复 >
                                                        </view>
                                                    </view>
                                                </view>
                                            </view>

                                            <view style="display: flex; flex-direction: row; flex: 1; width: 100%; margin-top: 7px">
                                                <text style="display: flex; flex-direction: row" :decode="true" :space="true">&nbsp;&nbsp;&nbsp;&nbsp;</text>
                                                <view style="display: flex; flex-direction: row; width: 95%">
                                                    <view style="height: 100%; color: #999999; font-size: 10px; text-align: left; width: 92%">
                                                        {{ common.friendlyTimeFormat(item.public_time, systime) }}
                                                    </view>
                                                    <view style="height: 100%; color: #999999; font-size: 10px; text-align: left">{{ common.toFloor(item._id, allList) }}楼</view>
                                                </view>
                                            </view>

                                            <view style="display: flex; flex-direction: row; flex: 1; width: 100%; height: 30rpx">
                                                <text style="display: flex; flex-direction: row" :decode="true" :space="true">&nbsp;&nbsp;&nbsp;&nbsp;</text>
                                                <view style="display: flex; flex-direction: row; width: 95%">
                                                    <view style="display: flex; flex-direction: row; align-items: center; background-color: #ffffff; width: 92%; height: 30rpx" />
                                                </view>
                                            </view>

                                            <view style="display: flex; flex-direction: row; flex: 1; width: 100%; height: 1px">
                                                <text style="display: flex; flex-direction: row" :decode="true" :space="true">&nbsp;&nbsp;&nbsp;&nbsp;</text>
                                                <view style="display: flex; flex-direction: row; width: 95%">
                                                    <view style="display: flex; flex-direction: row; align-items: center; background-color: #f5f5f5; width: 100%; height: 1px" />
                                                </view>
                                            </view>
                                        </view>
                                    </view>
                                </view>
                            </block>
                        </view>

                        <view v-else>
                            <view style="background: white; margin: 0rpx 24rpx 0rpx 24rpx">
                                <view class="netTipBg-Inner">
                                    <image class="netImg-Inner" src="/static/pages/subpages/images/groupSpace/commentOther.png"></image>
                                    <text class="netTip-Inner">来做第一个评论的人吧~</text>
                                </view>
                            </view>
                        </view>

                        <view class="comment_release" :style="'position:fixed;bottom:' + bottom + 'px'">
                            <textarea
                                class="text"
                                placeholder-class="place-holder"
                                :adjust-position="false"
                                :focus="textAreaFocus"
                                :fixed="true"
                                :value="isShowInput ? inputValue : ''"
                                maxlength="180"
                                :show-confirm-bar="false"
                                cursor-spacing="15"
                                :auto-height="true"
                                :placeholder="isShowInput ? placeholderValue : ''"
                                @blur="inputBlur"
                                @focus="inputFocus"
                                @input="bindInput"
                            />
                            <view class="send" v-if="textAreaFocus || (!inputEmpty && !textAreaFocus)" @tap="sendCommentConfirm">
                                <view class="send_view">
                                    <view :class="inputEmpty ? 'send_view_button' : 'send_view_button_on'">发送</view>
                                </view>
                            </view>
                            <view class="share" v-if="inputEmpty && !textAreaFocus">
                                <view class="share_view">
                                    <button class="share_view_button" open-type="share">
                                        <image class="share_view_button_icon" src="https://star-img.xingxiaoculture.com/2019/08/09/8a1efc8870b4fdbced7c2d66ae5e1adc.png"></image>
                                    </button>
                                </view>
                            </view>
                            <view class="zan" v-if="inputEmpty && !textAreaFocus" @tap="updateAttitude">
                                <view class="zan_view">
                                    <view class="zan_view_button">
                                        <image
                                            class="zan_view_button_icon"
                                            :src="
                                                (messageIsattituded == 1
                                                    ? 'https://star-img.xingxiaoculture.com/2019/08/09/2c710b2db76e2709b069160426d61da2.png'
                                                    : 'https://star-img.xingxiaoculture.com/2019/08/09/06a976507fd7533779f906b9f89fac6e.png') + ' '
                                            "
                                        ></image>
                                    </view>
                                </view>
                            </view>
                        </view>
                    </view>

                    <view v-if="!errorDic.needShowInnerErrorView">
                        <view class="refreshBg" v-if="!isRequestAll">
                            <refresh_view :is-animation="isLoading + ' '"></refresh_view>
                        </view>
                        <view class="bottom_desc" v-else>没有更多评论~</view>
                    </view>
                </view>
            </view>
            <!-- </view> -->
            <login_panel v-if="bShowLoginPanel" @loginCallback="loginCallback" @closeViewEvent="closeLoginView"></login_panel>
            <idol_toastTip_view
                v-if="isDisplayToast"
                :tipType="tipType"
                :toastIcon="tipToastIcon"
                :toastTitle="tipToastTitle"
                :alertTitle="alertTitle"
                alertLeftBtnTitle="取消"
                alertLeftBtnTitleColor="#262626"
                alertRightBtnTitle="确定"
                alertRightBtnColor="#262626"
                @leftBtnClick="myAlertLeftBtnClick"
                @rightBtnClick="myAlertRightBtnClick"
            ></idol_toastTip_view>
            <idol_toastTip_view
                v-if="isDisplayOnlyIKnowTip"
                tipType="alert"
                :alertTitle="isDisplayOnlyIKnowTipTitle"
                alertLeftBtnTitle="知道了"
                alertLeftBtnTitleColor="#262626"
                @oneBtnClick="myAlertLeftIKnowBtnClick"
            ></idol_toastTip_view>
            <group_spaceDynamicTip
                v-if="isFirstComePage"
                @tap.native="closeTipDisplay"
                tipImg="../../images/groupSpace/commentTip.png"
                btnImg="../../images/groupSpace/commentTipIcon.png"
                :tipImgFrame="tipFrameDic"
                :btnImgFrame="btnTipFrameDic"
            ></group_spaceDynamicTip>
        </view>
    </view>
</template>
<script module="common" lang="wxs" src="@/pages/subpages/pages/user_feed_detail/user_feed_detail.wxs"></script>
<script>
import error_view from '../../../../component/error_view/error_view';
import login_panel from '../../../../component/login_panel/login_panel';
import refresh_view from '../../../../component/refresh_view/refresh_view';
import idc_image from '../../../../component/idc_image/idc_image';
import idol_toastTip_view from '../../../../component/idol_toastTip_view/idol_toastTip_view';
import group_spaceDynamicTip from '../../component/group_spaceDynamicTip/group_spaceDynamicTip';
// pages/user_feed_detail/user_feed_detail.js
const app = getApp();
const IDLoginManager = require('../../../../commonscript/common/IDLoginManager.js');
const globalData = require('../../../../commonscript/common/globalData.js');
const net = require('../../../../commonscript/common/netLoad.js');
const utils = require('../../../../utils/util.js');
export default {
    components: {
        error_view,
        login_panel,
        refresh_view,
        idc_image,
        idol_toastTip_view,
        group_spaceDynamicTip
    },
    data() {
        return {
            userid: '',
            message_single: {},
            bShowNotGetGroupGid: false,

            //是否展示重试获取gid弹窗
            retryGetgroupgid: 0,

            WeChatGroupGid: '',

            // 公会id
            WeChatGroupCode: '',

            WeChatGroupiv: '',
            WeChatGroupEncrypted: '',
            gid: '',
            groupInfo: '',
            official_group: '',
            tipFrameDic: {},
            btnTipFrameDic: {},
            isFirstComePage: false,
            qzid: '',
            messageid: '',
            messageStatus: 1,
            sid: 0,
            isshare: 0,
            getofficial: 0,
            messageUserHead: '',
            messageUserName: '',
            messageUserId: '',
            messageImages: [],
            messageUserText: '',
            messageTime: '',
            messageIsattituded: 0,
            messageWebPage: '',
            getadmin: 1,
            getbanned: 1,
            count: 10,
            page: 1,
            order: 'new',
            offset: '',
            allcount: 0,
            allList: [],
            allListOri: [],
            uid: '',
            nickname: '',
            headImg: '',
            commentid: '',
            commentuserid: '',
            commentusername: '',
            recommentid: '',
            recommentuserid: '',
            recommentusername: '',
            alertdelcommenttype: 'delcomment',
            alertdelcommentid: '',
            systime: '',
            textAreaFocus: false,
            inputEmpty: true,
            inputValue: '',
            placeholderValue: '说点什么呢',

            errorDic: {
                needShowErrorView: false,
                needShowInnerErrorView: false,
                needShowDelErrorView: false,
                errorText: '断网啦！点击页面重新加载～'
            },

            bottom: 0,
            isDisplayToast: false,

            //toast alert 提示
            tipType: '',

            //提示类型
            tipToastIcon: '',

            // toast图片
            tipToastTitle: '',

            //toast 文案
            alertTitle: '确认删除该条评论吗?',

            //标题
            isDisplayOnlyIKnowTipType: 'comment',

            //我知道了 那种alert提示的弹窗类型
            isDisplayOnlyIKnowTip: false,

            //我知道了 那种alert提示的控制
            isDisplayOnlyIKnowTipTitle: '该条动态已被删除!',

            //标题
            scrollTop: 0,

            windowWidth: 0,
            isPreload: true,
            isLoading: false,
            isRequestAll: false,
            bShowLoginPanel: false,
            login: false,
            init: false,
            isRefresh: false,

            //是否刷新页面数据
            user_info: {},

            checkShowInput: false,
            isShowInput: true,
            userAngelModel: '',
            bShowGroupName: 1,

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

            myRankObject: {
                userinfo: {
                    nickname: ''
                }
            },

            pageDataObj: {
                single_user_rank_info_all_time: {
                    text: ''
                }
            },

            recomment: {
                _id: '',

                userinfo: {
                    _id: '',
                    nickname: ''
                },

                recomment: {
                    userinfo: {
                        nickname: ''
                    }
                },

                text: ''
            }
        };
    },
    /**
     * 生命周期函数--监听页面加载
     */
    onLoad: function (options) {
        // 设置分享带 shareticket
        uni.showShareMenu({
            withShareTicket: true
        });
        // 展示右上角三个点之后的分享
        uni.showShareMenu({});
        var that = this;
        console.log('++++++++===onLoad options.qz_id===', options.qz_id);
        console.log('++++++++===onLoad options.message_id===', options.message_id);
        console.log('++++++++===onLoad options.sid===', options.sid);
        console.log('++++++++===onLoad options.gid===', options.gid);
        console.log('++++++++===onLoad options.is_share===', options.is_share);
        console.log('++++++++===onLoad options.getofficial===', options.getofficial);
        var time = Date.parse(new Date());
        console.log('++++++++===onLoad time===', time);
        console.log('++++++++===onLoad options.bShowGroupName===', options.bShowGroupName);
        if (options.bShowGroupName) {
            this.setData({
                bShowGroupName: options.bShowGroupName
            });
        } else {
            this.setData({
                bShowGroupName: 1
            });
        }
        console.log('++++++++===onLoad data.bShowGroupName===', this.bShowGroupName);
        this.setData({
            systime: time,
            qzid: options.qz_id,
            messageid: options.message_id,
            windowWidth: uni.getSystemInfoSync().windowWidth
        });
        uni.getSystemInfo({
            success: function (res) {
                console.log('+++===onLoad wx.getSystemInfo res===', res);
                if (res != null && res.platform != null && utils.equalsIgnoreCase(res.platform, 'ios')) {
                    console.log('+++===onLoad platform ios++');
                    if (res != null && res.system != null && !utils.equalsIgnoreCase(res.system, '')) {
                        var strtoken = res.system.split(' ');
                        if (strtoken != null && strtoken.length > 1) {
                            var platformtoken = strtoken[0];
                            var systemtoken = strtoken[1];
                            console.log('+++===onLoad platformtoken ==' + platformtoken);
                            console.log('+++===onLoad systemtoken ==' + systemtoken);
                            var strsystemtoken = systemtoken.split('.');
                            if (strsystemtoken != null && strsystemtoken.length > 0) {
                                var strsystemtokenplatform = strsystemtoken[0];
                                console.log('+++===onLoad strsystemtokenplatform ==' + strsystemtokenplatform);
                                if (strsystemtokenplatform <= 10) {
                                    console.log('++==onLoad strsystemtokenplatform <= 10++');
                                    that.setData({
                                        checkShowInput: true
                                    });
                                }
                            }
                        }
                    }
                }
            }
        });
        if (options.sid !== 0 && options.sid !== undefined) {
            this.setData({
                sid: options.sid
            });
        }
        if (options.gid !== '' && options.gid !== undefined) {
            this.setData({
                gid: options.gid
            });
        }
        if (options.is_share !== 0 && options.is_share !== undefined) {
            this.setData({
                isshare: options.is_share
            });
        }
        if (options.getofficial !== 0 && options.getofficial !== undefined) {
            this.setData({
                getofficial: options.getofficial
            });
        }
        that.loginLogicStar();
        uni.getNetworkType({
            success: function (res) {
                // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
                var networkType = res.networkType;
                console.log('++++++++===onLoad networkType==', networkType);
                if (utils.equalsIgnoreCase('unknown', networkType) || utils.equalsIgnoreCase('none', networkType)) {
                    that.setData({
                        isPreload: false,
                        errorDic: {
                            needShowErrorView: true,
                            needShowDelErrorView: false,
                            needShowInnerErrorView: false,
                            errorText: '断网啦！点击页面重新加载～'
                        }
                    });
                } else {
                    that.setData({
                        isPreload: true
                    });
                    if (that.isshare == 1) {
                        if (options.gid !== '' && options.gid !== undefined) {
                            console.log('+++options.gid == 有gid');
                            that.setData({
                                WeChatGroupGid: options.gid
                            });
                            that.getGroupInfo(1);
                        } else {
                            console.log('+++options.gid == 没有gid');
                            console.log('+++shareTicket ==' + app.globalData.shareTicket);
                            if (app.globalData.shareTicket) {
                                uni.getShareInfo({
                                    shareTicket: app.globalData.shareTicket,
                                    success: function (res) {
                                        console.log(res.encryptedData);
                                        that.setData({
                                            WeChatGroupEncrypted: res.encryptedData,
                                            WeChatGroupiv: res.iv
                                        });
                                        uni.login({
                                            success: function (obj) {
                                                console.log(obj.code);
                                                that.setData({
                                                    WeChatGroupCode: obj.code
                                                });
                                                that.getwechatGid(0, 1);
                                            }
                                        });
                                    }
                                });
                            } else {
                                that.setData({
                                    isPreload: false,
                                    official_group: 0
                                });
                            }
                            that.requestDetail();
                        }
                    } else {
                        that.setData({
                            official_group: 1
                        });
                        that.requestDetail();
                    }
                }
            }
        });
    },
    /**
     * 生命周期函数--监听页面初次渲染完成
     */
    onReady: function () {
        var that = this;
    },
    //监听屏幕滚动 判断上下滚动
    onPageScroll: function (ev) {
        console.log('++onPageScroll ++');
        var that = this;
        //当滚动的top值最大或最小时，为什么要做这一步是因为在手机实测小程序的时候会发生滚动条回弹，所以为了处理回弹，设置默认最大最小值
        if (ev.scrollTop <= 0) {
            ev.scrollTop = 0;
        } else if (ev.scrollTop > uni.getSystemInfoSync().windowHeight) {
            ev.scrollTop = uni.getSystemInfoSync().windowHeight;
        }
        //判断浏览器滚动条上下滚动
        if (ev.scrollTop > that.scrollTop || ev.scrollTop == uni.getSystemInfoSync().windowHeight) {
            //向下滚动
            console.log('++onPageScroll 向下滚动 ++');
            console.log('++onPageScroll 向下滚动 ev.scrollTop==' + ev.scrollTop);
            console.log('++onPageScroll 向下滚动 oThis.data.scrollTop==' + that.scrollTop);
            console.log('++onPageScroll 向下滚动 windowHeight==' + uni.getSystemInfoSync().windowHeight);
            // oThis.setData({
            //     isShowInput: false,
            // });
        } else {
            //向上滚动
            console.log('++onPageScroll 向上滚动++');
            console.log('++onPageScroll 向上滚动 ev.scrollTop==' + ev.scrollTop);
            console.log('++onPageScroll 向上滚动 oThis.data.scrollTop==' + that.scrollTop);
            console.log('++onPageScroll 向上滚动 windowHeight==' + uni.getSystemInfoSync().windowHeight);
            // oThis.setData({
            //     isShowInput: false,
            // });
        }
        //给scrollTop重新赋值
        that.setData({
            scrollTop: ev.scrollTop
        });
    },
    /**
     * 生命周期函数--监听页面显示
     */
    onShow: function () {
        var that = this;
        console.log('++onShow ++');
        console.log('++onShow windowWidth ==' + that.windowWidth);
        if (!that.init) {
            that.setData({
                init: true
            });
        } else {
            var refresh = false;
            console.log('++onShow oThis.data.login++' + that.login);
            console.log('++onShow loginState++' + IDLoginManager.loginState());
            if (!that.login && IDLoginManager.loginState() == IDLoginManager.is_has_login) {
                refresh = true;
            }
            that.loginLogicStar();
            console.log('++onShow refresh++' + refresh);
            if (refresh) {
                that.refreshFeed();
            }
        }
    },
    /**
     * 生命周期函数--监听页面隐藏
     */
    onHide: function () {},
    /**
     * 生命周期函数--监听页面卸载
     */
    onUnload: function () {
        var that = this;
    },
    /**
     * 页面相关事件处理函数--监听用户下拉动作
     */
    onPullDownRefresh: function () {
        var that = this;
        uni.getNetworkType({
            success: function (res) {
                // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
                var networkType = res.networkType;
                console.log('++++++++===onPullDownRefresh networkType==', networkType);
                if (utils.equalsIgnoreCase('unknown', networkType) || utils.equalsIgnoreCase('none', networkType)) {
                    that.endRefresh();
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                } else {
                    that.setData({
                        page: 1,
                        offset: '',
                        isRequestAll: false,
                        isLoading: false
                    });
                    that.requestDetail();
                }
            }
        });
    },
    /**
     * 页面上拉触底事件的处理函数
     */
    onReachBottom: function () {
        var that = this;
        if (that.isRequestAll) {
            return;
        }
        that.page++;
        that.requestCommentList(that.qzid, that.messageid, that.order, that.count, that.page, that.getadmin);
    },
    onShareAppMessage: function (e) {
        var that = this;
        var messagePath =
            '/pages/index/index?action=gopage&page=user_feed_detail&qz_id=' + that.qzid + '&message_id=' + that.messageid + '&is_share=1' + '&sid=' + that.sid + '&getofficial=1';
        if (globalData.idolUserInfo !== '') {
            var messageText = globalData.idolUserInfo.nickname + '分享了新动态，快去看看吧~';
        } else {
            var messageText = that.messageUserText;
        }
        var messageImageUrl = 'https://star-img.xingxiaoculture.com/2023/02/21/0efb4d0b1604dfd4a7054df2e43f61d4.png';
        if (that.messageImages != null && that.messageImages.length > 0 && that.messageImages[0].img_url != null) {
            messageImageUrl = that.messageImages[0].img_url.middle_pic;
        }
        console.log('++onShareAppMessage messageText==', messageText);
        console.log('++onShareAppMessage messagePath==', messagePath);
        console.log('++onShareAppMessage messageImageUrl==', messageImageUrl);
        var p = {
            title: messageText,
            path: messagePath,
            imageUrl: messageImageUrl
        };

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
        inputFocus: function (e) {
            console.log('+++键盘聚焦 inputFocus +++');
            console.log('+++键盘聚焦 inputFocus e.detail.height+++' + e.detail.height);
            var that = this;
            that.setData({
                textAreaFocus: true
            });
            if (e.detail.height !== 0 && e.detail.height !== undefined) {
                that.setData({
                    bottom: e.detail.height
                });
            }
        },

        inputBlur: function (e) {
            console.log('+++键盘失焦 inputBlur+++');
            var that = this;
            that.setData({
                textAreaFocus: false,
                bottom: 0
            });
            if (!that.textAreaFocus) {
                that.emptyCommentConfirm();
            }
        },

        bindInput: function (e) {
            var that = this;
            console.log('+++===bindInput +++');
            console.log('+++===bindInput 刷新inputValue&inputEmpty+++');
            console.log('+++===bindInput e.detail.value==' + e.detail.value);
            console.log('+++===bindInput oThis.data.textAreaFocus==' + that.textAreaFocus);
            that.setData(
                {
                    inputValue: e.detail.value
                },
                function () {
                    if (utils.equalsIgnoreCase(e.detail.value, '')) {
                        that.setData({
                            inputEmpty: true
                        });
                    } else {
                        that.setData({
                            inputEmpty: false
                        });
                    }
                }
            );
        },

        onPageScrollIsShowInput: function (lastscrollTop) {
            console.log('++onPageScrollIsShowInput ++');
            var that = this;
            setTimeout(function () {
                that.setData(
                    {
                        isShowInput: lastscrollTop == that.scrollTop ? true : false
                    },
                    function () {
                        console.log(
                            '++onPageScrollIsShowInput lastscrollTop==' +
                                lastscrollTop +
                                '&oThis.data.scrollTop ==' +
                                that.scrollTop +
                                '&oThis.data.isShowInput ==' +
                                that.isShowInput
                        );
                    }
                );
            }, 0);
        },

        pageTouchStart: function (e) {
            console.log('++pageTouchStart ++');
            var that = this;
            if (that.checkShowInput) {
                that.setData({
                    isShowInput: false
                });
            }
        },

        pageTouchEnd: function (e) {
            console.log('++pageTouchEnd ++');
            var that = this;
            if (that.checkShowInput) {
                that.setData({
                    isShowInput: true
                });
            }
        },

        pageTouchCancel: function (e) {
            console.log('++pageTouchCancel ++');
            var that = this;
            if (that.checkShowInput) {
                that.setData({
                    isShowInput: true
                });
            }
        },

        pageTouchMove: function (e) {
            console.log('++pageTouchMove ++');
            var that = this;
            if (that.checkShowInput) {
                that.setData({
                    isShowInput: false
                });
            }
        },

        showTipDisplay: function () {
            console.log('++showTipDisplay++');
            //首次弹窗
            var that = this;
            var showTip = globalData.getDataForKey('show_first_comment_guide_tip');
            console.log('++showTipDisplay showTip ==' + showTip);
            if (!showTip) {
                globalData.saveDataForKey('show_first_comment_guide_tip', '1');
                if (that.windowWidth > 600) {
                    that.setData({
                        tipFrameDic: {
                            right: 12,
                            bottom: 60,
                            width: 124,
                            height: 58
                        },
                        btnTipFrameDic: {
                            right: 28,
                            bottom: 10,
                            width: 36,
                            height: 36
                        },
                        isFirstComePage: true
                    });
                } else {
                    that.setData({
                        tipFrameDic: {
                            right: 12,
                            bottom: 45,
                            width: 124,
                            height: 58
                        },
                        btnTipFrameDic: {
                            right: 12,
                            bottom: 5,
                            width: 36,
                            height: 36
                        },
                        isFirstComePage: true
                    });
                }
            }
        },

        closeTipDisplay: function () {
            console.log('++closeTipDisplay++');
            var that = this;
            that.setData({
                isFirstComePage: false
            });
        },

        showTripImages: function (e) {
            var urls = [];
            var nowUrl = '';
            var i = 0;
            var cellObj = this.message_single;
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

        goTop: function (e) {
            if (uni.pageScrollTo) {
                uni.pageScrollTo({
                    scrollTop: 0
                });
            } else {
                uni.showModal({
                    title: '提示',
                    content: '当前微信版本过低，无法使用该功能，请升级到最新微信版本后重试。'
                });
            }
        },

        refreshFeed: function () {
            var that = this;
            console.log('++++++++===refreshFeed==');
            uni.getNetworkType({
                success: function (res) {
                    // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
                    var networkType = res.networkType;
                    console.log('++++++++===refreshFeed networkType==', networkType);
                    if (utils.equalsIgnoreCase('unknown', networkType) || utils.equalsIgnoreCase('none', networkType)) {
                    } else {
                        that.setData({
                            page: 1,
                            offset: '',
                            isRequestAll: false,
                            isLoading: false
                        });
                        setTimeout(function () {
                            that.goTop();
                        }, 2000);
                        that.requestDetail();
                    }
                }
            });
        },

        endRefresh: function () {
            console.log('endRefresh');
            var that = this;
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

        requestDetail: function () {
            console.log('++++++++===requestDetail++');
            var that = this;
            that.requestFeedDetail(that.qzid, that.messageid, that.getbanned, that.getadmin);
        },

        requestFeedDetail: function (qzid, messageid, getbanned, getadmin) {
            // https://data.idol001.com/api_moblie_idol_userdt.php?action=get_message_single&messageid=57355c6677e07095378b4845&getbanned=1&notype=1&qzid=55c4afafcd4e70ab238b47bb&getadmin=1&version=199&channelId=S007&unique_id=C7200631145EA542B3DF1037E4F1179E&app_platform=android
            var that = this;
            var url = 'https://data.idol001.com/api_moblie_idol_userdt.php?action=get_message_single';
            var params = {
                messageid: messageid,
                getbanned: getbanned,
                notype: 1,
                qzid: qzid,
                getadmin: getadmin
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('++++++++requestFeedDetail resp==', resp);
                    if (resp.data != null) {
                        var message_single = resp.data.message_single;
                        console.log('requestFeedDetail oThis.data.bShowGroupName==', that.bShowGroupName);
                        console.log('requestFeedDetail message_single==', JSON.stringify(message_single));
                        if (that.bShowGroupName == 0) {
                            message_single.group_name = null;
                        }
                        console.log('requestFeedDetail message_single++==', JSON.stringify(message_single));
                        if (message_single != null && message_single.error_code > 0) {
                            that.setData({
                                messageStatus: 0
                            });
                            console.log('----requestFeedDetail message_single.error_code==' + message_single.error_code);
                            if (that.isRefresh) {
                                that.setData({
                                    isDisplayOnlyIKnowTip: true,
                                    isDisplayOnlyIKnowTipTitle: '该条动态已被删除!',
                                    isDisplayOnlyIKnowTipType: 'feed'
                                });
                            } else {
                                that.setData({
                                    isPreload: false,
                                    errorDic: {
                                        needShowErrorView: true,
                                        needShowDelErrorView: true,
                                        needShowInnerErrorView: false,
                                        errorText: '来晚了，该动态已被删除~'
                                    }
                                });
                            }
                        } else {
                            that.setData({
                                messageStatus: 1
                            });
                            var temp = {};
                            temp['data_quanzi'] = message_single;
                            temp.data_quanzi.public_time = utils.commentTimeHandle(temp.data_quanzi.public_time);
                            //判断一下是否需要展示删除按钮
                            if (that.user_info._id == temp.data_quanzi.userinfo._id) {
                                temp.data_quanzi.isShowDeleteBtn = true;
                            }
                            console.log('----requestFeedDetail message_single==' + message_single);
                            that.setData({
                                isRefresh: true,
                                messageUserHead: message_single.userinfo.image.middle_pic,
                                messageUserName: message_single.userinfo.nickname,
                                messageUserId: message_single.userinfo._id,
                                messageUserText: message_single.text,
                                messageImages: message_single.images,
                                messageTime: message_single.public_time,
                                messageIsattituded: message_single.isattituded,
                                allcount: message_single.comment_num,
                                message_single: temp,
                                errorDic: {
                                    needShowErrorView: false,
                                    needShowDelErrorView: false,
                                    needShowInnerErrorView: false,
                                    errorText: '断网啦！点击页面重新加载～'
                                }
                            });
                            that.requestAngelDetail();
                            that.requestCommentList(that.qzid, that.messageid, that.order, that.count, that.page, that.getadmin);
                        }
                    } else {
                        that.setData({
                            isPreload: false,
                            errorDic: {
                                needShowErrorView: true,
                                needShowDelErrorView: false,
                                needShowInnerErrorView: false,
                                errorText: resp.data && resp.data.error_description ? resp.data.error_description : '断网啦！点击页面重新加载～'
                            }
                        });
                    }
                },
                function (resp) {
                    console.log('+++===++resp.data===', resp.data);
                    that.setData({
                        isPreload: false,
                        errorDic: {
                            needShowErrorView: true,
                            needShowDelErrorView: false,
                            needShowInnerErrorView: false,
                            errorText: resp.data && resp.data.error_description ? resp.data.error_description : '断网啦！点击页面重新加载～'
                        }
                    });
                }
            );
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
                    user_id: that.messageUserId
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

        // recommentUser: function(item) {
        //     if (item.recomment != null && item.recomment.userinfo != null &&
        //         !utils.equalsIgnoreCase(item.recomment.userinfo.nickname, "")) {
        //         return item.userinfo.nickname + " 回复 " +
        //             item.recomment.userinfo.nickname;
        //     } else {
        //         return item.userinfo.nickname;
        //     }
        // },

        requestCommentList: function (qzid, messageid, order, count, page, getadmin) {
            //https://data.idol001.com/api_moblie_idol_userdt.php?action=get_message_comment_list&messageid=57355c6677e07095378b4845&order=time&page=1&qzid=55c4afafcd4e70ab238b47bb&getadmin=1&version=199&channelId=S007&unique_id=C7200631145EA542B3DF1037E4F1179E&app_platform=android
            var that = this;
            var url = 'https://data.idol001.com/api_moblie_idol_userdt.php?action=get_message_comment_list';
            var params = {
                messageid: messageid,
                order: order,
                count: count,
                page: page,
                qzid: qzid,
                getadmin: getadmin
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('++++++++===requestCommentList resp===', resp);
                    that.showTipDisplay();
                    if (resp.data != null && resp.data.list != null && resp.data.list.length > 0) {
                        var list = resp.data.list;
                        // if (list != null && list.length > 0) {
                        //     for (var i = 0; i < list.length; i++) {
                        //         var recomment_list = list[i].recomment_list;
                        //         if (recomment_list != null && recomment_list.list != null && recomment_list.list.length > 0) {
                        //             for (var j = 0; j < recomment_list.list.length; j++) {
                        //                 var recomment_user = oThis.recommentUser(recomment_list.list[j]);
                        //                 recomment_list.list[j].text = oThis.hilight_word(recomment_user + "：", recomment_user + "：" + recomment_list.list[j].text);
                        //             }
                        //         }
                        //     }
                        // }
                        var tempArr = that.allListOri;
                        if (params.page == 1) {
                            //下拉刷新
                            tempArr = list;
                            uni.stopPullDownRefresh();
                        } else {
                            //上拉加载更多
                            tempArr.push(...list);
                        }
                        // console.log(">>>+++tempArr==" + tempArr);
                        that.setData({
                            allListOri: tempArr
                        });
                        that.setData(
                            {
                                isRequestAll: tempArr.length < resp.data.allcount ? false : true,
                                isLoading: false,
                                isPreload: false,
                                errorDic: {
                                    needShowErrorView: false,
                                    needShowDelErrorView: false,
                                    needShowInnerErrorView: false,
                                    errorText: resp.data && resp.data.error_description ? resp.data.error_description : '断网啦！点击页面重新加载～'
                                }
                            },
                            function () {
                                that.setData({
                                    allList: tempArr
                                });
                                that.setData({
                                    // allcount: resp.data.allcount,
                                    offset: list[list.length - 1].public_time
                                });
                                that.endRefresh();
                            }
                        );
                    } else {
                        console.log('++===++resp.data==');
                        console.log('++===++resp.data oThis.data.page==' + that.page);
                        if (that.page == 1) {
                            uni.stopPullDownRefresh();
                        }
                        if (that.page == 1) {
                            that.setData(
                                {
                                    isRequestAll: true,
                                    isLoading: false,
                                    isPreload: false,
                                    allList: [],
                                    allcount: 0,
                                    errorDic: {
                                        needShowErrorView: false,
                                        needShowDelErrorView: false,
                                        needShowInnerErrorView: true,
                                        errorText: resp.data && resp.data.error_description ? resp.data.error_description : '断网啦！点击页面重新加载～'
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
                                        needShowDelErrorView: false,
                                        needShowInnerErrorView: false,
                                        errorText: resp.data && resp.data.error_description ? resp.data.error_description : '断网啦！点击页面重新加载～'
                                    }
                                },
                                function () {
                                    that.endRefresh();
                                }
                            );
                        }
                    }
                },
                function (resp) {
                    console.log('+++error==++resp.data===', resp.data);
                    that.showTipDisplay();
                    if (that.page == 1) {
                        uni.stopPullDownRefresh();
                    }
                    if (that.page == 1) {
                        that.setData(
                            {
                                isRequestAll: true,
                                isLoading: false,
                                isPreload: false,
                                allList: [],
                                allcount: 0,
                                errorDic: {
                                    needShowErrorView: false,
                                    needShowDelErrorView: false,
                                    needShowInnerErrorView: true,
                                    errorText: resp.data && resp.data.error_description ? resp.data.error_description : '断网啦！点击页面重新加载～'
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
                                    needShowDelErrorView: false,
                                    needShowInnerErrorView: false,
                                    errorText: resp.data && resp.data.error_description ? resp.data.error_description : '断网啦！点击页面重新加载～'
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

        backHome: function () {
            console.log('+++===backHome==');
            var that = this;
            if (that.official_group == 1) {
                console.log('+++===backHome oThis.data.official_group == 1');
                //未登录，弹登录框
                if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                    that.setData({
                        bShowLoginPanel: true
                    });
                    return;
                }
                uni.getNetworkType({
                    success: function (res) {
                        // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
                        var networkType = res.networkType;
                        console.log('++++++++===commitAttitude networkType==', networkType);
                        if (utils.equalsIgnoreCase('unknown', networkType) || utils.equalsIgnoreCase('none', networkType)) {
                            uni.showToast({
                                title: '网络似乎不太顺畅哦',
                                icon: 'none'
                            });
                        } else {
                            //请求公会分享gid
                            console.log('backHome gid==', that.gid);
                            if (that.gid !== '' && that.gid !== undefined) {
                                console.log('backHome 有gid==');
                                this.setData({
                                    WeChatGroupGid: that.gid
                                });
                                app.globalData.sid = that.sid;
                                app.globalData.gid = that.WeChatGroupGid;
                                app.globalData.getofficial = 1;
                                uni.switchTab({
                                    url: '../../../../pages/idol_home/idol_home'
                                });
                            } else {
                                console.log('backHome 没有gid==');
                                console.log('backHome app.globalData.shareTicket==' + app.globalData.shareTicket);
                                if (app.globalData.shareTicket) {
                                    uni.getShareInfo({
                                        shareTicket: app.globalData.shareTicket,
                                        success: function (res) {
                                            console.log('backHome res.encryptedData==' + res.encryptedData);
                                            that.setData({
                                                WeChatGroupEncrypted: res.encryptedData,
                                                WeChatGroupiv: res.iv
                                            });
                                            uni.login({
                                                success: function (obj) {
                                                    console.log('backHome obj.code==' + obj.code);
                                                    that.setData({
                                                        WeChatGroupCode: obj.code
                                                    });
                                                    that.getwechatGid(1, 0);
                                                }
                                            });
                                        }
                                    });
                                } else {
                                    this.setData({
                                        official_group: 0
                                    });
                                }
                            }
                        }
                    }
                });
            } else {
                console.log('+++===backHome oThis.data.official_group == 0');
                uni.getNetworkType({
                    success: function (res) {
                        // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
                        var networkType = res.networkType;
                        console.log('++++++++===commitAttitude networkType==', networkType);
                        if (utils.equalsIgnoreCase('unknown', networkType) || utils.equalsIgnoreCase('none', networkType)) {
                            uni.showToast({
                                title: '网络似乎不太顺畅哦',
                                icon: 'none'
                            });
                        } else {
                            console.log('+++===backHome oThis.messageStatus ==' + that.messageStatus);
                            if (that.messageStatus == 1) {
                                that.gotoHomePageAction();
                            } else {
                                uni.switchTab({
                                    url: '/pages/idol_sprite_guard/idol_sprite_guard'
                                });
                            }
                        }
                    }
                });
            }
        },

        // 获取微信gid
        getwechatGid: function (backhome, initpage) {
            console.log('getwechatGid backhome==' + backhome);
            console.log('getwechatGid initpage==' + initpage);
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=return_opengid';
            var params = {
                code: this.WeChatGroupCode,
                encrypted_data: this.WeChatGroupEncrypted,
                iv: this.WeChatGroupiv
            };
            var that = this;
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    console.log('getwechatGid==', resp.data);
                    if (resp.data == false && typeof resp.data == 'boolean') {
                        //第一次如果获取不到 重试获取
                        if (that.retryGetgroupgid == 0) {
                            uni.getShareInfo({
                                shareTicket: app.globalData.shareTicket,
                                success: function (res) {
                                    console.log(res.encryptedData);
                                    that.setData({
                                        WeChatGroupEncrypted: res.encryptedData,
                                        WeChatGroupiv: res.iv
                                    });
                                    uni.login({
                                        success: function (obj) {
                                            console.log(obj.code);
                                            that.setData({
                                                WeChatGroupCode: obj.code
                                            });
                                            that.getwechatGid(backhome, initpage);
                                        }
                                    });
                                }
                            });
                        }
                        if (that.retryGetgroupgid == 1) {
                            // 展示重试弹窗
                            // oThis.setData({
                            //     bShowNotGetGroupGid: true
                            // });
                        }
                        that.setData({
                            retryGetgroupgid: 1
                        });
                        return;
                    }
                    that.setData({
                        // 隐藏重试弹窗
                        // bShowNotGetGroupGid: false
                    });
                    app.globalData.groupGid = resp.data.openGId;
                    that.setData({
                        WeChatGroupGid: resp.data.openGId
                    });
                    if (backhome == 1) {
                        that.getGroupInfoBackHome(backhome);
                    } else {
                        that.getGroupInfo(initpage);
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                }
            );
        },

        getGroupInfoBackHome(backhome) {
            const oThis = this;
            console.log('getGroupInfoBackHome backhome==', backhome);
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_info';
            const params = {
                gid: this.WeChatGroupGid,
                starid: this.sid,
                getofficial: this.getofficial
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('getGroupInfoBackHome 公会信息--');
                console.log(resp);
                if (resp && resp.data) {
                    const data = resp.data;
                    // 是否官方公会
                    let is_official_group = parseFloat(data.official_group) || 0;
                    // 是否开启官方公会验证
                    // is_official_group = 1;
                    oThis.setData({
                        groupInfo: data,
                        official_group: is_official_group
                    });
                    //  是官方公会
                    if (is_official_group == 1) {
                        console.log('getGroupInfo++==', IDLoginManager.loginState());
                        if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                            if (backhome == 1) {
                                app.globalData.sid = resp.data.starinfo.sid;
                                app.globalData.gid = oThis.WeChatGroupGid;
                                app.globalData.getofficial = 1;
                                uni.switchTab({
                                    url: '../../../../pages/idol_home/idol_home'
                                });
                            }
                            return;
                        }
                        oThis.setOfficialGroup(this.WeChatGroupGid);
                        console.log('getGroupInfo====', IDLoginManager.loginState());
                    }
                }
                if (backhome == 1) {
                    app.globalData.sid = resp.data.starinfo.sid;
                    app.globalData.gid = oThis.WeChatGroupGid;
                    app.globalData.getofficial = 1;
                    uni.switchTab({
                        url: '../../../../pages/idol_home/idol_home'
                    });
                }
            });
        },

        getGroupInfo(initpage) {
            const oThis = this;
            console.log('getGroupInfo initpage==', initpage);
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_info';
            const params = {
                gid: this.WeChatGroupGid,
                starid: this.sid,
                getofficial: this.getofficial
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getGroupInfo 公会信息--');
                    console.log(resp);
                    if (resp && resp.data) {
                        const data = resp.data;
                        // 是否官方公会
                        let is_official_group = parseFloat(data.official_group) || 0;
                        // 是否开启官方公会验证
                        // is_official_group = 1;
                        oThis.setData({
                            groupInfo: data,
                            official_group: is_official_group
                        });
                        //  是官方公会
                        if (is_official_group == 1) {
                            console.log('getGroupInfo++==', IDLoginManager.loginState());
                            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                                if (initpage == 1) {
                                    oThis.requestDetail();
                                }
                                return;
                            }
                            oThis.setOfficialGroup(this.WeChatGroupGid);
                            console.log('getGroupInfo====', IDLoginManager.loginState());
                        }
                    }
                    if (initpage == 1) {
                        oThis.requestDetail();
                    }
                },
                function () {
                    if (initpage == 1) {
                        oThis.requestDetail();
                    }
                }
            );
        },

        /**
         * 写入官方公会记录
         * @param {string} gid
         */
        setOfficialGroup(gid) {
            const oThis = this;
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=set_idolwx_group_view_history';
            const params = {
                gid: gid,
                starid: oThis.sid
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                }
            });
        },

        updateCommentConfirmValue: function (e) {
            console.log('+++===updateCommentConfirmValue==');
            var that = this;
            if (e.currentTarget.dataset.commentid != null && !utils.equalsIgnoreCase(e.currentTarget.dataset.commentid, '')) {
                that.setData({
                    commentid: e.currentTarget.dataset.commentid
                });
            }
            if (e.currentTarget.dataset.commentuserid != null && !utils.equalsIgnoreCase(e.currentTarget.dataset.commentuserid, '')) {
                that.setData({
                    commentuserid: e.currentTarget.dataset.commentuserid
                });
            }
            if (e.currentTarget.dataset.commentusername != null && !utils.equalsIgnoreCase(e.currentTarget.dataset.commentusername, '')) {
                that.setData({
                    commentusername: e.currentTarget.dataset.commentusername
                });
            }
            if (e.currentTarget.dataset.recommentid != null && !utils.equalsIgnoreCase(e.currentTarget.dataset.recommentid, '')) {
                that.setData({
                    recommentid: e.currentTarget.dataset.recommentid
                });
            } else {
                that.setData({
                    recommentid: ''
                });
            }
            if (e.currentTarget.dataset.recommentuserid != null && !utils.equalsIgnoreCase(e.currentTarget.dataset.recommentuserid, '')) {
                that.setData({
                    recommentuserid: e.currentTarget.dataset.recommentuserid
                });
            } else {
                that.setData({
                    recommentuserid: ''
                });
            }
            if (e.currentTarget.dataset.recommentusername != null && !utils.equalsIgnoreCase(e.currentTarget.dataset.recommentusername, '')) {
                that.setData({
                    recommentusername: e.currentTarget.dataset.recommentusername
                });
            } else {
                that.setData({
                    recommentusername: ''
                });
            }
        },

        gotoHomePageAction: function () {
            console.log('++gotoHomePageAction++');
            var that = this;
            var cellObj = that.message_single;
            var userInfo = cellObj.data_quanzi.userinfo;
            if (userInfo) {
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(userInfo)) + '&showBackHomeButton=' + '1'
                });
            }
        },

        dynamicCellItemClick: function () {
            console.log('+++===dynamicCellItemClick==');
            var that = this;
            that.emptyCommentConfirm();
        },

        dynamicCellCommentItemClick: function (e) {
            console.log('+++===dynamicCellCommentItemClick ==');
            var that = this;
            var userInfo = e.currentTarget.dataset.userinfo;
            console.log('+++===dynamicCellCommentItemClick userInfo==' + userInfo);
            if (userInfo != null) {
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(userInfo))
                });
            }
        },

        myAlertLeftIKnowBtnClick: function () {
            console.log('+++===myAlertLeftIKnowBtnClick==');
            var that = this;
            that.setData({
                isDisplayOnlyIKnowTip: false
            });
            console.log('+++===myAlertLeftIKnowBtnClick oThis.data.isDisplayOnlyIKnowTipType==' + that.isDisplayOnlyIKnowTipType);
            if (that.isDisplayOnlyIKnowTipType != null && utils.equalsIgnoreCase(that.isDisplayOnlyIKnowTipType, 'feed')) {
                uni.navigateBack({
                    delta: 1
                });
            }
        },

        emptyCommentConfirm: function () {
            console.log('+++===emptyCommentConfirm==');
            var that = this;
            setTimeout(function () {
                that.setData({
                    commentid: '',
                    commentuserid: '',
                    commentusername: '',
                    recommentid: '',
                    recommentuserid: '',
                    recommentusername: '',
                    inputValue: '',
                    inputEmpty: true,
                    placeholderValue: '说点什么呢'
                });
            }, 100);
        },

        delCommentConfirm: function (e) {
            console.log('++++++++===delCommentConfirm e.currentTarget.dataset.commentid===', e.currentTarget.dataset.commentid);
            console.log('++++++++===delCommentConfirm e.currentTarget.dataset.commentuserid===', e.currentTarget.dataset.commentuserid);
            console.log('++++++++===delCommentConfirm e.currentTarget.dataset.commentusername===', e.currentTarget.dataset.commentusername);
            console.log('++++++++===delCommentConfirm e.currentTarget.dataset.recommentid===', e.currentTarget.dataset.recommentid);
            console.log('++++++++===delCommentConfirm e.currentTarget.dataset.recommentuserid===', e.currentTarget.dataset.recommentuserid);
            console.log('++++++++===delCommentConfirm e.currentTarget.dataset.recommentusername===', e.currentTarget.dataset.recommentusername);
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                that.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            if (e.currentTarget.dataset.commentid != null && !utils.equalsIgnoreCase(e.currentTarget.dataset.commentid, '')) {
                if (
                    e.currentTarget.dataset.commentid != null &&
                    e.currentTarget.dataset.recommentid != null &&
                    utils.equalsIgnoreCase(e.currentTarget.dataset.commentid, e.currentTarget.dataset.recommentid)
                ) {
                    if (utils.equalsIgnoreCase(e.currentTarget.dataset.commentuserid, that.uid)) {
                        //弹窗提示
                        that.setData({
                            alertdelcommenttype: 'delcomment',
                            alertdelcommentid: e.currentTarget.dataset.commentid,
                            isDisplayToast: true,
                            tipType: 'alert',
                            alertTitle: '确认删除该条评论吗?'
                        });
                    }
                }
            }
        },

        updateCommentConfirm: function (e) {
            console.log('++++++++===updateCommentConfirm e.currentTarget.dataset.commentid===', e.currentTarget.dataset.commentid);
            console.log('++++++++===updateCommentConfirm e.currentTarget.dataset.commentuserid===', e.currentTarget.dataset.commentuserid);
            console.log('++++++++===updateCommentConfirm e.currentTarget.dataset.commentusername===', e.currentTarget.dataset.commentusername);
            console.log('++++++++===updateCommentConfirm e.currentTarget.dataset.recommentid===', e.currentTarget.dataset.recommentid);
            console.log('++++++++===updateCommentConfirm e.currentTarget.dataset.recommentuserid===', e.currentTarget.dataset.recommentuserid);
            console.log('++++++++===updateCommentConfirm e.currentTarget.dataset.recommentusername===', e.currentTarget.dataset.recommentusername);
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                that.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            if (e.currentTarget.dataset.commentid != null && !utils.equalsIgnoreCase(e.currentTarget.dataset.commentid, '')) {
                if (
                    e.currentTarget.dataset.commentid != null &&
                    e.currentTarget.dataset.recommentid != null &&
                    utils.equalsIgnoreCase(e.currentTarget.dataset.commentid, e.currentTarget.dataset.recommentid)
                ) {
                    if (utils.equalsIgnoreCase(e.currentTarget.dataset.commentuserid, that.uid)) {
                        // //弹窗提示
                        // oThis.setData({
                        //     alertdelcommenttype: "delcomment",
                        //     alertdelcommentid: e.currentTarget.dataset.commentid,
                        //     isDisplayToast: true,
                        //     tipType: 'alert',
                        //     alertTitle: "确认删除该条评论吗?",
                        // });
                        // delCommentConfirm
                    } else {
                        that.updateCommentConfirmValue(e);
                        that.setData({
                            inputValue: '',
                            inputEmpty: true,
                            textAreaFocus: true,
                            placeholderValue: '回复' + e.currentTarget.dataset.commentusername
                        });
                    }
                } else {
                    if (utils.equalsIgnoreCase(e.currentTarget.dataset.recommentuserid, that.uid)) {
                        //弹窗提示
                        that.setData({
                            alertdelcommenttype: 'delrecomment',
                            alertdelcommentid: e.currentTarget.dataset.recommentid,
                            isDisplayToast: true,
                            tipType: 'alert',
                            alertTitle: '确认删除该条评论吗?'
                        });
                    } else {
                        that.updateCommentConfirmValue(e);
                        that.setData({
                            inputValue: '',
                            inputEmpty: true,
                            textAreaFocus: true,
                            placeholderValue: '回复' + e.currentTarget.dataset.recommentusername
                        });
                    }
                }
            } else {
                that.emptyCommentConfirm();
            }
        },

        sendCommentConfirm: function (e) {
            console.log('++++++++===sendCommentConfirm==');
            var that = this;
            uni.getNetworkType({
                success: function (res) {
                    // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
                    var networkType = res.networkType;
                    console.log('++++++++===sendCommentConfirm networkType==', networkType);
                    if (utils.equalsIgnoreCase('unknown', networkType) || utils.equalsIgnoreCase('none', networkType)) {
                        uni.showToast({
                            title: '网络似乎不太顺畅哦',
                            icon: 'none'
                        });
                    } else {
                        //未登录，弹登录框
                        if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                            that.setData({
                                bShowLoginPanel: true
                            });
                            return;
                        }
                        if (that.commentid != null && !utils.equalsIgnoreCase(that.commentid, '')) {
                            if (that.commentid != null && that.recommentid != null && utils.equalsIgnoreCase(that.commentid, that.recommentid)) {
                                if (!utils.equalsIgnoreCase(that.commentuserid, that.uid)) {
                                    if (that.inputValue != null && !utils.equalsIgnoreCase(that.inputValue, '')) {
                                        that.sendCommentReply(that.qzid, that.messageid, that.commentid, that.recommentid, that.inputValue);
                                    } else {
                                        uni.showToast({
                                            title: '没有填写评论内容哦',
                                            icon: 'none'
                                        });
                                    }
                                }
                            } else {
                                if (!utils.equalsIgnoreCase(that.recommentuserid, that.uid)) {
                                    if (that.inputValue != null && !utils.equalsIgnoreCase(that.inputValue, '')) {
                                        that.sendCommentReply(that.qzid, that.messageid, that.commentid, that.recommentid, that.inputValue);
                                    } else {
                                        uni.showToast({
                                            title: '没有填写评论内容哦',
                                            icon: 'none'
                                        });
                                    }
                                }
                            }
                        } else {
                            if (that.inputValue != null && !utils.equalsIgnoreCase(that.inputValue, '')) {
                                that.sendComment(that.qzid, that.messageid, that.inputValue);
                            } else {
                                uni.showToast({
                                    title: '没有填写评论内容哦',
                                    icon: 'none'
                                });
                            }
                        }
                    }
                }
            });
        },

        sendComment: function (qzid, messageid, text) {
            var that = this;
            var url = 'https://update.idol001.com/api_moblie_idol_userdt.php?action=commit_message_comment';
            var params = {
                qzid: qzid,
                messageid: messageid,
                text: text
            };
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    console.log('++++++++===sendComment resp===', resp);
                    if (resp.data != null && resp.data.error_code > 0) {
                        that.setData({
                            isDisplayOnlyIKnowTip: true,
                            isDisplayOnlyIKnowTipTitle: '该条动态已被删除!',
                            isDisplayOnlyIKnowTipType: 'feed'
                        });
                    } else {
                        if (resp.data != null && resp.data.comment != null && !utils.equalsIgnoreCase(resp.data.comment._id, '')) {
                            console.log('++===++sendComment success==');
                            //弹窗提示
                            that.setData({
                                isDisplayToast: true,
                                tipType: 'toast',
                                tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/95b6e4446fcfa31c4be482538cf894d2.png',
                                tipToastTitle: '评论成功'
                            });
                            setTimeout(function () {
                                that.setData({
                                    isDisplayToast: false
                                });
                            }, 2000);
                            that.emptyCommentConfirm();
                            that.refreshFeed();
                        } else {
                            console.log('++===++sendComment fail==');
                            //弹窗提示
                            that.setData({
                                isDisplayToast: true,
                                tipType: 'toast',
                                tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/95b6e4446fcfa31c4be482538cf894d2.png',
                                tipToastTitle: '评论失败'
                            });
                            setTimeout(function () {
                                that.setData({
                                    isDisplayToast: false
                                });
                            }, 2000);
                            that.emptyCommentConfirm();
                        }
                    }
                },
                function (resp) {
                    console.log('+++===++resp.data===', resp.data);
                    //弹窗提示
                    that.setData({
                        isDisplayToast: true,
                        tipType: 'toast',
                        tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/95b6e4446fcfa31c4be482538cf894d2.png',
                        tipToastTitle: '评论失败'
                    });
                    setTimeout(function () {
                        that.setData({
                            isDisplayToast: false
                        });
                    }, 2000);
                    that.emptyCommentConfirm();
                }
            );
        },

        /**
         * 删除动态
         */
        deleteDynamicCellClick: function (e) {
            var that = this;
            uni.getNetworkType({
                success: function (res) {
                    // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
                    var networkType = res.networkType;
                    console.log('++++++++===commitAttitude networkType==', networkType);
                    if (utils.equalsIgnoreCase('unknown', networkType) || utils.equalsIgnoreCase('none', networkType)) {
                        uni.showToast({
                            title: '网络似乎不太顺畅哦',
                            icon: 'none'
                        });
                    } else {
                        //弹窗提示
                        that.setData({
                            alertdelcommenttype: 'delmessage',
                            alertdelcommentid: '',
                            isDisplayToast: true,
                            tipType: 'alert',
                            alertTitle: '确定删除这条动态吗?'
                        });
                    }
                }
            });
        },

        /**
         * 删除动态请求
         */
        requestDeleteDynamic: function (objid) {
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
                        uni.showToast({
                            title: '删除成功',
                            duration: 2000
                        });
                        uni.navigateBack({
                            delta: 1
                        });
                    } else {
                        uni.showToast({
                            title: (resp.data && resp.data.error_description) || '网络似乎不太顺畅哦',
                            icon: 'none',
                            duration: 2000
                        });
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                }
            );
        },

        /**
         * alert 左边按钮点击
         */
        myAlertLeftBtnClick: function () {
            var that = this;
            that.setData({
                isDisplayToast: false
            });
        },

        /**
         * alert 右边按钮点击
         */
        myAlertRightBtnClick: function () {
            var that = this;
            that.setData(
                {
                    isDisplayToast: false
                },
                function () {
                    if (that.alertdelcommenttype != null && utils.equalsIgnoreCase(that.alertdelcommenttype, 'delmessage')) {
                        that.requestDeleteDynamic(that.messageid);
                    } else if (that.alertdelcommenttype != null && utils.equalsIgnoreCase(that.alertdelcommenttype, 'delcomment')) {
                        that.delComment(that.qzid, that.alertdelcommentid);
                    } else if (that.alertdelcommenttype != null && utils.equalsIgnoreCase(that.alertdelcommenttype, 'delrecomment')) {
                        that.delCommentReply(that.qzid, that.alertdelcommentid);
                    }
                }
            );
        },

        delComment: function (qzid, commentid) {
            var that = this;
            var url = 'https://update.idol001.com/api_moblie_idol_userdt.php?action=delete_message_comment';
            var params = {
                qzid: qzid,
                commentid: commentid
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('++++++++===delComment resp===', resp);
                if (resp.data != null && resp.data.error_code > 0) {
                    that.setData({
                        isDisplayOnlyIKnowTip: true,
                        isDisplayOnlyIKnowTipTitle: '该条评论已被删除!',
                        isDisplayOnlyIKnowTipType: 'comment'
                    });
                    that.refreshFeed();
                } else {
                    if (resp.data != null && resp.data.ok == 1) {
                        console.log('++===++delComment success==');
                        //弹窗提示
                        that.setData({
                            isDisplayToast: true,
                            tipType: 'toast',
                            tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/760594175eccc8e851e8624507e9e951.png',
                            tipToastTitle: '删除成功'
                        });
                        setTimeout(function () {
                            that.setData({
                                isDisplayToast: false
                            });
                        }, 2000);
                        that.refreshFeed();
                    } else {
                        console.log('++===++delComment fail==');
                        //弹窗提示
                        that.setData({
                            isDisplayToast: true,
                            tipType: 'toast',
                            tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/760594175eccc8e851e8624507e9e951.png',
                            tipToastTitle: '删除失败'
                        });
                        setTimeout(function () {
                            that.setData({
                                isDisplayToast: false
                            });
                        }, 2000);
                    }
                }
            });
        },

        sendCommentReply: function (qzid, messageid, commentid, recommentid, text) {
            var that = this;
            var url = 'https://update.idol001.com/api_moblie_idol_userdt.php?action=commit_message_recomment';
            var params = {
                qzid: qzid,
                messageid: messageid,
                commentid: commentid,
                recommentid: utils.equalsIgnoreCase(recommentid, commentid) ? '' : recommentid,
                text: text
            };
            net.fetchApi(url, params, 'POST').then((resp) => {
                console.log('++++++++===sendCommentReply resp===', resp);
                if (resp.data != null && resp.data.error_code > 0) {
                    that.setData({
                        isDisplayOnlyIKnowTip: true,
                        isDisplayOnlyIKnowTipTitle: '该条评论已被删除!',
                        isDisplayOnlyIKnowTipType: 'comment'
                    });
                    that.emptyCommentConfirm();
                    that.refreshFeed();
                } else {
                    if (resp.data != null && resp.data.recomment != null && !utils.equalsIgnoreCase(resp.data.recomment._id, '')) {
                        console.log('++===++sendCommentReply success==');
                        //弹窗提示
                        that.setData({
                            isDisplayToast: true,
                            tipType: 'toast',
                            tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/95b6e4446fcfa31c4be482538cf894d2.png',
                            tipToastTitle: '评论成功'
                        });
                        setTimeout(function () {
                            that.setData({
                                isDisplayToast: false
                            });
                        }, 2000);
                        that.emptyCommentConfirm();
                        that.refreshFeed();
                    } else {
                        console.log('++===++sendCommentReply fail==');
                        //弹窗提示
                        that.setData({
                            isDisplayToast: true,
                            tipType: 'toast',
                            tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/95b6e4446fcfa31c4be482538cf894d2.png',
                            tipToastTitle: '评论失败'
                        });
                        setTimeout(function () {
                            that.setData({
                                isDisplayToast: false
                            });
                        }, 2000);
                        that.emptyCommentConfirm();
                    }
                }
            });
        },

        delCommentReply: function (qzid, recommentid) {
            var that = this;
            var url = 'https://update.idol001.com/api_moblie_idol_userdt.php?action=delete_message_recomment';
            var params = {
                qzid: qzid,
                recommentid: recommentid
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('++++++++===delCommentReply resp===', resp);
                if (resp.data != null && resp.data.error_code > 0) {
                    that.setData({
                        isDisplayOnlyIKnowTip: true,
                        isDisplayOnlyIKnowTipTitle: '该条评论已被删除!',
                        isDisplayOnlyIKnowTipType: 'comment'
                    });
                    that.refreshFeed();
                } else {
                    if (resp.data != null && resp.data.ok == 1) {
                        console.log('++===++delCommentReply success==');
                        //弹窗提示
                        that.setData({
                            isDisplayToast: true,
                            tipType: 'toast',
                            tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/760594175eccc8e851e8624507e9e951.png',
                            tipToastTitle: '删除成功'
                        });
                        setTimeout(function () {
                            that.setData({
                                isDisplayToast: false
                            });
                        }, 2000);
                        that.refreshFeed();
                    } else {
                        console.log('++===++delCommentReply fail==');
                        //弹窗提示
                        that.setData({
                            isDisplayToast: true,
                            tipType: 'toast',
                            tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/760594175eccc8e851e8624507e9e951.png',
                            tipToastTitle: '删除失败'
                        });
                        setTimeout(function () {
                            that.setData({
                                isDisplayToast: false
                            });
                        }, 2000);
                    }
                }
            });
        },

        commitAttitude: function (attitude) {
            var that = this;
            uni.getNetworkType({
                success: function (res) {
                    // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
                    var networkType = res.networkType;
                    console.log('++++++++===commitAttitude networkType==', networkType);
                    if (utils.equalsIgnoreCase('unknown', networkType) || utils.equalsIgnoreCase('none', networkType)) {
                        uni.showToast({
                            title: '网络似乎不太顺畅哦',
                            icon: 'none'
                        });
                    } else {
                        var url = 'https://update.idol001.com/api_moblie_idol_userdt.php?action=commit_message_attitude';
                        var params = {
                            qzid: that.qzid,
                            objid: that.messageid,
                            attitude: attitude == 1 ? 0 : 1
                        };
                        net.fetchApi(url, params, 'GET').then(
                            (resp) => {
                                console.log('++++++++commit_message_attitude resp==', resp);
                                if (resp.data != null && resp.data.error_code > 0) {
                                    that.setData({
                                        isDisplayOnlyIKnowTip: true,
                                        isDisplayOnlyIKnowTipTitle: '该条动态已被删除!',
                                        isDisplayOnlyIKnowTipType: 'feed'
                                    });
                                    that.refreshFeed();
                                } else {
                                    if (resp.data != null && resp.data.ok == 1) {
                                        console.log('+++===++resp.data.ok == 1===');
                                        if (that.messageIsattituded == 1) {
                                            that.setData({
                                                messageIsattituded: 0
                                            });
                                        } else {
                                            that.setData({
                                                messageIsattituded: 1
                                            });
                                        }
                                    } else {
                                        console.log('+++===++resp.data.ok == 0===');
                                    }
                                }
                            },
                            function (resp) {
                                console.log('+++===++resp.data===', resp.data);
                            }
                        );
                    }
                }
            });
        },

        loginCallback: function (e) {
            console.log('++loginCallback');
            var that = this;
            var refresh = false;
            if (that.login == false && IDLoginManager.loginState() == IDLoginManager.is_has_login) {
                refresh = true;
            }
            that.loginLogicStar();
            if (e.detail.success) {
                console.log('++loginCallback e.detail.success++');
                that.setData({
                    bShowLoginPanel: false
                });
                const userInfo = globalData.idolUserInfo;
                this.setData({
                    uid: userInfo._id,
                    nickname: userInfo.nickname,
                    headImg: userInfo.image.origin_pic
                });
                //更新登陆状态
                app.globalData.isShouldCheckInLoginStatus = true;
                console.log('++++++++===loginCallback oThis.data.uid===', that.uid);
                console.log('++++++++===loginCallback oThis.data.nickname===', that.nickname);
                console.log('++++++++===loginCallback oThis.data.headImg===', that.headImg);
            } else {
                console.log('++loginCallback !e.detail.success++');
            }
            if (refresh) {
                that.setData({
                    page: 1,
                    offset: '',
                    isRequestAll: false,
                    isLoading: false
                });
                that.requestDetail();
            }
        },

        closeLoginView: function () {
            console.log('----closeLoginView----');
            var that = this;
            that.setData({
                bShowLoginPanel: false
            });
        },

        loginLogicStar: function () {
            var that = this;
            IDLoginManager.checkIsLogin(function (v) {
                if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                    console.log('++!= IDLoginManager.is_has_login');
                    that.setData({
                        login: false
                    });
                } else {
                    console.log('++= IDLoginManager.is_has_login');
                    that.setData({
                        login: true
                    });
                    const userInfo = globalData.idolUserInfo;
                    that.setData({
                        uid: userInfo._id,
                        nickname: userInfo.nickname,
                        headImg: userInfo.image.origin_pic,
                        user_info: globalData.idolUserInfo
                    });
                    console.log('++++++++===loginLogicStar oThis.data.uid===', that.uid);
                    console.log('++++++++===loginLogicStar oThis.data.nickname===', that.nickname);
                    console.log('++++++++===loginLogicStar oThis.data.headImg===', that.headImg);
                }
            });
        },

        // // 根据搜索字分割字符
        // hilight_word: function(key, word) {
        //     // console.log(">>>+++hilight_word key==" + key);
        //     // console.log(">>>+++hilight_word word==" + word);
        //     let idx = word.indexOf(key);
        //     let t = [];
        //     // console.log(">>>+++hilight_word idx==" + idx);
        //     if (idx > -1) {
        //         if (idx == 0) {
        //             t = this.hilight_word(key, word.substr(key.length));
        //             t.unshift({
        //                 key: true,
        //                 str: key
        //             });
        //             return t;
        //         }
        //         if (idx > 0) {
        //             t = this.hilight_word(key, word.substr(idx));
        //             t.unshift({
        //                 key: false,
        //                 str: word.substring(0, idx)
        //             });
        //             return t;
        //         }
        //     }
        //     return [{
        //         key: false,
        //         str: word
        //     }];
        // },

        jumpCommentDetail: function (e) {
            var commentid = e.currentTarget.dataset.commentid;
            console.log('++jumpCommentDetail commentid==' + commentid);
            var that = this;
            uni.navigateTo({
                url: '../user_feed_comment_detail/user_feed_comment_detail?qz_id=' + that.qzid + '&message_id=' + that.messageid + '&comment_id=' + commentid
            });
        },

        updateAttitude: function (e) {
            var that = this;
            console.log('++++++++===updateAttitude oThis.data.messageIsattituded==', that.messageIsattituded);
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                that.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            that.commitAttitude(that.messageIsattituded);
        },

        userhonorClick() {
            console.log('占位：函数 userhonorClick 未声明');
        },

        attitudeDynamicCellClick() {
            console.log('占位：函数 attitudeDynamicCellClick 未声明');
        },

        shareBtnClick() {
            console.log('占位：函数 shareBtnClick 未声明');
        }
    }
};
</script>
<style>
@import './user_feed_detail.css';
@import '@/templates/dynamicCellList/dynamicCellList.css';
@import '@/..';
</style>
