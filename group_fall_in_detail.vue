<template>
    <view style="height: 100%">
        <!-- pages/group_fall_in_detail/group_fall_in_detail.wxml -->
        <block v-if="is_main_group">
            <view v-if="bShowMainPage">
                <template name="loading_holder">
                    <view class="loading_holder_cell">
                        <view class="loading_holder_cell_img"></view>
                        <view class="loading_holder_cell_desc"></view>
                    </view>
                </template>

                <!-- template对应的原始代码，为保证正常显示，已对其进行隐藏。 -->
                <block name="group_managerList_cell" v-if="false">
                    <view class="group_managerList_frame">
                        <idc_image
                            :isShowPendant="true"
                            :pendant="item.userinfo.head_frame"
                            :width="96"
                            :height="96"
                            :padding="12"
                            class="group_managerList_userIcon"
                            :lazy-load="true"
                            :src="item.userinfo.image.thumbnail_pic"
                            place-holder-img="https://star-img.xingxiaoculture.com/2022/04/20/3d62644aba1b2e160208292b43c74567.png"
                            radius="96rpx"
                            @tap.native="userImgClick($event, { userinfo: item.userinfo })"
                            :data-userinfo="item.userinfo"
                        ></idc_image>

                        <view class="group_managerList_username_layout">
                            <text class="group_managerList_username" :style="'color:#' + (item.userinfo.is_vip == 1 ? 'F7941C' : '000000')">
                                {{ myRankObject.userinfo.nickname }}
                            </text>
                            <image
                                v-if="item.userinfo.is_vip == 1"
                                class="group_managerList_username_vip_icon"
                                src="https://star-img.xingxiaoculture.com/search/topics/images/2020/8/4/ecKCCpRzeR1596530582740.png"
                            ></image>
                            <image v-if="item.userinfo.medal" class="group_managerList_username_icon" :src="item.userinfo.medal.img"></image>
                        </view>

                        <view class="groupManager_list_cell_tag" v-if="item.is_excellent_admin">优秀公会管</view>

                        <view class="group_managerList_contentView">
                            <text class="group_managerList_text">今日集结{{ item.userinfo.gather_times }}次</text>
                        </view>
                        <view class="group_managerList_right_content">
                            <image class="group_managerList_icon" src="/static/pages/subpages_v2/images/personalMainPage/flowersNum.png" role="img"></image>
                            <text class="group_managerList_num">{{ item.guard_angle.total_flowers_count }}</text>
                        </view>

                        <block v-if="item.has_sended == 1">
                            <view
                                class="group_managerList_button"
                                style="background: linear-gradient(90deg, rgba(255, 181, 164, 1) 0%, rgba(255, 164, 199, 1) 100%)"
                                @tap="rewardFlowerClick"
                                :data-userid="item.userinfo._id"
                            >
                                已送
                            </view>
                        </block>
                        <block v-else>
                            <view
                                class="group_managerList_button"
                                style="background: linear-gradient(90deg, rgba(255, 127, 98, 1) 0%, rgba(255, 72, 141, 1) 100%)"
                                @tap="rewardFlowerClick"
                                :data-userid="item.userinfo._id"
                            >
                                送花
                            </view>
                        </block>
                    </view>
                </block>
                <view class="banner-bg">
                    <swiper
                        class="top_swiper"
                        :indicator-dots="!!topSwiperItems && topSwiperItems.length > 1"
                        indicator-color="rgba(255, 255, 255, .3)"
                        indicator-active-color="rgba(255, 255, 255, .6)"
                        :circular="true"
                        :autoplay="true"
                        :style="'display:' + (!!topSwiperItems ? 'block' : 'none') + '; margin: 0 auto;'"
                    >
                        <block v-for="(item, index) in topSwiperItems" :key="index">
                            <swiper-item style="border-radius: 5px">
                                <idc_image
                                    @tap.native="clickSwiperItems($event, { ind: index })"
                                    :data-ind="index"
                                    mode="aspectFill"
                                    style="width: 100%; height: 100%"
                                    :src="item.img"
                                    place-holder-img="https://star-img.xingxiaoculture.com/2019/08/09/475a85edf03d255fae7d30b2efa5a3b1.png"
                                ></idc_image>

                                <block v-if="item.starinfo">
                                    <image class="top_swiper_rl_no1" src="https://star-img.xingxiaoculture.com/2019/08/09/54c892e6713b2211f2969f4ac07b5e64.png">
                                        <view class="top_swiper_rl_no1_dec">
                                            {{
                                                item.type == 'week' ? '上周' + (rankType == 3 ? '总' : '新势力') + '榜冠军' : '上月' + (rankType == 3 ? '总' : '新势力') + '榜冠军'
                                            }}
                                        </view>
                                    </image>
                                    <view class="top_swiper_rl_no1_name">{{ myGuardStarItem.starinfo.name }}</view>
                                </block>
                                <block v-else>
                                    <view class="top_swiper_desc">{{ item.title }}</view>
                                </block>
                            </swiper-item>
                        </block>
                    </swiper>
                </view>
                <view class="mainView" style="width: 100%">
                    <view v-if="isShowButton == 1" class="goHomeBackFansPage" @tap="goHomeBackFansPageClick">回到公会></view>
                    <!-- 头像信息 -->
                    <view class="top_starInfo_personNum_contentView">
                        <view class="top_starInfo_outside">
                            <!-- 今日记录，规则 -->
                            <view class="todayRecord_and_rule_contentView">
                                <button class="today_record_group" data-noadd="1" @tap="todayRecordClick">
                                    <image class="today_record_icon" src="https://star-img.xingxiaoculture.com/2019/08/09/b4a21d4e535bb049fcf2dd7436894eed.png"></image>
                                    集结记录
                                </button>
                                <button class="rule_group" data-noadd="1" @tap="ruleClick">
                                    <image class="rule_group_icon" src="https://star-img.xingxiaoculture.com/2019/08/09/7fef511c8414f523df0bfffc92ff05e1.png"></image>
                                    规则
                                </button>
                            </view>

                            <image
                                class="todayRecord_tips"
                                src="https://img.xingxiaoculture.com/origin/2019/12/10/45f4dbfb53415a24b52164eef10bd2a11575950601.png"
                                v-if="bShowBeAttackTimesView"
                            >
                                <text class="todayRecord_tips_text">
                                    最近有
                                    <text style="color: #ff5e65">{{ groupBeSkillTimes }}</text>
                                    条被攻击记录快去回击！
                                </text>
                            </image>
                            <view class="top_starinfo_icon_bg"></view>
                            <idc_image class="top_starInfo_icon" radius="50%" :src="starIcon"></idc_image>
                            <view class="top_starName_content">
                                <view class="top_starInfo_left">
                                    <view class="top_starInfo_name">
                                        {{ groupInfo.name }}
                                        <!-- <text class="theMain_group" bindtap='gotoConcentrate'>去送星星</text> -->
                                    </view>
                                    <!-- 月排行 新势力排行 -->
                                    <view class="top_starRank_content">
                                        <!-- <swiper class='top_top_star_swiper' autoplay="{{swiperAutoPlay}}" vertical="true" circular="true" wx:if="{{starRankScroll}}"> -->
                                        <!-- <swiper-item wx:for="{{starRankScroll}}" class="top_swiper_item_out" style wx:key="index" data-rank-type="{{item.rank_type}}" data-rank-id="{{item.rank_id}}" bindtap="gotoRankPage"> -->
                                        <view class="top_star_swiper_item" v-if="starRankScroll">
                                            <text class="top_star_swiper_item_txt" style="color: #ffffff; font-size: 12px; line-height: 16px">
                                                公会潮汐墙(本月)：NO.{{ starRankScroll }} >
                                            </text>
                                        </view>
                                        <!-- </swiper-item> -->
                                        <!-- </swiper> -->
                                        <swiper class="top_top_star_swiper" :autoplay="swiperAutoPlay" :vertical="true" :circular="true" v-if="starRankScroll.length == 0">
                                            <swiper-item class="top_swiper_item_out" style v-for="(item, index) in 1" :key="index">
                                                <view class="top_star_swiper_item">
                                                    <!-- <text class="top_star_swiper_item_txt" style="color:#FFFFFF;font-size:12px;line-height:16px;">月总排行：未上排行></text> -->
                                                </view>
                                            </swiper-item>
                                        </swiper>
                                    </view>
                                </view>
                            </view>
                            <!-- <view class="top_right_button" bindtap="gotoConcentrate">去送星星</view> -->
                        </view>
                    </view>

                    <image v-if="spreadImg.img_src" class="group_join_act_image" :src="spreadImg.img_src" @tap="groupJoinActImage"></image>
                    <image v-if="spreadImg.animate_img_src" class="share_group_icon daily_welfare_animate" @tap="groupJoinActImage" :src="spreadImg.animate_img_src"></image>

                    <!-- 集结贡献排行 -->
                    <!-- <view class="section-box">
                <view class="section-box-title">集结贡献排行</view>
                <view class="section-box-right-button" bindtap="fallInContributeRank">排行详情></view>
                <view class="section-box-line_view"></view>
                <view class="section-box-contribute-rank-content">
                    <view class="section-box-contribute-rank-groupName">{{groupInfo.name}}</view>

                    <view class="section-box-contribute-rank-rankNum">
                        <block wx:if="{{groupAttributeRankArr.length == 0}}">
                            <view class="top_star_swiper_item">
                                <text class="top_star_swiper_item_txt" style="color:#FFFFFF;font-size:12px;line-height:16px;">未上排行</text>
                            </view>
                        </block>

                        <block wx:else>
                            <swiper class='top_top_star_swiper' autoplay="{{swiperAutoPlay}}" vertical="true" circular="true" wx:if="{{groupAttributeRankArr.length > 0}}">
                                <swiper-item wx:for="{{groupAttributeRankArr}}" class="top_swiper_item_out">
                                    <view class="top_star_swiper_item" style="float:right;">
                                        <text claws="top_star_swiper_item_txt" style="color:#FF5E65;font-size:26rpx;line-height:16px;font-family: PingFangSC, PingFangSC-Semibold;font-weight: 600;">{{item}}</text>
                                    </view>
                                </swiper-item>
                            </swiper>
                        </block>
                    </view>
                </view>
            </view> -->

                    <!-- 公会动态 -->
                    <view class="div_dongtai" v-if="dtObj && dtObj.data_quanzi">
                        <text class="txt_dt_title">公会动态</text>
                        <text class="text_dt_edt" @tap="gotoPublishPage">发布</text>
                        <view class="div_imgs" v-if="dtObj && dtObj.data_quanzi && dtObj.data_quanzi.images" @tap="dynamicCellItemClick" :data-item="dtObj">
                            <block v-if="index <= 2" v-for="(item, index) in dtObj.data_quanzi.images" :key="index">
                                <image
                                    class="img_dt"
                                    :style="index != imgs.length - 1 ? 'margin-right: 22rpx;' : ''"
                                    :src="dtObj.data_quanzi.images[index].img_url.origin_pic"
                                    mode="aspectFill"
                                ></image>
                            </block>
                        </view>
                        <text @tap="dynamicCellItemClick" :data-item="dtObj" class="txt_dt_content" v-if="dtObj.data_quanzi.text">{{ dtObj.data_quanzi.text }}</text>
                    </view>

                    <!-- 公会雪人被偷引导 -->
                    <image
                        v-if="bWarStart"
                        class="img_snowman"
                        src="https://star-img.xingxiaoculture.com/search/topics/images/2021/12/18/FHPxpFJwsr1639811699313.png"
                        @tap="goSnowmanWar"
                    ></image>

                    <!-- 本公会管理 -->
                    <view class="section-box" v-if="groupManagerList.length > 0">
                        <view class="section-box-title">本公会管理</view>
                        <block v-if="groupManagerList.length > 0">
                            <block v-for="(item, index) in groupManagerList" :key="index">
                                <!-- parse <template is="group_managerList_cell" :data="item:item"></template> -->
                                <block name="group_managerList_cell">
                                    <view class="group_managerList_frame">
                                        <idc_image
                                            :isShowPendant="true"
                                            :pendant="item.userinfo.head_frame"
                                            :width="96"
                                            :height="96"
                                            :padding="12"
                                            class="group_managerList_userIcon"
                                            :lazy-load="true"
                                            :src="item.userinfo.image.thumbnail_pic"
                                            place-holder-img="https://star-img.xingxiaoculture.com/2022/04/20/3d62644aba1b2e160208292b43c74567.png"
                                            radius="96rpx"
                                            @tap.native="userImgClick($event, { userinfo: item.userinfo })"
                                            :data-userinfo="item.userinfo"
                                        ></idc_image>

                                        <view class="group_managerList_username_layout">
                                            <text class="group_managerList_username" :style="'color:#' + (item.userinfo.is_vip == 1 ? 'F7941C' : '000000')">
                                                {{ myRankObject.userinfo.nickname }}
                                            </text>
                                            <image
                                                v-if="item.userinfo.is_vip == 1"
                                                class="group_managerList_username_vip_icon"
                                                src="https://star-img.xingxiaoculture.com/search/topics/images/2020/8/4/ecKCCpRzeR1596530582740.png"
                                            ></image>
                                            <image v-if="item.userinfo.medal" class="group_managerList_username_icon" :src="item.userinfo.medal.img"></image>
                                        </view>

                                        <view class="groupManager_list_cell_tag" v-if="item.is_excellent_admin">优秀公会管</view>

                                        <view class="group_managerList_contentView">
                                            <text class="group_managerList_text">今日集结{{ item.userinfo.gather_times }}次</text>
                                        </view>
                                        <view class="group_managerList_right_content">
                                            <image class="group_managerList_icon" src="/static/pages/subpages_v2/images/personalMainPage/flowersNum.png" role="img"></image>
                                            <text class="group_managerList_num">{{ item.guard_angle.total_flowers_count }}</text>
                                        </view>

                                        <block v-if="item.has_sended == 1">
                                            <view
                                                class="group_managerList_button"
                                                style="background: linear-gradient(90deg, rgba(255, 181, 164, 1) 0%, rgba(255, 164, 199, 1) 100%)"
                                                @tap="rewardFlowerClick"
                                                :data-userid="item.userinfo._id"
                                            >
                                                已送
                                            </view>
                                        </block>
                                        <block v-else>
                                            <view
                                                class="group_managerList_button"
                                                style="background: linear-gradient(90deg, rgba(255, 127, 98, 1) 0%, rgba(255, 72, 141, 1) 100%)"
                                                @tap="rewardFlowerClick"
                                                :data-userid="item.userinfo._id"
                                            >
                                                送花
                                            </view>
                                        </block>
                                    </view>
                                </block>
                            </block>
                        </block>
                    </view>

                    <!-- 我今日集结次数 -->
                    <view class="section-box-my" v-if="isLoginStatus">
                        <view class="section-box-title">我今日集结任务进展</view>
                        <view class="section-box-process-wrap">
                            <view class="section-box-process-active" :style="'width: ' + (groupSupportValue > 100 ? 100 : groupSupportValue) + '%'"></view>
                            <view class="section-box-process-num" :style="'left: ' + (groupSupportValue > 100 ? 100 : groupSupportValue) + '%'">{{ nowGroupTimes }}</view>
                        </view>

                        <view class="section-box-items">
                            <block v-for="(item, index) in groupTaskList.list" :key="index">
                                <view :class="groupTaskList.gather_times < item.task_num ? 'section-box-item' : 'section-box-item active'">
                                    <view class="section-box-item-title">参与{{ item.task_num }}次</view>
                                    <block v-for="(item, index1) in item.now_gift" :key="index1">
                                        <view class="section-box-item-txt1">{{ pageDataObj.single_user_rank_info_all_time.text }}</view>
                                    </block>
                                    <image class="section-box-item-icon" src="/static/pages/subpages_v2/images/common/icon-tick.png" />
                                </view>
                            </block>
                        </view>

                        <view class="vip_card">
                            <vip_guide_card
                                :refresh="refreshVipcard"
                                uiType="1"
                                :isGather="true"
                                :isSet="isSet"
                                :gid="WeChatGroupGid"
                                :starid="starIDStr"
                                :starName="starName"
                                @changeSwitch="onChangeSwitch"
                                @buyVip="buyVip"
                            ></vip_guide_card>
                        </view>
                    </view>

                    <!-- 头像信息，人数，贡献点 -->
                    <view class="starInfo_personNum_contentView">
                        <image class="starInfo_outside">
                            <!-- 今日记录，规则 -->
                            <view class="todayRecord_and_rule_contentView">
                                <button class="today_record_group" data-noadd="1" @tap="todayRecordClick">
                                    <image class="today_record_icon" src="https://star-img.xingxiaoculture.com/2019/08/09/b4a21d4e535bb049fcf2dd7436894eed.png"></image>
                                    集结记录
                                </button>

                                <button class="rule_group" data-noadd="1" @tap="ruleClick">
                                    <image class="rule_group_icon" src="https://star-img.xingxiaoculture.com/2019/08/09/7fef511c8414f523df0bfffc92ff05e1.png"></image>
                                    规则
                                </button>
                            </view>

                            <image
                                class="todayRecord_tips"
                                src="https://img.xingxiaoculture.com/origin/2019/12/10/45f4dbfb53415a24b52164eef10bd2a11575950601.png"
                                v-if="bShowBeAttackTimesView"
                            >
                                <text class="todayRecord_tips_text">
                                    最近有
                                    <text style="color: #ff5e65">{{ groupBeSkillTimes }}</text>
                                    条被攻击记录快去回击！
                                </text>
                            </image>

                            <!-- 公会集结被冻结状态 -->
                            <block v-if="bGroupBeFreeze">
                                <view class="success_fall_in_view" style="color: #0091ff">
                                    <image
                                        class="success_fall_in_icon"
                                        src="http://img.xingxiaoculture.com/origin/2019/12/04/c779898eac160ffa489ab9c5f6c30b7e1575424639.png"
                                    ></image>
                                    本公会集结冻结中
                                </view>
                            </block>

                            <block v-else>
                                <block v-if="joinUserList.length >= 3 && join_left_time_num == 0">
                                    <view class="success_fall_in_view">
                                        <image class="success_fall_in_icon" src="https://star-img.xingxiaoculture.com/2019/08/09/00426c57d9f2937f8e8293fc0f84ce9a.png"></image>
                                        集结成功 共获得星星+{{ total_guard_votes }}
                                    </view>
                                </block>

                                <block v-else-if="joinUserList.length < 3 && joinUserList.length > 0 && join_left_time_num == 0">
                                    <view class="success_fall_in_view">集结失败 继续加油!</view>
                                </block>

                                <block v-else-if="join_left_time_num">
                                    <view class="personNum_and_support_text">
                                        当前人数：
                                        <view class="personNum_and_support_num" style="display: inline-block">
                                            {{ total_member_num }}
                                        </view>
                                        获得星星：
                                        <view class="personNum_and_support_num" style="display: inline-block">
                                            {{ total_guard_votes }}
                                        </view>
                                    </view>
                                </block>
                            </block>
                        </image>
                        <!-- 守护兽 -->
                        <!-- x:if="{{join_left_time_num}}" -->
                        <view style="position: relative">
                            <image
                                @tap="showGuardianBeastDialog"
                                style="z-index: 1"
                                class="guardian_beast"
                                v-if="guardInfo.is_display && guardInfo.is_display == 1"
                                :src="guardInfo.img1"
                            ></image>
                        </view>
                    </view>

                    <block v-if="join_left_time_num || next_left_time_num || bGroupBeFreeze">
                        <image class="fall_in_time_bg_view" src="/static/pages/subpages_v2/images/common/group_line.png"></image>
                    </block>

                    <view class="contentView">
                        <block v-if="bGroupBeFreeze">
                            <button class="rule_group_frezze" data-noadd="1" @tap="attackRuleClick">
                                <image class="rule_group_frezze_icon" src="http://img.xingxiaoculture.com/origin/2019/12/05/4763cbaa6239dbf2543a301dd6c02e441575518120.png"></image>
                                解冻规则
                            </button>

                            <view class="has_done_fall_in_time_view" style="background: #ffffff; width: 100%; font-size: 12px">
                                攻击来自:{{ frezzeAttackFrom.from_star_name }}贡献?公会
                            </view>

                            <block v-if="frezzeArr.length == 0">
                                <view class="has_done_fall_in_time_view" style="background: #c7e7ff; width: 88%; margin-top: 10px; font-size: 12px">
                                    <text style="color: #ff5a84">{{ groupSkillInfo.skill.time_limit / 60 }}</text>
                                    分钟内
                                    <text style="color: #ff5a84">{{ groupSkillInfo.skill.min_people }}</text>
                                    位本公会成员助力方可解冻
                                </view>
                            </block>

                            <block v-else-if="groupSkillInfo.skill_status == 1">
                                <view class="has_done_fall_in_time_view" style="background: #c7e7ff; width: 88%; margin-top: 10px; font-size: 12px">
                                    <text style="color: #fe395b">解冻技能成功 正在释放技能</text>
                                </view>
                            </block>

                            <block v-else-if="groupSkillInfo.skill_status == -1">
                                <view class="has_done_fall_in_time_view" style="background: #c7e7ff; width: 88%; margin-top: 10px; font-size: 12px">
                                    公会集结解冻还需
                                    <text style="color: #ff5a84">{{ groupSkillInfo.skill.min_people - groupSkillInfo.member_num }}</text>
                                    人助力 助力剩余
                                    <text style="color: #ff5a84">{{ skillTimes }}</text>
                                    秒
                                </view>
                            </block>

                            <block v-else-if="groupSkillInfo.skill_status == 0">
                                <view class="has_done_fall_in_time_view" style="background: #c7e7ff; width: 88%; margin-top: 10px; font-size: 12px">
                                    <text style="color: #73abd5">技能失败 助力人数不足未能解冻公会集结</text>
                                </view>
                            </block>
                            <view class="user_item_top_view" style="height: 5px"></view>
                            <block v-for="(item, index) in groupSkillInfo.skill.min_people" :key="index">
                                <block v-if="frezzeArr[index]">
                                    <view class="user_item_contentView" :data-starid="item.sid" @tap="toUserHomePage" :data-userinfo="frezzeArr[index]">
                                        <image
                                            class="user_item_img"
                                            :src="
                                                frezzeArr[index].image.thumbnail_pic.length > 0
                                                    ? frezzeArr[index].image.thumbnail_pic
                                                    : 'https://star-img.xingxiaoculture.com/2022/04/24/79a49fcc69ca20f710cd86673cf537db.png'
                                            "
                                        >
                                            <view v-if="index == 0" class="user_item_tip_text" style="margin-left: 10px">发起人</view>
                                        </image>
                                        <view class="user_item_txt_content">
                                            <view class="user_item_txt">{{ frezzeArr[index].nickname }}</view>
                                        </view>
                                    </view>
                                </block>

                                <block v-else-if="!frezzeArr[index]">
                                    <view class="user_item_contentView" :data-starid="item.sid" @tap="toUserHomePage" :data-userinfo="frezzeArr[index + 3]">
                                        <image class="user_item_img" src="https://star-img.xingxiaoculture.com/2019/08/09/86cab7f7730ca25915d093e41e02b97f.png"></image>
                                        <view class="user_item_txt"></view>
                                    </view>
                                </block>
                            </block>

                            <view class="bottom_refresh" :style="'display:' + (showBottomRefresh ? 'block' : 'none') + ';'">
                                <refresh_view :is-animation="bottomAnimation"></refresh_view>
                            </view>
                        </block>

                        <block v-else>
                            <!-- 集结剩余时间 -->
                            <block v-if="join_left_time_num">
                                <view class="guard-tip" v-if="guardInfo.is_display && guardInfo.is_display == 1">{{ guardInfo.text }}</view>
                                <view class="fall_in_time_view">
                                    <view style="display: inline-block">本公会集结剩余时间:</view>
                                    <view class="fall_in_time_view_tiemNum" style="display: inline-block">{{ join_left_time }}</view>
                                </view>
                            </block>

                            <block v-else-if="next_left_time_num">
                                <view class="fall_in_time_view">
                                    <text>距本公会下次发起剩:</text>
                                    <view class="fall_in_time_view_tiemNum" style="display: inline-block">{{ next_left_time }}</view>
                                </view>
                            </block>

                            <!-- 三人集结部分视图 -->
                            <view class="user_item_top_view">
                                <block v-for="(item, index) in 'aaa'" :key="index">
                                    <block v-if="joinUserList[index]">
                                        <view class="user_item_contentView" :data-starid="item.sid" @tap="toUserHomePage" :data-userinfo="joinUserList[index]">
                                            <block v-if="joinUserList[index].angle.special == 2 && joinUserList[index].angle.not_renew_type == 2">
                                                <image
                                                    class="angel_wings_benben"
                                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2021/1/14/rSpBPwBA8K1610598332066.gif"
                                                ></image>
                                            </block>
                                            <block v-else-if="joinUserList[index].angle.special == 3 && joinUserList[index].angle.not_renew_type == 3">
                                                <image
                                                    class="angel_wings_benben"
                                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2022/1/19/PrxbpEc5Fz1642561959890.gif"
                                                ></image>
                                            </block>
                                            <block v-else-if="joinUserList[index].angle.special == 4 && joinUserList[index].angle.not_renew_type == 4">
                                                <image
                                                    class="angel_wings_benben"
                                                    src="https://star-img.xingxiaoculture.com/2022/12/14/d31237edded43ef0735023a0daa31886.gif"
                                                ></image>
                                            </block>
                                            <block v-else-if="joinUserList[index].angle.special == 1">
                                                <image
                                                    class="angel_wings_special"
                                                    src="https://img.xingxiaoculture.com/origin/2020/01/08/e0999cc1cb29636dfc52fb6b1964de241578471245.gif"
                                                ></image>
                                            </block>
                                            <block v-else-if="joinUserList[index].angle.acc_count > 0">
                                                <image
                                                    class="angel_wings"
                                                    src="https://img.xingxiaoculture.com/origin/2020/01/08/f6071534fb947dc505d96c1f8e34c78d1578471481.gif"
                                                ></image>
                                            </block>
                                            <block v-else-if="joinUserList[index].head_frame">
                                                <image class="img_head_frame" :src="joinUserList[index].head_frame.icon"></image>
                                            </block>

                                            <image
                                                class="user_item_img"
                                                :src="
                                                    joinUserList[index].image.thumbnail_pic.length > 0
                                                        ? joinUserList[index].image.thumbnail_pic
                                                        : 'https://star-img.xingxiaoculture.com/2022/04/24/79a49fcc69ca20f710cd86673cf537db.png'
                                                "
                                            >
                                                <view v-if="index == 0" class="user_item_tip_text" style="margin-left: 10px">发起人</view>
                                                <view v-else-if="index == 1" class="user_item_tip_text">先锋1号</view>
                                                <view v-else-if="index == 2" class="user_item_tip_text">先锋2号</view>
                                            </image>

                                            <view class="user_item_txt_content">
                                                <view class="user_item_txt" :style="'color:#' + (joinUserList[index].is_vip == 1 ? 'ff872d' : '666666')">
                                                    {{ joinUserList[index].nickname }}
                                                </view>
                                            </view>
                                            <view class="user_item_txt_content">
                                                <view class="user_group_manager_tips" v-if="joinUserList[index].is_admin">公会管</view>
                                                <view
                                                    class="user_group_manager_tips"
                                                    v-if="joinUserPlatforms[index].platform == 'android' || joinUserPlatforms[index].platform == 'ios'"
                                                >
                                                    APP
                                                </view>
                                                <image
                                                    v-if="joinUserList[index].is_vip == 1"
                                                    class="user_item_txt_vip_icon"
                                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2020/8/4/ecKCCpRzeR1596530582740.png"
                                                ></image>
                                                <image v-if="joinUserList[index].medal" class="user_item_txt_icon" :src="joinUserList[index].medal.img"></image>
                                            </view>
                                            <view class="user_support_times" v-if="joinUserList[index].gather_times > 0">今日集结 {{ joinUserList[index].gather_times }}次</view>
                                        </view>
                                    </block>

                                    <block v-else-if="!joinUserList[index]">
                                        <view class="user_item_contentView" :data-starid="item.sid" @tap="toUserHomePage" data-userinfo="joinUserList[index]">
                                            <image class="user_item_img" src="https://star-img.xingxiaoculture.com/2019/08/09/86cab7f7730ca25915d093e41e02b97f.png">
                                                <view v-if="index == 0" class="user_item_tip_text" style="margin-left: 10px">发起人</view>
                                                <view v-else-if="index == 1" class="user_item_tip_text">先锋1号</view>
                                                <view v-else-if="index == 2" class="user_item_tip_text">先锋2号</view>
                                            </image>
                                            <view class="user_item_txt"></view>
                                        </view>
                                    </block>
                                </block>
                                <view class="user_item_top_tips_view">
                                    <view class="user_item_title">至少3人集结视为成功，每加1人总体至少获得+10星星</view>
                                </view>
                            </view>

                            <block v-for="(item, index) in 'aaaaaa'" :key="index">
                                <block v-if="joinUserList[index + 3]">
                                    <view class="user_item_contentView" :data-starid="item.sid" @tap="toUserHomePage" :data-userinfo="joinUserList[index + 3]">
                                        <block v-if="joinUserList[index + 3].angle.special == 2 && joinUserList[index + 3].angle.not_renew_type == 2">
                                            <image
                                                class="angel_wings_benben"
                                                src="https://star-img.xingxiaoculture.com/search/topics/images/2021/1/14/rSpBPwBA8K1610598332066.gif"
                                            ></image>
                                        </block>
                                        <block v-else-if="joinUserList[index + 3].angle.special == 3 && joinUserList[index + 3].angle.not_renew_type == 3">
                                            <image
                                                class="angel_wings_benben"
                                                src="https://star-img.xingxiaoculture.com/search/topics/images/2022/1/19/PrxbpEc5Fz1642561959890.gif"
                                            ></image>
                                        </block>
                                        <block v-else-if="joinUserList[index + 3].angle.special == 4 && joinUserList[index + 3].angle.not_renew_type == 4">
                                            <image class="angel_wings_benben" src="https://star-img.xingxiaoculture.com/2022/12/14/d31237edded43ef0735023a0daa31886.gif"></image>
                                        </block>
                                        <block v-else-if="joinUserList[index + 3].angle.special == 1">
                                            <image
                                                class="angel_wings_special"
                                                src="https://img.xingxiaoculture.com/origin/2020/01/08/e0999cc1cb29636dfc52fb6b1964de241578471245.gif"
                                            ></image>
                                        </block>
                                        <block v-else-if="joinUserList[index + 3].angle.acc_count > 0">
                                            <image
                                                class="angel_wings"
                                                src="https://img.xingxiaoculture.com/origin/2020/01/08/f6071534fb947dc505d96c1f8e34c78d1578471481.gif"
                                            ></image>
                                        </block>
                                        <block v-else-if="joinUserList[index + 3].head_frame">
                                            <image class="img_head_frame" :src="joinUserList[index].head_frame.icon"></image>
                                        </block>

                                        <image
                                            class="user_item_img"
                                            :src="
                                                joinUserList[index + 3].image.thumbnail_pic.length > 0
                                                    ? joinUserList[index + 3].image.thumbnail_pic
                                                    : 'https://star-img.xingxiaoculture.com/2022/04/24/79a49fcc69ca20f710cd86673cf537db.png'
                                            "
                                        >
                                            <view class="user_item_tip_text" style="background-color: #ffaf2b; margin-left: 25px">
                                                +{{ joinUserList[index + 3].grou_gather_vote_times > 0 ? joinUserList[index + 3].grou_gather_vote_times : 10 }}
                                            </view>
                                        </image>

                                        <view class="user_item_txt_content">
                                            <view class="user_item_txt" :style="'color:#' + (joinUserList[index + 3].is_vip == 1 ? 'ff872d' : '666666')">
                                                {{ joinUserList[index + 3].nickname }}
                                            </view>
                                        </view>
                                        <view class="user_item_txt_content">
                                            <view class="user_group_manager_tips" v-if="joinUserList[index + 3].is_admin">公会管</view>
                                            <view
                                                class="user_group_manager_tips"
                                                v-if="joinUserPlatforms[index + 3].platform == 'android' || joinUserPlatforms[index + 3].platform == 'ios'"
                                            >
                                                APP
                                            </view>
                                            <image
                                                v-if="joinUserList[index + 3].is_vip == 1"
                                                class="user_item_txt_vip_icon"
                                                src="https://star-img.xingxiaoculture.com/search/topics/images/2020/8/4/ecKCCpRzeR1596530582740.png"
                                            ></image>
                                            <image v-if="joinUserList[index + 3].medal" class="user_item_txt_icon" :src="joinUserList[index + 3].medal.img"></image>
                                        </view>
                                        <view class="user_support_times" v-if="joinUserList[index + 3].gather_times > 0">
                                            今日集结 {{ joinUserList[index + 3].gather_times }}次
                                        </view>
                                    </view>
                                </block>

                                <block v-else-if="!joinUserList[index + 3]">
                                    <view class="user_item_contentView" :data-starid="item.sid" @tap="toUserHomePage" :data-userinfo="joinUserList[index + 3]">
                                        <image class="user_item_img" src="https://star-img.xingxiaoculture.com/2019/08/09/86cab7f7730ca25915d093e41e02b97f.png">
                                            <view class="user_item_tip_text" style="background-color: #ffaf2b; margin-left: 25px">+10</view>
                                        </image>
                                        <view class="user_item_txt"></view>
                                    </view>
                                </block>
                            </block>

                            <block v-if="joinUserList.length >= 9">
                                <block v-for="(item, index) in joinUserList" :key="index">
                                    <block v-if="index >= 9 && joinUserList[index]">
                                        <view class="user_item_contentView" :data-starid="item.sid" @tap="toUserHomePage" :data-userinfo="joinUserList[index]">
                                            <block v-if="joinUserList[index].angle.special == 2 && joinUserList[index].angle.not_renew_type == 2">
                                                <image
                                                    class="angel_wings_benben"
                                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2021/1/14/rSpBPwBA8K1610598332066.gif"
                                                ></image>
                                            </block>
                                            <block v-else-if="joinUserList[index].angle.special == 3 && joinUserList[index].angle.not_renew_type == 3">
                                                <image
                                                    class="angel_wings_benben"
                                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2022/1/19/PrxbpEc5Fz1642561959890.gif"
                                                ></image>
                                            </block>
                                            <block v-else-if="joinUserList[index].angle.special == 1">
                                                <image
                                                    class="angel_wings_special"
                                                    src="https://img.xingxiaoculture.com/origin/2020/01/08/e0999cc1cb29636dfc52fb6b1964de241578471245.gif"
                                                ></image>
                                            </block>
                                            <block v-else-if="joinUserList[index].angle.acc_count > 0">
                                                <image
                                                    class="angel_wings"
                                                    src="https://img.xingxiaoculture.com/origin/2020/01/08/f6071534fb947dc505d96c1f8e34c78d1578471481.gif"
                                                ></image>
                                            </block>
                                            <block v-else-if="joinUserList[index].head_frame">
                                                <image class="img_head_frame" :src="joinUserList[index].head_frame.icon"></image>
                                            </block>

                                            <image
                                                class="user_item_img"
                                                :src="
                                                    joinUserList[index].image.thumbnail_pic.length > 0
                                                        ? joinUserList[index].image.thumbnail_pic
                                                        : 'https://star-img.xingxiaoculture.com/2022/04/24/79a49fcc69ca20f710cd86673cf537db.png'
                                                "
                                            >
                                                <view class="user_item_tip_text" style="background-color: #ffaf2b; margin-left: 25px">
                                                    +{{ joinUserList[index].grou_gather_vote_times > 0 ? joinUserList[index].grou_gather_vote_times : 10 }}
                                                </view>
                                            </image>

                                            <view class="user_item_txt_content">
                                                <view class="user_item_txt" :style="'color:#' + (joinUserList[index].is_vip == 1 ? 'ff872d' : '666666')">
                                                    {{ joinUserList[index].nickname }}
                                                </view>
                                            </view>
                                            <view class="user_item_txt_content">
                                                <view class="user_group_manager_tips" v-if="joinUserList[index].is_admin">公会管</view>
                                                <view
                                                    class="user_group_manager_tips"
                                                    v-if="joinUserPlatforms[index].platform == 'android' || joinUserPlatforms[index].platform == 'ios'"
                                                >
                                                    APP
                                                </view>
                                                <image
                                                    v-if="joinUserList[index].is_vip == 1"
                                                    class="user_item_txt_vip_icon"
                                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2020/8/4/ecKCCpRzeR1596530582740.png"
                                                ></image>
                                                <image v-if="joinUserList[index].medal" class="user_item_txt_icon" :src="joinUserList[index].medal.img"></image>
                                            </view>
                                            <view class="user_support_times" v-if="joinUserList[index].gather_times > 0">今日集结 {{ joinUserList[index].gather_times }}次</view>
                                        </view>
                                    </block>
                                </block>

                                <view class="user_item_contentView" :data-starid="item.sid" @tap="toUserHomePage" :data-userinfo="joinUserList[index]">
                                    <image class="user_item_img" src="https://star-img.xingxiaoculture.com/2019/08/09/86cab7f7730ca25915d093e41e02b97f.png">
                                        <view class="user_item_tip_text" style="background-color: #ffaf2b; margin-left: 25px">+10</view>
                                    </image>
                                    <view class="user_item_txt"></view>
                                </view>
                            </block>

                            <!-- 底部刷新 -->
                            <view class="bottom_refresh" :style="'display:' + (showBottomRefresh ? 'block' : 'none') + ';'">
                                <refresh_view :is-animation="bottomAnimation"></refresh_view>
                            </view>
                        </block>
                    </view>
                </view>

                <!-- 弹窗 -->
                <!-- parse <template is="gatherAlert" :data="type:alertType,rules:rulesdetail" v-if="alertType!='hide'"></template> -->
                <block name="gatherAlert" v-if="false" v-if="alertType != 'hide'">
                    <view class="ga_container" @tap.stop.prevent="onHideAlert" @touchmove.stop.prevent="noAction">
                        <!-- 分享 -->
                        <view class="ga_whiteBg" style="height: 180px" @tap.stop.prevent="noAction" v-if="alertType == 'share'">
                            <text class="ga_shareText" style="margin-top: 43px"></text>
                            <text class="ga_shareText">去查看公会内集结记录吧</text>
                            <!-- <text class="ga_doBtn" catchtap="onShareGroupGather">立即分享</text> -->
                            <button class="ga_doBtn" open-type="share">查看公会记录</button>
                        </view>
                        <!-- 规则 -->
                        <view
                            class="ga_whiteBg"
                            style="top: 10%; transform: translateY(0); height: auto; margin-bottom: 160rpx"
                            @tap.stop.prevent="noAction"
                            v-if="alertType == 'rule'"
                        >
                            <text class="ga_ruleTitle">集结规则</text>
                            <text class="ga_ruleText">{{ rulesdetail }}</text>
                            <text class="ga_doBtn" style="margin: 18px 0 20px 0" @tap.stop.prevent="onHideAlert">知道了</text>
                        </view>
                        <!-- 解冻规则 -->
                        <view class="ga_whiteBg" @tap.stop.prevent="noAction" v-if="alertType == 'attackRule'">
                            <text class="ga_ruleTitle">解冻规则</text>
                            <text class="ga_ruleText">{{ filter.filter_N(rulesdetail) }}</text>
                            <text class="ga_doBtn" style="margin: 18px 0 20px 0" @tap.stop.prevent="onHideAlert">知道了</text>
                        </view>
                    </view>
                </block>

                <share_panel
                    :panel_title="is_freeze ? '公会技能需要在公会内参与哦~' : '快和公会友一起完成公会集结吧'"
                    :panel_button_title="is_freeze ? '召集公会成员参与' : '邀请公会成员集结'"
                    isShare="1"
                    :is_freeze="is_freeze"
                    v-if="bShowSharePanel"
                    @btnClick="beginConcentrate"
                    @gotoIndex="gotoIndex"
                    :groupNoMe="bShowGroupNoMePanel"
                    :groupName="groupInfo.star_name + groupInfo.share_name"
                ></share_panel>

                <!-- 点击用户头像后弹窗 -->
                <group_fallin_windows
                    class="groupFallinWindows"
                    :userName="selectUserInfo.name"
                    :userImage="selectUserInfo.image.thumbnail_pic"
                    :userAngelModel="selectUserInfo.angle"
                    :myAngelLevel="selectUserInfo.angle.level"
                    :myAngelName="selectUserInfo.angle.level_name"
                    :specialBenben="selectUserInfo.angle.special == 2 && selectUserInfo.angle.not_renew_type == 2 ? 1 : 0"
                    :specialMenghu="selectUserInfo.angle.special == 3 && selectUserInfo.angle.not_renew_type == 3 ? 1 : 0"
                    :specialHongtu="selectUserInfo.angle.special == 4 && selectUserInfo.angle.not_renew_type == 4 ? 1 : 0"
                    :special="selectUserInfo.angle.special"
                    :exist="selectUserInfo.angle.acc_count > 0"
                    :spiritPics="spiritPicsUser"
                    v-if="bShowGroupFallinWindows"
                    @userPersenalPage="userPersenalPage"
                    @closeClick="closeClick"
                    @myAngelPage="myAngelPage"
                ></group_fallin_windows>

                <!-- 公会按钮 -->
                <!-- <view class="idol_home_btn" wx:if="{{bShowIdolHomeBtn}}" bindtap='pushFansGroup'>公会</view> -->

                <!-- 参与成功弹窗 bShowFallinSuccess bShowFallinSuccessType -->
                <group_fallin_joinin_success
                    class="group_fallin_joinin_success"
                    :bShowFallinSuccessType="bShowFallinSuccessType"
                    :seeTheAdvSuccess="seeTheAdvSuccess"
                    :guardianValue="guardianValue"
                    :specialBenBen="userAngelModel.special == 2 && userAngelModel.not_renew_type == 2 ? 1 : 0"
                    :special="userAngelModel.special == 1 ? 1 : 0"
                    :hasExist="userAngelModel.acc_count > 0 ? 1 : 0"
                    v-if="bShowFallinSuccess"
                    :fallin_num="taskModel.now_gift[0].num"
                    @sureClick="sureClick"
                    @closeClick="closeClick"
                    uiType="1"
                    :isGather="true"
                    :isSet="isSet"
                    :gid="WeChatGroupGid"
                    :starid="starIDStr"
                    :starName="starName"
                    @changeSwitch="onChangeSwitch"
                    @buyVip="buyVip"
                    @autoClose="autoClose"
                    :idol_point="userAngelModel.idol_point"
                    :birthActData="userAngelModel.star_info"
                    :isAct="isAct"
                    @doubleRewardClick="doubleRewardClick"
                    :is4sAct="is4sAct"
                    :gatherResp="gatherResp"
                    :showAppTips="!isApp"
                    @goTiefenLevel="goTiefenLevel"
                    @inviteFriend="inviteFriend"
                    :hasInvited="hasInvited"
                    :hasSeeAd="hasSeeAd"
                    :invitedReward="invitedReward"
                ></group_fallin_joinin_success>

                <!-- 参与弹窗 -->
                <group_fallin_filmAdv_windows
                    class="group_fallin_filmAdv_windows"
                    :bShowFallinFilmAdvType="bShowFallinFilmAdvType"
                    v-if="bShowFallinFilmAdv"
                    :guardianValue="guardianValue"
                    :specialBenBen="userAngelModel.special == 2 && userAngelModel.not_renew_type == 2 ? 1 : 0"
                    :special="userAngelModel.special == 1 ? 1 : 0"
                    :hasExist="userAngelModel.acc_count > 0 ? 1 : 0"
                    :fallin_num="taskModel.now_gift[0].num"
                    :star_num="taskModel.now_gift[0].num"
                    :coin_num="taskModel.now_gift[1].num"
                    :grow_num="taskModel.now_gift[2].num"
                    @seeAdvAttendGroup="seeAdvAttendGroup"
                    @justAttendGroup="justAttendGroup"
                    @closeClick="closeClick"
                    @pushAdvClick="pushAdvClick"
                    @vipGather="vipGather"
                    :is_vip="is_vip"
                    @buyVip="buyVip"
                    :idol_point="userAngelModel.idol_point"
                    :birthActData="userAngelModel.star_info"
                    :isAct="isAct"
                ></group_fallin_filmAdv_windows>

                <!-- 天使集结倒计时弹窗 -->
                <group_fallin_angel_windows
                    class="group_fallin_angel_windows"
                    :bShowFallinAngelType="bShowFallinAngelType"
                    v-if="bShowFallinAngel && !isShowSetingMainGroupWindows"
                    :starVotetimes="starVotetimes"
                    :time_text="join_left_time_num ? join_left_time : next_left_time"
                    :angelModel="userAngelModel"
                    :spiritPics="spiritPics"
                    @pushClick="pushClick"
                    @closeangelClick="closeangelClick"
                ></group_fallin_angel_windows>

                <idolSprite :angleResp="angleModel" :needInit="needInitSp" id="sprite" class="star_sprite_view" />

                <group_freeze_windows
                    v-if="bFrezzeWindows"
                    @confirmCancel="confirmCancel"
                    @closeClick="closeClick"
                    @backRewardClick="backRewardClick"
                    :successTimes="successTimes"
                    :windowsType="bFrezzeWindowsType"
                    :fromGroupinfo="frezzeAttackFrom"
                ></group_freeze_windows>

                <image class="bottom_frezze_tips_img" v-if="bShowToAttackTimesView" src="/static/pages/subpages_v2/images/common/frezze_group.png">
                    <text class="todayRecord_tips_text" style="top: 10px; color: #ffffff">
                        {{ bShowToAttackTimesType == 1 ? '点击助力公会集结解冻！' : '点击邀请公会成员助力吧！' }}
                    </text>
                </image>

                <!-- 底部按钮 -->
                <view class="bottom_btn_frame" style="z-index: 6">
                    <block v-if="bGroupBeFreeze">
                        <block v-if="showBottomButton">
                            <view class="bottom_btn_concentrate" @tap="gotoConcentrate">
                                <text class="bottom_btn_concentrate_txt">去送星星</text>
                            </view>
                        </block>

                        <block v-if="frezzeArr.length == 0 || groupSkillInfo.skill_status == 0">
                            <form @submit="pushFormSubmit" :report-submit="true">
                                <button
                                    class="bottom_btn_frame_btn"
                                    formType="submit"
                                    :hover="true"
                                    hover-class="bottom_btn_frame_btn_hover"
                                    :data-sid="item.starinfo.sid"
                                    @tap="bottomButtonClick"
                                    style="width: 508rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                >
                                    发起解冻技能
                                </button>
                            </form>
                        </block>

                        <block v-else-if="groupSkillInfo.skill_status == 1">
                            <block v-if="successTimes > 0">
                                <form @submit="pushFormSubmit" :report-submit="true">
                                    <button
                                        class="bottom_btn_frame_btn"
                                        formType="submit"
                                        :hover="true"
                                        hover-class="bottom_btn_frame_btn_hover"
                                        :data-sid="item.starinfo.sid"
                                        @tap="bottomButtonClick"
                                        style="width: 508rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                    >
                                        公会集结还剩
                                        <text style="color: #fff500">{{ successTimes }}</text>
                                        秒解冻
                                    </button>
                                </form>
                            </block>

                            <block v-else>
                                <form @submit="pushFormSubmit" :report-submit="true">
                                    <button
                                        class="bottom_btn_frame_btn"
                                        formType="submit"
                                        :hover="true"
                                        hover-class="bottom_btn_frame_btn_hover"
                                        :data-sid="item.starinfo.sid"
                                        @tap="bottomButtonClick"
                                        style="width: 508rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                    >
                                        释放中请稍后
                                    </button>
                                </form>
                            </block>
                        </block>

                        <block v-else-if="groupSkillInfo.skill_status == -1 && groupSkillInfo.join == 0">
                            <form @submit="pushFormSubmit" :report-submit="true">
                                <button
                                    class="bottom_btn_frame_btn"
                                    formType="submit"
                                    :hover="true"
                                    hover-class="bottom_btn_frame_btn_hover"
                                    :data-sid="item.starinfo.sid"
                                    @tap="bottomButtonClick"
                                    style="width: 508rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                >
                                    助力公会集结解冻
                                </button>
                            </form>
                        </block>

                        <block v-else-if="groupSkillInfo.skill_status == -1 && groupSkillInfo.join == 1">
                            <form @submit="pushFormSubmit" :report-submit="true">
                                <button
                                    class="bottom_btn_frame_btn"
                                    formType="submit"
                                    :hover="true"
                                    hover-class="bottom_btn_frame_btn_hover"
                                    open-type="share"
                                    :share-type="1"
                                    @tap="bottomButtonClick"
                                    style="width: 508rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                >
                                    邀请公会成员助力
                                </button>
                            </form>
                        </block>

                        <block v-else>
                            <form @submit="pushFormSubmit" :report-submit="true">
                                <button
                                    class="bottom_btn_frame_btn"
                                    formType="submit"
                                    :hover="true"
                                    hover-class="bottom_btn_frame_btn_hover"
                                    :data-sid="item.starinfo.sid"
                                    @tap="bottomButtonClick"
                                    style="width: 508rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                >
                                    发起解冻技能
                                </button>
                            </form>
                        </block>
                    </block>

                    <block v-else>
                        <block v-if="nowGather && nowGather.gather && nowGather.gather._id && nowGather.gather.gid != WeChatGroupGid">
                            <block v-if="joinUserList.length > 0">
                                <block v-if="nowGather && nowGather.gather.join_left == 0 && nowGather.gather.next_left > 0">
                                    <block v-if="join_left_time_num > 0">
                                        <button class="bottom_btn_concentrate" open-type="share">
                                            <text class="bottom_btn_concentrate_txt">邀请集结</text>
                                        </button>
                                        <form @submit="pushFormSubmit" :report-submit="true">
                                            <button
                                                class="bottom_btn_frame_btn"
                                                formType="submit"
                                                :hover="true"
                                                :data-sid="item.starinfo.sid"
                                                @tap="bottomButtonClick"
                                                style="
                                                    width: 508rpx;
                                                    background: linear-gradient(90deg, #ff9a83, #ff6ca3);
                                                    box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4);
                                                    line-height: 34rpx;
                                                    display: flex;
                                                    flex-direction: column;
                                                    align-items: center;
                                                    justify-content: center;
                                                "
                                            >
                                                <text style="font-size: 28rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    距离我下次发起/参与集结还需
                                                </text>
                                                <text style="font-size: 20rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    {{ next_left_time_now_gather }}
                                                </text>
                                            </button>
                                        </form>
                                    </block>
                                    <block v-else>
                                        <form @submit="pushFormSubmit" :report-submit="true">
                                            <button
                                                class="bottom_btn_frame_btn"
                                                formType="submit"
                                                :hover="true"
                                                :data-sid="item.starinfo.sid"
                                                @tap="bottomButtonClick"
                                                style="
                                                    width: 671rpx;
                                                    background: linear-gradient(90deg, #ff9a83, #ff6ca3);
                                                    box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4);
                                                    line-height: 34rpx;
                                                    display: flex;
                                                    flex-direction: column;
                                                    align-items: center;
                                                    justify-content: center;
                                                "
                                            >
                                                <text style="font-size: 28rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    距离我下次发起/参与集结还需
                                                </text>
                                                <text style="font-size: 20rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    {{ next_left_time_now_gather }}
                                                </text>
                                            </button>
                                        </form>
                                    </block>
                                </block>
                                <block v-else-if="nowGather && nowGather.gather.join_left > 0">
                                    <block v-if="join_left_time_num > 0">
                                        <button class="bottom_btn_concentrate" open-type="share">
                                            <text class="bottom_btn_concentrate_txt">邀请集结</text>
                                        </button>
                                        <form @submit="pushFormSubmit" :report-submit="true">
                                            <button
                                                class="bottom_btn_frame_btn"
                                                formType="submit"
                                                :hover="true"
                                                :data-sid="item.starinfo.sid"
                                                @tap="bottomButtonClick"
                                                style="
                                                    width: 508rpx;
                                                    background: linear-gradient(90deg, #ff9a83, #ff6ca3);
                                                    box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4);
                                                    line-height: 34rpx;
                                                    display: flex;
                                                    flex-direction: column;
                                                    align-items: center;
                                                    justify-content: center;
                                                "
                                            >
                                                <text style="font-size: 27rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    正在【{{ nowGather.gather.group_simple_name }}】中集结
                                                </text>
                                                <text style="font-size: 20rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    {{ join_left_time_now_gather }}
                                                </text>
                                            </button>
                                        </form>
                                    </block>
                                    <block v-else>
                                        <form @submit="pushFormSubmit" :report-submit="true">
                                            <button
                                                class="bottom_btn_frame_btn"
                                                formType="submit"
                                                :hover="true"
                                                :data-sid="item.starinfo.sid"
                                                @tap="bottomButtonClick"
                                                style="
                                                    width: 671rpx;
                                                    background: linear-gradient(90deg, #ff9a83, #ff6ca3);
                                                    box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4);
                                                    line-height: 34rpx;
                                                    display: flex;
                                                    flex-direction: column;
                                                    align-items: center;
                                                    justify-content: center;
                                                "
                                            >
                                                <text style="font-size: 27rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    正在【{{ nowGather.gather.group_simple_name }}】中集结
                                                </text>
                                                <text style="font-size: 20rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    {{ join_left_time_now_gather }}
                                                </text>
                                            </button>
                                        </form>
                                    </block>
                                </block>
                                <block v-else-if="nowGather && nowGather.gather.next_left > 0">
                                    <block v-if="join_left_time_num > 0">
                                        <button class="bottom_btn_concentrate" open-type="share">
                                            <text class="bottom_btn_concentrate_txt">邀请集结</text>
                                        </button>
                                        <form @submit="pushFormSubmit" :report-submit="true">
                                            <button
                                                class="bottom_btn_frame_btn"
                                                formType="submit"
                                                :hover="true"
                                                :data-sid="item.starinfo.sid"
                                                @tap="bottomButtonClick"
                                                style="
                                                    width: 508rpx;
                                                    background: linear-gradient(90deg, #ff9a83, #ff6ca3);
                                                    box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4);
                                                    line-height: 34rpx;
                                                    display: flex;
                                                    flex-direction: column;
                                                    align-items: center;
                                                    justify-content: center;
                                                "
                                            >
                                                <text style="font-size: 28rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    距离我下次发起/参与集结还需
                                                </text>
                                                <text style="font-size: 20rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    {{ next_left_time_now_gather }}
                                                </text>
                                            </button>
                                        </form>
                                    </block>
                                    <block v-else>
                                        <form @submit="pushFormSubmit" :report-submit="true">
                                            <button
                                                class="bottom_btn_frame_btn"
                                                formType="submit"
                                                :hover="true"
                                                :data-sid="item.starinfo.sid"
                                                @tap="bottomButtonClick"
                                                style="
                                                    width: 671rpx;
                                                    background: linear-gradient(90deg, #ff9a83, #ff6ca3);
                                                    box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4);
                                                    line-height: 34rpx;
                                                    display: flex;
                                                    flex-direction: column;
                                                    align-items: center;
                                                    justify-content: center;
                                                "
                                            >
                                                <text style="font-size: 28rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    距离我下次发起/参与集结还需
                                                </text>
                                                <text style="font-size: 20rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                    {{ next_left_time_now_gather }}
                                                </text>
                                            </button>
                                        </form>
                                    </block>
                                </block>
                            </block>
                            <block v-else>
                                <form @submit="pushFormSubmit" :report-submit="true">
                                    <block v-if="nowGather && nowGather.gather.join_left > 0">
                                        <button
                                            class="bottom_btn_frame_btn"
                                            formType="submit"
                                            :hover="true"
                                            :data-sid="item.starinfo.sid"
                                            @tap="bottomButtonClick"
                                            style="
                                                width: 671rpx;
                                                background: linear-gradient(90deg, #ff9a83, #ff6ca3);
                                                box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4);
                                                line-height: 34rpx;
                                                display: flex;
                                                flex-direction: column;
                                                align-items: center;
                                                justify-content: center;
                                            "
                                        >
                                            <text style="font-size: 27rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                正在【{{ nowGather.gather.group_simple_name }}】中集结
                                            </text>
                                            <text style="font-size: 20rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                {{ join_left_time_now_gather }}
                                            </text>
                                        </button>
                                    </block>
                                    <block v-else-if="nowGather && nowGather.gather.next_left > 0">
                                        <button
                                            class="bottom_btn_frame_btn"
                                            formType="submit"
                                            :hover="true"
                                            :data-sid="item.starinfo.sid"
                                            @tap="bottomButtonClick"
                                            style="
                                                width: 671rpx;
                                                background: linear-gradient(90deg, #ff9a83, #ff6ca3);
                                                box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4);
                                                line-height: 34rpx;
                                                display: flex;
                                                flex-direction: column;
                                                align-items: center;
                                                justify-content: center;
                                            "
                                        >
                                            <text style="font-size: 28rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                距离我下次发起/参与集结还需
                                            </text>
                                            <text style="font-size: 20rpx; font-family: PingFangSC, PingFangSC-Medium; font-weight: 500; text-align: left; color: #ffffff">
                                                {{ next_left_time_now_gather }}
                                            </text>
                                        </button>
                                    </block>
                                </form>
                            </block>
                        </block>
                        <block v-else>
                            <form @submit="pushFormSubmit" :report-submit="true">
                                <block v-if="joinUserList.length == 0">
                                    <button
                                        class="bottom_btn_frame_btn"
                                        formType="submit"
                                        :hover="true"
                                        hover-class="bottom_btn_frame_btn_hover"
                                        :data-sid="item.starinfo.sid"
                                        @tap="bottomButtonClick"
                                        style="width: 671rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                    >
                                        立即发起
                                    </button>
                                </block>

                                <block v-else-if="joinUserList.length > 0 && join_left_time_num > 0 && isOriginator">
                                    <view
                                        v-if="self_is_auto_gather == 1"
                                        class="bottom_btn_vip_tip"
                                        :style="
                                            'background: url(' +
                                            'https://star-img.xingxiaoculture.com/search/topics/images/2020/8/12/EE3MHfWhw71597209566239.png' +
                                            '); background-size: cover'
                                        "
                                    >
                                        尊敬的VIP，本次集结已为您自动参与，若将本次集结改为“手动参与”则不需消耗本次自动集结机会~
                                    </view>
                                    <button
                                        v-if="self_is_auto_gather == 1"
                                        class="bottom_btn_frame_btn"
                                        formType="submit"
                                        :hover="true"
                                        hover-class="bottom_btn_frame_btn_hover"
                                        :data-sid="item.starinfo.sid"
                                        @tap="bottomButtonClick"
                                        style="width: 671rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                    >
                                        将本次集结改为“手动参与”
                                    </button>
                                    <button
                                        v-else
                                        class="bottom_btn_frame_btn"
                                        formType="submit"
                                        :hover="true"
                                        hover-class="bottom_btn_frame_btn_hover"
                                        :data-sid="item.starinfo.sid"
                                        open-type="share"
                                        @tap="inviteFriend"
                                        style="width: 671rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                    >
                                        邀请公会成员集结
                                    </button>
                                </block>

                                <block v-else-if="joinUserList.length > 0 && join_left_time_num > 0 && !isOriginator">
                                    <button
                                        class="bottom_btn_frame_btn"
                                        formType="submit"
                                        :hover="true"
                                        hover-class="bottom_btn_frame_btn_hover"
                                        :data-sid="item.starinfo.sid"
                                        @tap="bottomButtonClick"
                                        style="width: 671rpx; background: linear-gradient(90deg, #ff7f62, #ff488d); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                    >
                                        立即参与
                                    </button>
                                </block>

                                <block v-else-if="joinUserList.length > 0 && join_left_time_num == 0 && next_left_time_num > 0">
                                    <button
                                        class="bottom_btn_frame_btn"
                                        formType="submit"
                                        :hover="true"
                                        :data-sid="item.starinfo.sid"
                                        @tap="bottomButtonClick"
                                        style="width: 671rpx; background: linear-gradient(90deg, #ff9a83, #ff6ca3); box-shadow: 0px 4px 6px 0px rgba(255, 75, 97, 0.4)"
                                    >
                                        立即发起
                                    </button>
                                </block>
                            </form>
                        </block>
                    </block>
                </view>
            </view>

            <view v-else>
                <pageload :bShowPage="bShowMainPage"></pageload>
            </view>

            <mainGroup_settingWindows
                v-if="isShowSetingMainGroupWindows"
                :windowsType="mainGroupSetingWindowsType"
                :groupInfo="groupInfo"
                :mainGroupInfo="mainGroupInfo"
                :starName="starName"
                @closeMainGroupWindowsClick="closeMainGroupWindowsClick"
                @singleSetingClick="singleSetingClick"
                @leftSetingOthersClick="leftSetingOthersClick"
                @rightSetingThisClick="rightSetingThisClick"
                :setGroupMsg="setGroupMsg"
            ></mainGroup_settingWindows>

            <vip_auto_dialog
                :show="showVipAuto"
                :gid="WeChatGroupGid"
                :starid="starIDStr"
                :defaultGroup="defaultGroup"
                :chooseGroup="groupInfo.star_name + groupInfo.share_name"
                :gatherTimes="gatherTimes"
                @sureClick="setAutoGather"
            ></vip_auto_dialog>

            <vip_buy_tip v-if="buyVipDialog" :registerTime="myUserInfo.register_time" @sureClick="buyVipSureClick" @closeClick="buyVipCloseClick"></vip_buy_tip>

            <teen_model_dialog :show="teenModelDilaogShow" :type="1"></teen_model_dialog>

            <real_name_authentication v-if="isItRealName" @closeViewEvent="closeViewEvent" :official_group="official_group"></real_name_authentication>
        </block>
        <block v-else>
            <view style="width: 100%; padding: 50px 0" :style="'display:' + (errorDic && errorDic.needShowErrorView ? 'block' : 'none') + ';'">
                <error_view :error-dic="errorDic" @clickRetryEvent="retryGetData"></error_view>
            </view>
        </block>

        <!-- 您还未加入该公会 -->
        <view v-if="notJoinedTheGuildShow" class="mask">
            <view class="dialog">
                <view class="dialog-title-text">您还未加入该公会</view>
                <view class="dialog-text1" style="margin-top: 30rpx">您还未加入{{ groupInfo.name }}，请先加入该公会，并将该公会设置为主公会，才可正常参与公会集结</view>
                <view class="dialog-btn" style="margin-top: 30rpx" @tap="handleJoinGuild">立即加入公会</view>
            </view>
        </view>

        <login_panel :isMustLogin="true" v-if="bShowLoginPanel" @loginCallback="loginCallback" @closeViewEvent="closeLoginView"></login_panel>
        <bindPhone v-if="bindPhoneShow" @phoneCallback="phoneCallback"></bindPhone>

        <!-- 更新弹窗-用于重定向到新小程序 -->
        <update_dialog class=".dialog_numone_view" v-if="showUpdate" :updateContent="updateContent"></update_dialog>

        <view class="mask" v-if="bShowGotoActivityDialogue">
            <view class="dialog1" v-if="isApp">
                <image src="https://star-img.xingxiaoculture.com/2019/08/10/b1dcab7df054af79972d344b97f8a524.png" class="i-close" @tap="closeModalActivityDialogue" />
                <view class="modal-txt" style="margin-top: 28rpx">点击下方按钮，把链接发送到微信</view>
                <view class="modal-txt">再从微信内打开即可</view>
                <button class="modal-btn" open-type="share">点击发送链接</button>
            </view>
            <view class="dialog1" v-else-if="">
                <image src="https://star-img.xingxiaoculture.com/2019/08/10/b1dcab7df054af79972d344b97f8a524.png" class="i-close" @tap="closeModalActivityDialogue" />
                <view class="modal-txt" style="margin-top: 28rpx">点击按钮进入客服消息后</view>
                <view class="modal-txt">回复{{ gotoActivityNumber }}即可</view>
                <button class="modal-btn" open-type="contact" :session-from="gotoActivitySessionFrom">点击后回复"{{ gotoActivityNumber }}"</button>
            </view>
        </view>

        <rewardWelfareDialog id="rewardWelfareDialog"></rewardWelfareDialog>

        <view class="mask" v-if="guardianBeastDialog">
            <!-- 守护兽描述 -->
            <view class="dialog1">
                <image src="https://star-img.xingxiaoculture.com/2019/08/10/b1dcab7df054af79972d344b97f8a524.png" class="i-close" @tap="closeGuardianBeastDialog" />
                <view class="dialog1-title">{{ guardDesc.title }}</view>
                <view class="dialog1-pink-txt" style="margin-top: 28rpx">{{ guardDesc.text }}</view>
                <text style="width: 100%; margin-top: 28rpx">{{ guardDesc.content }}</text>
            </view>
        </view>
        <water_mark @refreshEvent="refreshFun"></water_mark>
    </view>
</template>
<script module="filter" lang="wxs" src="@/templates/gatherAlert/gatherAlert.wxs"></script>
<script>
import pageload from '../../../../component/pageload/pageload';
import error_view from '../../../../component/error_view/error_view';
import login_panel from '../../../../component/login_panel/login_panel';
import refresh_view from '../../../../component/refresh_view/refresh_view';
import idc_image from '../../../../component/idc_image/idc_image';
import share_panel from '../../../../component/share_panel/share_panel';
import group_fallin_windows from '../../../../component/group_fallin_windows/group_fallin_windows';
import idolSprite from '../../../../component/idol_sprite_guard_entrance/idol_sprite_guard_entrance';
import group_fallin_filmAdv_windows from '../../../../component/group_fallin_filmAdv_windows/group_fallin_filmAdv_windows';
import group_fallin_angel_windows from '../../../../component/group_fallin_angel_windows/group_fallin_angel_windows';
import group_fallin_joinin_success from '../../../../component/group_fallin_joinin_success/group_fallin_joinin_success';
import group_freeze_windows from '../../../../component/group_freeze_windows/group_freeze_windows';
import mainGroup_settingWindows from '../../../../component/mainGroup_settingWindows/mainGroup_settingWindows';
import vip_guide_card from '../../../../component/vip_guide_card/vip_guide_card';
import vip_auto_dialog from '../../../../component/vip_auto_dialog/vip_auto_dialog';
import vip_buy_tip from '../../../../component/vip_center_dialog/vip_buy_tip';
import teen_model_dialog from '../../../../component/teen_model_dialog/teen_model_dialog';
import real_name_authentication from '../../../../component/real_name_authentication/real_name_authentication';
import bindPhone from '../../../../component/bind_phone/bind_phone';
import update_dialog from '../../../../component/update_dialog/update_dialog';
import topImage from '@/component/top_image/top_image';
import rewardWelfareDialog from '../../../../component/rewardWelfareDialog/rewardWelfareDialog';
import water_mark from '../../../../component/water_mark/water_mark';
// pages/group_fall_in_detail/group_fall_in_detail.js
var net = require('../../../../commonscript/common/netLoad.js');
var IDLoginManager = require('../../../../commonscript/common/IDLoginManager.js');
var globalData = require('../../../../commonscript/common/globalData.js');
var time = require('../../../../utils/util.js');
const push = require('../../../../commonscript/common/pushPlugin.js');
var freezetime = require('../../../../utils/util.js');
var freezetimeTwo = require('../../../../utils/util.js');
var requestTime = require('../../../../utils/util.js');
var Config = require('../../../../Config.js').miniConfig();
import * as JumpUtils from '../../../../commonscript/common/JumpUtils.js';
const app = getApp();
let rewardedVideoAd = null;
var urlI = 'https://data.idol001.com';
export default {
    components: {
        pageload,
        error_view,
        login_panel,
        refresh_view,
        idc_image,
        share_panel,
        group_fallin_windows,
        idolSprite,
        group_fallin_filmAdv_windows,
        group_fallin_angel_windows,
        group_fallin_joinin_success,
        group_freeze_windows,
        mainGroup_settingWindows,
        vip_guide_card,
        vip_auto_dialog,
        vip_buy_tip,
        teen_model_dialog,
        real_name_authentication,
        bindPhone,
        update_dialog,
        topImage,
        rewardWelfareDialog,
        water_mark
    },
    data() {
        return {
            isApp: Config.IS_APP ? true : false,
            bannerData: {},

            //轮播图
            showUpdate: false,

            // 更新弹窗是否开启
            updateContent: {},

            // 更新弹窗数据
            isItRealName: false,

            // 是否开启实名认证
            official_group: 0,

            // 是否青少年模式官方公会 1或0
            setTeenagers: 0,

            bShowPreLoad: false,
            bShowMainPage: true,

            errorDic: {
                needShowErrorView: false,
                needShowRetryBtn: false,
                isNothing: false,
                isNoSupportData: false,
                isSomethingError: false,
                errorText: '没有数据哦，请点击重试'
            },

            starName: '',
            starIDStr: null,
            rulesdetail: '',
            showBottomButton: true,
            showBottomButtonShare: false,
            windowHeight: 0,
            windowWidth: 0,
            joinUserList: [],
            joinUserPlatforms: [],
            self_is_auto_gather: 0,
            gatherid: '',
            alertType: 'hide',
            join_left_time: '',
            join_left_time_num: '',
            next_left_time: '',
            next_left_time_num: '',
            isNextTime: false,
            total_member_num: 0,
            total_guard_votes: 0,
            isOriginator: false,
            bShowGroupFallinWindows: false,

            selectUserInfo: {
                name: '',

                image: {
                    thumbnail_pic: ''
                },

                angle: {
                    level: '',
                    level_name: '',
                    special: 0,
                    not_renew_type: 0,
                    acc_count: 0
                }
            },

            myUserInfo: {
                register_time: ''
            },

            guardianValue: 0,

            //集结获得的贡献点

            bottomAnimation: true,

            isNotNetWorkType: false,

            //网络状态
            bShowLoginPanel: false,

            //是否展示登录弹窗
            bShowSharePanel: false,

            //是否展示分享弹窗
            bShowGroupNoMePanel: false,

            //是否在此官方公会
            isLoginStatus: false,

            //登录状态

            retryGetgroupgid: 0,

            WeChatGroupGid: '',
            WeChatGroupCode: '',
            WeChatGroupiv: '',
            WeChatGroupEncrypted: '',
            isAlready_guard_added: false,
            bShowIdolHomeBtn: false,
            bShowFallinSuccess: false,

            //参与集结成功弹窗是否显示
            bShowFallinSuccessType: 0,

            //参与集结成功弹窗展示类型
            seeTheAdvSuccess: false,

            //是否看过广告

            bShowFallinFilmAdv: false,

            //参与集结点击后弹窗 （直接参与或看视频x10倍参与）
            bShowFallinFilmAdvType: 0,

            //参与集结点击后弹窗  1.选择参与集结方式 2.获得邀请奖励 50星星

            bShowFallinAngel: false,

            //已参与集结倒计时弹窗
            bShowFallinAngelType: 0,

            //已参与集结倒计时弹窗 1.已参与 2.此次已结束

            userAngelModel: {
                special: 0,
                not_renew_type: 0,
                acc_count: 0,
                idol_point: '',
                star_info: ''
            },

            //登录用户天使model
            starVotetimes: 0,

            //登录用户可领取星星数

            roomPicsUser: [],

            spiritPicsUser: [],
            roomPicsDefUser: [],

            //天使屋默认部件
            spiritPicsDefUser: [],

            //益天使默认部件

            roomPics: [],

            spiritPics: [],
            roomPicsDef: [],

            //天使屋默认部件
            spiritPicsDef: [],

            //益天使默认部件

            alreadyShowFallinAngelView: false,

            //在公会集结中进入页面弹天使倒计时弹窗 关闭后不再弹

            bShowIdolSpriteStarDialogAndVideoAD: true,

            //拉取广告资源是否成功

            shareTag: false,

            //是否点击分享按钮

            taskModel: {
                now_gift: ''
            },

            //视频广告任务model

            rankScroll: [],

            //排名轮播
            groupRankDayAndWeek: '',

            swiperAutoPlay: true,
            groupRankDayAndWeekModel: '',
            showBottomRefresh: false,
            bottomAnimation: true,
            advLoading: false,
            bGroupBeFreeze: 0,
            bFrezzeWindows: false,
            bFrezzeWindowsType: 0,
            userSkillTimes: 0,
            groupBeSkillTimes: 0,

            groupSkillInfo: {
                skill: {
                    time_limit: 0,
                    min_people: ''
                },

                skill_status: 0,
                join: 0
            },

            //公会发起解冻信息
            bShowBeAttackTimesView: false,

            bShowToAttackTimesView: false,
            bShowToAttackTimesType: 0,
            skillTimes: 0,
            successTimes: 0,

            frezzeAttackFrom: {
                from_star_name: ''
            },

            //公会被攻击信息
            frezzeArr: [],

            isShowButton: 0,
            is_freeze: 0,
            rulesInfo: '',
            isShowSetingMainGroupWindows: false,
            mainGroupSetingWindowsType: 'setRemind',
            setGroupMsg: '',
            mainGroupInfo: '',

            groupInfo: {
                star_name: '',
                share_name: '',
                name: ''
            },

            requestDoneOne: false,
            requestDoneTwo: false,
            starRankScroll: '',

            //月排行 新势力排行排名
            groupManagerList: [],

            //公会管列表
            groupTaskList: [],

            groupTaskLength: 0,
            groupSupportValue: 0,
            nowGroupTimes: 0,
            hasRequest: false,
            needInitSp: false,
            angleModel: '',

            nowGather: {
                gather: {
                    _id: '',
                    gid: '',
                    join_left: 0,
                    next_left: 0,
                    group_simple_name: ''
                }
            },

            join_left_time_now_gather: 0,
            join_left_time_num_now_gather: 0,
            next_left_time_now_gather: 0,
            next_left_time_num_now_gather: 0,
            isNextTime_now_gather: false,
            groupAttributeRankArr: '',
            showVipAuto: false,

            //自动集结开启确认弹窗
            isSet: false,

            //自动集结设置开关
            defaultGroup: '',

            //已设置的公会名称
            chooseGroup: '',

            //当前公会名称
            gatherTimes: 0,

            //剩余自动贡献次数
            is_vip: false,

            buyVipDialog: false,

            //兑换vip弹窗
            refresh: false,

            //改变这个值 刷新vip组件

            teenModelDilaogShow: false,

            isAct: false,
            fromPlatform: '',
            isInGroup: false,
            refreshVipcard: false,

            spreadImg: {
                img_src: '',
                animate_img_src: ''
            },

            //裂变推广资源位

            is4sAct: false,

            watchVideoCount: 0,
            watchVideoUrl: '',
            is_main_group: false,
            notJoinedTheGuildShow: false,

            //是否显示未加入该公会弹窗
            bindPhoneShow: false,

            //是否显示绑定手机号
            options: {},

            topSwiperItems: null,
            gotoActivityNumber: 3,
            gotoActivitySessionFrom: 'clock_in_activity_banner',

            guardInfo: {
                is_display: '',
                img1: '',
                text: ''
            },

            //守护兽信息
            guardianBeastDialog: false,

            // 守护兽弹窗展示控制
            guardDesc: {
                title: '',
                text: '',
                content: ''
            },

            // 守护兽弹窗信息
            gotoActivityNumber: 3,

            gotoActivitySessionFrom: 'clock_in_activity_banner',

            dtObj: {
                data_quanzi: {
                    images: '',
                    text: ''
                }
            },

            //群动态
            bWarStart: false,

            //雪人大战是否开启
            shareGid: '',

            errorObj: {},
            shareticketGid: '',

            //从微信获取到的gid
            debugInfo: '',

            hasInvited: '',

            //本次集结是否已经邀请
            invitedReward: 0,

            //本次邀请是否获得奖励
            hasSeeAd: false,

            //查询某次集结是不是已经看过广告,seeTheAdvSuccess这个本地标志位好些地方都被重置,所以重新定一个
            thisLunboObj: null,

            myPage: 1,

            //分页
            isloading: false,

            //获取可领取星星个数
            // requestGuardSprite: function (options) {
            //     console.log("++++++++===idol_sprite_guard requestGuardSprite==");
            //     var oThis = this;
            //     var url = "https://data.idol001.com/api_guard_angle.php?action=get_guard_angle";
            //     var params = {
            //         "with_acc": 1
            //     };
            //     net.fetchApi(url, params, 'GET').then(resp => {
            //         console.log("++++++++===++idol_sprite_guard requestGuardSprite resp==", resp);
            //         if (resp.data != null && resp.data.exist == 1) {

            //             if (resp.data.votetimes > 0 && !oThis.data.alreadyShowFallinAngelView) {

            //                 oThis.setData({
            //                     starVotetimes: resp.data.votetimes
            //                 })

            //                 if (options == 1) {
            //                     //已参与集结
            //                     oThis.setData({
            //                         bShowFallinAngel: true,
            //                         bShowFallinAngelType: 0
            //                     })
            //                 } else {
            //                     //集结已结束 倒计时状态
            //                     oThis.setData({
            //                         bShowFallinAngel: true,
            //                         bShowFallinAngelType: 1
            //                     })
            //                 }
            //             }
            //         }

            //     }, function (resp) {
            //         console.log("+++error==++idol_sprite_guard requestGuardSprite resp.data===", resp.data);
            //         oThis.hideIdolSprite();
            //     });
            // },

            clickJoinWxGatherTime: 0,

            canShowDoubleReward: false,
            idolVideoPlayRequest: false,
            swiperName: '',
            starIcon: '',
            gatherResp: '',
            hideTime: '',
            bShowGotoActivityDialogue: false,

            myRankObject: {
                userinfo: {
                    nickname: ''
                }
            },

            myGuardStarItem: {
                starinfo: {
                    name: ''
                }
            },

            imgs: [],

            img_url: {
                origin_pic: ''
            },

            pageDataObj: {
                single_user_rank_info_all_time: {
                    text: ''
                }
            },

            image: {
                thumbnail_pic: []
            },

            nickname: '',

            angle: {
                special: 0,
                not_renew_type: 0,
                acc_count: 0
            },

            head_frame: {
                icon: ''
            },

            is_admin: '',
            platform: '',

            medal: {
                img: ''
            },

            gather_times: 0,
            grou_gather_vote_times: 0,
            num: ''
        };
    },
    /**
     * 生命周期函数--监听页面加载
     */
    onLoad: function (options) {
        this.onLoadClone3389(options);
    },
    /**
     * 生命周期函数--监听页面初次渲染完成
     */
    onReady: function () {},
    /**
     * 生命周期函数--监听页面显示
     */
    onShow: function () {
        var that = this;
        if (!that.bShowPreLoad) {
            that.setData({
                bShowPreLoad: true
            });
            that.refreshFun();
        } else {
            if (!that.bShowMainPage) {
                setTimeout(function () {
                    that.setData({
                        bShowMainPage: true
                    });
                    that.refreshFun();
                }, 10);
            } else {
                that.refreshFun();
            }
        }
        if (app.globalData.teenModelNeedInit) {
            this.initTeenModelStatus();
        }
        this.getIsItRealName();
    },
    /**
     * 生命周期函数--监听页面隐藏
     */
    onHide: function () {
        console.log('group_fall_in_detail onHide');
        var that = this;
        that.setData({
            bShowMainPage: false
        });
    },
    /**
     * 生命周期函数--监听页面卸载
     */
    onUnload: function () {
        clearTimeout(this.activeCountDownGather.timer);
        clearTimeout(this.activeCountDown.timer);
        clearInterval(freezetime);
        clearInterval(freezetimeTwo);
        clearInterval(requestTime);
    },
    /**
     * 页面相关事件处理函数--监听用户下拉动作
     */
    onPullDownRefresh: function () {
        if (this.isNotNetWorkType) {
            uni.showToast({
                title: '网络似乎不太顺畅哦',
                icon: 'none',
                duration: 2000
            });
            return;
        }
        this.getGroupFallInDetailInfo(1);
        this.getGroupSupportTimes();
        this.getGroupManager();
        this.getGroupAtributeRank();
        this.getSpreadImg();
        this.requestDynamicDataList();
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
                    that.getGroupFallInDetailInfo(that.myPage + 1);
                }
            );
        }
    },
    /**
     * 用户点击右上角分享
     */
    onShareAppMessage: function (e) {
        if (!e.target) {
            e.target = {
                dataset: {}
            };
        }
        var that = this;
        if (this.groupInfo.gid) {
            var shareGid = this.groupInfo.gid;
        } else {
            var shareGid = this.shareGid;
        }
        var shareObj = {};
        console.log('onShareAppMessage myUserInfo==' + JSON.stringify(that.myUserInfo));
        console.log('shareGid', shareGid);
        console.log(
            '集结参数',
            '/pages/index/index?action=gopage&page=group_fall_in_detail&sid=' + that.starIDStr + '&starName=' + that.starName + '&isShowButton=1' + '&shareGid=' + shareGid
        );
        if (!e.target) {
            e.target = {
                dataset: {}
            };
        }
        if (that.bGroupBeFreeze) {
            setTimeout(function () {
                if (e.from == 'button' && e.target.dataset.attack == '1') {
                    that.setData({
                        bFrezzeWindows: true,
                        bFrezzeWindowsType: 2
                    });
                } else if (e.from == 'button' && e.target.dataset.maingroup == '1') {
                    that.setData({
                        mainGroupSetingWindowsType: 'continueAgainSet'
                    });
                } else if (e.from == 'button') {
                    that.getOtherGroupBeSkillStatus();
                }
            }, 1000);
            if (e.from == 'button' && e.target.dataset.attack == '1') {
                console.log('share=====>1');
                shareObj = {
                    path:
                        '/pages/subpages_v2/pages/group_freezing/group_freezing?from_sid=' +
                        that.starIDStr +
                        '&to_sid=' +
                        this.frezzeAttackFrom.from_sid +
                        '&to_gid=' +
                        this.frezzeAttackFrom.from_gid +
                        '&isShowButton=1' +
                        '&is_share=1' +
                        '&is_return=1' +
                        '&shareGid=' +
                        shareGid,
                    title: '回击对象' + this.frezzeAttackFrom.from_star_name + '贡献者?公会，家人们战斗啦！',
                    imageUrl: 'http://img.xingxiaoculture.com/origin/2019/12/13/b19e554fb76200527f65effe505ba3c61576220962.png'
                };
            } else if (e.from == 'button' && e.target.dataset.continuemaingroup == '1') {
                console.log('share=====>2');
                shareObj = {
                    path: `/pages/index/index?action=gopage&page=mainGroup_setting&sid=${that.starIDStr}` + '&starName=' + that.starName + '&shareGid=' + shareGid,
                    title: '我正在将此公会设为公会主公会，请大家多多关照~',
                    imageUrl: 'https://img.xingxiaoculture.com/origin/2020/01/20/d430a2e424ee3d76523fb6ceaab02b5c1579488498.png'
                };
            } else if (e.from == 'button' && e.target.dataset.maingroup == '1') {
                console.log('share=====>3');
                shareObj = {
                    path: `/pages/index/index?action=gopage&page=mainGroup_setting&sid=${that.starIDStr}` + '&starName=' + that.starName + '&shareGid=' + shareGid,
                    title: '我正在将此公会设为公会主公会，请大家多多关照~',
                    imageUrl: 'https://img.xingxiaoculture.com/origin/2020/01/20/d430a2e424ee3d76523fb6ceaab02b5c1579488498.png'
                };
            } else {
                if (that.frezzeArr.length == 0) {
                    console.log('share=====>4');
                    shareObj = {
                        title: '公会集结被冻结了，还需' + that.groupSkillInfo.skill.min_people + '人才能解冻！',
                        imageUrl: 'http://img.xingxiaoculture.com/origin/2019/12/13/b3520b27408e7e147646bf16959023e01576220983.png',
                        path:
                            '/pages/index/index?action=gopage&page=group_fall_in_detail&sid=' +
                            that.starIDStr +
                            '&starName=' +
                            that.starName +
                            '&isShowButton=1' +
                            '&is_freeze=1' +
                            '&shareGid=' +
                            shareGid
                    };
                } else if (that.groupSkillInfo.skill_status == -1) {
                    console.log('share=====>5');
                    shareObj = {
                        title: '公会集结被冻结了，还需' + (that.groupSkillInfo.skill.min_people - that.groupSkillInfo.member_num) + '人才能解冻！',
                        imageUrl: 'http://img.xingxiaoculture.com/origin/2019/12/13/b3520b27408e7e147646bf16959023e01576220983.png',
                        path:
                            '/pages/index/index?action=gopage&page=group_fall_in_detail&sid=' +
                            that.starIDStr +
                            '&starName=' +
                            that.starName +
                            '&isShowButton=1' +
                            '&is_freeze=1' +
                            '&shareGid=' +
                            shareGid
                    };
                } else if (that.groupSkillInfo.skill_status == 1 || that.groupSkillInfo.skill_status == 0) {
                    console.log('share=====>6');
                    shareObj = {
                        title: '助力解冻技能已结束，快来查看结果吧！',
                        imageUrl: 'http://img.xingxiaoculture.com/origin/2019/12/13/73b48b3b15cf4c1f5772fd2c75c4cae61576220934.png',
                        path:
                            '/pages/index/index?action=gopage&page=group_fall_in_detail&sid=' +
                            that.starIDStr +
                            '&starName=' +
                            that.starName +
                            '&isShowButton=1' +
                            '&is_freeze=1' +
                            '&shareGid=' +
                            shareGid
                    };
                }
            }
        } else {
            if (e.from == 'button' && e.target.dataset.maingroup == '1') {
                that.setData({
                    mainGroupSetingWindowsType: 'continueAgainSet'
                });
            } else if (e.from == 'button') {
                that.setData({
                    shareTag: true
                });
            }
            if (e.from == 'button' && e.target.dataset.continuemaingroup == '1') {
                console.log('share=====>7');
                shareObj = {
                    path: `/pages/index/index?action=gopage&page=mainGroup_setting&sid=${that.starIDStr}` + '&starName=' + that.starName + '&shareGid=' + shareGid,
                    title: '我正在将此公会设为公会主公会，请大家多多关照~',
                    imageUrl: 'https://img.xingxiaoculture.com/origin/2020/01/20/d430a2e424ee3d76523fb6ceaab02b5c1579488498.png'
                };
            } else if (e.from == 'button' && e.target.dataset.maingroup == '1') {
                console.log('share=====>8');
                shareObj = {
                    path: `/pages/index/index?action=gopage&page=mainGroup_setting&sid=${that.starIDStr}` + '&starName=' + that.starName + '&shareGid=' + shareGid,
                    title: '我正在将此公会设为公会主公会，请大家多多关照~',
                    imageUrl: 'https://img.xingxiaoculture.com/origin/2020/01/20/d430a2e424ee3d76523fb6ceaab02b5c1579488498.png'
                };
            } else if (e.from == 'button' && this.thisLunboObj && this.isApp) {
                shareObj = {
                    path: this.thisLunboObj.url,
                    title: this.thisLunboObj.name ? this.thisLunboObj.name : '参与活动',
                    imageUrl: this.thisLunboObj.img
                };
            } else if (that.starName != 'undefined' && that.starName && that.starName.length > 0) {
                if (that.groupTaskList.gather_times > 0 && that.isLoginStatus) {
                    console.log('share=====>9');
                    shareObj = {
                        title: that.myUserInfo.nickname + '今日已参与' + that.groupTaskList.gather_times + '次公会集结，' + '号召大家来一起贡献',
                        imageUrl: that.groupTaskList.share_image,
                        path:
                            '/pages/index/index?action=gopage&page=group_fall_in_detail&sid=' +
                            that.starIDStr +
                            '&starName=' +
                            that.starName +
                            '&isShowButton=1' +
                            '&shareGid=' +
                            shareGid
                    };
                } else {
                    console.log('share=====>10');
                    shareObj = {
                        title: '吹响公会集结号角，助力更多贡献点！',
                        imageUrl: 'http://img.xingxiaoculture.com/origin/2019/12/14/b28e078ad391038f050b516f27c468c31576296592.png',
                        path:
                            '/pages/index/index?action=gopage&page=group_fall_in_detail&sid=' +
                            that.starIDStr +
                            '&starName=' +
                            that.starName +
                            '&isShowButton=1' +
                            '&shareGid=' +
                            shareGid
                    };
                    console.log('分享参数：', shareObj);
                }
            } else {
                console.log('share=====>11');
                shareObj = {
                    title: '吹响公会集结号角，助力爱豆更多贡献点！',
                    imageUrl: 'http://img.xingxiaoculture.com/origin/2019/12/14/b28e078ad391038f050b516f27c468c31576296592.png',
                    path: '/pages/index/index?action=gopage&page=group_fall_in_detail&sid=' + that.starIDStr + '&isShowButton=1' + '&shareGid=' + shareGid
                };
            }
        }

        // #if MP
        return shareObj;
        // #else
        uni.miniapp.invokeMiniappNativeExtension({
            method: 'shareCustomAppMesssage',
            param: shareObj,
            success: (res) => {
                console.log('shareCustomAppMesssage success', res);
            }
        });
        // #endif
    },
    methods: {
        /**
         * 生命周期函数--监听页面加载
         */
        onLoadClone3389: function (options) {
            console.log('debug options:::', options);
            var that = this;
            that.versionUpdate(); // 判断是否更新-主要用于跳转至备用小程序

            var starid = options.sid;
            that.setData({
                starIDStr: starid,
                starName: options.starName,
                isShowButton: options.isShowButton,
                is_freeze: options.is_freeze,
                fromPlatform: options.from,
                options
            });
            const gid = options.gid || '';
            this.shareGid = options.shareGid;
            console.log('传递的gid是：' + gid);
            IDLoginManager.stepLoginState();
            that.loginLogicStar();
            that.getfallinGroupRules();
            that.requestStarInfo();
            that.requestSnowwarTime();
            if (app.globalData.idolUserInfo) {
                that.setData({
                    myUserInfo: app.globalData.idolUserInfo,
                    is_vip: app.globalData.idolUserInfo.is_vip == 1
                });
            }

            // 获取屏幕高度
            if (!app.globalData.windowHeight) {
                uni.getSystemInfo({
                    success: function (res) {
                        app.globalData.windowHeight = res.windowHeight;
                        that.setData({
                            windowHeight: app.globalData.windowHeight,
                            windowWidth: res.windowWidth
                        });
                    }
                });
            } else {
                that.setData({
                    windowHeight: app.globalData.windowHeight,
                    windowWidth: uni.getSystemInfoSync().windowWidth
                });
            }

            //请求公会分享gid
            console.log('debug app.globalData.shareTicket', app.globalData);
            if (gid != '') {
                console.log('onLoad 请求公会分享gid==' + JSON.stringify(gid));
                app.globalData.groupGid = gid;
                that.setData({
                    WeChatGroupGid: gid
                });
                if (that.fromPlatform == 'h5') {
                    if (that.isLoginStatus) {
                        that.requestisInGroup();
                    } else {
                        that.setData({
                            bShowLoginPanel: true
                        });
                    }
                } else {
                    that.requestIsJoinOfficialGroup();
                    that.groupDayAndWeekRank(gid);
                    that.checkOnJoinStatus();
                }
            } else if (app.globalData.shareTicket || this.shareGid) {
                //从分享进入
                console.log('分享进入的', this.shareGid);
                that.setData({
                    WeChatGroupGid: this.shareGid
                });
                if (app.globalData.shareTicket) {
                    that.getWxInfo();
                } else {
                    that.setData({
                        shareticketGid: this.shareGid
                    });
                    that.requestIsJoinOfficialGroup();
                }
            } else {
                console.log('debug onLoad 显示分享邀请弹窗');
                this.setData({
                    bShowSharePanel: true
                });
            }
            if (uni.createRewardedVideoAd) {
                rewardedVideoAd = uni.createRewardedVideoAd({
                    adUnitId: Config.ADUNIT_GROUP_FALL_IN
                });
                rewardedVideoAd.onLoad(() => {
                    console.log('onLoad event emit');
                    that.setData({
                        bShowIdolSpriteStarDialogAndVideoAD: true
                    });
                });
                rewardedVideoAd.onError((err) => {
                    console.log('激励视频广告加载失败 onError event emit', JSON.stringify(err));
                    that.setData({
                        // bShowIdolSpriteStarDialogAndVideoAD: false
                        bShowIdolSpriteStarDialogAndVideoAD: true
                    });
                    var report_param = {
                        star_name: that.starName,
                        paly_result: 2
                    };
                    console.log('groupconcentrate_playaudio report_param==' + JSON.stringify(report_param));
                    // wx.reportAnalytics('groupconcentrate_playaudio', report_param);
                });

                rewardedVideoAd.onClose((res) => {
                    console.log('rewardedVideoAd.onClose res==' + JSON.stringify(res));
                    if (res && res.isEnded) {
                        console.log('激励视频广告播放结束');
                        // oThis.seeAdvVideoSuccess();

                        that.seeAdvAndJoingroup();
                        var report_param = {
                            star_name: that.starName,
                            paly_result: 1
                        };
                        console.log('groupconcentrate_playaudio report_param==' + JSON.stringify(report_param));
                        // wx.reportAnalytics('groupconcentrate_playaudio', report_param);
                    } else {
                        // 播放中途退出，不下发游戏奖励
                        console.log('播放中途退出，不下发游戏奖励');
                        that.setData({
                            bShowFallinSuccess: true,
                            bShowFallinSuccessType: 5
                        });
                        // if (oThis.data.joinUserList.length > 0) {
                        //     oThis.addGroupfallin();
                        // } else {
                        //     oThis.getfallinGroup();
                        // }
                        // oThis.setData({
                        //     bShowFallinFilmAdv: false
                        // })
                        var report_param = {
                            star_name: that.starName,
                            paly_result: 2
                        };
                        console.log('groupconcentrate_playaudio report_param==' + JSON.stringify(report_param));
                        // wx.reportAnalytics('groupconcentrate_playaudio', report_param);
                    }
                });
            }

            uni.showShareMenu({
                withShareTicket: true
            });
            this.initTeenModelStatus();
            this.getSpreadImg();
            this.init4sTime();
            this.getOfficialGroup();
            this.getGroupFallInDetailInfo(1);
            this.getGroupSupportTimes();
            this.getGroupManager();
            this.getRankBanner();
            this.getGroupAtributeRank();
            this.requestDynamicDataList();
        },

        /**
         * 获取是否官方群
         */
        getOfficialGroup() {
            const url = 'https://data.idol001.com/api_activity_2021_08s.php?action=get_group_main_list';
            const params = {
                from_page: 'group_fall_in_detail'
            };
            const oThis = this;
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('道具icon：' + JSON.stringify(resp.data));
                // 公会介绍弹窗
                uni.getStorage({
                    key: 'group_desc',
                    success(res) {
                        console.log(res.data);
                    },
                    fail(err) {
                        oThis.openDialog(12);
                    }
                });
                if (resp && resp.data && resp.data.is_office_group == 1) {
                    oThis.setData({
                        is_main_group: true
                    });
                } else {
                    oThis.setData({
                        errorDic: {
                            needShowErrorView: true,
                            needShowRetryBtn: false,
                            isNothing: false,
                            isNoSupportData: false,
                            isSomethingError: false,
                            errorText: '页面走丢啦~'
                        }
                    });
                }
            });
        },

        bottomButtonClick: function (e) {
            console.log('bottomButtonClick.click');
            var that = this;
            var userList = that.joinUserList;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                that.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            if (that.bGroupBeFreeze) {
                if (that.WeChatGroupGid.length > 0) {
                    if (that.frezzeArr.length == 0 || that.groupSkillInfo.skill_status == 0) {
                        //可发起解冻
                        that.getNoFrezze();
                    } else if (that.groupSkillInfo.skill_status == 1) {
                        //解冻成功倒计时
                        if (that.successTimes > 0) {
                        } else {
                            that.getGroupFallInDetailInfo(1);
                        }
                    } else if (that.groupSkillInfo.skill_status == -1 && that.groupSkillInfo.join == 0) {
                        //解冻中 未参与
                        that.addNoFrezze();
                    } else if (that.groupSkillInfo.skill_status == -1 && that.groupSkillInfo.join == 1) {
                    } else {
                        //可重新发起发起解冻
                        that.getNoFrezze();
                    }
                } else {
                    uni.showToast({
                        title: '请求繁忙，再试一下吧',
                        icon: 'none'
                    });
                }
            } else {
                if (that.WeChatGroupGid.length > 0) {
                    if (that.nowGather && that.nowGather.gather && that.nowGather.gather._id && that.nowGather.gather.gid != that.WeChatGroupGid) {
                        if (that.nowGather && that.nowGather.gather && that.nowGather.gather.join_left > 0) {
                            uni.showToast({
                                title: '同一时间段内只能为同一发起或参与一次集结',
                                icon: 'none'
                            });
                        } else if (that.nowGather && that.nowGather.gather && that.nowGather.gather.next_left > 0) {
                            uni.showToast({
                                title: '同一时间段内只能为同一发起或参与一次集结',
                                icon: 'none'
                            });
                        }
                    } else {
                        if (userList.length == 0) {
                            console.log('可发起集结 userList.length == 0 && bShowIdolSpriteStarDialogAndVideoAD=' + JSON.stringify(that.bShowIdolSpriteStarDialogAndVideoAD));
                            //可发起集结
                            // if (oThis.data.bShowIdolSpriteStarDialogAndVideoAD) {
                            //     oThis.setData({
                            //         bShowFallinFilmAdv: true,
                            //         bShowFallinFilmAdvType: 1,
                            //     })
                            // } else {
                            that.getfallinGroup();
                            // }
                        } else if (userList.length > 0 && that.join_left_time_num > 0 && !that.isOriginator) {
                            console.log('可参与集结状态 userList.length > 0 && bShowIdolSpriteStarDialogAndVideoAD=' + JSON.stringify(that.bShowIdolSpriteStarDialogAndVideoAD));
                            //可参与集结状态
                            // if (oThis.data.bShowIdolSpriteStarDialogAndVideoAD) {
                            //     oThis.setData({
                            //         bShowFallinFilmAdv: true,
                            //         bShowFallinFilmAdvType: 1,
                            //     })
                            // } else {
                            that.addGroupfallin();
                            // }
                        } else if (userList.length > 0 && that.join_left_time_num > 0 && that.isOriginator && that.self_is_auto_gather == 1) {
                            console.log('将本次集结改为“手动参与”');
                            that.addGroupfallin(2);
                        } else if (userList.length > 0 && that.next_left_time_num == 0 && that.join_left_time_num == 0) {
                            console.log('可发起集结 userList.length > 0 && bShowIdolSpriteStarDialogAndVideoAD=' + JSON.stringify(that.bShowIdolSpriteStarDialogAndVideoAD));
                            //可发起集结
                            // if (oThis.data.bShowIdolSpriteStarDialogAndVideoAD) {
                            //     oThis.setData({
                            //         bShowFallinFilmAdv: true,
                            //         bShowFallinFilmAdvType: 1,
                            //     })
                            // } else {
                            that.getfallinGroup();
                            // }
                        } else if (userList.length > 0 && that.join_left_time_num == 0 && that.next_left_time_num > 0) {
                            console.log('集结已结束 下次可集结倒计时');
                            //集结已结束 下次可集结倒计时
                            uni.showToast({
                                title: '本公会暂不可发起',
                                icon: 'none'
                            });
                        }
                    }
                } else {
                    uni.showToast({
                        title: '请求繁忙，再试一下吧',
                        icon: 'none'
                    });
                }
            }
        },

        /**
         * 去送星星
         */
        gotoConcentrate: function (e) {
            var that = this;
            if (that.showBottomButton) {
                //获取obj
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/support_list/support_list_new?sid=' + that.starIDStr
                });
            }
        },

        /**
         * 集结贡献排行
         */
        fallInContributeRank: function (e) {
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/fall_in_contribution_rank/fall_in_contribution_rank?sid=' + this.starIDStr + '&gid=' + this.WeChatGroupGid
            });
        },

        // 回首页
        gotoIndex: function () {
            uni.switchTab({
                url: '/pages/idol_sprite_guard/idol_sprite_guard'
            });
        },

        //获取轮播图
        getRankBanner: function (dateType) {
            var that = this;
            var url = 'https://data.idol001.com/api_moblie_idol_v2.php?action=get_wx_star_rank_lunbotu';
            var params = {
                type: dateType ? dateType : 'day',
                rank_type: 3,
                getwmrank: '1',
                starid: this.starIDStr
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data && resp.data.lunbotu && resp.data.lunbotu.length > 0) {
                        var list = resp.data.lunbotu;
                        if (that.topSwiperItems) {
                            var swipterItems = that.topSwiperItems;
                        } else {
                            var swipterItems = [];
                        }
                        var swiperName = 'topSwiperItems';
                        if (that.rankType == 5) {
                            swiperName = 'topNewPowerSwiperItems';
                        }
                        console.log('轮播图数据', list);
                        that.setData(
                            {
                                [swiperName]: list
                            },
                            function () {}
                        );
                    } else {
                    }
                },
                function (resp) {}
            );
        },

        closeViewEvent: function () {
            this.setData({
                isItRealName: false,
                setTeenagers: 1
            });
            // wx.setStorageSync('setTeenagers', 1)
        },

        /** 获取是否有实名认证过  */
        getIsItRealName: function () {
            var setTeenagers = uni.getStorageSync('setTeenagers');
            var teen_mode = uni.getStorageSync('APP_IDOL_TEEN_MODEL_STATUS');
            var times = Date.now() / 1000;
            times = time.formatTimeTwo(times, 'Y-M-D');
            var Day = uni.getStorageSync('Day');
            if (setTeenagers == 1 || teen_mode['status'] == 1) {
                this.setData({
                    isItRealName: false
                });
                return;
            }
            const url = `${urlI}/api_moblie_idol.php?action=get_has_real_name_authentication`;
            const params = {
                set_young_limit: 1
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('实名认证======', resp);
                var { real_name_authentication, userid, official_group } = resp.data;
                if (official_group == 1) {
                    this.setData({
                        official_group
                    });
                }
                if (real_name_authentication !== 1 && userid.length > 0 && Day != times) {
                    console.log('实名认证======', real_name_authentication !== 1);
                    this.setData({
                        isItRealName: true
                    });
                }
            });
        },

        //4S活动时间
        init4sTime: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_idol_activity_vote.php?action=get_activity_time_info_2021_4_s';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    that.setData({
                        is4sAct: resp.data.now_time >= resp.data.star_time
                    });
                }
            });
        },

        checkOnJoinStatus: function () {
            console.log('checkOnJoinStatus');
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_now_star_group_gather';
            var params = {
                starid: that.starIDStr
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('checkOnJoinStatus==', resp.data);
                    that.setData({
                        nowGather: resp.data
                    });
                    console.log('checkOnJoinStatus nowGather==' + JSON.stringify(that.nowGather));
                    console.log('checkOnJoinStatus nowGather.gather==' + JSON.stringify(that.nowGather.gather));
                    if (that.nowGather && that.nowGather.gather && that.nowGather.gather.join_left > 0) {
                        that.setData({
                            isNextTime_now_gather: false
                        });
                        that.activeCountDownGather(that.nowGather.gather.join_left);
                    } else if (that.nowGather && that.nowGather.gather && that.nowGather.gather.next_left > 0) {
                        that.setData({
                            isNextTime_now_gather: true
                        });
                        that.activeCountDownGather(that.nowGather.gather.next_left);
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        seeAdvVideoSuccess: function () {
            var that = this;
            // 正常播放结束，可以下发游戏奖励

            if (that.bShowFallinFilmAdv) {
                if (that.joinUserList.length == 0) {
                    //可发起集结
                    that.getfallinGroup(1);
                } else if (that.joinUserList.length > 0 && that.join_left_time_num > 0 && !that.isOriginator) {
                    //可参与集结状态
                    //观看广告后参与 10倍奖励
                    that.addGroupfallin(1);
                } else if (that.joinUserList.length > 0 && that.next_left_time_num == 0 && that.join_left_time_num == 0) {
                    //可发起集结
                    that.getfallinGroup(1);
                }
                that.setData({
                    bShowFallinFilmAdv: false
                });
            }
        },

        // 获取公会集结详情
        getGroupFallInDetailInfo: function (page) {
            console.log('获取公会集结详情 getGroupFallInDetailInfo');
            this.isloading = true;
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_wx_star_group_gather';
            var params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr,
                page: page,
                count: '21',
                group_guard_animal: 'elk'
                // 'has_join':'1',
            };

            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    var starRankInfo = resp.data;
                    console.log('get_wx_star_group_gather==', resp.data);
                    if (resp && resp.data) {
                        that.setData(
                            {
                                starIcon: resp.data.star.logo_img ? resp.data.star.logo_img.group_emblem_img : '',
                                starName: resp.data.star.name,
                                join_left_time_num: resp.data.join_left ? resp.data.join_left : 0,
                                next_left_time_num: resp.data.next_left ? resp.data.next_left : 0,
                                total_member_num: resp.data.member_num || 0,
                                total_guard_votes: resp.data.guard_votes || 0,
                                isAlready_guard_added: !!resp.data.guard_added,
                                gatherid: resp.data._id || 0,
                                bGroupBeFreeze: resp.data.freeze,
                                hasRequest: true,
                                chooseGroup: resp.data.name,
                                self_is_auto_gather: resp.data.self_is_auto_gather ? resp.data.self_is_auto_gather : 0
                            },
                            function () {
                                if (that.join_left_time_num > 0) {
                                    that.setData({
                                        isNextTime: false
                                    });
                                    that.activeCountDown(that.join_left_time_num);
                                } else if (that.next_left_time_num > 0) {
                                    that.setData({
                                        isNextTime: true
                                    });
                                    that.activeCountDown(that.next_left_time_num);
                                }
                                if (that.canShowDoubleReward) {
                                    that.doubleRewardClick();
                                    uni.hideLoading();
                                    that.canShowDoubleReward = false;
                                }
                            }
                        );
                        that.getGuardInfo(resp.data.group_guard_animal); // 获取公会守护兽
                    }

                    if (that.bGroupBeFreeze) {
                        //公会冻结详情
                        that.getGroupSkillInfo();
                        //用户可发起攻击次数
                        that.getUserSkillTimes();
                        //公会被攻击详情
                        that.getBeSkillStatus();
                    } else {
                        if (resp && resp.data && resp.data.userInfo) {
                            that.isloading = false;
                            that.myPage = page;
                            var list = that.joinUserList;
                            var platformList = that.joinUserPlatforms;
                            if (page == 1) {
                                list = resp.data.userInfo;
                                platformList = resp.data.platforms;
                            } else {
                                list = list.concat(resp.data.userInfo);
                                platformList = platformList.concat(resp.data.platforms);
                            }
                            var bShowBottomRefresh = false;
                            if (resp.data.userInfo.length > 0 && resp.data.userInfo.length >= 21) {
                                bShowBottomRefresh = true;
                            }
                            if (page == 1) {
                                if (list.length > 0) {
                                    that.setData(
                                        {
                                            joinUserList: list,
                                            joinUserPlatforms: platformList,
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
                                            joinUserList: [],
                                            joinUserPlatforms: [],
                                            showBottomRefresh: false
                                        },
                                        function () {
                                            that.endRefresh();
                                        }
                                    );
                                }
                            } else {
                                that.setData(
                                    {
                                        joinUserList: list,
                                        joinUserPlatforms: platformList,
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
                            if (resp.data.userInfo.length > 0) {
                                if (resp.data.has_join == 1) {
                                    that.setData({
                                        isOriginator: true
                                    });
                                } else {
                                    that.setData({
                                        isOriginator: false
                                    });
                                }
                            }

                            //进入页面如果是参与集结中状态 弹窗判断
                            if (resp.data.userInfo.length > 0 && that.join_left_time_num > 0 && that.isOriginator) {
                                // oThis.requestGuardSprite(1);

                                if (that.userAngelModel == null) {
                                    clearInterval(requestTime);

                                    //循环递减
                                    requestTime = setInterval(function () {
                                        if (that.userAngelModel != null) {
                                            that.requestGuardSprite(1);
                                            clearInterval(requestTime);
                                        }
                                    }, 500);
                                } else {
                                    that.requestGuardSprite(1);
                                }
                            } else if (resp.data.userInfo.length > 0 && that.join_left_time_num == 0 && that.next_left_time_num > 0) {
                                // oThis.requestGuardSprite(2);
                                // oThis.fffaf(2);

                                if (that.userAngelModel == null) {
                                    clearInterval(requestTime);

                                    //循环递减
                                    requestTime = setInterval(function () {
                                        if (that.userAngelModel != null) {
                                            that.requestGuardSprite(2);
                                            clearInterval(requestTime);
                                        }
                                    }, 500);
                                } else {
                                    that.requestGuardSprite(2);
                                }
                            } else {
                                //如果第一次请求不是集结中 标记下 避免之后又弹窗
                                that.setData({
                                    alreadyShowFallinAngelView: true,
                                    bShowFallinAngel: false
                                });
                            }
                        } else {
                            if (page == 1) {
                                that.setData(
                                    {
                                        joinUserList: [],
                                        joinUserPlatforms: [],
                                        alreadyShowFallinAngelView: true,
                                        bShowFallinAngel: false,
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
                    }

                    //wx.hideLoading();
                },
                function () {
                    //wx.hideLoading();

                    if (page == 1) {
                        uni.stopPullDownRefresh(); //停止下拉刷新
                        that.setData(
                            {
                                joinUserList: [],
                                joinUserPlatforms: [],
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

        getGuardInfo(guard) {
            // 获取公会守护兽信息
            var oThis = this;
            var url = 'https://data.idol001.com/api_guard_animal.php?action=get_group_guard_animal';
            var params = {
                gid: oThis.WeChatGroupGid,
                starid: oThis.starIDStr,
                gather_use_guard_animal: guard
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        guardInfo: resp.data
                    });
                    var url1 = 'https://data.idol001.com/api_guard_animal.php?action=group_guard_animal_intro';
                    var params1 = {
                        gid: oThis.WeChatGroupGid,
                        starid: oThis.starIDStr
                    };
                    net.fetchApi(url1, params1, 'GET').then((resp1) => {
                        console.log('守护兽详情', resp1);
                        if (resp1.data) {
                            oThis.setData({
                                guardDesc: resp1.data
                            });
                        }
                    });
                }
            });
        },

        requestGuardSprite: function (options) {
            var that = this;
            if (that.userAngelModel != null && that.userAngelModel.exist == 1) {
                if (that.userAngelModel.votetimes > 0 && !that.alreadyShowFallinAngelView) {
                    that.setData({
                        starVotetimes: this.userAngelModel.votetimes
                    });
                    if (options == 1) {
                        //已参与集结
                        that.setData({
                            bShowFallinAngel: true,
                            bShowFallinAngelType: 0
                        });
                    } else {
                        //集结已结束 倒计时状态
                        that.setData({
                            bShowFallinAngel: true,
                            bShowFallinAngelType: 1
                        });
                    }
                }
            }
        },

        // 新增公会集结
        getfallinGroup: function (options) {
            if (new Date().getTime() - this.clickJoinWxGatherTime < 500) {
                return;
            }
            this.clickJoinWxGatherTime = new Date().getTime();
            console.log('getfallinGroup 新增公会集结');
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=create_wx_gather';
            var teenModelSt = '0';
            if (globalData.teenModelStatus.status == 1) {
                teenModelSt = '1';
            } else {
                teenModelSt = '0';
            }
            var params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr,
                younger_limit_open: teenModelSt
            };
            var that = this;
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    if (resp.data && resp.data.error && resp.data.error_code == 10115) {
                        IDLoginManager.setLoginState(IDLoginManager.is_not_login);
                        that.setData({
                            bShowLoginPanel: true
                        });
                    } else if (resp.data && resp.data.error) {
                        uni.showToast({
                            title: resp.data.error_description ? resp.data.error_description : '',
                            icon: 'none'
                        });
                    } else {
                        if (resp && resp.data && resp.data.ok == 1) {
                            that.setData({
                                seeTheAdvSuccess: that.is_vip ? true : false,
                                bShowFallinSuccess: true,
                                bShowFallinSuccessType: that.is_vip ? 2 : 3,
                                guardianValue: resp.data.gather_rank_star_info.group_gather_vote_times,
                                gatherResp: resp.data.gather_rank_star_info
                            });
                            that.getGroupFallInDetailInfo(1);
                            that.getGroupSupportTimes();
                            that.getGroupManager();
                            that.getGroupAtributeRank();

                            // if (oThis.data.bShowIdolSpriteStarDialogAndVideoAD && oThis.data.taskModel.status == -1) {
                            //     //参与集结成功并可播放广告
                            //     if (options == 1) {
                            //         oThis.setData({
                            //             bShowFallinSuccess: true,
                            //             bShowFallinSuccessType: 1,
                            //             seeTheAdvSuccess: true,
                            //         })
                            //         oThis.seeAdvAndJoingroup();
                            //     } else {
                            //         oThis.setData({
                            //             bShowFallinSuccess: true,
                            //             bShowFallinSuccessType: 1,
                            //         })
                            //         oThis.getGroupFallInDetailInfo(1);
                            //         oThis.getGroupSupportTimes();
                            //         oThis.getGroupManager();
                            //         oThis.getGroupAtributeRank();
                            //     }
                            // } else {
                            //     if (options == 1) {
                            //         //参与集结成功不可播放广告
                            //         oThis.setData({
                            //             bShowFallinSuccess: true,
                            //             bShowFallinSuccessType: 0,
                            //             seeTheAdvSuccess: true,
                            //         })
                            //         oThis.seeAdvAndJoingroup();
                            //     } else {
                            //         //参与集结成功不可播放广告
                            //         oThis.setData({
                            //             bShowFallinSuccess: true,
                            //             bShowFallinSuccessType: 0,
                            //         })
                            //         oThis.getGroupFallInDetailInfo(1);
                            //         oThis.getGroupSupportTimes();
                            //         oThis.getGroupManager();
                            //         oThis.getGroupAtributeRank();
                            //     }
                            // }
                        } else if (resp && resp.data && resp.data.ok == 0 && resp.data.younger_limit_open == 1) {
                            that.setData({
                                teenModelDilaogShow: true
                            });
                        } else {
                            uni.showToast({
                                title: resp.data.msg ? resp.data.msg : '',
                                icon: 'none'
                            });
                        }
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        showChangeSuccess: function () {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_now_auto_gather';
            net.fetchApi(url, {}, 'GET').then((resp) => {
                if (resp.data) {
                    uni.showToast({
                        title: '更改成功，今日还剩余' + resp.data.today_remain_auto_gather_count + '次自动集结机会~',
                        icon: 'none'
                    });
                }
            });
        },

        // 获取公会集结规则接口
        getfallinGroupRules: function (re) {
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_group_gather_description';
            var params = {};
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getfallinGroupRules ==', resp.data);
                    if (resp.data && resp.data.error && resp.data.error_code == 10115) {
                        IDLoginManager.setLoginState(IDLoginManager.is_not_login);
                        that.setData({
                            bShowLoginPanel: true
                        });
                    } else {
                        that.setData({
                            rulesInfo: resp.data.data
                        });
                        console.log('getfallinGroupRules oThis.data.rulesdetail==', that.rulesInfo);
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        //看完广告加入公会集结
        seeAdvAndJoingroup: function () {
            console.log('seeAdvAndJoingroup 看完广告加入公会集结');
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=set_group_gather_ad_status';
            var params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr
            };
            var that = this;
            if (this.hasInvited != this.gatherid) {
                that.setData({
                    hasInvited: ''
                });
            }
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.ok == 1) {
                        that.setData({
                            bShowFallinSuccess: true,
                            bShowFallinSuccessType: 4,
                            seeTheAdvSuccess: true,
                            guardianValue: resp.data.gather_rank_star_info.group_gather_vote_times
                        });
                    } else {
                        that.setData({
                            bShowFallinSuccess: false
                        });
                        uni.showToast({
                            title: resp.data.msg ? resp.data.msg : '请求失败',
                            icon: 'none',
                            duration: 2000
                        });
                    }
                    that.getGroupFallInDetailInfo(1);
                    that.getGroupSupportTimes();
                    that.getGroupManager();
                    that.getGroupAtributeRank();
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

        // 加入集结
        addGroupfallin: function (options) {
            if (new Date().getTime() - this.clickJoinWxGatherTime < 500) {
                return;
            }
            this.clickJoinWxGatherTime = new Date().getTime();
            console.log('addGroupfallin 加入集结');
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=join_wx_gather';
            var teenModelSt = '0';
            if (globalData.teenModelStatus.status == 1) {
                teenModelSt = '1';
            } else {
                teenModelSt = '0';
            }
            var params = {
                gatherid: this.gatherid,
                younger_limit_open: teenModelSt
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    var starRankInfo = resp.data;
                    console.log('join_wx_gather==', resp.data);
                    if (resp.data && resp.data.error && resp.data.error_code == 10115) {
                        IDLoginManager.setLoginState(IDLoginManager.is_not_login);
                        that.setData({
                            bShowLoginPanel: true
                        });
                    } else if (resp.data && resp.data.error) {
                        uni.showToast({
                            title: resp.data.error_description ? resp.data.error_description : '',
                            icon: 'none'
                        });
                    } else {
                        that.getGroupFallInDetailInfo(1);
                        that.getGroupSupportTimes();
                        that.getGroupManager();
                        that.getGroupAtributeRank();
                        if (resp && resp.data && resp.data.ok == 1) {
                            if (options == 2) {
                                that.showChangeSuccess();
                            } else {
                                that.setData({
                                    seeTheAdvSuccess: that.is_vip ? true : false,
                                    bShowFallinSuccess: true,
                                    bShowFallinSuccessType: that.is_vip ? 2 : 3,
                                    guardianValue: resp.data.gather_rank_star_info.group_gather_vote_times,
                                    gatherResp: resp.data.gather_rank_star_info
                                });
                            }
                            that.getGroupFallInDetailInfo(1);
                            that.getGroupSupportTimes();
                            that.getGroupManager();
                            that.getGroupAtributeRank();

                            // if (options == 2) {
                            //     oThis.showChangeSuccess();
                            // } else if (oThis.data.bShowIdolSpriteStarDialogAndVideoAD && oThis.data.taskModel.status == -1) {
                            //     //参与集结成功并可播放广告
                            //     if (options == 1) {
                            //         oThis.setData({
                            //             bShowFallinSuccess: true,
                            //             bShowFallinSuccessType: 1,
                            //             seeTheAdvSuccess: true,
                            //         })
                            //         oThis.seeAdvAndJoingroup();
                            //     } else {
                            //         oThis.setData({
                            //             bShowFallinSuccess: true,
                            //             bShowFallinSuccessType: 1,
                            //         })
                            //     }
                            // } else {
                            //     if (options == 1) {
                            //         //参与集结成功不可播放广告
                            //         oThis.setData({
                            //             bShowFallinSuccess: true,
                            //             bShowFallinSuccessType: 0,
                            //             seeTheAdvSuccess: true,
                            //         })
                            //         oThis.seeAdvAndJoingroup();
                            //     } else {
                            //         //参与集结成功不可播放广告
                            //         oThis.setData({
                            //             bShowFallinSuccess: true,
                            //             bShowFallinSuccessType: 0,
                            //         })
                            //     }
                            // }
                        } else if (resp && resp.data && resp.data.ok == 0 && resp.data.younger_limit_open == 1) {
                            that.setData({
                                teenModelDilaogShow: true
                            });
                        } else {
                            uni.showToast({
                                title: resp.data.msg ? resp.data.msg : '',
                                icon: 'none'
                            });
                        }
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        //加入公会排行请求
        getJoingroup: function (re) {
            console.log('加入公会排行请求 getJoingroup');
            var url = 'https://update.idol001.com/api_mobile_star_rank.php?action=join_group';
            var params = {
                gid: re,
                starid: this.starIDStr
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getJoingroup==', resp.data);
                    that.getGroupFallInDetailInfo(1);
                    that.getGroupSupportTimes();
                    that.getGroupManager();
                    that.getGroupAtributeRank();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        pushFansGroup: function () {
            app.globalData.sid = this.starIDStr;
            app.globalData.gid = this.WeChatGroupGid;
            uni.switchTab({
                url: '/pages/idol_home/idol_home'
            });
        },

        activeCountDownGather: function (nowTime) {
            console.log('activeCountDownGather nowTime==' + nowTime);
            var that = this;
            clearTimeout(that.activeCountDownGather.timer);
            var localNowTime = new Date().getTime() / 1000;
            var localServerPlus = nowTime + localNowTime;
            function action() {
                // 防止以后写代码时多写了一些地方忘了清除定时器，这里多做一下clearTimeout
                clearTimeout(that.activeCountDownGather.timer);
                var calServerNowtime = localServerPlus - new Date().getTime() / 1000;
                if (calServerNowtime > 0) {
                    if (that.isNextTime_now_gather) {
                        that.setData({
                            next_left_time_num_now_gather: calServerNowtime,
                            next_left_time_now_gather: calServerNowtime ? time.formatTimeTwo(calServerNowtime, '00:m:s') : 0
                        });
                    } else {
                        that.setData({
                            join_left_time_num_now_gather: calServerNowtime,
                            join_left_time_now_gather: calServerNowtime ? time.formatTimeTwo(calServerNowtime, '00:m:s') : 0
                        });
                    }
                    that.activeCountDownGather.timer = setTimeout(action, 1000);
                } else {
                    that.checkOnJoinStatus();
                    that.getGroupFallInDetailInfo(1);
                }
            }
            action();
        },

        activeCountDown: function (nowTime) {
            var that = this;
            clearTimeout(that.activeCountDown.timer);
            var localNowTime = new Date().getTime() / 1000;
            var localServerPlus = nowTime + localNowTime;
            // var first = true;
            function action() {
                // 防止以后写代码时多写了一些地方忘了清除定时器，这里多做一下clearTimeout
                clearTimeout(that.activeCountDown.timer);
                var calServerNowtime = localServerPlus - new Date().getTime() / 1000;
                if (calServerNowtime > 0) {
                    if (that.isNextTime) {
                        that.setData({
                            next_left_time_num: calServerNowtime,
                            next_left_time: calServerNowtime ? time.formatTimeTwo(calServerNowtime, '00:m:s') : 0
                        });
                    } else {
                        that.setData({
                            join_left_time_num: calServerNowtime,
                            join_left_time: calServerNowtime ? time.formatTimeTwo(calServerNowtime, '00:m:s') : 0
                        });
                    }
                    // if(first){
                    //     localServerPlus = localServerPlus + 1;
                    //     first = false;
                    // }
                    that.activeCountDown.timer = setTimeout(action, 1000);
                } else {
                    that.getGroupFallInDetailInfo(1);
                }
            }
            action();
        },

        /**
         * 点击登录后回调
         */
        loginCallback: function (e) {
            var that = this;
            if (e.detail.success) {
                that.setData({
                    bShowLoginPanel: false,
                    isLoginStatus: true,
                    myUserInfo: app.globalData.idolUserInfo,
                    is_vip: app.globalData.idolUserInfo.is_vip == 1,
                    refreshVipcard: true
                });
                that.requestAngelDetail();
                that.getADUpdateTime();
                that.getGroupSupportTimes();
                if (that.fromPlatform == 'h5') {
                    that.requestisInGroup();
                } else {
                    if (this.options.shareGid) {
                        this.requestIsJoinOfficialGroup();
                    }
                }
                that.initTeenModelStatus();
                that.getUserWatchVideoCount();
                that.getOfficialGroup();
                that.getUserIdolInfo();
                that.versionUpdate();
            } else {
                that.setData({
                    isLoginStatus: false
                });
            }
        },

        /**
         * 获取登录状态
         */
        loginLogicStar: function () {
            var that = this;
            console.log('globalData信息', globalData.idolUserInfo.phone);
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                IDLoginManager.checkIsLogin(function (v) {
                    if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                        that.setData({
                            isLoginStatus: false,
                            bShowLoginPanel: true
                        });
                    } else {
                        that.setData({
                            isLoginStatus: true
                        });
                        that.requestAngelDetail();
                        that.getADUpdateTime();
                        that.getGroupSupportTimes();
                        that.getUserIdolInfo();
                        that.getUserWatchVideoCount();
                    }
                    // wx.reportAnalytics('group_gather_views', {
                    //     user_id: app.globalData.idolUserInfo._id ? app.globalData.idolUserInfo._id : '',
                    //     star_id: oThis.data.starIDStr,
                    // });
                });
            } else {
                that.setData({
                    isLoginStatus: true
                });
                that.requestAngelDetail();
                that.getADUpdateTime();
                that.getGroupSupportTimes();
                that.getUserIdolInfo();
                that.getUserWatchVideoCount();
                // wx.reportAnalytics('group_gather_views', {
                //     user_id: app.globalData.idolUserInfo._id ? app.globalData.idolUserInfo._id : '',
                //     star_id: oThis.data.starIDStr,
                // });
            }
        },

        getUserIdolInfo: function () {
            var that = this;
            const url = 'https://data.idol001.com/api_moblie_idol.php?action=get_user_info';
            const params = {
                userid: app.globalData && app.globalData.idolUserInfo ? app.globalData.idolUserInfo._id : ''
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                // console.log("更新缓存用户信息：" + JSON.stringify(resp.data));
                app.globalData.idolUserInfo = resp.data;
                that.isBindPhone();
                // console.log("缓存用户信息 app.globalData.idolUserInfo：" + JSON.stringify(app.globalData.idolUserInfo));
                that.setData({
                    myUserInfo: resp.data,
                    is_vip: resp.data.is_vip == 1,
                    refreshVipcard: true
                });
            });
        },

        // 展示登录弹窗
        loginBtnAction: function (e) {
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
        },

        // 关闭登录弹窗
        closeLoginView: function () {
            this.setData({
                bShowLoginPanel: false
            });
        },

        // 今日记录
        todayRecordClick: function (e) {
            var params = {
                gid: this.WeChatGroupGid,
                sid: this.starIDStr,
                prePage: 2,
                name: this.groupInfo.name
            };
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/group_fall_in_detail/groupDayRecord?params=' + JSON.stringify(params)
            });
        },

        // 规则
        ruleClick: function (e) {
            console.log(this.rulesInfo);
            this.setData({
                alertType: 'rule',
                rulesdetail: this.rulesInfo
            });
        },

        // 解冻规则
        attackRuleClick: function (e) {
            this.setData({
                alertType: 'attackRule',
                rulesdetail: this.groupSkillInfo.skill.rule
            });
        },

        /**
         * 隐藏弹窗
         */
        onHideAlert: function () {
            this.setData({
                alertType: 'hide'
            });
        },

        refreshFun: function () {
            var that = this;
            if (that.WeChatGroupGid.length > 0) {
                if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                    that.setData({
                        isLoginStatus: false
                    });
                } else {
                    that.setData({
                        isLoginStatus: true
                    });
                    if (that.hasRequest) {
                        if (that.options.shareGid) {
                            that.requestIsJoinOfficialGroup();
                        } // oThis.getIsOfficalGroupMemberAndActiveInfo();
                    }
                }
            }

            //分享后获取奖励 每次集结可做一次（这里应该要请求后台新增接口 分享获取奖励）
            if (that.shareTag && that.joinUserList.length > 0 && that.join_left_time_num > 0) {
                //oThis.getADUpdateTime(1)
                that.setData({
                    // bShowFallinSuccess: false,
                    seeTheAdvSuccess: false
                });
            }
        },

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

        /**
         * 获取是否官方公会
         */
        checkIsOfficialGroup(gid, isGetData) {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_info';
            const params = {
                gid: gid,
                starid: this.starIDStr
            };
            console.log('请求群信息参数==>', params);
            const oThis = this;
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    console.log('群信息groupInfo====>', resp.data);
                    oThis.setData({
                        groupInfo: resp.data
                    });
                    if (isGetData) {
                        return;
                    }
                    this.getUserMainGroupInfo();
                    if (!oThis.bShowGroupNoMePanel) {
                        if (resp.data.official_group == 1) {
                            oThis.setData({
                                bShowIdolHomeBtn: true
                            });
                            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                                return;
                            }
                            oThis.setOfficialGroup(gid);
                        } else {
                            //  是官方公会
                            if (oThis.isLoginStatus) {
                                // this.getUserMainGroupInfo();
                            }
                        }
                    }
                }
            });
        },

        /**
         * 获取本公会日 周排名
         */
        groupDayAndWeekRank(gid) {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_group_gather_now_hotvalue_rank';
            const params = {
                gid: gid,
                sid: this.starIDStr
            };
            const oThis = this;
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    oThis.setData({
                        groupRankDayAndWeekModel: resp.data
                    });
                    if (
                        resp.data.day &&
                        resp.data.week &&
                        resp.data.day.rank > 0 &&
                        resp.data.week.rank > 0 &&
                        resp.data.day.can_hotvalue_rank == 1 &&
                        resp.data.week.can_hotvalue_rank == 1
                    ) {
                        var rankScro = [];
                        if (resp.data.day.rank > 0) {
                            rankScro = rankScro.concat('NO.' + resp.data.day.rank);
                        }
                        if (resp.data.week.rank > 0) {
                            rankScro = rankScro.concat('NO.' + resp.data.week.rank);
                        }
                        oThis.setData({
                            rankScroll: rankScro
                        });
                    } else if (resp.data.day || resp.data.week) {
                        if (resp.data.day && resp.data.day.rank > 0 && resp.data.day.can_hotvalue_rank == 1) {
                            oThis.setData({
                                groupRankDayAndWeek: 'NO.' + resp.data.day.rank
                            });
                        } else if (resp.data.week && resp.data.week.rank > 0 && resp.data.week.can_hotvalue_rank == 1) {
                            oThis.setData({
                                groupRankDayAndWeek: 'NO.' + resp.data.week.rank
                            });
                        }
                    }
                }
            });
        },

        /**
         * 写入官方公会记录
         * @param {string} gid
         */
        setOfficialGroup(gid) {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=set_idolwx_group_view_history';
            const params = {
                gid: gid,
                starid: this.starIDStr
            };
            const oThis = this;
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                }
            });
        },

        toUserHomePage: function (e) {
            console.log('group_fall_in_detail toUserHomePage' + e.currentTarget.dataset.sid);
            var userinfo = e.currentTarget.dataset.userinfo;
            console.log('group_fall_in_detail toUserHomePage userinfo==' + userinfo);
            if (userinfo._id && userinfo.angle.acc_count > 0) {
                this.setData({
                    selectUserInfo: userinfo,
                    bShowGroupFallinWindows: true
                });
                // 天使详情
                this.requestUserAngelDetail(this.selectUserInfo._id);
            } else if (userinfo._id) {
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(userinfo))
                });
            }
        },

        userPersenalPage: function () {
            if (this.selectUserInfo && this.selectUserInfo._id) {
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(this.selectUserInfo))
                });
            }
            this.setData({
                bShowGroupFallinWindows: false
            });
        },

        myAngelPage: function () {
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(globalData.idolUserInfo))
            });
            this.setData({
                bShowGroupFallinWindows: false
            });
        },

        /**
         * 天使详情
         */
        requestUserAngelDetail: function (_id) {
            var url = 'https://data.idol001.com/api_guard_angle.php?action=get_guard_angle';
            var that = this;
            var params = {
                with_acc: 1,
                user_id: _id
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data && resp.data.acc_v2) {
                        var roomPicsTem = new Array();
                        var spiritPicsTem = new Array();
                        resp.data.acc_v2.forEach(function (value, index) {
                            console.log('遍历益天使数据：' + value.name);
                        });
                        for (let i = 0; i < resp.data.acc_v2.length; i++) {
                            var part = resp.data.acc_v2[i].part;
                            if (part == 'wall' || part == 'floor' || part == 'deck_hanging' || part == 'deck_put') {
                                roomPicsTem.push(resp.data.acc_v2[i]);
                            } else {
                                spiritPicsTem.push(resp.data.acc_v2[i]);
                            }
                        }
                        that.setData({
                            roomPicsUser: roomPicsTem,
                            spiritPicsUser: spiritPicsTem,
                            roomPicsDefUser: roomPicsTem.concat(),
                            spiritPicsDefUser: spiritPicsTem.concat()
                        });
                    }
                },
                function () {}
            );
        },

        closeangelClick: function () {
            //在公会集结中进入页面弹天使倒计时弹窗 关闭后不再弹
            if (this.bShowFallinAngel) {
                this.setData({
                    alreadyShowFallinAngelView: true
                });
            }
            this.setData({
                bShowFallinAngel: false
            });
        },

        closeClick: function () {
            this.setData({
                bShowGroupFallinWindows: false,
                bShowFallinSuccess: false,
                bShowFallinFilmAdv: false,
                seeTheAdvSuccess: false,
                bFrezzeWindows: false
            });
        },

        //去观看视频赚取贡献奖励
        sureClick: function () {
            //跳转观看视频赚取贡献奖励页

            uni.navigateTo({
                url: '/pages/subpages_v2/pages/video_get_star/video_get_star'
            });
            this.setData({
                bShowFallinSuccess: false,
                seeTheAdvSuccess: false
            });
        },

        //分享成功获取奖励弹窗 去观看视频点击
        pushAdvClick: function () {
            //跳转观看视频赚取贡献奖励页

            uni.navigateTo({
                url: '/pages/subpages_v2/pages/video_get_star/video_get_star'
            });
            this.setData({
                bShowFallinFilmAdv: false
            });
        },

        //去益天使领取 点击
        pushClick: function () {
            uni.switchTab({
                url: '/pages/idol_sprite_guard/idol_sprite_guard'
            });
        },

        //看视频参与x10倍点击
        doubleRewardClick: function () {
            var that = this;

            //等待集结数据加载中
            if (that.isloading && !that.gatherid) {
                that.canShowDoubleReward = true;
                uni.showLoading({
                    title: '数据加载中...',
                    mask: true
                });
                setTimeout(function () {
                    uni.hideLoading();
                    that.canShowDoubleReward = false;
                }, 5000);
                return;
            }
            if (that.join_left_time_num == 0 || that.join_left_time_num < 0) {
                that.setData({
                    bShowFallinSuccess: false
                });
                uni.showToast({
                    title: '集结已结束',
                    icon: 'none'
                });
            } else {
                if (!that.advLoading) {
                    // if (oThis.data.myUserInfo.sys_time - oThis.data.myUserInfo.register_time <= 3 * 24 * 3600 * 1000 || oThis.data.watchVideoCount == 0) {
                    //     oThis.rewardedVideoAdError('进入宣传视频');
                    //     return;
                    // }

                    that.setData({
                        advLoading: true
                    });
                    try {
                        rewardedVideoAd
                            .load()
                            .then(() => {
                                console.log('激励视频加载成功');
                                rewardedVideoAd
                                    .show()
                                    .then(() => {
                                        console.log('激励视频 广告显示成功');
                                        that.setData({
                                            advLoading: false
                                        });
                                    })
                                    .catch((err) => {
                                        that.rewardedVideoAdError(err);
                                    });
                            })
                            .catch((err) => {
                                that.rewardedVideoAdError(err);
                            });
                    } catch (error) {
                        console.log('CatchClause', error);
                        console.log('CatchClause', error);
                        that.rewardedVideoAdError(error);
                    }
                }
            }
        },

        rewardedVideoAdError: function (err) {
            console.log('暂无适合的广告，但仍翻倍奖励 show err==' + JSON.stringify(err));
            //{"errMsg":"can't invoke show() while other video-ad is showed"}
            uni.showToast({
                title: '暂无适合的广告，但仍翻倍奖励',
                icon: 'none'
            });
            this.setData({
                advLoading: false
            });
            this.seeAdvAndJoingroup();
            // this.idolVideoPlayRequest = false;
            // wx.navigateTo({
            //     url: '/pages/subpages_v2/pages/video_support/video_play_page?videoUrl=' + this.data.watchVideoUrl,
            // })
        },

        idolVideoPlayEnd: function (videoPlayEnd) {
            if (this.idolVideoPlayRequest) {
                return;
            }
            this.idolVideoPlayRequest = true;
            var that = this;
            if (videoPlayEnd) {
                this.setData({
                    watchVideoCount: this.watchVideoCount + 1
                });
                that.seeAdvAndJoingroup();
                var report_param = {
                    star_name: that.starName,
                    paly_result: 1
                };
                // wx.reportAnalytics('groupconcentrate_playaudio', report_param);
            } else {
                that.setData({
                    bShowFallinSuccess: true,
                    bShowFallinSuccessType: 5
                });
                var report_param = {
                    star_name: that.starName,
                    paly_result: 2
                };
                // wx.reportAnalytics('groupconcentrate_playaudio', report_param);
            }
        },

        //获取用户当天观看视频的次数
        getUserWatchVideoCount: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_user_watch_video_count.php?action=get_user_watch_video_count';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    that.setData({
                        watchVideoCount: resp.data.count,
                        watchVideoUrl: resp.data.video_url
                    });
                } else {
                    that.setData({
                        watchVideoCount: 0
                    });
                }
            });
        },

        //直接参与点击
        justAttendGroup: function () {
            if (this.joinUserList.length > 0) {
                this.addGroupfallin();
            } else {
                this.getfallinGroup();
            }
            this.setData({
                bShowFallinFilmAdv: false
            });
        },

        /**
         * 获取天使详情
         */
        requestAngelDetail: function () {
            var url = 'https://data.idol001.com/api_guard_angle.php?action=get_guard_angle';
            var that = this;
            var params = {
                with_acc: 1,
                user_id: app.globalData.idolUserInfo._id,
                starid: this.starIDStr
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data) {
                        //标识活动状态
                        if (resp.data.star_info) {
                            that.handleAct(resp.data.star_info);
                        }
                        that.setData({
                            userAngelModel: resp.data,
                            angleModel: resp
                        });
                        if (resp.data && resp.data.acc_v2) {
                            var roomPicsTem = new Array();
                            var spiritPicsTem = new Array();
                            resp.data.acc_v2.forEach(function (value, index) {
                                console.log('遍历益天使数据：' + value.name);
                            });
                            for (let i = 0; i < resp.data.acc_v2.length; i++) {
                                var part = resp.data.acc_v2[i].part;
                                if (part == 'wall' || part == 'floor' || part == 'deck_hanging' || part == 'deck_put') {
                                    roomPicsTem.push(resp.data.acc_v2[i]);
                                } else {
                                    spiritPicsTem.push(resp.data.acc_v2[i]);
                                }
                            }
                            that.setData({
                                roomPics: roomPicsTem,
                                spiritPics: spiritPicsTem,
                                roomPicsDef: roomPicsTem.concat(),
                                spiritPicsDef: spiritPicsTem.concat()
                            });
                        }
                    }
                },
                function () {}
            );
        },

        /**
         * 获取视频广告任务
         */
        getADUpdateTime: function (options) {
            const url = 'https://data.idol001.com/api_wxapp_list.php?action=coin_task_log';
            const params = {
                userid: globalData.idolUserInfo._id,
                taskid: '5cd13385ebea8e785ae49d75'
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('getADUpdateTime resp==' + JSON.stringify(resp));
                if (resp.data.task) {
                    this.setData({
                        taskModel: resp.data.task
                    });
                    if (resp.data.task.status == -1 && this.bShowIdolSpriteStarDialogAndVideoAD && options == 1) {
                        this.setData({
                            bShowFallinFilmAdv: true,
                            bShowFallinFilmAdvType: 0,
                            shareTag: false
                        });
                    }
                }
            });
        },

        checkRankClick: function () {
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/fall_in_hot_rank/fall_in_hot_rank?sid=' + this.starIDStr + '&gid=' + this.WeChatGroupGid
            });
        },

        //发起解冻
        getNoFrezze: function () {
            var url = 'https://data.idol001.com/api_group_skill.php?action=initiate';
            var params = {
                from_gid: this.WeChatGroupGid,
                from_sid: this.starIDStr,
                skill: 'group_gather_thaw'
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getNoFrezze==', resp.data);
                    if (resp.data && resp.data.ok == 1) {
                        that.getGroupSkillInfo();
                        uni.showToast({
                            title: '发起成功 快邀请公会成员助力吧',
                            icon: 'none'
                        });
                        that.setData({
                            bShowToAttackTimesView: true,
                            bShowToAttackTimesType: 0
                        });
                        setTimeout(() => {
                            this.setData({
                                bShowToAttackTimesView: false
                            });
                        }, 3000);
                    } else if (resp.data && resp.data.code == 6) {
                        that.getNewGroupSkillInfo();
                    }
                    that.endRefresh();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        //参与解冻
        addNoFrezze: function () {
            var url = 'https://data.idol001.com/api_group_skill.php?action=add';
            var params = {
                logid: this.groupSkillInfo._id
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('addNoFrezze==', resp.data);
                    if (resp.data && resp.data.ok == 1) {
                        that.getGroupSkillInfo();
                        uni.showToast({
                            title: '参与成功 快邀请公会成员助力吧',
                            icon: 'none'
                        });
                        that.setData({
                            bShowToAttackTimesView: true,
                            bShowToAttackTimesType: 1
                        });
                        setTimeout(() => {
                            this.setData({
                                bShowToAttackTimesView: false
                            });
                        }, 3000);
                    } else {
                        that.getGroupFallInDetailInfo(1);
                    }
                    that.endRefresh();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        //用户可发起技能次数
        getUserSkillTimes: function () {
            var url = 'https://data.idol001.com/api_group_skill.php?action=user_skill_times';
            var params = {
                userid: globalData.idolUserInfo ? globalData.idolUserInfo._id : ''
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getJoingroup==', resp.data);
                    if (resp.data) {
                        that.setData({
                            userSkillTimes: resp.data.skill_times
                        });
                    }
                    that.endRefresh();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        //公会被攻击次数
        getBeSkillTimes: function () {
            var url = 'https://data.idol001.com/api_group_skill.php?action=group_skill_be_times';
            var params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getJoingroup==', resp.data);
                    if (resp.data) {
                        that.setData({
                            groupBeSkillTimes: resp.data.freeze_times
                        });
                        if (resp.data.freeze_times > 0) {
                            that.setData({
                                bShowBeAttackTimesView: true
                            });
                            setTimeout(() => {
                                this.setData({
                                    bShowBeAttackTimesView: false
                                });
                            }, 3000);
                        }
                    }
                    that.endRefresh();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        //公会被攻击信息
        getBeSkillStatus: function () {
            var url = 'https://data.idol001.com/api_group_skill.php?action=group_skill_be_status';
            var params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr,
                skill: 'group_gather_freeze',
                userid: globalData.idolUserInfo ? globalData.idolUserInfo._id : '',
                in: '1'
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getJoingroup==', resp.data);
                    if (resp.data) {
                        that.setData({
                            frezzeAttackFrom: resp.data
                        });
                    }
                    that.endRefresh();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        //攻击我的公会 被攻击的信息
        getOtherGroupBeSkillStatus: function () {
            var url = 'https://data.idol001.com/api_group_skill.php?action=group_skill_be_status';
            var params = {
                gid: this.frezzeAttackFrom.from_gid,
                starid: this.frezzeAttackFrom.from_sid,
                skill: 'group_gather_freeze',
                userid: globalData.idolUserInfo ? globalData.idolUserInfo._id : '',
                in: '1'
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getJoingroup==', resp.data);
                    if (resp.data) {
                        if (resp.data.in_group == 1 || resp.data.status == 'freeze' || resp.data.group_attack_times == 0) {
                            that.getBeSkillTimes();
                        } else if (resp.data.status == 'attack' && resp.data.from_gid != this.WeChatGroupGid) {
                            that.getBeSkillTimes();
                        } else {
                            that.getNowHasAttack();
                        }
                    }
                    that.endRefresh();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        //公会发起攻击信息
        getNowHasAttack: function () {
            var url = 'https://data.idol001.com/api_group_skill.php?action=latest_skill_log';
            var params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr,
                skill: 'group_gather_freeze',
                userid: globalData.idolUserInfo ? globalData.idolUserInfo._id : '',
                to_sid: this.frezzeAttackFrom.from_sid,
                to_gid: this.frezzeAttackFrom.from_gid
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data && resp.data._id && resp.data.skill_status == -1) {
                        that.setData({
                            bFrezzeWindows: true,
                            bFrezzeWindowsType: 1
                        });
                    } else {
                        that.setData({
                            bFrezzeWindows: true,
                            bFrezzeWindowsType: 0
                        });
                    }
                    that.endRefresh();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        //公会解冻信息
        getGroupSkillInfo: function () {
            var url = 'https://data.idol001.com/api_group_skill.php?action=latest_skill_log';
            var params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr,
                skill: 'group_gather_thaw',
                userid: globalData.idolUserInfo ? globalData.idolUserInfo._id : ''
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data) {
                        that.setData({
                            groupSkillInfo: resp.data
                        });
                        if (resp.data.member_info && resp.data.member_info.length > 0) {
                            that.setData({
                                frezzeArr: resp.data.member_info
                            });
                        } else {
                            that.setData({
                                frezzeArr: []
                            });
                        }
                        if (resp.data.skill_status == 1) {
                            that.setData({
                                successTimes: resp.data.success_delay_s
                            });
                            that.setData({
                                bFrezzeWindows: true,
                                bFrezzeWindowsType: 3
                            });
                            if (resp.data.success_delay_s <= 0) {
                                that.setData({
                                    bFrezzeWindows: true,
                                    bFrezzeWindowsType: 5
                                });
                            } else {
                                clearInterval(freezetimeTwo);
                                freezetimeTwo = setInterval(function () {
                                    var myTime = that.successTimes - 1;
                                    if (myTime > 0) {
                                        that.setData({
                                            successTimes: myTime
                                        });
                                    } else {
                                        that.setData({
                                            successTimes: 0
                                        });
                                        that.getGroupFallInDetailInfo(1);
                                        that.setData({
                                            bFrezzeWindows: true,
                                            bFrezzeWindowsType: 5
                                        });
                                        clearInterval(freezetimeTwo);
                                    }
                                }, 1000);
                            }
                        } else if (resp.data.skill_status == -1) {
                            that.setData({
                                skillTimes: resp.data.expired_s
                            });
                            clearInterval(freezetime);
                            freezetime = setInterval(function () {
                                var myTime = that.skillTimes - 1;
                                if (myTime > 0) {
                                    that.setData({
                                        skillTimes: myTime
                                    });
                                } else {
                                    that.setData({
                                        skillTimes: 0
                                    });
                                    that.getGroupFallInDetailInfo(1);
                                    clearInterval(freezetime);
                                }
                            }, 1000);
                        } else {
                            that.setData({
                                skillTimes: 0,
                                successTimes: 0
                            });
                            clearInterval(freezetime);
                            clearInterval(freezetimeTwo);
                        }
                    }
                    that.endRefresh();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        backRewardClick: function () {
            this.setData({
                bFrezzeWindows: false
            });
            if (this.bFrezzeWindowsType == 1) {
                uni.navigateTo({
                    url:
                        '/pages/subpages_v2/pages/group_freezing/group_freezing?from_sid=' +
                        this.starIDStr +
                        '&to_sid=' +
                        this.frezzeAttackFrom.from_sid +
                        '&to_gid=' +
                        this.frezzeAttackFrom.from_gid +
                        '&is_return=1'
                });
            } else if (this.bFrezzeWindowsType == 5) {
                oThis.getGroupFallInDetailInfo(1);
            }
        },

        confirmCancel: function () {
            var params = {
                gid: this.WeChatGroupGid,
                sid: this.starIDStr,
                prePage: 2
            };
            this.setData({
                bFrezzeWindows: false
            });
            uni.navigateTo({
                url: 'groupDayRecord?params=' + JSON.stringify(params)
            });
        },

        getNewGroupSkillInfo: function () {
            var url = 'https://data.idol001.com/api_group_skill.php?action=latest_skill_log';
            var params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr,
                skill: 'group_gather_thaw',
                userid: globalData.idolUserInfo ? globalData.idolUserInfo._id : ''
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    that.setData({
                        groupSkillInfo: resp.data
                    });
                    that.addNoFrezze();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        addDebuginfo(name, s) {
            this.setData({
                debugInfo: this.debugInfo + ' ' + name + '==' + s + '    '
            });
        },

        // 是否加入过主公会
        getUserMainGroupInfo: function () {
            var url = 'https://data.idol001.com/api_mobile_idol_group_main.php?action=get_main_group_info';
            var params = {
                starid: this.starIDStr,
                gid: this.shareticketGid ? this.shareticketGid : this.WeChatGroupGid
            };
            console.log('api WeChatGroupGid = ', this.WeChatGroupGid);
            console.log('api shareGid = ', this.shareGid);
            console.log('api shareticketGid = ', this.shareticketGid);
            this.addDebuginfo('WeChatGroupGid', this.WeChatGroupGid);
            this.addDebuginfo('WeChatshareGidGroupGid', this.shareGid);
            this.addDebuginfo('shareticketGid', this.shareticketGid);
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('api data = ' + JSON.stringify(resp.data));
                    console.log('api data ok = ', resp.data.ok);
                    this.addDebuginfo('get_main_group_info = ', JSON.stringify(resp.data));
                    if (resp && resp.data) {
                        that.setData({
                            mainGroupInfo: resp.data.group_info
                        });
                        if (resp.data.ok == 1) {
                            that.setData({
                                isShowSetingMainGroupWindows: false
                            });
                            if (that.shareGid) {
                                if (resp.data.group_info.gid != that.shareGid) {
                                    if (that.shareGid == that.shareticketGid) {
                                        that.setData({
                                            mainGroupInfo: resp.data.group_info,
                                            isShowSetingMainGroupWindows: true,
                                            mainGroupSetingWindowsType: 'treeComfirmChangeGroup'
                                        });
                                    } else {
                                        that.setData({
                                            isShowSetingMainGroupWindows: true,
                                            mainGroupSetingWindowsType: 'error_card'
                                        });
                                    }
                                } else {
                                    //直接参加
                                }
                            }
                        } else {
                            //主群检查出错
                            that.setData({
                                errorObj: resp.data,
                                isShowSetingMainGroupWindows: true,
                                mainGroupSetingWindowsType: 'error_msg' //setTreeRemindSimple
                            });
                        }
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

        closeMainGroupWindowsClick: function () {
            this.setData({
                isShowSetingMainGroupWindows: false
            });
        },

        singleSetingClick: function () {
            if (this.mainGroupSetingWindowsType == 'shareGroup') {
                app.globalData.aldstat.sendEvent('Setup_MainGroup_share_Popupshow', {});
            }
        },

        leftSetingOthersClick: function () {
            if (this.mainGroupSetingWindowsType == 'setRemind') {
                this.setData({
                    mainGroupSetingWindowsType: 'shareGroup'
                });
            } else if (this.mainGroupSetingWindowsType == 'comfirmSet') {
                this.setData({
                    isShowSetingMainGroupWindows: false
                });
            } else if (this.mainGroupSetingWindowsType == 'changeGroup') {
                this.setData({
                    isShowSetingMainGroupWindows: false
                });
            } else if (this.mainGroupSetingWindowsType == 'continueAgainSet') {
            } else if (this.mainGroupSetingWindowsType == 'treeComfirmChangeGroup') {
                this.setData({
                    mainGroupSetingWindowsType: 'treeChangeGroup'
                });
            } else if (this.mainGroupSetingWindowsType == 'treeChangeGroup') {
                uni.reLaunch({
                    url: '/pages/idol_home/idol_home'
                });
            }
        },

        rightSetingThisClick: function () {
            if (this.mainGroupSetingWindowsType == 'setRemind') {
                this.setData({
                    mainGroupSetingWindowsType: 'comfirmSet'
                });
            } else if (this.mainGroupSetingWindowsType == 'comfirmSet') {
                this.setMainGroupRequest();
                app.globalData.aldstat.sendEvent('Setup_MainGroup_fix', {});
            } else if (this.mainGroupSetingWindowsType == 'continueAgainSet') {
                this.setData({
                    isShowSetingMainGroupWindows: false
                });
            } else if (this.mainGroupSetingWindowsType == 'changeGroup') {
                this.setMainGroupRequest();
                app.globalData.aldstat.sendEvent('Change_MainGroup_fix', {});
            } else if (this.mainGroupSetingWindowsType == 'treeChangeGroup') {
                this.setMainGroupRequest();
                app.globalData.aldstat.sendEvent('Change_MainGroup_fix', {});
            } else if (this.mainGroupSetingWindowsType == 'treeComfirmChangeGroup') {
                uni.reLaunch({
                    url: '/pages/idol_home/idol_home'
                });
            }
        },

        // 设置主公会
        setMainGroupRequest: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_idol_group_main.php?action=set_main_group';
            var params = {
                starid: that.starIDStr,
                gid: that.groupInfo.gid
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.ok == 1) {
                        uni.showToast({
                            title: '设置主公会成功',
                            icon: 'none'
                        });
                        that.setData({
                            isShowSetingMainGroupWindows: false
                        });
                    } else if (resp && resp.data && resp.data.ok == 0 && resp.data.type == 1) {
                        that.setData({
                            mainGroupSetingWindowsType: 'groupManageDidSet',
                            setGroupMsg: resp.data.error_msg
                        });
                    } else {
                        if (resp.data.error_msg) {
                            uni.showToast({
                                title: resp.data.error_msg,
                                icon: 'none'
                            });
                        } else if (resp.data.error_description && resp.data.error_code == 10110) {
                            uni.showToast({
                                title: resp.data.error_description,
                                icon: 'none'
                            });
                        }
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        getGroupManager: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_group_admins';
            var params = {
                starid: that.starIDStr,
                gid: that.WeChatGroupGid
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        that.setData({
                            groupManagerList: resp.data.list
                        });
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        getGroupAtributeRank: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_group_gather_rank.php?action=get_my_group_current_rank';
            var params = {
                starid: that.starIDStr,
                gid: that.WeChatGroupGid
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        if (resp.data.rankscroll) {
                            var list = [];
                            if (resp.data.rankscroll.day > 0) {
                                list = list.concat('日排名：NO.' + resp.data.rankscroll.day);
                            } else {
                                list = list.concat('日排名：' + resp.data.rankscroll.day);
                            }
                            if (resp.data.rankscroll.week > 0) {
                                list = list.concat('周排名：NO.' + resp.data.rankscroll.week);
                            } else {
                                list = list.concat('周排名：' + resp.data.rankscroll.week);
                            }
                        }
                        that.setData({
                            groupAttributeRankArr: list
                        });
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        getGroupSupportTimes: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_group_gather_task';
            var params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        that.setData({
                            groupTaskList: resp.data
                        });
                        for (var i in resp.data.list) {
                            var item = resp.data.list[i];
                            that.setData({
                                groupSupportValue: (resp.data.gather_times / item.task_num) * 100
                            });
                            if (resp.data.gather_times >= item.task_num) {
                                that.setData({
                                    nowGroupTimes: '已完成'
                                });
                            } else {
                                that.setData({
                                    nowGroupTimes: resp.data.gather_times
                                });
                            }
                        }
                    }
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        requestStarInfo: function () {
            var url = 'http://data.idol001.com/api_mobile_star_rank.php?action=get_group_score_history';
            var that = this;
            var params = {
                starid: that.starIDStr,
                type: 'month',
                gid: that.WeChatGroupGid
                // 'starid':7460,
                // 'type': 'month',
                // 'gid':'GUvVY5GcixhGbMQNluSPRBN_zbEs'
            };

            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    var starRankInfo = resp.data;
                    uni.stopPullDownRefresh(); //停止下拉刷新
                    console.log(resp, 'resprespresp');
                    if (resp && resp.data) {
                        // var rankArray = [];
                        if (resp.data.rank) {
                            that.setData(
                                {
                                    starRankScroll: resp.data.rank
                                },
                                function () {}
                            );
                        }
                    }
                },
                function () {}
            );
        },

        /**
         * 点击送鲜花
         */
        rewardFlowerClick: function (e) {
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                that.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var url = 'https://data.idol001.com/api_guard_angle.php?action=send_gather_flower';
            var params = {
                owner_id: e.currentTarget.dataset.userid
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        if (resp.data.ok == 1) {
                            uni.showToast({
                                title: '送花成功 星星+' + resp.data.vote_star,
                                icon: 'none'
                            });
                            this.getGroupManager();
                        } else if (resp.data.error_description) {
                            if (resp.data.error == 'time limit') {
                                uni.showToast({
                                    title: '今日已送ta鲜花',
                                    icon: 'none'
                                });
                            } else {
                                uni.showToast({
                                    title: resp.data.error_description,
                                    icon: 'none'
                                });
                            }
                        }
                    }
                },
                function (resp) {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                }
            );
        },

        userImgClick: function (e, _dataset) {
            /* ---处理dataset begin--- */
            this.handleDataset(e, _dataset);
            /* ---处理dataset end--- */
            var userinfo = e.currentTarget.dataset.userinfo;
            if (userinfo._id) {
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(userinfo))
                });
            }
        },

        gotoRankPage: function (e) {
            var url = '/pages/rank_list/general_rank_list';
            console.log('gotoRankPage e.currentTarget.dataset==' + JSON.stringify(e.currentTarget.dataset));
            if (e.currentTarget.dataset.rankType == 3) {
                app.globalData.currentIndex = 0;
            } else if (e.currentTarget.dataset.rankType == 5) {
                app.globalData.currentIndex = 1;
            } else if (e.currentTarget.dataset.rankType == 10) {
                app.globalData.currentIndex = 2;
            } else if (e.currentTarget.dataset.rankType == 11) {
                app.globalData.currentIndex = 2;
            }
            uni.switchTab({
                url: url
            });
        },

        //自动集结开关状态改变
        onChangeSwitch: function (e) {
            console.log('onChangeSwitch = ' + JSON.stringify(e.detail));
            this.setData({
                showVipAuto: e.detail.setAuto,
                defaultGroup: e.detail.oldGroupName,
                gatherTimes: e.detail.gatherTimes
            });
        },

        //自动集结开关关闭
        autoClose: function (e) {
            this.setData({
                isSet: false
            });
        },

        //设置自动集结
        setAutoGather: function (e) {
            console.log('setAutoGather');
            this.setData({
                isSet: true
            });
        },

        //设置自动集结
        vipGather: function (e) {
            console.log('vipGather');
            this.setData({
                advLoading: false
            });
            // this.seeAdvVideoSuccess();
            this.seeAdvAndJoingroup();
        },

        buyVipSureClick: function () {
            this.setData({
                buyVipDialog: false,
                hideTime: Date.parse(new Date())
            });
        },

        buyVipCloseClick: function () {
            this.setData({
                buyVipDialog: false
            });
        },

        buyVip: function (e) {
            // this.setData({
            //     buyVipDialog: true
            // });
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/vip_center/pay_vip'
            });
        },

        initTeenModelStatus: function () {
            var url = 'https://data.idol001.com/api_teenagers_mode.php?action=get_mode_info';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    globalData.teenModelStatus = resp.data;
                    app.globalData.teenModelNeedInit = false;
                    if (resp.data.status == 3) {
                        oThis.teenModelResetDone();
                    }
                }
            });
        },

        teenModelResetDone: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_teenagers_mode.php?action=reset_done';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data && resp.data.ok == 1) {
                    that.initTeenModelStatus();
                }
            });
        },

        versionUpdate: function () {
            //判断是否有更新-需要引导到新小程序
            var that = this;
            var url = 'https://data.idol001.com/api_2021_08s.php?action=get_navi_config';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data && resp.data.data) {
                    that.setData({
                        showUpdate: resp.data.data.need_show,
                        updateContent: resp.data.data // 接口中有跳转需要的appid,路径,文本提示
                    });
                }
            });
        },

        handleAct(actData) {
            var timeEN = actData.activity_end_time - actData.now_time;
            var timeSN = actData.activity_star_time - actData.now_time;
            if (timeEN > 0 && timeSN < 0) {
                this.setData({
                    isAct: true
                });
            } else {
                this.setData({
                    isAct: false
                });
            }
        },

        requestisInGroup: function () {
            if (oThis.data.options.shareGid) {
                oThis.requestIsJoinOfficialGroup();
            }
            oThis.checkIsOfficialGroup(oThis.data.WeChatGroupGid);
            oThis.groupDayAndWeekRank(oThis.data.WeChatGroupGid);
            oThis.checkOnJoinStatus();
        },

        goHomeBackFansPageClick: function () {
            uni.switchTab({
                url: '/pages/idol_home/idol_home'
            });
        },

        getSpreadImg: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_split_task.php?action=get_spread_img';
            var params = {
                starid: that.starIDStr,
                gid: that.WeChatGroupGid
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.ok == 1) {
                        that.setData({
                            spreadImg: resp.data
                        });
                    } else {
                        that.setData({
                            spreadImg: {}
                        });
                    }
                },
                function () {
                    that.setData({
                        spreadImg: {}
                    });
                }
            );
        },

        //公会集结页-裂变推广资源位点击
        groupJoinActImage: function () {
            var that = this;
            // wx.reportAnalytics('new_users_invite_btn_click', {
            //     user_id: app.globalData.idolUserInfo._id ? app.globalData.idolUserInfo._id : '',
            //     star_id: oThis.data.starIDStr,
            //     activity_id: oThis.data.spreadImg.task_id,
            //     join_from: 1,
            // });
            console.log('打开兴趣公会小程序');
            var path = '';
            if (this.groupInfo.official_group == 1) {
                path = 'pages/invite/invite?type=billboard&task_id=' + this.spreadImg.task_id;
            } else {
                path = 'pages/pullnew_newuser_act/pullnew_newuser_act?isShareIn=true';
            }
            uni.navigateToMiniProgram({
                appId: 'wx316d1f9289e879b2',
                path: path,
                extraData: {
                    foo: 'bar'
                },
                envVersion: 'release',
                //develop	开发版    trial	体验版    release	正式版
                success(res) {
                    console.log('打开兴趣公会小程序成功');
                }
            });
        },

        /* 绑定手机号回调 */
        phoneCallback() {
            this.getUserIdolInfo();
            this.setData({
                bindPhoneShow: false
            });
        },

        /* 是否在官方群 */
        requestIsJoinOfficialGroup() {
            // let param = {
            //     gid
            // }
            // this.setData({
            //     WeChatGroupGid: gid,
            // });
            // net.fetchApi("https://data.idol001.com/api_moblie_idol.php?action=is_join_official_group", param, "GET").then(response => {
            console.log('是否在官方群 ', this.WeChatGroupGid);
            this.checkIsOfficialGroup(this.WeChatGroupGid);
            app.globalData.groupGid = this.WeChatGroupGid;
            this.groupDayAndWeekRank(this.WeChatGroupGid);
            this.checkOnJoinStatus();
            // })
        },

        /* 点击加入公会 */
        handleJoinGuild() {
            if (!this.groupInfo) {
                return;
            }
            const { gid, name, starid } = this.groupInfo;
            let appId = 'wx997f4bf7ba2f8a49';
            let path = `/pages/set_main_group/set_main_group?from=navigate&starid=${starid}&gid=${gid}&groupName=${name}`;
            this.jumpApplet(appId, path);
        },

        /* 跳转其他小程序 */
        jumpApplet(appId, path) {
            uni.navigateToMiniProgram({
                appId,
                path,
                envVersion: 'trial'
            });
        },

        /* 验证是否绑定手机号 */
        isBindPhone() {
            console.log('app.globalData.idolUserInfo', app.globalData.idolUserInfo);
            if (!app.globalData.idolUserInfo.phone) {
                this.setData({
                    bindPhoneShow: true
                });
                return false;
            }
            return true;
        },

        /* 获取微信信息 */
        getWxInfo() {
            var target = 0;
            var oThis = this;
            uni.getShareInfo({
                shareTicket: app.globalData.shareTicket,
                success: function (res) {
                    console.log('res.encryptedData', res.encryptedData);
                    target++;
                    oThis.setData({
                        WeChatGroupEncrypted: res.encryptedData,
                        WeChatGroupiv: res.iv
                    });
                    console.log('res.encryptedData——target', target);
                    if (target == 2) {
                        oThis.getwechatGid();
                    }
                }
            });
            uni.login({
                success: function (obj) {
                    console.log('obj.code', obj.code);
                    target++;
                    oThis.setData({
                        WeChatGroupCode: obj.code
                    });
                    console.log('res.encryptedData——code', target);
                    if (target == 2) {
                        oThis.getwechatGid();
                    }
                }
            });
        },

        // 获取微信gid
        getwechatGid: function () {
            console.log('获取微信gid');
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
                    that.setData({
                        shareticketGid: resp.data.openGId
                    });
                    that.requestIsJoinOfficialGroup();
                },
                function () {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none',
                        duration: 2000
                    });
                    that.endRefresh();
                }
            );
        },

        clickSwiperItems: function (e, _dataset) {
            /* ---处理dataset begin--- */
            this.handleDataset(e, _dataset);
            /* ---处理dataset end--- */
            var that = this;
            var list = this.topSwiperItems;
            switch (that.rankType) {
                case 3:
                    {
                        list = that.topSwiperItems;
                    }
                    break;
                case 5:
                    {
                        list = that.topNewPowerSwiperItems;
                    }
                    break;
                case 11:
                    {
                        list = that.topActivitySwiperItems;
                        if (that.currentTab == 4) {
                            //以团之名
                            list = that.topActivitySwiperItemsOther;
                        }
                    }
                    break;
                case 10:
                    {
                        list = that.topBrandActivitySwiperItems;
                    }
                    break;
            }
            //这里需要添加逻辑 判断是走网页 还是走本地协议跳转
            if (list && list[e.currentTarget.dataset.ind] && list[e.currentTarget.dataset.ind].type == 1) {
                this.setData({
                    bShowGotoActivityDialogue: true,
                    gotoActivityNumber: 3,
                    gotoActivitySessionFrom: 'clock_in_activity_banner',
                    thisLunboObj: list[e.currentTarget.dataset.ind]
                });
            } else if (list && list[e.currentTarget.dataset.ind] && list[e.currentTarget.dataset.ind].type == 2) {
                this.go2BindUserClockInActivity(
                    list[e.currentTarget.dataset.ind].url,
                    globalData.idolUserInfo['wx_app_openid_' + Config.PLATFORM],
                    list[e.currentTarget.dataset.ind].reply_num
                );
                this.setData({
                    bShowGotoActivityDialogue: true,
                    gotoActivityNumber: list[e.currentTarget.dataset.ind].reply_num,
                    gotoActivitySessionFrom: list[e.currentTarget.dataset.ind].session_from,
                    thisLunboObj: list[e.currentTarget.dataset.ind]
                });
            } else if (list && list[e.currentTarget.dataset.ind] && list[e.currentTarget.dataset.ind].url && list[e.currentTarget.dataset.ind].url.length > 0) {
                var myUrl = list[e.currentTarget.dataset.ind].url;
                console.log('gotoLunbo myUrl = ' + myUrl);
                var str_before = myUrl.indexOf('http');
                var str_befores = myUrl.indexOf('https');
                var isGotoHttp = str_before == 0 || str_befores == 0;
                var lunboObj = list[e.currentTarget.dataset.ind];
                console.log('gotoLunbo isGotoHttp = ' + isGotoHttp);
                console.log('gotoLunbo lunboObj = ' + JSON.stringify(lunboObj));
                if (isGotoHttp) {
                    if (lunboObj.needLogin && !IDLoginManager.isLoginByCheckLocalData()) {
                        that.setData({
                            bShowLoginPanel: true
                        });
                        return;
                    }
                    var url = lunboObj.url;
                    console.log('gotoLunbo lunboObj.url = ' + JSON.stringify(url));
                    console.log('gotoLunbo lunboObj.needUserID = ' + JSON.stringify(lunboObj.needUserID));
                    if (lunboObj.needUserID) {
                        var addIndex = 0;
                        var sessionCookie = globalData.cookie ? globalData.cookie.split(/^PHPSESSID=([^\s;]+)\;/g)[1] : '';
                        sessionCookie = sessionCookie || '';
                        if (url.indexOf('?') > 0) {
                            addIndex = url.indexOf('?');
                            if (addIndex + 1 > url.length - 1) {
                                url = url + 'sessionid=' + sessionCookie;
                            } else {
                                url = url.slice(0, addIndex + 1) + 'sessionid=' + sessionCookie + '&' + url.slice(addIndex + 1);
                            }
                        } else if (url.indexOf('#') > 0) {
                            addIndex = url.indexOf('#');
                            url = url.slice(0, addIndex) + '?sessionid=' + sessionCookie + url.slice(addIndex);
                        } else {
                            url = url + '?sessionid=' + sessionCookie;
                        }
                    }
                    console.log('gotoLunbo navigateTo web_page url = ' + url);
                    uni.navigateTo({
                        url: '/pages/web_page/web_page?url=' + encodeURIComponent(url)
                    });
                } else if (lunboObj.appid && lunboObj.appid.length > 0) {
                    uni.navigateToMiniProgram({
                        appId: lunboObj.appid,
                        path: lunboObj.url,
                        success(res) {},
                        fail(res) {}
                    });
                } else {
                    if (myUrl == '/pages/idol_sprite_guard/idol_sprite_guard' || myUrl == '/pages/rank_list/general_rank_list' || myUrl == '/pages/idol_home/idol_home') {
                        uni.switchTab({
                            url: myUrl
                        });
                    } else {
                        uni.navigateTo({
                            url: myUrl
                        });
                    }
                }
            }
        },

        go2BindUserClockInActivity: function (lunbo_url, wx_open_id, word) {
            console.log('go2BindUserClockInActivity ');
            console.log('go2BindUserClockInActivity lunbo_url==' + lunbo_url);
            console.log('go2BindUserClockInActivity wx_open_id==' + wx_open_id);
            const url = 'https://data.idol001.com/api_wxapp_list.php?action=set_kefu_url';
            const params = {
                url: lunbo_url,
                wx_openid: wx_open_id,
                word: word
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('set_kefu_url resp ==' + resp);
            });
        },

        closeModalActivityDialogue() {
            this.setData({
                bShowGotoActivityDialogue: false
            });
        },

        openDialog(type) {
            this.zpSelectComponent('#rewardWelfareDialog').showView(type);
        },

        /**
         * 发布按钮点击
         */
        gotoPublishPage: function () {
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            uni.navigateTo({
                url: '../../../subpages/pages/group_spaceDynamic/publish_dynamicMessage?from=admin&gid=' + this.WeChatGroupGid
            });
        },

        /**
         * 动态cell点击
         */
        dynamicCellItemClick: function (e) {
            var that = this;
            var cellObj = e.currentTarget.dataset.item.data_quanzi;
            if (cellObj) {
                uni.navigateTo({
                    url: '../../../subpages/pages/user_feed_detail/user_feed_detail?qz_id=' + cellObj.qzid + '&message_id=' + cellObj._id + '&sid=' + that.starIDStr
                });
            }
        },

        /**
         * 请求列表数据
         */
        requestDynamicDataList: function (page) {
            var url = 'https://data.idol001.com/api_moblie_idol_userdt.php?action=get_star_group_dongtai';
            var that = this;
            var params = {
                group_id: that.WeChatGroupGid && that.WeChatGroupGid.length > 0 ? that.WeChatGroupGid : '',
                starid: that.starIDStr,
                page: page,
                count: '1',
                data_from: '2021_11_11'
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('requestDynamicDataList = ', resp.data.list);
                    if (resp && resp.data && resp.data.list && resp.data.list.length > 0) {
                        that.setData({
                            dtObj: resp.data.list[0]
                        });
                    }
                },
                function () {}
            );
        },

        /**
         * 雪人大战是否开启
         */
        requestSnowwarTime: function () {
            var url = 'https://data.idol001.com/snowman_battle.php?action=get_snowman_battle_time';
            var that = this;
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('是否在活动时间内:', resp);
                that.setData({
                    bWarStart: resp.data.data.in_act_time
                });
            });
        },

        //雪人大战
        goSnowmanWar() {
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            let sessionCookie = globalData.cookie ? globalData.cookie.split(/^PHPSESSID=([^\s;]+)\;/g)[1] : '';
            sessionCookie = sessionCookie || '';
            let web_url = `https://m.idol001.com/2021_12s/#/pages/snowman_war/snowman_war?from=minip&sessionid=${sessionCookie}`;
            uni.navigateTo({
                url: '/pages/web_page/web_page?url=' + encodeURIComponent(web_url)
            });
        },

        openDialog(type) {
            this.zpSelectComponent('#rewardWelfareDialog').showView(type);
        },

        showGuardianBeastDialog() {
            this.setData({
                guardianBeastDialog: true
            });
        },

        closeGuardianBeastDialog() {
            this.setData({
                guardianBeastDialog: false
            });
        },

        goTiefenLevel() {
            let param = {
                sid: this.starIDStr
            };
            JumpUtils.jump2MyLevel(param);
        },

        inviteFriend(e) {
            console.log('inviteFriend.click');
            var url = 'https://data.xingxiaoculture.com/api_mobile_star_rank.php?action=get_gather_invite_prize';
            var oThis = this;
            var params = {
                id: this.gatherid
            };
            this.setData({
                hasInvited: this.gatherid
            });
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    if (resp.data.ok == 1) {
                        this.setData({
                            invitedReward: resp.data.num
                        });
                    } else {
                        this.setData({
                            invitedReward: 0
                        });
                    }
                    oThis.checkHashSeeAd();
                    this.setData({
                        bShowFallinSuccess: true,
                        bShowFallinSuccessType: 6
                    });
                }
            });
        },

        checkHashSeeAd() {
            var url = 'https://data.xingxiaoculture.com/api_mobile_star_rank.php?action=is_user_view_ads';
            var oThis = this;
            var params = {
                id: this.gatherid
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data) {
                    if (resp.data.ok == 1) {
                        this.setData({
                            hasSeeAd: true
                        });
                    } else {
                        this.setData({
                            hasSeeAd: false
                        });
                    }
                } else {
                    this.setData({
                        hasSeeAd: false
                    });
                }
            });
        },

        noAction() {
            console.log('占位：函数 noAction 未声明');
        },

        beginConcentrate() {
            console.log('占位：函数 beginConcentrate 未声明');
        },

        seeAdvAttendGroup() {
            console.log('占位：函数 seeAdvAttendGroup 未声明');
        },

        pushFormSubmit() {
            console.log('占位：函数 pushFormSubmit 未声明');
        },

        retryGetData() {
            console.log('占位：函数 retryGetData 未声明');
        }
    }
};
</script>
<style>
@import './group_fall_in_detail.css';
@import '@/pages/subpages_v2/pages/group_fall_in_detail/group_fall_in_detail.css';
@import '@/templates/gatherAlert/gatherAlert.css';
</style>
