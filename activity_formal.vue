<template>
    <view style="height: 100%">
        <!-- template对应的原始代码，为保证正常显示，已对其进行隐藏。 -->
        <block name="rank_list_empty_view" v-if="false">
            <view class="act_rank_list_empty">
                <image class="act_rank_list_empty_icon" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/12/29/rjmDKGfwX31609236901495.png"></image>
                <view class="act_rank_list_empty_text">{{ '暂无排名信息，快去为TA送' + tool_b_name + '吧！' }}</view>
            </view>
        </block>
        <!-- template对应的原始代码，为保证正常显示，已对其进行隐藏。 -->
        <block name="rank_list_item_view" v-if="false">
            <view class="act_rank_list_item" :style="showLine && 0 == 0 ? 'margin-top: 0rpx;' : ''">
                <view class="act_rank_list_item_line" v-if="showLine && 0 != 0"></view>
                <view class="act_rank_list_item_msg">
                    <view
                        :class="
                            'act_rank_list_item_rank ' +
                            (pageDataObj.single_user_rank_info_all_time.rank == 1
                                ? 'act_rank_list_item_rank_one'
                                : pageDataObj.single_user_rank_info_all_time.rank == 2
                                ? 'act_rank_list_item_rank_two'
                                : pageDataObj.single_user_rank_info_all_time.rank == 3
                                ? 'act_rank_list_item_rank_stree'
                                : '')
                        "
                        :style="
                            pageDataObj.single_user_rank_info_all_time.rank == -1 || pageDataObj.single_user_rank_info_all_time.rank == '未上榜'
                                ? 'color: #333333;line-height:30rpx;margin-top:-40rpx'
                                : pageDataObj.single_user_rank_info_all_time.rank < 4
                                ? 'color: #ffffff;'
                                : 'color: #333333;'
                        "
                    >
                        {{ pageDataObj.single_user_rank_info_all_time.rank == -1 ? '未上榜' : pageDataObj.single_user_rank_info_all_time.rank }}
                    </view>
                    <view :class="true ? 'act_rank_list_item_head' : 'act_rank_list_item_head'">
                        <image
                            @tap="gotoDressRoom"
                            class="act_rank_list_item_head_image"
                            :data-userid="pageDataObj.single_user_rank_info_all_time.userinfo._id"
                            :src="pageDataObj.single_user_rank_info_all_time.userinfo.image.middle_pic"
                            v-if="true"
                        ></image>
                        <image
                            class="act_rank_list_item_head_image"
                            :src="
                                pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                    ? pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                    : pageDataObj.single_user_rank_info_all_time.starball.logo_url
                            "
                            v-else
                        ></image>
                    </view>
                    <view class="act_rank_list_item_column" style="flex: 1">
                        <view class="act_rank_list_item_name">
                            {{
                                true
                                    ? pageDataObj.single_user_rank_info_all_time.userinfo.nickname
                                    : pageDataObj.single_user_rank_info_all_time.starinfo
                                    ? pageDataObj.single_user_rank_info_all_time.starinfo.name
                                    : pageDataObj.single_user_rank_info_all_time.starball.name
                            }}
                        </view>
                        <view class="act_rank_list_item_soce">{{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}</view>
                    </view>
                    <view class="act_rank_list_item_column" style="align-items: center; margin-right: 22rpx">
                        <view class="act_rank_list_item_soce" v-if="true">
                            <image class="act_rank_list_item_head" :src="pageDataObj.single_user_rank_info_all_time.max_starinfo.logo_img"></image>
                        </view>
                        <view class="act_rank_list_item_soce" v-else-if="showScore">{{ pageDataObj.single_user_rank_info_all_time.score }}积分</view>
                        <block v-else>
                            <view
                                @tap="sendLeaderBoardClick"
                                class="act_rank_list_item_tuan"
                                :data-item="pageDataObj.single_user_rank_info_all_time"
                                :data-zoneinfo="pageDataObj.bp_time_arr[pageDataObj.phase]"
                                v-if="true"
                            ></view>
                        </block>
                    </view>
                </view>

                <view class="star_buff_box" v-if="pageDataObj.single_user_rank_info_all_time.buff_list">
                    <block v-if="pageDataObj.single_user_rank_info_all_time.buff_list.length > 0">
                        <view class="star_buff_inner_box" @tap="getStarBuffTeamList" :data-starid="pageDataObj.single_user_rank_info_all_time.starid">
                            <image style="width: 177rpx; height: 24rpx" src="https://star-img.xingxiaoculture.com/2023/03/16/7075d915e89af609564323cfbf20e8cc.png"></image>
                            <view class="star_buff_item" v-for="(b_item,0) in (pageDataObj.single_user_rank_info_all_time.buff_list)" :key="0">{{ b_item }}</view>
                        </view>
                        <view
                            class="star_buff_button"
                            :style="pageDataObj.single_user_rank_info_all_time.buff_list.length > 4 ? 'height:72rpx' : ''"
                            @tap="getStarBuffTeamList"
                            :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                        >
                            <image style="width: 27rpx; height: 51rpx" src="https://star-img.xingxiaoculture.com/2023/03/16/2aaedafaef55928f5d223f71aa120999.png"></image>
                        </view>
                    </block>
                    <block v-else-if="">
                        <image
                            style="width: 100%; height: 55rpx"
                            @tap="gotoTreeRule"
                            data-type="2"
                            src="https://star-img.xingxiaoculture.com/2023/05/15/c1360019d293eaaa2ccbcfc9754250d9.png"
                        ></image>
                    </block>
                </view>

                <view class="flex_row flex_center" style="flex-wrap: wrap; width: 100%; margin-top: 20rpx" v-if="pageDataObj.single_user_rank_info_all_time.card_list">
                    <text style="font-size: 11px; color: #333333; font-weight: 600; width: 150rpx">
                        获得卡片
                        <text style="color: #ff5d26">{{ pageDataObj.single_user_rank_info_all_time.card_num }}</text>
                        /8
                    </text>
                    <text style="font-size: 10px; color: #333333; width: 484rpx; text-align: right">
                        每张获得的卡片已为星球增加
                        <text style="color: #ff5d26">200万</text>
                        {{ tool_b_name }}
                    </text>
                    <view class="fengzhen_item" style="width: 148rpx" v-for="(c_item,0) in (pageDataObj.single_user_rank_info_all_time.card_list)" :key="0">
                        <image class="fengzhen_item_image" style="width: 148rpx; height: 190rpx" :src="c_item.card_img"></image>

                        <view
                            v-if="!c_item.userinfo"
                            style="z-index: 1; width: 148rpx; height: 190rpx; background-color: rgba(0, 0, 0, 0.6); margin-top: -190rpx; border-radius: 10rpx"
                        ></view>

                        <view
                            class="flex_column flex_center"
                            v-if="c_item.userinfo"
                            style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx; background: linear-gradient(180deg, #dbf2ff, #b3d4ff)"
                            @tap="gotoDressRoom"
                            :data-userid="c_item.userinfo._id"
                        >
                            <image class="fengzhen_item_user_image" :src="c_item.userinfo.image.thumbnail_pic"></image>
                            <view class="fengzhen_item_star" style="margin-top: 0rpx; color: #333333">{{ c_item.userinfo.nickname }}</view>
                        </view>

                        <view class="flex_column flex_center" v-else-if="" style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx">
                            <view class="fengzhen_item_ding" @tap="tabItemClick" data-index="1" style="font-size: 9px" :data-item="pageDataObj.single_user_rank_info_all_time">
                                帮星球得卡片
                            </view>
                        </view>
                    </view>
                </view>

                <view
                    class="act_rank_list_item_column"
                    style="margin-left: 0; margin-top: 20rpx; width: 632rpx; border-radius: 20rpx; padding: 8rpx; background-color: #77b3f2"
                    v-if="pageDataObj.single_user_rank_info_all_time.top3group"
                >
                    <view class="flex_row flex_center" style="margin-top: 15rpx">
                        <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的公会</view>
                        <view
                            @tap="gotoGonghuiList"
                            class="act_rank_list_more"
                            :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                            style="margin: 0; text-align: right; color: #ffffff"
                        >
                            查看更多>
                        </view>
                    </view>
                    <view
                        class="flex_row flex_center"
                        style="width: 622rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 10rpx; margin-top: 10rpx"
                        v-for="(g_item,0) in (pageDataObj.single_user_rank_info_all_time.top3group)"
                        :key="0"
                    >
                        <view style="width: 38%; height: 64rpx" v-if="g_item.top3user">
                            <image
                                @tap="gotoDressRoom"
                                :data-userid="gu_item.userinfo._id"
                                :src="gu_item.userinfo.image.thumbnail_pic"
                                style="width: 64rpx; height: 64rpx; border-radius: 50%; margin: 2rpx"
                                v-for="(gu_item, index1) in g_item.top3user"
                                :key="index1"
                            ></image>
                        </view>

                        <view class="act_rank_list_item_soce" style="width: 30%; color: #5f758d">{{ g_item.group_name }}</view>

                        <view class="act_rank_list_item_soce" style="width: 25%; text-align: right; color: #5f758d">{{ g_item.votetimes }}</view>
                    </view>
                    <view style="height: 25rpx"></view>
                </view>
                <view
                    class="act_rank_list_item_column"
                    style="margin-left: 0; width: 632rpx; border-radius: 20rpx; padding: 8rpx; margin-top: 20rpx; background-color: #77b3f2"
                    v-if="pageDataObj.single_user_rank_info_all_time.top3user"
                >
                    <view class="flex_row flex_center" style="margin-top: 15rpx">
                        <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的用户</view>
                        <view
                            @tap="gotoPersonList"
                            class="act_rank_list_more"
                            :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                            style="margin: 0; text-align: right; color: #ffffff"
                        >
                            查看更多>
                        </view>
                    </view>
                    <view class="flex_row flex_center">
                        <view
                            class="flex_column flex_center"
                            style="width: 186rpx; height: 146rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 20rpx; margin: 12rpx"
                            v-for="(u_item,0) in (pageDataObj.single_user_rank_info_all_time.top3user)"
                            :key="0"
                        >
                            <image
                                @tap="gotoDressRoom"
                                :data-userid="u_item.userinfo._id"
                                :src="u_item.userinfo.image.thumbnail_pic"
                                style="width: 70rpx; height: 70rpx; border-radius: 50%; margin: 2rpx"
                            ></image>

                            <view class="act_rank_list_item_soce" style="width: 150rpx; overflow: hidden; white-space: nowrap; text-align: center; color: #5e758f">
                                {{ u_item.userinfo.nickname }}
                            </view>

                            <view class="act_rank_list_item_soce" style="color: #5e758f">{{ u_item.votetimes }}</view>
                        </view>
                    </view>
                </view>
                <text class="act_rank_list_item_text" v-if="pageDataObj.single_user_rank_info_all_time.text">{{ pageDataObj.single_user_rank_info_all_time.text }}</text>
                <view style="height: 25rpx" v-else></view>
            </view>
        </block>
        <!-- template对应的原始代码，为保证正常显示，已对其进行隐藏。 -->
        <block name="tree_info" v-if="false">
            <view class="tree_item">
                <view class="btn-group" style="margin-top: 0rpx; border-radius: 5rpx 5rpx 0 0">
                    <view
                        @tap="changeTabTeam"
                        :class="item.tab == 0 ? 'type-btn-on' : 'type-btn'"
                        :data-index="index"
                        data-tab="0"
                        :data-team_type="'inteam'"
                        :style="item.tab == 0 ? 'border-radius: 5rpx 5rpx 0 0 ;' : ''"
                    >
                        队伍信息
                    </view>
                    <view
                        @tap="changeTabTeam"
                        :class="item.tab == 1 ? 'type-btn-on' : 'type-btn'"
                        :data-index="index"
                        data-tab="1"
                        :data-team_type="'inteam'"
                        :style="item.tab == 1 ? 'border-radius: 5rpx 5rpx 0 0 ;' : ''"
                    >
                        助力信息
                    </view>
                    <!-- <view bindtap="changeTabTeam" class="{{item.tab==2?'type-btn-on':'type-btn'}}" data-index="{{index}}" data-tab="2"  data-team_type="{{type}}" style="{{item.tab==1?'border-radius: 5rpx 5rpx 0 0 ;':''}}">
            解锁拼图
        </view> -->
                </view>
                <block v-if="item.tab == 0">
                    <view class="tree_team">
                        <view class="tree_team_title">
                            {{ item.my_team.team_name }}
                            <image
                                @tap="showShortnameSet"
                                :data-item="item.my_team"
                                src="https://star-img.xingxiaoculture.com/2023/03/10/a695c2e50d04260e6696e43be60a82a8.png"
                                style="width: 24rpx; height: 24rpx"
                                v-if="item.my_team.is_captain"
                            ></image>
                        </view>
                        <view class="flex_row" style="position: absolute; right: 20rpx">
                            <view @tap="showChangeCap" class="tree_team_applay" :data-item="item.my_team" v-if="item.is_captain && item.my_team.members.length > 1">转让队长</view>
                            <block v-if="(arenaInfo.status == 'not_begin' && arenaInfo.term_id == item.my_team.term_id) || arenaInfo.term_id < item.my_team.term_id">
                                <view @tap="showDismissTeam" class="tree_team_applay" :data-team_id="item.my_team.team_id" v-if="item.is_captain">解散队伍</view>
                                <view @tap="showQuitTeam" class="tree_team_applay" :data-team_id="item.my_team.team_id" v-else-if="!item.is_captain">退出队伍</view>
                            </block>
                            <view @tap="handleTeamApply" class="tree_team_applay" :data-item="item.my_team" data-type="1" v-if="item.is_captain">
                                申请列表
                                <view class="content_set_cloud_log_point" v-if="item.is_captain && item.not_agree_num > 0">{{ item.not_agree_num }}</view>
                            </view>
                        </view>
                    </view>
                    <text style="color: #666666; font-size: 10px; margin-left: 20rpx; margin-top: 10rpx" v-if="item.auto_gen_team">
                        本队伍是系统将上个月星球最强的五名粉丝自动创建的队伍
                    </text>

                    <view class="tree_info">
                        <!-- <view class="tree_info_column">
                    <image class="tree_info_image" src="{{item.my_team.star_tree_inner}}"></image>
                </view>
                <view class="tree_info_column">
                    <view class="tree_info_item" style="margin-top: 15rpx;">
                        <view class="candy_img2">
                            <image mode="aspectFit" src="https://star-img.xingxiaoculture.com/2023/01/04/771d7841a205aef5ed408e5f9b6f9090.png" style="width: 70rpx;height:70rpx;margin-left: 6rpx;"></image>
                        </view>
                        <view class="tree_info_text3" style="line-height: 76rpx;" wx:if="{{(!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status=='not_begin'||balloonTermId>arenaInfo.term_id}}">
                            <text>{{(!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status=='not_begin'?actArenaCountdownTimeStr:'未开始'}}</text>
                        </view>
                        <view class="tree_info_text3" style="line-height: 76rpx;" wx:elif="{{(!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status=='is_end'||balloonTermId < arenaInfo.term_id}}">
                            <text>当前场次已结束</text>
                        </view>
                        <view class="tree_info_text3" wx:elif="{{arenaInfo.status=='first_half'}}">
                            <text>当前时段15:'00'-18:'00'\n上半场进行中</text>
                        </view>
                        <view class="tree_info_text3" wx:elif="{{arenaInfo.status=='half_time'}}">
                            <text>当前时段18:'00'-19:'00'\n中场休息中</text>
                        </view>
                        <view class="tree_info_text3" wx:elif="{{arenaInfo.status=='second_half'}}">
                            <text>当前时段19:'00'-21:'00'\n下半场进行中</text>
                        </view>
                    </view>
                    <view class="flex_row">
                        <button open-type="share" class="tree_info_button" data-item="{{item.my_team}}" data-team_id="{{item.my_team.team_id}}" data-action="zhuli" style="background-image:url(https://star-img.xingxiaoculture.com/2023/04/10/dc3c6fae213b225bd31ae15a797dcb16.png)">
                        </button>
                    </view>
                    <view class="flex_row">
                        
                        <view bindtap="showVotePanel" class="tree_info_button" data-item="{{item.my_team}}" data-team_id="{{item.my_team.team_id}}" style="{{item.my_team.in_team?'background-image:url(https://star-img.xingxiaoculture.com/2023/04/10/6ca99e34fcef891277f4db5d346c1483.png)':'background-image:url(https://star-img.xingxiaoculture.com/2023/04/10/fc74dfea7b17dc58415c400e4131778d.png)'}}">
                        </view>
                    </view>
                </view> -->

                        <view class="tree_info_column" style="width: 30%">
                            <image class="tree_info_image" :src="item.my_team.star_tree_inner"></image>
                        </view>
                        <view class="tree_info_column" style="width: 70%">
                            <view
                                class="tree_info_text3"
                                v-if="((!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status == 'not_begin') || balloonTermId > arenaInfo.term_id"
                            >
                                <text>{{ (!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status == 'not_begin' ? actArenaCountdownTimeStr : '未开始' }}</text>
                            </view>
                            <view
                                class="tree_info_text3"
                                v-else-if="((!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status == 'is_end') || balloonTermId < arenaInfo.term_id"
                            >
                                <text>当前场次已结束</text>
                            </view>
                            <view class="tree_info_text3" v-else-if="arenaInfo.status == 'first_half'">
                                <text>上半场进行中(15:'00'-18:'00')</text>
                            </view>
                            <view class="tree_info_text3" v-else-if="arenaInfo.status == 'half_time'">
                                <text>中场休息中(18:'00'-19:'00')</text>
                            </view>
                            <view class="tree_info_text3" v-else-if="arenaInfo.status == 'second_half'">
                                <text>下半场进行中(19:'00'-21:'00')</text>
                            </view>
                            <view class="flex_row flex_center" style="margin-top: 20rpx">
                                <image
                                    style="width: 63rpx; height: 63rpx"
                                    v-if="item.my_team.rank == 1"
                                    src="https://star-img.xingxiaoculture.com/2023/05/17/5db7e3d7885370616a5180409c6f5598.png"
                                ></image>
                                <image
                                    style="width: 63rpx; height: 63rpx"
                                    v-else-if="item.my_team.rank == 2"
                                    src="https://star-img.xingxiaoculture.com/2023/05/18/f8ec4f4a011af5f3b0dde4aa546547c3.png"
                                ></image>
                                <image
                                    style="width: 63rpx; height: 63rpx"
                                    v-else-if="item.my_team.rank == 3"
                                    src="https://star-img.xingxiaoculture.com/2023/05/18/e641e98b76e432e25c24dd1de7cf4bb9.png"
                                ></image>
                                <view
                                    style="
                                        width: 63rpx;
                                        height: 63rpx;
                                        background: url(https://star-img.xingxiaoculture.com/2023/05/18/7f069ba185f9e95efce1d8dd4f8ac9ce.png);
                                        background-size: 100% 100%;
                                        line-height: 66rpx;
                                        text-align: center;
                                        font-size: 10px;
                                        color: #a56a59;
                                        width: 63rpx;
                                    "
                                    v-else
                                >
                                    {{ item.my_team.rank == '未上榜' ? '' : item.my_team.rank }}
                                </view>

                                <view style="font-size: 13px; color: #333333; margin-left: 10rpx">
                                    <text>排名\n</text>
                                    <text style="color: #ff7b1e; font-size: 10px">{{ item.my_team.rank == '未上榜' ? '--' : 'NO.' + item.my_team.rank + ' >' }}</text>
                                </view>
                                <image
                                    style="width: 63rpx; height: 63rpx; margin-left: 20rpx"
                                    src="https://star-img.xingxiaoculture.com/2023/05/17/d5158ae3bfce5a45f7b69d2dc0a66365.png"
                                ></image>
                                <view style="font-size: 13px; color: #333333; margin-left: 10rpx">
                                    <text>已行驶距离\n</text>
                                    <text style="color: #ff7b1e; font-size: 10px">{{ item.my_team.team_fruit ? item.my_team.team_fruit + '米' : '0米' }}</text>
                                </view>
                            </view>
                        </view>
                    </view>

                    <view class="tree_info3">
                        <view class="flex_row" style="align-items: center; height: 45%">
                            <image
                                style="width: 130rpx; height: 66rpx; background-color: #e8f2f8; margin-left: 20rpx"
                                src="https://star-img.xingxiaoculture.com/2023/05/17/79b2c76727f9c57b7cc0840bc7e8a4b5.png"
                                mode="aspectFit"
                            ></image>
                            <text style="font-size: 13px; margin-left: 20rpx">
                                <text style="font-weight: 600">
                                    当前行驶速度：
                                    <text style="color: #ff881a">{{ item.my_team.race_info.speed ? item.my_team.race_info.speed : 0 }}</text>
                                    米/分钟
                                </text>
                                \n < text style="font-size: 10px;" wx:if="{{
                                    item.my_team.race_info.energy_consume_per_minute && item.my_team.race_info.energy_consume_per_minute > item.my_team.race_info.energy
                                }}">因能源耗尽，当前行驶速度已经降到初始速度
                            </text>
                            <text style="font-size: 10px" v-else-if="item.my_team.race_info.distance">
                                若保持，结束时共可行驶
                                <text style="color: #ff881a">{{ item.my_team.race_info.distance }}</text>
                                米
                            </text>
                        </view>
                        <view class="flex_row" style="align-items: center; height: 55%; position: relative">
                            <image
                                style="width: 130rpx; height: 66rpx; background-color: #e8f2f8; margin-left: 20rpx"
                                src="https://star-img.xingxiaoculture.com/2023/05/17/e7de941ab56fe9bf0de516634d43163e.png"
                                mode="aspectFit"
                            ></image>

                            <block v-if="item.my_team.race_info.energy_consume_per_minute && item.my_team.race_info.energy_consume_per_minute > item.my_team.race_info.energy">
                                <text style="font-size: 13px; margin-left: 20rpx; font-weight: 600">
                                    <text style="color: #ff881a">星能源已耗尽！</text>
                                    请尽快补充！
                                </text>
                            </block>
                            <block v-else>
                                <text style="font-size: 13px; margin-left: 20rpx">
                                    <text style="font-weight: 600">
                                        剩余星能源：
                                        <text style="color: #ff881a">{{ item.my_team.race_info.energy ? item.my_team.race_info.energy : 0 }}</text>
                                        点
                                    </text>
                                    \n < text style="font-size: 10px;" wx:if="{{ item.my_team.race_info.energy_consume_per_minute }}">当前速度消耗能源：
                                    <text style="color: #ff881a">{{ item.my_team.race_info.energy_consume_per_minute }}</text>
                                    点/分钟\n
                                </text>
                                <text style="font-size: 10px" v-if="item.my_team.race_info.energy_left_minute && item.my_team.race_info.energy_consume_per_minute">
                                    预计还可消耗：
                                    <text style="color: #ff881a">{{ item.my_team.race_info.energy_left_minute }}</text>
                                    分钟
                                </text>
                            </block>

                            <view
                                v-if="item.my_team.race_info.energy_consume_per_minute && item.my_team.race_info.energy_consume_per_minute > item.my_team.race_info.energy"
                                style="
                                    color: #ffffff;
                                    background-color: #ff2020;
                                    border-radius: 50%;
                                    text-align: center;
                                    width: 28rpx;
                                    height: 28rpx;
                                    line-height: 28rpx;
                                    position: absolute;
                                    left: 130rpx;
                                    top: 15rpx;
                                    font-size: 12px;
                                "
                            >
                                !
                            </view>
                        </view>
                    </view>

                    <view class="flex_row flex_center">
                        <!-- <text class="team_info_line_text">队伍排名：{{item.my_team.team_fruit_rank!='未上榜'?'No. ':''}}{{item.my_team.team_fruit_rank}}</text>
                <text class="team_info_line_text">队伍总积分：{{item.my_team.team_fruit}}分</text>
                <view bindtap="handleTeamMore" class="team_info_line_right" data-type="2">查看排名></view> -->
                        <button
                            open-type="share"
                            :data-item="item.my_team"
                            :data-team_id="item.my_team.team_id"
                            data-action="zhuli"
                            style="
                                background-image: url(https://star-img.xingxiaoculture.com/2023/05/17/16f406fe8fed5b650d2f9035a0f37f99.png);
                                width: 304rpx;
                                height: 74rpx;
                                margin: 20rpx;
                                background-size: 100% 100%;
                            "
                        ></button>
                        <image
                            v-if="!item.my_team.in_team"
                            @tap="gotoTeamBattle"
                            :data-item="item.my_team"
                            :data-team_id="item.my_team.team_id"
                            src="https://star-img.xingxiaoculture.com/2023/05/17/a2f902f496e29cdde58ca8318f5c1b3f.png"
                            style="width: 304rpx; height: 74rpx; margin: 20rpx"
                        ></image>
                        <image
                            v-else
                            @tap="gotoTeamBattle"
                            :data-item="item.my_team"
                            :data-team_id="item.my_team.team_id"
                            src="https://star-img.xingxiaoculture.com/2023/05/17/032c57b72f0dbfcde9bd1d235e511b55.png"
                            style="width: 304rpx; height: 74rpx; margin: 20rpx"
                        ></image>
                    </view>

                    <view class="team_info_line" style="height: 30rpx; background: none">
                        <text class="team_info_line_text3" style="width: 200rpx; margin-left: 30rpx">队伍角色</text>
                        <text class="team_info_line_text3" style="width: 210rpx">当前场上成员</text>
                        <text class="team_info_line_text3" style="width: 360rpx">说明</text>
                    </view>
                    <view class="team_info_line" v-for="(member, index) in item.my_team.members_pos" :key="index">
                        <view class="flex_column">
                            <image class="team_info_line_area_image" :src="member.img"></image>
                            <text class="team_info_line_text" style="width: 150rpx; overflow: hidden; white-space: nowrap">
                                {{ member.userinfo ? member.userinfo.nickname : '空缺中' }}
                            </text>
                        </view>

                        <view class="team_info_line_user_image" style="margin-left: 20rpx">
                            <image
                                @tap="gotoDressRoom"
                                class="team_info_line_user_image"
                                :data-userid="member.userinfo._id"
                                :src="
                                    member.userinfo ? member.userinfo.image.thumbnail_pic : 'https://star-img.xingxiaoculture.com/2023/05/17/5816227c5defe1cf9ed6627933bb6c35.png'
                                "
                            ></image>
                            <view class="team_info_line_user_image_cap" v-if="member.is_captain">队长</view>
                            <button class="tree_team_join_button" :data-pos="index" :data-team_id="item.my_team.team_id" openType="share" v-if="!member.userinfo">+</button>
                        </view>

                        <view class="flex_row" style="align-items: center; width: 300rpx; margin-left: 120rpx">
                            <!-- <view class="flex_column" style="align-items: flex-end;">
                        <text class="team_info_line_text2">{{member.score?member.score:0}}积分</text>
                        <block wx:if="{{member.diff}}">
                            <text class="team_info_line_text2" wx:if="{{member.diff>0}}">领先第二名<text style="color:#3a3833 ;">{{member.diff}}</text>点</text>
                            <text class="team_info_line_text2" wx:else>差上一名<text style="color:#3a3833 ;">{{-member.diff}}</text>{{tool_b_unit}}</text>
                        </block>
                    </view> -->
                            <view class="flex_column" style="align-items: flex-start">
                                <view class="team_info_line_text2">
                                    <text :style="titem.hl ? 'color:#FFFFFF' : ''" v-for="(titem, tindex) in loactionDescArr[index]" :key="tindex">{{ titem.text }}</text>
                                </view>
                            </view>
                            <!-- <view wx:if="{{!item.my_team.in_team}}" style="{{!member.userinfo?'visibility:hidden':''}}" class="team_info_zhuli_button" bindtap="showVotePanel" data-item="{{item.my_team}}" data-team_id="{{item.my_team.team_id}}" data-pos="{{index}}">
                    助力
                    </view> -->
                        </view>

                        <!-- <view bindtap="handleTeamMore" class="team_info_line_right" data-loc="{{index}}" data-type="2">
                    <image src="https://star-img.xingxiaoculture.com/2023/03/10/a44cdb933170081060d381e4ebe37f64.png" style="width: 12rpx;height: 20rpx;"></image>
                </view> -->
                    </view>
                    <view class="tree_info_button_box" v-if="item.my_team.in_team">
                        <view
                            @tap="showChangeLocation"
                            class="tree_info_button3"
                            :data-item="item.my_team"
                            style="
                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/554b28b836b48aac9bbdc1b70ac80863.png);
                                background-size: 100% 100%;
                                line-height: 30rpx;
                            "
                        >
                            <text>
                                位置调换
                                <text style="font-size: 10px">（剩余{{ item.my_team.left_times }}次）</text>
                            </text>
                        </view>

                        <view
                            v-if="
                                (balloonTermId && balloonTermId != arenaInfo.term_id) ||
                                ((!balloonTermId || balloonTermId == arenaInfo.term_id) && (arenaInfo.status == 'not_begin' || arenaInfo.status == 'is_end'))
                            "
                            class="tree_info_button3"
                            style="
                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/10aeefb9ad989448863a6df23f24167a.png);
                                background-size: 100% 100%;
                                line-height: 50rpx;
                                width: 242rpx;
                            "
                        >
                            <text>更换成员上场</text>
                        </view>
                        <view
                            v-else-if="arenaInfo.status == 'second_half'"
                            @tap="showSwitchMember"
                            class="tree_info_button3"
                            :data-item="item.my_team"
                            style="
                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/e394d545704b0e5009c5d515e757f025.png);
                                background-size: 100% 100%;
                                line-height: 30rpx;
                                width: 242rpx;
                            "
                        >
                            <text>
                                更换成员上场
                                <text style="font-size: 10px">（剩余{{ item.my_team.replace_member_left_times }}次）</text>
                            </text>
                        </view>
                        <view
                            v-else
                            class="tree_info_button3"
                            :data-item="item.my_team"
                            style="
                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/10aeefb9ad989448863a6df23f24167a.png);
                                background-size: 100% 100%;
                                line-height: 30rpx;
                                width: 242rpx;
                            "
                        >
                            <text>
                                更换成员上场
                                <text style="font-size: 10px">下半场可换</text>
                            </text>
                        </view>
                        <view
                            @tap="showTeamInfo"
                            class="tree_info_button3"
                            :data-item="item.my_team"
                            style="background: url(https://star-img.xingxiaoculture.com/2023/05/17/ed3fac5acbeeeb401f683cbd909a3f91.png); background-size: 100% 100%"
                        >
                            查看队伍详情
                        </view>
                    </view>
                    <view class="tree_info_button_box" v-else>
                        <view
                            @tap="showTeamInfo"
                            class="tree_info_button3"
                            :data-item="item.my_team"
                            data-tab="1"
                            style="background: linear-gradient(180deg, #4fcaff, #217ef9); line-height: 67rpx; width: 635rpx; border-radius: 34rpx; border: 5rpx double #9dddff"
                        >
                            <text>查看我的助力信息</text>
                        </view>
                    </view>
                </block>
                <block v-else-if="item.tab == 1">
                    <view style="width: 100%; display: flex; flex-direction: column; align-items: center" v-if="item.help_info && item.help_info.msg_list.length > 0">
                        <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                            <view class="group_pk_prizes_list_line"></view>
                            <view class="group_pk_prizes_list_text2">助力动态</view>
                        </view>
                        <view class="team_info_line" style="height: 30rpx; background: none">
                            <!-- <text class="team_info_line_text3" style="width: 220rpx;margin-left: 20rpx;">魅力类型</text> -->
                            <text class="team_info_line_text3" style="width: 350rpx; margin-left: 20rpx">助力成员</text>
                            <text class="team_info_line_text3" style="width: 350rpx">助力体力点</text>
                            <text class="team_info_line_text3" style="width: 170rpx">助力时间</text>
                        </view>
                        <scroll-view class="zhuli_dongtai_box" :scroll-y="true">
                            <view class="zhuli_dongtai_box_item" v-for="(m_item, index) in item.help_info.msg_list" :key="index">
                                <!-- <image style="width: 116rpx;height: 28rpx;margin-left: 20rpx;" src="{{m_item.pos_img}}"></image> -->

                                <image class="zhuli_dongtai_box_item_icon" style="margin-left: 20rpx" :src="m_item.userinfo.image.middle_pic"></image>

                                <view class="zhuli_dongtai_box_item_text" style="width: 200rpx">{{ m_item.userinfo.nickname }}</view>

                                <view class="zhuli_dongtai_box_item_text" style="width: 260rpx">
                                    助力
                                    <text style="color: #ff6800">{{ m_item.votetimes }}</text>
                                </view>

                                <view class="zhuli_dongtai_box_item_text">{{ m_item.time }}</view>
                            </view>
                        </scroll-view>
                        <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                            <view class="group_pk_prizes_list_line"></view>
                            <view class="group_pk_prizes_list_text2">助力用户</view>
                        </view>
                        <view class="team_info_line" style="height: 30rpx; background: none">
                            <text class="team_info_line_text3" style="width: 350rpx; margin-left: 30rpx">用户信息</text>
                            <text class="team_info_line_text3" style="width: 400rpx">共助力数量</text>
                            <text class="team_info_line_text3" style="width: 180rpx">全队占比</text>
                        </view>
                        <view class="zhuli_dongtai_box" style="margin-bottom: 30rpx; height: 332rpx">
                            <view class="zhuli_dongtai_box_item" v-for="(m_item, index) in item.help_info.user_list" :key="index">
                                <image class="zhuli_dongtai_box_item_icon" style="margin-left: 30rpx" :src="m_item.userinfo.image.middle_pic"></image>

                                <view class="zhuli_dongtai_box_item_text" style="width: 180rpx">{{ m_item.userinfo.nickname }}</view>

                                <view class="zhuli_dongtai_box_item_text" style="width: 260rpx">
                                    已助力
                                    <text style="color: #ff6800">{{ m_item.votetimes }}</text>
                                </view>

                                <view class="zhuli_dongtai_box_item_text" style="color: #ff6800">{{ m_item.percent }}</view>
                            </view>
                            <view class="zhuli_dongtai_box_more_info" @tap="showTeamInfo" :data-item="item.my_team">查看队伍信息</view>
                        </view>
                    </view>
                    <view class="zhuli_none_box" v-else>
                        <image src="https://star-img.xingxiaoculture.com/2023/05/17/6f1197e16c976fa2a6fa3476686c5983.png" style="width: 265rpx; height: 196rpx"></image>
                        <view class="zhuli_box_none_text">抱歉，当前暂无助力信息~</view>
                    </view>
                </block>
                <block v-else-if="item.tab == 2">
                    <view style="width: 100%; display: flex; flex-direction: column; align-items: center">
                        <view class="team_info_line" style="height: 52rpx; margin-top: 20rpx">
                            <image class="team_info_line_area_image" :src="item.my_team.members_pos[item.puzzle_pos ? item.puzzle_pos : 0].img"></image>
                            <view @tap="showBalloonLocationPanel" class="team_info_line_right" :data-item="item.my_team" :data-team_type="'inteam'">切换魅力类型></view>
                        </view>
                        <!-- <text class="tree_info_text4" style="width: 600rpx;text-align: center;margin-top: 10rpx;">全部解锁后，在当前魅力类型贡献过的成员（包括助力成员） 都可获得<text style="color: #FF6800;">奖励N个</text></text> -->
                        <view style="width: 660rpx; height: 990rpx; position: relative; margin-top: 10rpx; margin-bottom: 20rpx">
                            <image style="width: 660rpx; height: 990rpx" :src="item.puzzle.img"></image>
                            <view
                                style="
                                    width: 660rpx;
                                    height: 990rpx;
                                    position: absolute;
                                    display: flex;
                                    flex-wrap: wrap;
                                    top: 0;
                                    left: 0;
                                    align-items: center;
                                    justify-content: center;
                                "
                            >
                                <view
                                    class="puzzle_block"
                                    :style="p_item.userid != '' ? 'opacity: 0;' : ''"
                                    :data-item="item"
                                    :data-team_type="'inteam'"
                                    :data-piece="p_index"
                                    :data-index="index"
                                    data-tab="2"
                                    @tap="parseEventDynamicCode($event, p_item.userid != '' ? '' : 'unlockJigsawClick')"
                                    v-for="(p_item, p_index) in item.puzzle.list"
                                    :key="p_index"
                                ></view>
                            </view>
                        </view>
                    </view>
                </block>
            </view>
        </block>
        <block v-if="isGroupUser">
            <view class="main_view">
                <view
                    @tap="switchTopBanner"
                    class="act_rank_list_rule2"
                    :style="
                        (isShowTopBannerManager
                            ? 'background:url(https://star-img.xingxiaoculture.com/2023/01/31/69f7cec8af198b7400ee6d4627baabb2.png); '
                            : 'background: url(https://star-img.xingxiaoculture.com/2023/05/18/c36fc3e6b2ab0a54109653176af3ccf6.png);') + 'background-size: 100% 100%;'
                    "
                ></view>

                <!-- <view bindtap="switchTopDanmu" class="act_rank_list_rule2" style=" top: 180rpx;;{{isShowTopDanmu?'background:url(https://star-img.xingxiaoculture.com/2023/04/17/dd4264ccec4ff0cf516e8246229eee34.png); ':'background: url(https://star-img.xingxiaoculture.com/2023/05/18/dc7691ca44f7aba61c559afbbe07266b.png);'}}background-size: 100% 100%;"></view> -->

                <view class="top_view">
                    <view @tap="showDialogEnter" class="act_rank_list_rule">背景故事</view>
                    <view @tap="activityrule" class="act_rank_list_rule" style="top: 120rpx">活动规则</view>
                </view>
                <view class="tab_view_father" :style="nowPageScrollValue > 300 && false ? 'position:fixed;top: -20rpx;left: 10rpx;z-index:3' : ''">
                    <view class="tab_view">
                        <view class="tab_item_view" v-for="(item, index) in selectDate" :key="index">
                            <view @tap="tabItemClick" :class="index == selectIndex ? 'tab_item_btn_select' : 'tab_item_btn_normal'" :data-index="index">{{ item }}</view>
                        </view>
                    </view>
                </view>
                <view class="act_schedule_view" :style="nowPageScrollValue > 300 && false ? 'margin-top:132rpx' : ''">
                    <view class="act_schedule_viewline" style="top: 304rpx"></view>
                    <scroll-view class="act_schedule_scrollview" :scrollIntoView="'index' + scheduleActSelectIndex" :scrollX="true">
                        <view style="display: flex; margin-top: 170rpx">
                            <view class="act_schedule_item_view" :id="'index' + index" v-for="(item, index) in pageDataObj.bp_time_arr" :key="index">
                                <view
                                    class="schedule_item_view_left"
                                    :style="index == pageDataObj.bp_time_arr.length - 1 ? 'margin-right: 70rpx;' : index == 0 ? 'margin-left: 60rpx;' : ''"
                                >
                                    <view
                                        @tap="scheduleTabClick"
                                        class="schedule_item_view_act"
                                        :data-index="index"
                                        :style="
                                            'background-image: url(' +
                                            (item.status == 0
                                                ? 'https://star-img.xingxiaoculture.com/2023/05/15/466205247971a893b70939ecd6085e89.png'
                                                : item.status == 1
                                                ? 'https://star-img.xingxiaoculture.com/2023/05/15/c037deefa57b59fb208ac96e6494eb37.png'
                                                : 'https://star-img.xingxiaoculture.com/2023/05/15/ae1087059583682370026075927ab668.png') +
                                            ')'
                                        "
                                    >
                                        <text
                                            class="schedule_item_view_text_title"
                                            :style="item.status == 0 ? ' color: #FFFFFF;' : item.status == 1 ? ' color: #FFFFFF;' : ' color: #355251;'"
                                        >
                                            {{ item.title_bar }}
                                        </text>
                                    </view>
                                    <view
                                        class="schedule_item_view_dot"
                                        :style="
                                            item.status == 0
                                                ? 'background:#B6F3F6;border:4rpx solid #8ED2D1'
                                                : item.status == 1
                                                ? 'background:#FFC529;border:4rpx solid #EF8E18'
                                                : 'background:#558685;border:4rpx solid #689E9D'
                                        "
                                    ></view>
                                    <text
                                        class="schedule_item_view_text"
                                        :style="
                                            item.status == 0
                                                ? ' color: #FFFFFF;background: #89D1D0;'
                                                : item.status == 1
                                                ? 'color: #FFFFFF;background: #F88606;'
                                                : 'color: #345251;background: #6F9D9C;'
                                        "
                                    >
                                        {{ pageDataObj.single_user_rank_info_all_time.text }}
                                    </text>
                                </view>

                                <view
                                    :class="'schedule_item_view_time ' + (item.rest_status == 1 ? 'schedule_item_view_time_se' : '')"
                                    v-if="index != pageDataObj.bp_time_arr.length - 1 && item.rest"
                                >
                                    <text
                                        @tap="stopDayTabClick"
                                        class="schedule_item_view_time_text"
                                        :data-index="index"
                                        :style="item.rest_status == 1 ? 'color: #FFFFFF;' : 'color: #FFFFFF;'"
                                    >
                                        {{ item.rest }} 休整
                                    </text>
                                </view>
                            </view>
                        </view>
                    </scroll-view>
                </view>

                <image
                    style="width: 750rpx; height: 157rpx; margin-top: 80rpx"
                    src="https://star-img.xingxiaoculture.com/2023/05/27/b929a8c7730462c95a418fb88ea6498e.png"
                    @tap="gotoSearchGame"
                ></image>

                <view class="act_rank_list_view tabPage1">
                    <image class="act_tb_head_image" src=""></image>
                    <image class="act_rank_list_image" src="https://star-img.xingxiaoculture.com/2023/05/17/b006c53638a6534763ef36d81dc58e04.png"></image>
                    <view class="act_rank_list_time" v-if="balloonTermId > arenaInfo.term_id">当前场次未开始</view>
                    <view class="act_rank_list_time" v-else>当前场次{{ actArenaCountdownTimeStr }}</view>
                    <view @tap="showArenaDateSelect" class="act_rank_list_date">
                        {{ balloonTermDate }}
                        <text v-if="!bShowBalloonDateSelect">▼</text>
                        <text v-else>▲</text>
                    </view>
                    <view class="act_rank_list_date_select" v-if="bShowBalloonDateSelect">
                        <view @tap="changeArenaRank" class="act_rank_list_date_item" :data-item="item" v-for="(item, index) in arenaList" :key="index">{{ item.datetime }}</view>
                    </view>
                    <view class="act_rank_list_phase_box">
                        <text
                            :class="
                                'act_rank_list_phase_item ' +
                                (arenaInfo.status == 'first_half'
                                    ? 'act_rank_list_phase_item_begin'
                                    : areanStatusToNum[arenaInfo.status] > areanStatusToNum['first_half']
                                    ? 'act_rank_list_phase_item_end'
                                    : '')
                            "
                        >
                            <text class="act_rank_list_phase_item_text">上半场</text>
                            15:'00'～18:'00'
                        </text>
                        <image class="act_rank_list_phase_arrow" src="https://star-img.xingxiaoculture.com/2023/05/17/3b559f7ef01b11ee5b050af9462f3ba8.png"></image>
                        <text
                            :class="
                                'act_rank_list_phase_item ' +
                                (arenaInfo.status == 'half_time'
                                    ? 'act_rank_list_phase_item_begin'
                                    : areanStatusToNum[arenaInfo.status] > areanStatusToNum['half_time']
                                    ? 'act_rank_list_phase_item_end'
                                    : '')
                            "
                        >
                            <text class="act_rank_list_phase_item_text">中场休息</text>
                            18:'00'～19:'00'
                        </text>
                        <image class="act_rank_list_phase_arrow" src="https://star-img.xingxiaoculture.com/2023/05/17/3b559f7ef01b11ee5b050af9462f3ba8.png"></image>
                        <text
                            :class="
                                'act_rank_list_phase_item ' +
                                (arenaInfo.status == 'second_half'
                                    ? 'act_rank_list_phase_item_begin'
                                    : areanStatusToNum[arenaInfo.status] > areanStatusToNum['second_half']
                                    ? 'act_rank_list_phase_item_end'
                                    : '')
                            "
                        >
                            <text class="act_rank_list_phase_item_text">下半场</text>
                            19:'00'～21:'00' 可换人上场
                        </text>
                    </view>
                    <image @tap="gotoTreeRule" class="act_rank_list_banner2" :data-type="2" :src="arenaBanner" style="height: 98rpx"></image>
                    <image
                        @tap="gotoMengHuList"
                        src="https://star-img.xingxiaoculture.com/2023/05/17/bec06dfea5246082acf6b694f49ceb2f.png"
                        style="width: 683rpx; height: 68rpx; margin-top: 20rpx"
                    ></image>
                    <image
                        src="https://star-img.xingxiaoculture.com/2023/05/17/9630d14a443df92fe7fba336bd0a3892.png"
                        style="width: 316rpx; height: 35rpx; margin-top: 40rpx"
                    ></image>
                    <view class="tree_top3_area">
                        <image src="https://star-img.xingxiaoculture.com/2023/05/17/04a5f25b80a739765578b6723c366ea7.png" style="width: 680rpx; height: 727rpx"></image>
                        <view class="tree_top3_area_item_text2">{{ allBalloonteamTree[0].slogan }}</view>
                        <image
                            @tap="showShortnameSet"
                            :data-item="allBalloonteamTree[0]"
                            :data-type="2"
                            src="https://star-img.xingxiaoculture.com/2023/05/17/7f44b9be6b65a481099d950348cad692.png"
                            style="width: 98rpx; height: 36rpx; position: absolute; top: 11rpx; right: 6rpx"
                            v-if="allBalloonteamTree[0].in_team"
                        ></image>
                        <view :class="'flex-c_center tree_top3_area_item_position_r' + index" v-for="(item, index) in allBalloonteamTree" :key="index">
                            <image :src="item.star_tree" :style="index == 0 ? 'width: 126rpx;height: 126rpx;' : 'width: 126rpx;height: 126rpx;'"></image>

                            <view :class="'tree_top3_area_item tree_top3_area_item_r' + index">
                                <view class="tree_top3_area_item_title">{{ item.team_name }}</view>
                                <view class="tree_top3_area_item_line">
                                    <image
                                        @tap="gotoDressRoom"
                                        class="tree_top3_area_item_head"
                                        :data-obj="member.userinfo"
                                        :data-userid="member.userinfo._id"
                                        :src="member.userinfo.image.middle_pic"
                                        v-for="(member, index1) in item.members"
                                        :key="index1"
                                    ></image>
                                </view>
                                <text
                                    style="
                                        color: #e0110f;
                                        font-size: 9px;
                                        width: 125rpx;
                                        line-height: 32rpx;
                                        position: absolute;
                                        bottom: 0rpx;
                                        left: 40rpx;
                                        text-align: center;
                                        font-weight: 500;
                                    "
                                >
                                    {{ item.team_fruit }}米
                                </text>
                            </view>

                            <view
                                style="
                                    width: 144rpx;
                                    line-height: 40rpx;
                                    border-radius: 26rpx;
                                    color: #ffffff;
                                    font-size: 11px;
                                    text-align: center;
                                    background: linear-gradient(180deg, #4fcaff, #217ef9);
                                    margin-top: 10rpx;
                                "
                                @tap="gotoTeamBattle"
                                :data-item="item"
                                :data-team_id="item.team_id"
                                v-if="!item.in_team"
                            >
                                助力队伍
                            </view>
                        </view>
                        <view @tap="handleTeamMore" class="tree_top3_area_text" data-type="2">查看全部队伍排名 >></view>
                    </view>
                    <!-- <block wx:if="{{( (!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status!='not_begin'||arenaInfo.term_id>balloonTermId )&&myBalloonTeam&&myBalloonTeam.length>0}}"> -->
                    <block v-if="false">
                        <image
                            src="https://star-img.xingxiaoculture.com/2023/04/06/c4981738bc6af74bfe318a433eff74f4.png"
                            style="width: 280rpx; height: 42rpx; margin-top: 40rpx"
                        ></image>
                        <view class="btn-group">
                            <view @tap="changeTab" :class="tabIndex == 0 ? 'type-btn-on' : 'type-btn'" data-tab="0">积分</view>
                            <view @tap="changeTab" :class="tabIndex == 1 ? 'type-btn-on' : 'type-btn'" data-tab="1">魅力值</view>
                            <view @tap="changeTab" :class="tabIndex == 3 ? 'type-btn-on' : 'type-btn'" data-tab="3">距离上一名差距</view>
                        </view>
                        <view class="top3_list">
                            <view class="top3_list_line_ab">
                                <view class="top3_list_num">1</view>
                                <view class="top3_list_num">2</view>
                                <view class="top3_list_num">3</view>
                            </view>
                            <view class="top3_list_line" v-for="(item, index) in locaationTop3" :key="index">
                                <view class="top3_list_title">{{ loactionArr[index] }}</view>

                                <view
                                    class="top3_list_item"
                                    :style="
                                        people.team_score_rank == 1
                                            ? 'border: 3rpx solid #FFC714;'
                                            : people.team_score_rank == 2
                                            ? 'border: 3rpx solid #FF3C77;'
                                            : people.team_score_rank == 3
                                            ? 'border: 3rpx solid #23CB36;'
                                            : ''
                                    "
                                    v-for="(people, index1) in item"
                                    :key="index1"
                                >
                                    <view class="top3_list_item_text">
                                        {{ tabIndex == 0 ? people.score + '积分' : tabIndex == 1 ? people.tools + '点' : people.diff > 0 ? '差' + people.diff + '点' : '' }}
                                    </view>

                                    <view @tap="gotoDressRoom" class="top3_list_item_nickname" :data-userid="people.userinfo._id">{{ people.userinfo.nickname }}</view>
                                </view>
                            </view>
                        </view>
                        <view class="top3_jifen_list">
                            <view
                                class="top3_jifen_item"
                                :style="index == 1 ? 'border: 4rpx solid #FF3C77;' : index == 2 ? 'border: 4rpx solid #23CB36;' : ''"
                                v-for="(item, index) in allBalloonteamTree"
                                :key="index"
                            >
                                <text>{{ item.team_name }} {{ item.team_fruit }}积分</text>
                            </view>
                        </view>
                        <view
                            style="
                                width: 412rpx;
                                background-color: #ffe7fd;
                                color: #333333;
                                font-size: 10px;
                                border-radius: 9rpx;
                                margin-top: 20rpx;
                                text-align: center;
                                line-height: 32rpx;
                            "
                        >
                            根据各魅力类型排行榜实时更新数据
                        </view>
                    </block>
                    <!-- <block wx:if="{{pageDataObj.exceed_msg_list&&pageDataObj.exceed_msg_list.length>0}}"> -->
                    <block v-if="false">
                        <image
                            src="https://star-img.xingxiaoculture.com/2023/04/06/3ac785cb7fda196bfa3cfaaa091e1853.png"
                            style="width: 240rpx; height: 42rpx; margin-top: 40rpx"
                        ></image>
                        <image
                            src="https://star-img.xingxiaoculture.com/2023/04/14/d886f56acd2687f39d88cc1696b3befb.png"
                            style="width: 683rpx; height: 68rpx; margin-top: 20rpx"
                        ></image>
                        <scroll-view class="content_roll_view" :scrollIntoView="messageBottom" :scrollY="true">
                            <view class="content_roll_line" v-for="(item, index) in pageDataObj.exceed_msg_list" :key="index">
                                <image class="content_roll_image" src="https://star-img.xingxiaoculture.com/2022/06/18/70f73a84b717a52c93b31c0cc7f17f71.png"></image>

                                <view class="content_roll_text">
                                    <text
                                        :style="citem.highlight == 1 ? 'color:#ffffff;' : citem.highlight == 2 ? 'color:#4D4DCA;' : ''"
                                        v-for="(citem, index1) in item.content"
                                        :key="index1"
                                    >
                                        {{ citem.text + ' ' }}
                                    </text>
                                </view>

                                <view class="content_roll_time">{{ item.time }}</view>
                            </view>
                            <view class="content_roll_line" id="messageBottom"></view>
                        </scroll-view>
                        <view @tap="handleTeamMore" class="message_box_bottom" data-level_tab="1" data-type="2">查看动态 ></view>
                    </block>

                    <image
                        src="https://star-img.xingxiaoculture.com/2023/05/17/01108d20434acb1402162756af2aa373.png"
                        style="width: 316rpx; height: 35rpx; margin-top: 40rpx"
                    ></image>
                    <view class="tree_item" v-if="arenaInfo.status == 'is_end' && arenaInfo.next_term_id && (!balloonTermId || balloonTermId === arenaInfo.term_list[0].term_id)">
                        <view class="tree_team" style="text-align: center; height: 82rpx">
                            <text class="tree_team_title" style="width: 100%">当前场次已结束 您可以提前选择创建或者加入一个下一场次的队伍</text>
                        </view>
                        <view class="tree_info_column" style="width: 100%">
                            <text class="tree_info_text4">组成队伍后系统会赠送{{ tool_s_name }}*200{{ tool_s_unit }}用于水晶海之战</text>
                            <view class="tree_info_button_box">
                                <view @tap="showBalloonCreateTeamPanel" class="tree_info_button2" data-action="create_team" data-type="1">创建队伍</view>
                                <view @tap="handleTeamJoinlist" class="tree_info_button2">加入其他队伍</view>
                            </view>
                        </view>
                    </view>
                    <block v-if="myBalloonTeam && myBalloonTeam.length > 0">
                        <!-- parse <template is="tree_info" :data="item:item,index:index,showTuan:true,type:'inteam',tool_b_unit:tool_b_unit,arenaInfo:arenaInfo,actArenaCountdownTimeStr:actArenaCountdownTimeStr,loactionDescArr:loactionDescArr,balloonTermId:balloonTermId" v-for="(item,index) in (myBalloonTeam)" :key="index"></template> -->
                        <block name="tree_info" v-for="(item, index) in myBalloonTeam" :key="index">
                            <view class="tree_item">
                                <view class="btn-group" style="margin-top: 0rpx; border-radius: 5rpx 5rpx 0 0">
                                    <view
                                        @tap="changeTabTeam"
                                        :class="item.tab == 0 ? 'type-btn-on' : 'type-btn'"
                                        :data-index="index"
                                        data-tab="0"
                                        :data-team_type="'inteam'"
                                        :style="item.tab == 0 ? 'border-radius: 5rpx 5rpx 0 0 ;' : ''"
                                    >
                                        队伍信息
                                    </view>
                                    <view
                                        @tap="changeTabTeam"
                                        :class="item.tab == 1 ? 'type-btn-on' : 'type-btn'"
                                        :data-index="index"
                                        data-tab="1"
                                        :data-team_type="'inteam'"
                                        :style="item.tab == 1 ? 'border-radius: 5rpx 5rpx 0 0 ;' : ''"
                                    >
                                        助力信息
                                    </view>
                                    <!-- <view bindtap="changeTabTeam" class="{{item.tab==2?'type-btn-on':'type-btn'}}" data-index="{{index}}" data-tab="2"  data-team_type="{{type}}" style="{{item.tab==1?'border-radius: 5rpx 5rpx 0 0 ;':''}}">
            解锁拼图
        </view> -->
                                </view>
                                <block v-if="item.tab == 0">
                                    <view class="tree_team">
                                        <view class="tree_team_title">
                                            {{ item.my_team.team_name }}
                                            <image
                                                @tap="showShortnameSet"
                                                :data-item="item.my_team"
                                                src="https://star-img.xingxiaoculture.com/2023/03/10/a695c2e50d04260e6696e43be60a82a8.png"
                                                style="width: 24rpx; height: 24rpx"
                                                v-if="item.my_team.is_captain"
                                            ></image>
                                        </view>
                                        <view class="flex_row" style="position: absolute; right: 20rpx">
                                            <view @tap="showChangeCap" class="tree_team_applay" :data-item="item.my_team" v-if="item.is_captain && item.my_team.members.length > 1">
                                                转让队长
                                            </view>
                                            <block
                                                v-if="(arenaInfo.status == 'not_begin' && arenaInfo.term_id == item.my_team.term_id) || arenaInfo.term_id < item.my_team.term_id"
                                            >
                                                <view @tap="showDismissTeam" class="tree_team_applay" :data-team_id="item.my_team.team_id" v-if="item.is_captain">解散队伍</view>
                                                <view @tap="showQuitTeam" class="tree_team_applay" :data-team_id="item.my_team.team_id" v-else-if="!item.is_captain">退出队伍</view>
                                            </block>
                                            <view @tap="handleTeamApply" class="tree_team_applay" :data-item="item.my_team" data-type="1" v-if="item.is_captain">
                                                申请列表
                                                <view class="content_set_cloud_log_point" v-if="item.is_captain && item.not_agree_num > 0">{{ item.not_agree_num }}</view>
                                            </view>
                                        </view>
                                    </view>
                                    <text style="color: #666666; font-size: 10px; margin-left: 20rpx; margin-top: 10rpx" v-if="item.auto_gen_team">
                                        本队伍是系统将上个月星球最强的五名粉丝自动创建的队伍
                                    </text>

                                    <view class="tree_info">
                                        <!-- <view class="tree_info_column">
                    <image class="tree_info_image" src="{{item.my_team.star_tree_inner}}"></image>
                </view>
                <view class="tree_info_column">
                    <view class="tree_info_item" style="margin-top: 15rpx;">
                        <view class="candy_img2">
                            <image mode="aspectFit" src="https://star-img.xingxiaoculture.com/2023/01/04/771d7841a205aef5ed408e5f9b6f9090.png" style="width: 70rpx;height:70rpx;margin-left: 6rpx;"></image>
                        </view>
                        <view class="tree_info_text3" style="line-height: 76rpx;" wx:if="{{(!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status=='not_begin'||balloonTermId>arenaInfo.term_id}}">
                            <text>{{(!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status=='not_begin'?actArenaCountdownTimeStr:'未开始'}}</text>
                        </view>
                        <view class="tree_info_text3" style="line-height: 76rpx;" wx:elif="{{(!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status=='is_end'||balloonTermId < arenaInfo.term_id}}">
                            <text>当前场次已结束</text>
                        </view>
                        <view class="tree_info_text3" wx:elif="{{arenaInfo.status=='first_half'}}">
                            <text>当前时段15:'00'-18:'00'\n上半场进行中</text>
                        </view>
                        <view class="tree_info_text3" wx:elif="{{arenaInfo.status=='half_time'}}">
                            <text>当前时段18:'00'-19:'00'\n中场休息中</text>
                        </view>
                        <view class="tree_info_text3" wx:elif="{{arenaInfo.status=='second_half'}}">
                            <text>当前时段19:'00'-21:'00'\n下半场进行中</text>
                        </view>
                    </view>
                    <view class="flex_row">
                        <button open-type="share" class="tree_info_button" data-item="{{item.my_team}}" data-team_id="{{item.my_team.team_id}}" data-action="zhuli" style="background-image:url(https://star-img.xingxiaoculture.com/2023/04/10/dc3c6fae213b225bd31ae15a797dcb16.png)">
                        </button>
                    </view>
                    <view class="flex_row">
                        
                        <view bindtap="showVotePanel" class="tree_info_button" data-item="{{item.my_team}}" data-team_id="{{item.my_team.team_id}}" style="{{item.my_team.in_team?'background-image:url(https://star-img.xingxiaoculture.com/2023/04/10/6ca99e34fcef891277f4db5d346c1483.png)':'background-image:url(https://star-img.xingxiaoculture.com/2023/04/10/fc74dfea7b17dc58415c400e4131778d.png)'}}">
                        </view>
                    </view>
                </view> -->

                                        <view class="tree_info_column" style="width: 30%">
                                            <image class="tree_info_image" :src="item.my_team.star_tree_inner"></image>
                                        </view>
                                        <view class="tree_info_column" style="width: 70%">
                                            <view
                                                class="tree_info_text3"
                                                v-if="
                                                    ((!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status == 'not_begin') || balloonTermId > arenaInfo.term_id
                                                "
                                            >
                                                <text>
                                                    {{
                                                        (!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status == 'not_begin'
                                                            ? actArenaCountdownTimeStr
                                                            : '未开始'
                                                    }}
                                                </text>
                                            </view>
                                            <view
                                                class="tree_info_text3"
                                                v-else-if="
                                                    ((!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status == 'is_end') || balloonTermId < arenaInfo.term_id
                                                "
                                            >
                                                <text>当前场次已结束</text>
                                            </view>
                                            <view class="tree_info_text3" v-else-if="arenaInfo.status == 'first_half'">
                                                <text>上半场进行中(15:'00'-18:'00')</text>
                                            </view>
                                            <view class="tree_info_text3" v-else-if="arenaInfo.status == 'half_time'">
                                                <text>中场休息中(18:'00'-19:'00')</text>
                                            </view>
                                            <view class="tree_info_text3" v-else-if="arenaInfo.status == 'second_half'">
                                                <text>下半场进行中(19:'00'-21:'00')</text>
                                            </view>
                                            <view class="flex_row flex_center" style="margin-top: 20rpx">
                                                <image
                                                    style="width: 63rpx; height: 63rpx"
                                                    v-if="item.my_team.rank == 1"
                                                    src="https://star-img.xingxiaoculture.com/2023/05/17/5db7e3d7885370616a5180409c6f5598.png"
                                                ></image>
                                                <image
                                                    style="width: 63rpx; height: 63rpx"
                                                    v-else-if="item.my_team.rank == 2"
                                                    src="https://star-img.xingxiaoculture.com/2023/05/18/f8ec4f4a011af5f3b0dde4aa546547c3.png"
                                                ></image>
                                                <image
                                                    style="width: 63rpx; height: 63rpx"
                                                    v-else-if="item.my_team.rank == 3"
                                                    src="https://star-img.xingxiaoculture.com/2023/05/18/e641e98b76e432e25c24dd1de7cf4bb9.png"
                                                ></image>
                                                <view
                                                    style="
                                                        width: 63rpx;
                                                        height: 63rpx;
                                                        background: url(https://star-img.xingxiaoculture.com/2023/05/18/7f069ba185f9e95efce1d8dd4f8ac9ce.png);
                                                        background-size: 100% 100%;
                                                        line-height: 66rpx;
                                                        text-align: center;
                                                        font-size: 10px;
                                                        color: #a56a59;
                                                        width: 63rpx;
                                                    "
                                                    v-else
                                                >
                                                    {{ item.my_team.rank == '未上榜' ? '' : item.my_team.rank }}
                                                </view>

                                                <view style="font-size: 13px; color: #333333; margin-left: 10rpx">
                                                    <text>排名\n</text>
                                                    <text style="color: #ff7b1e; font-size: 10px">
                                                        {{ item.my_team.rank == '未上榜' ? '--' : 'NO.' + item.my_team.rank + ' >' }}
                                                    </text>
                                                </view>
                                                <image
                                                    style="width: 63rpx; height: 63rpx; margin-left: 20rpx"
                                                    src="https://star-img.xingxiaoculture.com/2023/05/17/d5158ae3bfce5a45f7b69d2dc0a66365.png"
                                                ></image>
                                                <view style="font-size: 13px; color: #333333; margin-left: 10rpx">
                                                    <text>已行驶距离\n</text>
                                                    <text style="color: #ff7b1e; font-size: 10px">{{ item.my_team.team_fruit ? item.my_team.team_fruit + '米' : '0米' }}</text>
                                                </view>
                                            </view>
                                        </view>
                                    </view>

                                    <view class="tree_info3">
                                        <view class="flex_row" style="align-items: center; height: 45%">
                                            <image
                                                style="width: 130rpx; height: 66rpx; background-color: #e8f2f8; margin-left: 20rpx"
                                                src="https://star-img.xingxiaoculture.com/2023/05/17/79b2c76727f9c57b7cc0840bc7e8a4b5.png"
                                                mode="aspectFit"
                                            ></image>
                                            <text style="font-size: 13px; margin-left: 20rpx">
                                                <text style="font-weight: 600">
                                                    当前行驶速度：
                                                    <text style="color: #ff881a">{{ item.my_team.race_info.speed ? item.my_team.race_info.speed : 0 }}</text>
                                                    米/分钟
                                                </text>
                                                \n < text style="font-size: 10px;" wx:if="{{
                                                    item.my_team.race_info.energy_consume_per_minute &&
                                                    item.my_team.race_info.energy_consume_per_minute > item.my_team.race_info.energy
                                                }}">因能源耗尽，当前行驶速度已经降到初始速度
                                            </text>
                                            <text style="font-size: 10px" v-else-if="item.my_team.race_info.distance">
                                                若保持，结束时共可行驶
                                                <text style="color: #ff881a">{{ item.my_team.race_info.distance }}</text>
                                                米
                                            </text>
                                        </view>
                                        <view class="flex_row" style="align-items: center; height: 55%; position: relative">
                                            <image
                                                style="width: 130rpx; height: 66rpx; background-color: #e8f2f8; margin-left: 20rpx"
                                                src="https://star-img.xingxiaoculture.com/2023/05/17/e7de941ab56fe9bf0de516634d43163e.png"
                                                mode="aspectFit"
                                            ></image>

                                            <block
                                                v-if="
                                                    item.my_team.race_info.energy_consume_per_minute &&
                                                    item.my_team.race_info.energy_consume_per_minute > item.my_team.race_info.energy
                                                "
                                            >
                                                <text style="font-size: 13px; margin-left: 20rpx; font-weight: 600">
                                                    <text style="color: #ff881a">星能源已耗尽！</text>
                                                    请尽快补充！
                                                </text>
                                            </block>
                                            <block v-else>
                                                <text style="font-size: 13px; margin-left: 20rpx">
                                                    <text style="font-weight: 600">
                                                        剩余星能源：
                                                        <text style="color: #ff881a">{{ item.my_team.race_info.energy ? item.my_team.race_info.energy : 0 }}</text>
                                                        点
                                                    </text>
                                                    \n < text style="font-size: 10px;" wx:if="{{ item.my_team.race_info.energy_consume_per_minute }}">当前速度消耗能源：
                                                    <text style="color: #ff881a">{{ item.my_team.race_info.energy_consume_per_minute }}</text>
                                                    点/分钟\n
                                                </text>
                                                <text style="font-size: 10px" v-if="item.my_team.race_info.energy_left_minute && item.my_team.race_info.energy_consume_per_minute">
                                                    预计还可消耗：
                                                    <text style="color: #ff881a">{{ item.my_team.race_info.energy_left_minute }}</text>
                                                    分钟
                                                </text>
                                            </block>

                                            <view
                                                v-if="
                                                    item.my_team.race_info.energy_consume_per_minute &&
                                                    item.my_team.race_info.energy_consume_per_minute > item.my_team.race_info.energy
                                                "
                                                style="
                                                    color: #ffffff;
                                                    background-color: #ff2020;
                                                    border-radius: 50%;
                                                    text-align: center;
                                                    width: 28rpx;
                                                    height: 28rpx;
                                                    line-height: 28rpx;
                                                    position: absolute;
                                                    left: 130rpx;
                                                    top: 15rpx;
                                                    font-size: 12px;
                                                "
                                            >
                                                !
                                            </view>
                                        </view>
                                    </view>

                                    <view class="flex_row flex_center">
                                        <!-- <text class="team_info_line_text">队伍排名：{{item.my_team.team_fruit_rank!='未上榜'?'No. ':''}}{{item.my_team.team_fruit_rank}}</text>
                <text class="team_info_line_text">队伍总积分：{{item.my_team.team_fruit}}分</text>
                <view bindtap="handleTeamMore" class="team_info_line_right" data-type="2">查看排名></view> -->
                                        <button
                                            open-type="share"
                                            :data-item="item.my_team"
                                            :data-team_id="item.my_team.team_id"
                                            data-action="zhuli"
                                            style="
                                                background-image: url(https://star-img.xingxiaoculture.com/2023/05/17/16f406fe8fed5b650d2f9035a0f37f99.png);
                                                width: 304rpx;
                                                height: 74rpx;
                                                margin: 20rpx;
                                                background-size: 100% 100%;
                                            "
                                        ></button>
                                        <image
                                            v-if="!item.my_team.in_team"
                                            @tap="gotoTeamBattle"
                                            :data-item="item.my_team"
                                            :data-team_id="item.my_team.team_id"
                                            src="https://star-img.xingxiaoculture.com/2023/05/17/a2f902f496e29cdde58ca8318f5c1b3f.png"
                                            style="width: 304rpx; height: 74rpx; margin: 20rpx"
                                        ></image>
                                        <image
                                            v-else
                                            @tap="gotoTeamBattle"
                                            :data-item="item.my_team"
                                            :data-team_id="item.my_team.team_id"
                                            src="https://star-img.xingxiaoculture.com/2023/05/17/032c57b72f0dbfcde9bd1d235e511b55.png"
                                            style="width: 304rpx; height: 74rpx; margin: 20rpx"
                                        ></image>
                                    </view>

                                    <view class="team_info_line" style="height: 30rpx; background: none">
                                        <text class="team_info_line_text3" style="width: 200rpx; margin-left: 30rpx">队伍角色</text>
                                        <text class="team_info_line_text3" style="width: 210rpx">当前场上成员</text>
                                        <text class="team_info_line_text3" style="width: 360rpx">说明</text>
                                    </view>
                                    <view class="team_info_line" v-for="(member, index) in item.my_team.members_pos" :key="index">
                                        <view class="flex_column">
                                            <image class="team_info_line_area_image" :src="member.img"></image>
                                            <text class="team_info_line_text" style="width: 150rpx; overflow: hidden; white-space: nowrap">
                                                {{ member.userinfo ? member.userinfo.nickname : '空缺中' }}
                                            </text>
                                        </view>

                                        <view class="team_info_line_user_image" style="margin-left: 20rpx">
                                            <image
                                                @tap="gotoDressRoom"
                                                class="team_info_line_user_image"
                                                :data-userid="member.userinfo._id"
                                                :src="
                                                    member.userinfo
                                                        ? member.userinfo.image.thumbnail_pic
                                                        : 'https://star-img.xingxiaoculture.com/2023/05/17/5816227c5defe1cf9ed6627933bb6c35.png'
                                                "
                                            ></image>
                                            <view class="team_info_line_user_image_cap" v-if="member.is_captain">队长</view>
                                            <button class="tree_team_join_button" :data-pos="index" :data-team_id="item.my_team.team_id" openType="share" v-if="!member.userinfo">
                                                +
                                            </button>
                                        </view>

                                        <view class="flex_row" style="align-items: center; width: 300rpx; margin-left: 120rpx">
                                            <!-- <view class="flex_column" style="align-items: flex-end;">
                        <text class="team_info_line_text2">{{member.score?member.score:0}}积分</text>
                        <block wx:if="{{member.diff}}">
                            <text class="team_info_line_text2" wx:if="{{member.diff>0}}">领先第二名<text style="color:#3a3833 ;">{{member.diff}}</text>点</text>
                            <text class="team_info_line_text2" wx:else>差上一名<text style="color:#3a3833 ;">{{-member.diff}}</text>{{tool_b_unit}}</text>
                        </block>
                    </view> -->
                                            <view class="flex_column" style="align-items: flex-start">
                                                <view class="team_info_line_text2">
                                                    <text :style="titem.hl ? 'color:#FFFFFF' : ''" v-for="(titem, tindex) in loactionDescArr[index]" :key="tindex">
                                                        {{ titem.text }}
                                                    </text>
                                                </view>
                                            </view>
                                            <!-- <view wx:if="{{!item.my_team.in_team}}" style="{{!member.userinfo?'visibility:hidden':''}}" class="team_info_zhuli_button" bindtap="showVotePanel" data-item="{{item.my_team}}" data-team_id="{{item.my_team.team_id}}" data-pos="{{index}}">
                    助力
                    </view> -->
                                        </view>

                                        <!-- <view bindtap="handleTeamMore" class="team_info_line_right" data-loc="{{index}}" data-type="2">
                    <image src="https://star-img.xingxiaoculture.com/2023/03/10/a44cdb933170081060d381e4ebe37f64.png" style="width: 12rpx;height: 20rpx;"></image>
                </view> -->
                                    </view>
                                    <view class="tree_info_button_box" v-if="item.my_team.in_team">
                                        <view
                                            @tap="showChangeLocation"
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/554b28b836b48aac9bbdc1b70ac80863.png);
                                                background-size: 100% 100%;
                                                line-height: 30rpx;
                                            "
                                        >
                                            <text>
                                                位置调换
                                                <text style="font-size: 10px">（剩余{{ item.my_team.left_times }}次）</text>
                                            </text>
                                        </view>

                                        <view
                                            v-if="
                                                (balloonTermId && balloonTermId != arenaInfo.term_id) ||
                                                ((!balloonTermId || balloonTermId == arenaInfo.term_id) && (arenaInfo.status == 'not_begin' || arenaInfo.status == 'is_end'))
                                            "
                                            class="tree_info_button3"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/10aeefb9ad989448863a6df23f24167a.png);
                                                background-size: 100% 100%;
                                                line-height: 50rpx;
                                                width: 242rpx;
                                            "
                                        >
                                            <text>更换成员上场</text>
                                        </view>
                                        <view
                                            v-else-if="arenaInfo.status == 'second_half'"
                                            @tap="showSwitchMember"
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/e394d545704b0e5009c5d515e757f025.png);
                                                background-size: 100% 100%;
                                                line-height: 30rpx;
                                                width: 242rpx;
                                            "
                                        >
                                            <text>
                                                更换成员上场
                                                <text style="font-size: 10px">（剩余{{ item.my_team.replace_member_left_times }}次）</text>
                                            </text>
                                        </view>
                                        <view
                                            v-else
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/10aeefb9ad989448863a6df23f24167a.png);
                                                background-size: 100% 100%;
                                                line-height: 30rpx;
                                                width: 242rpx;
                                            "
                                        >
                                            <text>
                                                更换成员上场
                                                <text style="font-size: 10px">下半场可换</text>
                                            </text>
                                        </view>
                                        <view
                                            @tap="showTeamInfo"
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/ed3fac5acbeeeb401f683cbd909a3f91.png);
                                                background-size: 100% 100%;
                                            "
                                        >
                                            查看队伍详情
                                        </view>
                                    </view>
                                    <view class="tree_info_button_box" v-else>
                                        <view
                                            @tap="showTeamInfo"
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            data-tab="1"
                                            style="
                                                background: linear-gradient(180deg, #4fcaff, #217ef9);
                                                line-height: 67rpx;
                                                width: 635rpx;
                                                border-radius: 34rpx;
                                                border: 5rpx double #9dddff;
                                            "
                                        >
                                            <text>查看我的助力信息</text>
                                        </view>
                                    </view>
                                </block>
                                <block v-else-if="item.tab == 1">
                                    <view
                                        style="width: 100%; display: flex; flex-direction: column; align-items: center"
                                        v-if="item.help_info && item.help_info.msg_list.length > 0"
                                    >
                                        <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                                            <view class="group_pk_prizes_list_line"></view>
                                            <view class="group_pk_prizes_list_text2">助力动态</view>
                                        </view>
                                        <view class="team_info_line" style="height: 30rpx; background: none">
                                            <!-- <text class="team_info_line_text3" style="width: 220rpx;margin-left: 20rpx;">魅力类型</text> -->
                                            <text class="team_info_line_text3" style="width: 350rpx; margin-left: 20rpx">助力成员</text>
                                            <text class="team_info_line_text3" style="width: 350rpx">助力体力点</text>
                                            <text class="team_info_line_text3" style="width: 170rpx">助力时间</text>
                                        </view>
                                        <scroll-view class="zhuli_dongtai_box" :scroll-y="true">
                                            <view class="zhuli_dongtai_box_item" v-for="(m_item, index) in item.help_info.msg_list" :key="index">
                                                <!-- <image style="width: 116rpx;height: 28rpx;margin-left: 20rpx;" src="{{m_item.pos_img}}"></image> -->

                                                <image class="zhuli_dongtai_box_item_icon" style="margin-left: 20rpx" :src="m_item.userinfo.image.middle_pic"></image>

                                                <view class="zhuli_dongtai_box_item_text" style="width: 200rpx">{{ m_item.userinfo.nickname }}</view>

                                                <view class="zhuli_dongtai_box_item_text" style="width: 260rpx">
                                                    助力
                                                    <text style="color: #ff6800">{{ m_item.votetimes }}</text>
                                                </view>

                                                <view class="zhuli_dongtai_box_item_text">{{ m_item.time }}</view>
                                            </view>
                                        </scroll-view>
                                        <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                                            <view class="group_pk_prizes_list_line"></view>
                                            <view class="group_pk_prizes_list_text2">助力用户</view>
                                        </view>
                                        <view class="team_info_line" style="height: 30rpx; background: none">
                                            <text class="team_info_line_text3" style="width: 350rpx; margin-left: 30rpx">用户信息</text>
                                            <text class="team_info_line_text3" style="width: 400rpx">共助力数量</text>
                                            <text class="team_info_line_text3" style="width: 180rpx">全队占比</text>
                                        </view>
                                        <view class="zhuli_dongtai_box" style="margin-bottom: 30rpx; height: 332rpx">
                                            <view class="zhuli_dongtai_box_item" v-for="(m_item, index) in item.help_info.user_list" :key="index">
                                                <image class="zhuli_dongtai_box_item_icon" style="margin-left: 30rpx" :src="m_item.userinfo.image.middle_pic"></image>

                                                <view class="zhuli_dongtai_box_item_text" style="width: 180rpx">{{ m_item.userinfo.nickname }}</view>

                                                <view class="zhuli_dongtai_box_item_text" style="width: 260rpx">
                                                    已助力
                                                    <text style="color: #ff6800">{{ m_item.votetimes }}</text>
                                                </view>

                                                <view class="zhuli_dongtai_box_item_text" style="color: #ff6800">{{ m_item.percent }}</view>
                                            </view>
                                            <view class="zhuli_dongtai_box_more_info" @tap="showTeamInfo" :data-item="item.my_team">查看队伍信息</view>
                                        </view>
                                    </view>
                                    <view class="zhuli_none_box" v-else>
                                        <image
                                            src="https://star-img.xingxiaoculture.com/2023/05/17/6f1197e16c976fa2a6fa3476686c5983.png"
                                            style="width: 265rpx; height: 196rpx"
                                        ></image>
                                        <view class="zhuli_box_none_text">抱歉，当前暂无助力信息~</view>
                                    </view>
                                </block>
                                <block v-else-if="item.tab == 2">
                                    <view style="width: 100%; display: flex; flex-direction: column; align-items: center">
                                        <view class="team_info_line" style="height: 52rpx; margin-top: 20rpx">
                                            <image class="team_info_line_area_image" :src="item.my_team.members_pos[item.puzzle_pos ? item.puzzle_pos : 0].img"></image>
                                            <view @tap="showBalloonLocationPanel" class="team_info_line_right" :data-item="item.my_team" :data-team_type="'inteam'">
                                                切换魅力类型>
                                            </view>
                                        </view>
                                        <!-- <text class="tree_info_text4" style="width: 600rpx;text-align: center;margin-top: 10rpx;">全部解锁后，在当前魅力类型贡献过的成员（包括助力成员） 都可获得<text style="color: #FF6800;">奖励N个</text></text> -->
                                        <view style="width: 660rpx; height: 990rpx; position: relative; margin-top: 10rpx; margin-bottom: 20rpx">
                                            <image style="width: 660rpx; height: 990rpx" :src="item.puzzle.img"></image>
                                            <view
                                                style="
                                                    width: 660rpx;
                                                    height: 990rpx;
                                                    position: absolute;
                                                    display: flex;
                                                    flex-wrap: wrap;
                                                    top: 0;
                                                    left: 0;
                                                    align-items: center;
                                                    justify-content: center;
                                                "
                                            >
                                                <view
                                                    class="puzzle_block"
                                                    :style="p_item.userid != '' ? 'opacity: 0;' : ''"
                                                    :data-item="item"
                                                    :data-team_type="'inteam'"
                                                    :data-piece="p_index"
                                                    :data-index="index"
                                                    data-tab="2"
                                                    @tap="parseEventDynamicCode($event, p_item.userid != '' ? '' : 'unlockJigsawClick')"
                                                    v-for="(p_item, p_index) in item.puzzle.list"
                                                    :key="p_index"
                                                ></view>
                                            </view>
                                        </view>
                                    </view>
                                </block>
                            </view>
                        </block>
                    </block>
                    <block v-if="myBalloonTeamHelp && myBalloonTeamHelp.length > 0">
                        <!-- parse <template is="tree_info" :data="item:item,index:index,showTuan:true,type:'help',tool_b_unit:tool_b_unit,arenaInfo:arenaInfo,actArenaCountdownTimeStr:actArenaCountdownTimeStr,loactionDescArr:loactionDescArr,balloonTermId:balloonTermId" v-for="(item,index) in (myBalloonTeamHelp)" :key="index"></template> -->
                        <block name="tree_info" v-if="false" v-for="(item, index) in myBalloonTeamHelp" :key="index">
                            <view class="tree_item">
                                <view class="btn-group" style="margin-top: 0rpx; border-radius: 5rpx 5rpx 0 0">
                                    <view
                                        @tap="changeTabTeam"
                                        :class="item.tab == 0 ? 'type-btn-on' : 'type-btn'"
                                        :data-index="index"
                                        data-tab="0"
                                        :data-team_type="'inteam'"
                                        :style="item.tab == 0 ? 'border-radius: 5rpx 5rpx 0 0 ;' : ''"
                                    >
                                        队伍信息
                                    </view>
                                    <view
                                        @tap="changeTabTeam"
                                        :class="item.tab == 1 ? 'type-btn-on' : 'type-btn'"
                                        :data-index="index"
                                        data-tab="1"
                                        :data-team_type="'inteam'"
                                        :style="item.tab == 1 ? 'border-radius: 5rpx 5rpx 0 0 ;' : ''"
                                    >
                                        助力信息
                                    </view>
                                    <!-- <view bindtap="changeTabTeam" class="{{item.tab==2?'type-btn-on':'type-btn'}}" data-index="{{index}}" data-tab="2"  data-team_type="{{type}}" style="{{item.tab==1?'border-radius: 5rpx 5rpx 0 0 ;':''}}">
            解锁拼图
        </view> -->
                                </view>
                                <block v-if="item.tab == 0">
                                    <view class="tree_team">
                                        <view class="tree_team_title">
                                            {{ item.my_team.team_name }}
                                            <image
                                                @tap="showShortnameSet"
                                                :data-item="item.my_team"
                                                src="https://star-img.xingxiaoculture.com/2023/03/10/a695c2e50d04260e6696e43be60a82a8.png"
                                                style="width: 24rpx; height: 24rpx"
                                                v-if="item.my_team.is_captain"
                                            ></image>
                                        </view>
                                        <view class="flex_row" style="position: absolute; right: 20rpx">
                                            <view @tap="showChangeCap" class="tree_team_applay" :data-item="item.my_team" v-if="item.is_captain && item.my_team.members.length > 1">
                                                转让队长
                                            </view>
                                            <block
                                                v-if="(arenaInfo.status == 'not_begin' && arenaInfo.term_id == item.my_team.term_id) || arenaInfo.term_id < item.my_team.term_id"
                                            >
                                                <view @tap="showDismissTeam" class="tree_team_applay" :data-team_id="item.my_team.team_id" v-if="item.is_captain">解散队伍</view>
                                                <view @tap="showQuitTeam" class="tree_team_applay" :data-team_id="item.my_team.team_id" v-else-if="!item.is_captain">退出队伍</view>
                                            </block>
                                            <view @tap="handleTeamApply" class="tree_team_applay" :data-item="item.my_team" data-type="1" v-if="item.is_captain">
                                                申请列表
                                                <view class="content_set_cloud_log_point" v-if="item.is_captain && item.not_agree_num > 0">{{ item.not_agree_num }}</view>
                                            </view>
                                        </view>
                                    </view>
                                    <text style="color: #666666; font-size: 10px; margin-left: 20rpx; margin-top: 10rpx" v-if="item.auto_gen_team">
                                        本队伍是系统将上个月星球最强的五名粉丝自动创建的队伍
                                    </text>

                                    <view class="tree_info">
                                        <!-- <view class="tree_info_column">
                    <image class="tree_info_image" src="{{item.my_team.star_tree_inner}}"></image>
                </view>
                <view class="tree_info_column">
                    <view class="tree_info_item" style="margin-top: 15rpx;">
                        <view class="candy_img2">
                            <image mode="aspectFit" src="https://star-img.xingxiaoculture.com/2023/01/04/771d7841a205aef5ed408e5f9b6f9090.png" style="width: 70rpx;height:70rpx;margin-left: 6rpx;"></image>
                        </view>
                        <view class="tree_info_text3" style="line-height: 76rpx;" wx:if="{{(!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status=='not_begin'||balloonTermId>arenaInfo.term_id}}">
                            <text>{{(!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status=='not_begin'?actArenaCountdownTimeStr:'未开始'}}</text>
                        </view>
                        <view class="tree_info_text3" style="line-height: 76rpx;" wx:elif="{{(!balloonTermId||balloonTermId==arenaInfo.term_id)&&arenaInfo.status=='is_end'||balloonTermId < arenaInfo.term_id}}">
                            <text>当前场次已结束</text>
                        </view>
                        <view class="tree_info_text3" wx:elif="{{arenaInfo.status=='first_half'}}">
                            <text>当前时段15:'00'-18:'00'\n上半场进行中</text>
                        </view>
                        <view class="tree_info_text3" wx:elif="{{arenaInfo.status=='half_time'}}">
                            <text>当前时段18:'00'-19:'00'\n中场休息中</text>
                        </view>
                        <view class="tree_info_text3" wx:elif="{{arenaInfo.status=='second_half'}}">
                            <text>当前时段19:'00'-21:'00'\n下半场进行中</text>
                        </view>
                    </view>
                    <view class="flex_row">
                        <button open-type="share" class="tree_info_button" data-item="{{item.my_team}}" data-team_id="{{item.my_team.team_id}}" data-action="zhuli" style="background-image:url(https://star-img.xingxiaoculture.com/2023/04/10/dc3c6fae213b225bd31ae15a797dcb16.png)">
                        </button>
                    </view>
                    <view class="flex_row">
                        
                        <view bindtap="showVotePanel" class="tree_info_button" data-item="{{item.my_team}}" data-team_id="{{item.my_team.team_id}}" style="{{item.my_team.in_team?'background-image:url(https://star-img.xingxiaoculture.com/2023/04/10/6ca99e34fcef891277f4db5d346c1483.png)':'background-image:url(https://star-img.xingxiaoculture.com/2023/04/10/fc74dfea7b17dc58415c400e4131778d.png)'}}">
                        </view>
                    </view>
                </view> -->

                                        <view class="tree_info_column" style="width: 30%">
                                            <image class="tree_info_image" :src="item.my_team.star_tree_inner"></image>
                                        </view>
                                        <view class="tree_info_column" style="width: 70%">
                                            <view
                                                class="tree_info_text3"
                                                v-if="
                                                    ((!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status == 'not_begin') || balloonTermId > arenaInfo.term_id
                                                "
                                            >
                                                <text>
                                                    {{
                                                        (!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status == 'not_begin'
                                                            ? actArenaCountdownTimeStr
                                                            : '未开始'
                                                    }}
                                                </text>
                                            </view>
                                            <view
                                                class="tree_info_text3"
                                                v-else-if="
                                                    ((!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status == 'is_end') || balloonTermId < arenaInfo.term_id
                                                "
                                            >
                                                <text>当前场次已结束</text>
                                            </view>
                                            <view class="tree_info_text3" v-else-if="arenaInfo.status == 'first_half'">
                                                <text>上半场进行中(15:'00'-18:'00')</text>
                                            </view>
                                            <view class="tree_info_text3" v-else-if="arenaInfo.status == 'half_time'">
                                                <text>中场休息中(18:'00'-19:'00')</text>
                                            </view>
                                            <view class="tree_info_text3" v-else-if="arenaInfo.status == 'second_half'">
                                                <text>下半场进行中(19:'00'-21:'00')</text>
                                            </view>
                                            <view class="flex_row flex_center" style="margin-top: 20rpx">
                                                <image
                                                    style="width: 63rpx; height: 63rpx"
                                                    v-if="item.my_team.rank == 1"
                                                    src="https://star-img.xingxiaoculture.com/2023/05/17/5db7e3d7885370616a5180409c6f5598.png"
                                                ></image>
                                                <image
                                                    style="width: 63rpx; height: 63rpx"
                                                    v-else-if="item.my_team.rank == 2"
                                                    src="https://star-img.xingxiaoculture.com/2023/05/18/f8ec4f4a011af5f3b0dde4aa546547c3.png"
                                                ></image>
                                                <image
                                                    style="width: 63rpx; height: 63rpx"
                                                    v-else-if="item.my_team.rank == 3"
                                                    src="https://star-img.xingxiaoculture.com/2023/05/18/e641e98b76e432e25c24dd1de7cf4bb9.png"
                                                ></image>
                                                <view
                                                    style="
                                                        width: 63rpx;
                                                        height: 63rpx;
                                                        background: url(https://star-img.xingxiaoculture.com/2023/05/18/7f069ba185f9e95efce1d8dd4f8ac9ce.png);
                                                        background-size: 100% 100%;
                                                        line-height: 66rpx;
                                                        text-align: center;
                                                        font-size: 10px;
                                                        color: #a56a59;
                                                        width: 63rpx;
                                                    "
                                                    v-else
                                                >
                                                    {{ item.my_team.rank == '未上榜' ? '' : item.my_team.rank }}
                                                </view>

                                                <view style="font-size: 13px; color: #333333; margin-left: 10rpx">
                                                    <text>排名\n</text>
                                                    <text style="color: #ff7b1e; font-size: 10px">
                                                        {{ item.my_team.rank == '未上榜' ? '--' : 'NO.' + item.my_team.rank + ' >' }}
                                                    </text>
                                                </view>
                                                <image
                                                    style="width: 63rpx; height: 63rpx; margin-left: 20rpx"
                                                    src="https://star-img.xingxiaoculture.com/2023/05/17/d5158ae3bfce5a45f7b69d2dc0a66365.png"
                                                ></image>
                                                <view style="font-size: 13px; color: #333333; margin-left: 10rpx">
                                                    <text>已行驶距离\n</text>
                                                    <text style="color: #ff7b1e; font-size: 10px">{{ item.my_team.team_fruit ? item.my_team.team_fruit + '米' : '0米' }}</text>
                                                </view>
                                            </view>
                                        </view>
                                    </view>

                                    <view class="tree_info3">
                                        <view class="flex_row" style="align-items: center; height: 45%">
                                            <image
                                                style="width: 130rpx; height: 66rpx; background-color: #e8f2f8; margin-left: 20rpx"
                                                src="https://star-img.xingxiaoculture.com/2023/05/17/79b2c76727f9c57b7cc0840bc7e8a4b5.png"
                                                mode="aspectFit"
                                            ></image>
                                            <text style="font-size: 13px; margin-left: 20rpx">
                                                <text style="font-weight: 600">
                                                    当前行驶速度：
                                                    <text style="color: #ff881a">{{ item.my_team.race_info.speed ? item.my_team.race_info.speed : 0 }}</text>
                                                    米/分钟
                                                </text>
                                                \n < text style="font-size: 10px;" wx:if="{{
                                                    item.my_team.race_info.energy_consume_per_minute &&
                                                    item.my_team.race_info.energy_consume_per_minute > item.my_team.race_info.energy
                                                }}">因能源耗尽，当前行驶速度已经降到初始速度
                                            </text>
                                            <text style="font-size: 10px" v-else-if="item.my_team.race_info.distance">
                                                若保持，结束时共可行驶
                                                <text style="color: #ff881a">{{ item.my_team.race_info.distance }}</text>
                                                米
                                            </text>
                                        </view>
                                        <view class="flex_row" style="align-items: center; height: 55%; position: relative">
                                            <image
                                                style="width: 130rpx; height: 66rpx; background-color: #e8f2f8; margin-left: 20rpx"
                                                src="https://star-img.xingxiaoculture.com/2023/05/17/e7de941ab56fe9bf0de516634d43163e.png"
                                                mode="aspectFit"
                                            ></image>

                                            <block
                                                v-if="
                                                    item.my_team.race_info.energy_consume_per_minute &&
                                                    item.my_team.race_info.energy_consume_per_minute > item.my_team.race_info.energy
                                                "
                                            >
                                                <text style="font-size: 13px; margin-left: 20rpx; font-weight: 600">
                                                    <text style="color: #ff881a">星能源已耗尽！</text>
                                                    请尽快补充！
                                                </text>
                                            </block>
                                            <block v-else>
                                                <text style="font-size: 13px; margin-left: 20rpx">
                                                    <text style="font-weight: 600">
                                                        剩余星能源：
                                                        <text style="color: #ff881a">{{ item.my_team.race_info.energy ? item.my_team.race_info.energy : 0 }}</text>
                                                        点
                                                    </text>
                                                    \n < text style="font-size: 10px;" wx:if="{{ item.my_team.race_info.energy_consume_per_minute }}">当前速度消耗能源：
                                                    <text style="color: #ff881a">{{ item.my_team.race_info.energy_consume_per_minute }}</text>
                                                    点/分钟\n
                                                </text>
                                                <text style="font-size: 10px" v-if="item.my_team.race_info.energy_left_minute && item.my_team.race_info.energy_consume_per_minute">
                                                    预计还可消耗：
                                                    <text style="color: #ff881a">{{ item.my_team.race_info.energy_left_minute }}</text>
                                                    分钟
                                                </text>
                                            </block>

                                            <view
                                                v-if="
                                                    item.my_team.race_info.energy_consume_per_minute &&
                                                    item.my_team.race_info.energy_consume_per_minute > item.my_team.race_info.energy
                                                "
                                                style="
                                                    color: #ffffff;
                                                    background-color: #ff2020;
                                                    border-radius: 50%;
                                                    text-align: center;
                                                    width: 28rpx;
                                                    height: 28rpx;
                                                    line-height: 28rpx;
                                                    position: absolute;
                                                    left: 130rpx;
                                                    top: 15rpx;
                                                    font-size: 12px;
                                                "
                                            >
                                                !
                                            </view>
                                        </view>
                                    </view>

                                    <view class="flex_row flex_center">
                                        <!-- <text class="team_info_line_text">队伍排名：{{item.my_team.team_fruit_rank!='未上榜'?'No. ':''}}{{item.my_team.team_fruit_rank}}</text>
                <text class="team_info_line_text">队伍总积分：{{item.my_team.team_fruit}}分</text>
                <view bindtap="handleTeamMore" class="team_info_line_right" data-type="2">查看排名></view> -->
                                        <button
                                            open-type="share"
                                            :data-item="item.my_team"
                                            :data-team_id="item.my_team.team_id"
                                            data-action="zhuli"
                                            style="
                                                background-image: url(https://star-img.xingxiaoculture.com/2023/05/17/16f406fe8fed5b650d2f9035a0f37f99.png);
                                                width: 304rpx;
                                                height: 74rpx;
                                                margin: 20rpx;
                                                background-size: 100% 100%;
                                            "
                                        ></button>
                                        <image
                                            v-if="!item.my_team.in_team"
                                            @tap="gotoTeamBattle"
                                            :data-item="item.my_team"
                                            :data-team_id="item.my_team.team_id"
                                            src="https://star-img.xingxiaoculture.com/2023/05/17/a2f902f496e29cdde58ca8318f5c1b3f.png"
                                            style="width: 304rpx; height: 74rpx; margin: 20rpx"
                                        ></image>
                                        <image
                                            v-else
                                            @tap="gotoTeamBattle"
                                            :data-item="item.my_team"
                                            :data-team_id="item.my_team.team_id"
                                            src="https://star-img.xingxiaoculture.com/2023/05/17/032c57b72f0dbfcde9bd1d235e511b55.png"
                                            style="width: 304rpx; height: 74rpx; margin: 20rpx"
                                        ></image>
                                    </view>

                                    <view class="team_info_line" style="height: 30rpx; background: none">
                                        <text class="team_info_line_text3" style="width: 200rpx; margin-left: 30rpx">队伍角色</text>
                                        <text class="team_info_line_text3" style="width: 210rpx">当前场上成员</text>
                                        <text class="team_info_line_text3" style="width: 360rpx">说明</text>
                                    </view>
                                    <view class="team_info_line" v-for="(member, index) in item.my_team.members_pos" :key="index">
                                        <view class="flex_column">
                                            <image class="team_info_line_area_image" :src="member.img"></image>
                                            <text class="team_info_line_text" style="width: 150rpx; overflow: hidden; white-space: nowrap">
                                                {{ member.userinfo ? member.userinfo.nickname : '空缺中' }}
                                            </text>
                                        </view>

                                        <view class="team_info_line_user_image" style="margin-left: 20rpx">
                                            <image
                                                @tap="gotoDressRoom"
                                                class="team_info_line_user_image"
                                                :data-userid="member.userinfo._id"
                                                :src="
                                                    member.userinfo
                                                        ? member.userinfo.image.thumbnail_pic
                                                        : 'https://star-img.xingxiaoculture.com/2023/05/17/5816227c5defe1cf9ed6627933bb6c35.png'
                                                "
                                            ></image>
                                            <view class="team_info_line_user_image_cap" v-if="member.is_captain">队长</view>
                                            <button class="tree_team_join_button" :data-pos="index" :data-team_id="item.my_team.team_id" openType="share" v-if="!member.userinfo">
                                                +
                                            </button>
                                        </view>

                                        <view class="flex_row" style="align-items: center; width: 300rpx; margin-left: 120rpx">
                                            <!-- <view class="flex_column" style="align-items: flex-end;">
                        <text class="team_info_line_text2">{{member.score?member.score:0}}积分</text>
                        <block wx:if="{{member.diff}}">
                            <text class="team_info_line_text2" wx:if="{{member.diff>0}}">领先第二名<text style="color:#3a3833 ;">{{member.diff}}</text>点</text>
                            <text class="team_info_line_text2" wx:else>差上一名<text style="color:#3a3833 ;">{{-member.diff}}</text>{{tool_b_unit}}</text>
                        </block>
                    </view> -->
                                            <view class="flex_column" style="align-items: flex-start">
                                                <view class="team_info_line_text2">
                                                    <text :style="titem.hl ? 'color:#FFFFFF' : ''" v-for="(titem, tindex) in loactionDescArr[index]" :key="tindex">
                                                        {{ titem.text }}
                                                    </text>
                                                </view>
                                            </view>
                                            <!-- <view wx:if="{{!item.my_team.in_team}}" style="{{!member.userinfo?'visibility:hidden':''}}" class="team_info_zhuli_button" bindtap="showVotePanel" data-item="{{item.my_team}}" data-team_id="{{item.my_team.team_id}}" data-pos="{{index}}">
                    助力
                    </view> -->
                                        </view>

                                        <!-- <view bindtap="handleTeamMore" class="team_info_line_right" data-loc="{{index}}" data-type="2">
                    <image src="https://star-img.xingxiaoculture.com/2023/03/10/a44cdb933170081060d381e4ebe37f64.png" style="width: 12rpx;height: 20rpx;"></image>
                </view> -->
                                    </view>
                                    <view class="tree_info_button_box" v-if="item.my_team.in_team">
                                        <view
                                            @tap="showChangeLocation"
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/554b28b836b48aac9bbdc1b70ac80863.png);
                                                background-size: 100% 100%;
                                                line-height: 30rpx;
                                            "
                                        >
                                            <text>
                                                位置调换
                                                <text style="font-size: 10px">（剩余{{ item.my_team.left_times }}次）</text>
                                            </text>
                                        </view>

                                        <view
                                            v-if="
                                                (balloonTermId && balloonTermId != arenaInfo.term_id) ||
                                                ((!balloonTermId || balloonTermId == arenaInfo.term_id) && (arenaInfo.status == 'not_begin' || arenaInfo.status == 'is_end'))
                                            "
                                            class="tree_info_button3"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/10aeefb9ad989448863a6df23f24167a.png);
                                                background-size: 100% 100%;
                                                line-height: 50rpx;
                                                width: 242rpx;
                                            "
                                        >
                                            <text>更换成员上场</text>
                                        </view>
                                        <view
                                            v-else-if="arenaInfo.status == 'second_half'"
                                            @tap="showSwitchMember"
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/e394d545704b0e5009c5d515e757f025.png);
                                                background-size: 100% 100%;
                                                line-height: 30rpx;
                                                width: 242rpx;
                                            "
                                        >
                                            <text>
                                                更换成员上场
                                                <text style="font-size: 10px">（剩余{{ item.my_team.replace_member_left_times }}次）</text>
                                            </text>
                                        </view>
                                        <view
                                            v-else
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/10aeefb9ad989448863a6df23f24167a.png);
                                                background-size: 100% 100%;
                                                line-height: 30rpx;
                                                width: 242rpx;
                                            "
                                        >
                                            <text>
                                                更换成员上场
                                                <text style="font-size: 10px">下半场可换</text>
                                            </text>
                                        </view>
                                        <view
                                            @tap="showTeamInfo"
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            style="
                                                background: url(https://star-img.xingxiaoculture.com/2023/05/17/ed3fac5acbeeeb401f683cbd909a3f91.png);
                                                background-size: 100% 100%;
                                            "
                                        >
                                            查看队伍详情
                                        </view>
                                    </view>
                                    <view class="tree_info_button_box" v-else>
                                        <view
                                            @tap="showTeamInfo"
                                            class="tree_info_button3"
                                            :data-item="item.my_team"
                                            data-tab="1"
                                            style="
                                                background: linear-gradient(180deg, #4fcaff, #217ef9);
                                                line-height: 67rpx;
                                                width: 635rpx;
                                                border-radius: 34rpx;
                                                border: 5rpx double #9dddff;
                                            "
                                        >
                                            <text>查看我的助力信息</text>
                                        </view>
                                    </view>
                                </block>
                                <block v-else-if="item.tab == 1">
                                    <view
                                        style="width: 100%; display: flex; flex-direction: column; align-items: center"
                                        v-if="item.help_info && item.help_info.msg_list.length > 0"
                                    >
                                        <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                                            <view class="group_pk_prizes_list_line"></view>
                                            <view class="group_pk_prizes_list_text2">助力动态</view>
                                        </view>
                                        <view class="team_info_line" style="height: 30rpx; background: none">
                                            <!-- <text class="team_info_line_text3" style="width: 220rpx;margin-left: 20rpx;">魅力类型</text> -->
                                            <text class="team_info_line_text3" style="width: 350rpx; margin-left: 20rpx">助力成员</text>
                                            <text class="team_info_line_text3" style="width: 350rpx">助力体力点</text>
                                            <text class="team_info_line_text3" style="width: 170rpx">助力时间</text>
                                        </view>
                                        <scroll-view class="zhuli_dongtai_box" :scroll-y="true">
                                            <view class="zhuli_dongtai_box_item" v-for="(m_item, index) in item.help_info.msg_list" :key="index">
                                                <!-- <image style="width: 116rpx;height: 28rpx;margin-left: 20rpx;" src="{{m_item.pos_img}}"></image> -->

                                                <image class="zhuli_dongtai_box_item_icon" style="margin-left: 20rpx" :src="m_item.userinfo.image.middle_pic"></image>

                                                <view class="zhuli_dongtai_box_item_text" style="width: 200rpx">{{ m_item.userinfo.nickname }}</view>

                                                <view class="zhuli_dongtai_box_item_text" style="width: 260rpx">
                                                    助力
                                                    <text style="color: #ff6800">{{ m_item.votetimes }}</text>
                                                </view>

                                                <view class="zhuli_dongtai_box_item_text">{{ m_item.time }}</view>
                                            </view>
                                        </scroll-view>
                                        <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                                            <view class="group_pk_prizes_list_line"></view>
                                            <view class="group_pk_prizes_list_text2">助力用户</view>
                                        </view>
                                        <view class="team_info_line" style="height: 30rpx; background: none">
                                            <text class="team_info_line_text3" style="width: 350rpx; margin-left: 30rpx">用户信息</text>
                                            <text class="team_info_line_text3" style="width: 400rpx">共助力数量</text>
                                            <text class="team_info_line_text3" style="width: 180rpx">全队占比</text>
                                        </view>
                                        <view class="zhuli_dongtai_box" style="margin-bottom: 30rpx; height: 332rpx">
                                            <view class="zhuli_dongtai_box_item" v-for="(m_item, index) in item.help_info.user_list" :key="index">
                                                <image class="zhuli_dongtai_box_item_icon" style="margin-left: 30rpx" :src="m_item.userinfo.image.middle_pic"></image>

                                                <view class="zhuli_dongtai_box_item_text" style="width: 180rpx">{{ m_item.userinfo.nickname }}</view>

                                                <view class="zhuli_dongtai_box_item_text" style="width: 260rpx">
                                                    已助力
                                                    <text style="color: #ff6800">{{ m_item.votetimes }}</text>
                                                </view>

                                                <view class="zhuli_dongtai_box_item_text" style="color: #ff6800">{{ m_item.percent }}</view>
                                            </view>
                                            <view class="zhuli_dongtai_box_more_info" @tap="showTeamInfo" :data-item="item.my_team">查看队伍信息</view>
                                        </view>
                                    </view>
                                    <view class="zhuli_none_box" v-else>
                                        <image
                                            src="https://star-img.xingxiaoculture.com/2023/05/17/6f1197e16c976fa2a6fa3476686c5983.png"
                                            style="width: 265rpx; height: 196rpx"
                                        ></image>
                                        <view class="zhuli_box_none_text">抱歉，当前暂无助力信息~</view>
                                    </view>
                                </block>
                                <block v-else-if="item.tab == 2">
                                    <view style="width: 100%; display: flex; flex-direction: column; align-items: center">
                                        <view class="team_info_line" style="height: 52rpx; margin-top: 20rpx">
                                            <image class="team_info_line_area_image" :src="item.my_team.members_pos[item.puzzle_pos ? item.puzzle_pos : 0].img"></image>
                                            <view @tap="showBalloonLocationPanel" class="team_info_line_right" :data-item="item.my_team" :data-team_type="'inteam'">
                                                切换魅力类型>
                                            </view>
                                        </view>
                                        <!-- <text class="tree_info_text4" style="width: 600rpx;text-align: center;margin-top: 10rpx;">全部解锁后，在当前魅力类型贡献过的成员（包括助力成员） 都可获得<text style="color: #FF6800;">奖励N个</text></text> -->
                                        <view style="width: 660rpx; height: 990rpx; position: relative; margin-top: 10rpx; margin-bottom: 20rpx">
                                            <image style="width: 660rpx; height: 990rpx" :src="item.puzzle.img"></image>
                                            <view
                                                style="
                                                    width: 660rpx;
                                                    height: 990rpx;
                                                    position: absolute;
                                                    display: flex;
                                                    flex-wrap: wrap;
                                                    top: 0;
                                                    left: 0;
                                                    align-items: center;
                                                    justify-content: center;
                                                "
                                            >
                                                <view
                                                    class="puzzle_block"
                                                    :style="p_item.userid != '' ? 'opacity: 0;' : ''"
                                                    :data-item="item"
                                                    :data-team_type="'inteam'"
                                                    :data-piece="p_index"
                                                    :data-index="index"
                                                    data-tab="2"
                                                    @tap="parseEventDynamicCode($event, p_item.userid != '' ? '' : 'unlockJigsawClick')"
                                                    v-for="(p_item, p_index) in item.puzzle.list"
                                                    :key="p_index"
                                                ></view>
                                            </view>
                                        </view>
                                    </view>
                                </block>
                            </view>
                        </block>
                    </block>
                    <view class="tree_item" v-if="balloonTermId && balloonTermId < arenaInfo.term_id && myBalloonTeam.length == 0">
                        <view class="tree_team" style="text-align: center; height: 82rpx">
                            <text class="tree_team_title" style="width: 100%">当前场次已结束</text>
                        </view>
                        <view class="tree_info_column" style="width: 100%; margin-bottom: 40rpx">
                            <text class="tree_info_text4">当前场次没有队伍信息</text>
                        </view>
                    </view>
                    <view class="tree_item" v-if="(!balloonTermId || balloonTermId == arenaInfo.term_id) && arenaInfo.status != 'is_end' && myBalloonTeam.length < teamNumMax">
                        <view class="tree_team" style="text-align: center; height: 82rpx">
                            <text class="tree_team_title" style="width: 100%">您可以选择创建或者加入一个队伍</text>
                        </view>
                        <view class="tree_info_column" style="width: 100%">
                            <text class="tree_info_text4">组成队伍后即可获得{{ tool_s_name }}*200</text>
                            <view class="tree_info_button_box">
                                <view @tap="showBalloonCreateTeamPanel" class="tree_info_button2" data-action="create_team" data-type="1">创建队伍</view>
                                <view @tap="handleTeamJoinlist" class="tree_info_button2">加入其他队伍</view>
                            </view>
                        </view>
                    </view>
                </view>

                <!-- 顶气球 -->
                <view class="act_rank_list_view tabPage2">
                    <image class="act_tb_head_image" src=""></image>
                    <image class="act_rank_list_image" src="https://star-img.xingxiaoculture.com/2023/05/15/739aa9037a690d4e5ab0f2e8c42fb202.png"></image>
                    <view class="act_rank_list_time">{{ actFengzhengAllCountdownTimeStr }}</view>
                    <text class="act_rank_list_tip4">
                        召唤海神雨获得{{ tool_b_name }}，海神雨等级>=200级并占领曝光位 即可获得对应的卡片，每张可为星球增加
                        <text style="color: #ff5d26">200万{{ tool_b_name }}</text>
                        哦
                    </text>
                    <!-- <view bindtap="showGiftDateSelect" class="act_rank_list_date">{{giftTermObj.term_text}}<text wx:if="{{!bShowGiftDateSelect}}">▼</text>
                <text wx:else>▲</text>
            </view>
            <view class="act_rank_list_date_select" wx:if="{{bShowGiftDateSelect}}">
                <view bindtap="changeGiftDate" class="act_rank_list_date_item" data-item="{{item}}" wx:for="{{giftDateList}}" wx:key="index">{{item.term_text}}</view>
            </view> -->
                    <view class="act_rank_list_banner4">
                        <view class="fengzhen_item" v-for="(item, index) in voteAllList" :key="index">
                            <block v-if="item.data.userid">
                                <!-- <view class="fengzhen_item_time" wx:if="{{item.data.left_time_text && actFengzhengAllCountdownTimeStr!='本场次已结束'}}">{{item.data.left_time_text}}</view> -->
                                <image class="fengzhen_item_image" :src="item.data.balloon_icon"></image>
                                <view
                                    class="fengzhen_item_info"
                                    :style="'background-color: ' + item.data.balloon_bg + ';border-color: ' + item.data.balloon_border + ';'"
                                    @tap="gotoDressRoom"
                                    :data-userid="item.data.userid"
                                >
                                    <view class="fengzhen_item_star">Lv.{{ item.data.balloon_level }}</view>
                                    <image style="width: 150rpx; height: 108rpx; margin-top: 20rpx; margin-bottom: 20rpx" :src="item.data.user_guard_angle.share_img"></image>
                                    <view class="fengzhen_item_user">
                                        <image class="fengzhen_item_user_image" :src="item.data.user_img.thumbnail_pic"></image>
                                        <view class="fengzhen_item_user_info">
                                            <view class="fengzhen_item_user_level">{{ item.data.starball.name }}</view>
                                            <!-- <view class="fengzhen_item_user_level">Lv.{{item.data.balloon_level}}</view> -->
                                            <view class="fengzhen_item_user_name">{{ item.data.nickname }}</view>
                                        </view>
                                    </view>
                                </view>
                                <view class="fengzhen_item_ding" @tap="showVotePanel2" :data-item="item">{{ item.data.userid == user_info._id ? '提升等级' : '顶掉海神雨' }}</view>
                            </block>

                            <block v-else-if="">
                                <image class="fengzhen_item_image" :src="item.data.balloon_icon"></image>
                                <view
                                    class="fengzhen_item_info"
                                    :style="'justify-content: center;background-color: ' + item.data.balloon_bg + ';border-color: ' + item.data.balloon_border + ';'"
                                >
                                    <image style="width: 86rpx; height: 86rpx" src="https://star-img.xingxiaoculture.com/2023/05/15/c7b3276d402c09fdc48fa4377c5b0d65.png"></image>
                                </view>
                                <view class="fengzhen_item_ding" @tap="showVotePanel2" :data-item="item">召唤海神雨</view>
                            </block>
                        </view>
                    </view>
                    <view class="act_rank_list_more" style="width: 500rpx">
                        <text @tap="showBalloonRecordPanel">海神雨记录></text>
                        <text @tap="showBalloonRecordPanel" style="margin-left: 50rpx" data-type="1">卡片记录></text>
                        <text @tap="showBalloonRulePanel" style="margin-left: 50rpx">海神雨规则></text>
                    </view>
                </view>
                <view class="act_rank_list_view tabPage3">
                    <image class="act_tb_head_image" src=""></image>
                    <image class="act_rank_list_image" :src="pageDataObj.bp_time_arr[pageDataObj.phase].phase_title_img"></image>
                    <view class="act_rank_list_time">{{ actCountdownTimeStr }}</view>
                    <image @tap="activityrule" class="act_rank_list_banner" :data-type="3" :src="pageDataObj.bp_time_arr[pageDataObj.phase].phase_img"></image>
                    <text class="act_rank_list_tip4">{{ pageDataObj.bp_time_arr[pageDataObj.phase].phase_from_to }}</text>
                    <view @tap="gotoPageClick" class="content_search_view" :data-type="1">
                        <image class="content_search_image" src="https://star-img.xingxiaoculture.com/2023/05/15/31ef24def4d53d890e362cfd7fc6c272.png"></image>
                        <view class="content_search_text">点击搜索星球赠送{{ tool_b_name }}</view>
                    </view>
                    <!-- <view class="act_rank_list_time2" wx:if="{{actSupplyCountdownTimeStr}}">{{actSupplyCountdownTimeStr}}</view>
            <text class="act_rank_list_tip4">到达福利站时排名前五的星球可获奖
第一名星球根据{{tool_b_name}}{{tool_b_unit}}数最后一位数字获得其他大屏奖励</text>
            <view bindtap="switchFulizhan" class="act_rank_list_time2">{{bShowFulizhan?'收起▲':'展开▼'}}</view>
            <view class="table" wx:if="{{bShowFulizhan}}">
                <view class="tr">
                    <view class="th t1">排名</view>
                    <view class="th t2">获得奖励</view>
                    <view class="th t3">获奖星球</view>
                </view>
                <block wx:if="{{pageDataObj.bp_time_arr[pageDataObj.phase].fill_station_list[0]}}">
                    <view class="tr" wx:for="{{pageDataObj.bp_time_arr[pageDataObj.phase].fill_station_list[0].prize_list}}" wx:key="index">
                        <view class="td t1">{{index+1}}</view>
                        <view class="td t2" wx:if="{{item.buff}}">
                            <text bindtap="activityrule" data-type="{{3}}" style="text-decoration: underline;color:#FF1F06" wx:if="{{item.prize}}">{{item.prize}}</text>
                            <text wx:if="{{item.buff}}">{{item.prize?'+':''}}{{item.buff}}</text>
                        </view>
                        <view class="td t3" wx:if="{{item.starball}}">
                            <text>{{item.starball+'\n'+item.votetimes}}</text>
                        </view>
                        <view class="td t3" wx:else></view>
                    </view>
                </block>
            </view> -->
                    <view class="group_pk_prizes_list2" v-if="is_login && pageDataObj.user_vote_stars_per_phase.length > 0">
                        <view class="group_pk_prizes_list_line"></view>
                        <view class="group_pk_prizes_list_text2">我的星球</view>
                    </view>
                    <!-- parse <template is="rank_list_item_view" :data="item:item,index:index,showTuan:true,zoneinfo:pageDataObj.bp_time_arr[pageDataObj.phase],tool_b_unit:tool_b_unit" v-if="index < 1" v-for="(item,index) in (pageDataObj.user_vote_stars_per_phase)" :key="index"></template> -->
                    <block name="rank_list_item_view" v-if="index < 1" v-for="(item, index) in pageDataObj.user_vote_stars_per_phase" :key="index">
                        <view class="act_rank_list_item" :style="showLine && index == 0 ? 'margin-top: 0rpx;' : ''">
                            <view class="act_rank_list_item_line" v-if="showLine && index != 0"></view>
                            <view class="act_rank_list_item_msg">
                                <view
                                    :class="
                                        'act_rank_list_item_rank ' +
                                        (item.rank == 1
                                            ? 'act_rank_list_item_rank_one'
                                            : item.rank == 2
                                            ? 'act_rank_list_item_rank_two'
                                            : item.rank == 3
                                            ? 'act_rank_list_item_rank_stree'
                                            : '')
                                    "
                                    :style="
                                        item.rank == -1 || item.rank == '未上榜'
                                            ? 'color: #333333;line-height:30rpx;margin-top:-40rpx'
                                            : item.rank < 4
                                            ? 'color: #ffffff;'
                                            : 'color: #333333;'
                                    "
                                >
                                    {{ pageDataObj.single_user_rank_info_all_time.rank == -1 ? '未上榜' : pageDataObj.single_user_rank_info_all_time.rank }}
                                </view>
                                <view :class="showStar ? 'act_rank_list_item_head' : 'act_rank_list_item_head'">
                                    <image
                                        @tap="gotoDressRoom"
                                        class="act_rank_list_item_head_image"
                                        :data-userid="item.userinfo._id"
                                        :src="item.userinfo.image.middle_pic"
                                        v-if="showStar"
                                    ></image>
                                    <image class="act_rank_list_item_head_image" :src="item.starinfo.logo_img ? item.starinfo.logo_img : item.starball.logo_url" v-else></image>
                                </view>
                                <view class="act_rank_list_item_column" style="flex: 1">
                                    <view class="act_rank_list_item_name">
                                        {{
                                            true
                                                ? pageDataObj.single_user_rank_info_all_time.userinfo.nickname
                                                : pageDataObj.single_user_rank_info_all_time.starinfo
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.name
                                                : pageDataObj.single_user_rank_info_all_time.starball.name
                                        }}
                                    </view>
                                    <view class="act_rank_list_item_soce">{{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}</view>
                                </view>
                                <view class="act_rank_list_item_column" style="align-items: center; margin-right: 22rpx">
                                    <view class="act_rank_list_item_soce" v-if="showStar">
                                        <image class="act_rank_list_item_head" :src="item.max_starinfo.logo_img"></image>
                                    </view>
                                    <view class="act_rank_list_item_soce" v-else-if="showScore">{{ pageDataObj.single_user_rank_info_all_time.score }}积分</view>
                                    <block v-else>
                                        <view
                                            @tap="sendLeaderBoardClick"
                                            class="act_rank_list_item_tuan"
                                            :data-item="item"
                                            :data-zoneinfo="pageDataObj.bp_time_arr[pageDataObj.phase]"
                                            v-if="true"
                                        ></view>
                                    </block>
                                </view>
                            </view>

                            <view class="star_buff_box" v-if="item.buff_list">
                                <block v-if="item.buff_list.length > 0">
                                    <view class="star_buff_inner_box" @tap="getStarBuffTeamList" :data-starid="item.starid">
                                        <image
                                            style="width: 177rpx; height: 24rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/7075d915e89af609564323cfbf20e8cc.png"
                                        ></image>
                                        <view class="star_buff_item" v-for="(b_item, index) in item.buff_list" :key="index">{{ b_item }}</view>
                                    </view>
                                    <view class="star_buff_button" :style="item.buff_list.length > 4 ? 'height:72rpx' : ''" @tap="getStarBuffTeamList" :data-starid="item.starid">
                                        <image
                                            style="width: 27rpx; height: 51rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/2aaedafaef55928f5d223f71aa120999.png"
                                        ></image>
                                    </view>
                                </block>
                                <block v-else-if="">
                                    <image
                                        style="width: 100%; height: 55rpx"
                                        @tap="gotoTreeRule"
                                        data-type="2"
                                        src="https://star-img.xingxiaoculture.com/2023/05/15/c1360019d293eaaa2ccbcfc9754250d9.png"
                                    ></image>
                                </block>
                            </view>

                            <view class="flex_row flex_center" style="flex-wrap: wrap; width: 100%; margin-top: 20rpx" v-if="item.card_list">
                                <text style="font-size: 11px; color: #333333; font-weight: 600; width: 150rpx">
                                    获得卡片
                                    <text style="color: #ff5d26">{{ pageDataObj.single_user_rank_info_all_time.card_num }}</text>
                                    /8
                                </text>
                                <text style="font-size: 10px; color: #333333; width: 484rpx; text-align: right">
                                    每张获得的卡片已为星球增加
                                    <text style="color: #ff5d26">200万</text>
                                    {{ tool_b_name }}
                                </text>
                                <view class="fengzhen_item" style="width: 148rpx" v-for="(c_item, index) in item.card_list" :key="index">
                                    <image class="fengzhen_item_image" style="width: 148rpx; height: 190rpx" :src="c_item.card_img"></image>

                                    <view
                                        v-if="!c_item.userinfo"
                                        style="z-index: 1; width: 148rpx; height: 190rpx; background-color: rgba(0, 0, 0, 0.6); margin-top: -190rpx; border-radius: 10rpx"
                                    ></view>

                                    <view
                                        class="flex_column flex_center"
                                        v-if="c_item.userinfo"
                                        style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx; background: linear-gradient(180deg, #dbf2ff, #b3d4ff)"
                                        @tap="gotoDressRoom"
                                        :data-userid="c_item.userinfo._id"
                                    >
                                        <image class="fengzhen_item_user_image" :src="c_item.userinfo.image.thumbnail_pic"></image>
                                        <view class="fengzhen_item_star" style="margin-top: 0rpx; color: #333333">{{ c_item.userinfo.nickname }}</view>
                                    </view>

                                    <view class="flex_column flex_center" v-else-if="" style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx">
                                        <view class="fengzhen_item_ding" @tap="tabItemClick" data-index="1" style="font-size: 9px" :data-item="item">帮星球得卡片</view>
                                    </view>
                                </view>
                            </view>

                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; margin-top: 20rpx; width: 632rpx; border-radius: 20rpx; padding: 8rpx; background-color: #77b3f2"
                                v-if="item.top3group"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的公会</view>
                                    <view @tap="gotoGonghuiList" class="act_rank_list_more" :data-starid="item.starid" style="margin: 0; text-align: right; color: #ffffff">
                                        查看更多>
                                    </view>
                                </view>
                                <view
                                    class="flex_row flex_center"
                                    style="width: 622rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 10rpx; margin-top: 10rpx"
                                    v-for="(g_item, index) in item.top3group"
                                    :key="index"
                                >
                                    <view style="width: 38%; height: 64rpx" v-if="g_item.top3user">
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="gu_item.userinfo._id"
                                            :src="gu_item.userinfo.image.thumbnail_pic"
                                            style="width: 64rpx; height: 64rpx; border-radius: 50%; margin: 2rpx"
                                            v-for="(gu_item, index1) in g_item.top3user"
                                            :key="index1"
                                        ></image>
                                    </view>

                                    <view class="act_rank_list_item_soce" style="width: 30%; color: #5f758d">{{ g_item.group_name }}</view>

                                    <view class="act_rank_list_item_soce" style="width: 25%; text-align: right; color: #5f758d">{{ g_item.votetimes }}</view>
                                </view>
                                <view style="height: 25rpx"></view>
                            </view>
                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; width: 632rpx; border-radius: 20rpx; padding: 8rpx; margin-top: 20rpx; background-color: #77b3f2"
                                v-if="item.top3user"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的用户</view>
                                    <view @tap="gotoPersonList" class="act_rank_list_more" :data-starid="item.starid" style="margin: 0; text-align: right; color: #ffffff">
                                        查看更多>
                                    </view>
                                </view>
                                <view class="flex_row flex_center">
                                    <view
                                        class="flex_column flex_center"
                                        style="width: 186rpx; height: 146rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 20rpx; margin: 12rpx"
                                        v-for="(u_item, index) in item.top3user"
                                        :key="index"
                                    >
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="u_item.userinfo._id"
                                            :src="u_item.userinfo.image.thumbnail_pic"
                                            style="width: 70rpx; height: 70rpx; border-radius: 50%; margin: 2rpx"
                                        ></image>

                                        <view class="act_rank_list_item_soce" style="width: 150rpx; overflow: hidden; white-space: nowrap; text-align: center; color: #5e758f">
                                            {{ u_item.userinfo.nickname }}
                                        </view>

                                        <view class="act_rank_list_item_soce" style="color: #5e758f">{{ u_item.votetimes }}</view>
                                    </view>
                                </view>
                            </view>
                            <text class="act_rank_list_item_text" v-if="item.text">{{ pageDataObj.single_user_rank_info_all_time.text }}</text>
                            <view style="height: 25rpx" v-else></view>
                        </view>
                    </block>
                    <view @tap="lookMoreClick" class="act_rank_list_more" :data-type="0" v-if="pageDataObj.user_vote_stars_per_phase.length >= 2">查看更多></view>
                    <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                        <view class="group_pk_prizes_list_line"></view>
                        <view class="group_pk_prizes_list_text2">星球阶段排名</view>
                    </view>
                    <!-- parse <template is="rank_list_item_view" :data="item:item,index:index,showTuan:true,zoneinfo:pageDataObj.bp_time_arr[pageDataObj.phase],tool_b_unit:tool_b_unit" v-for="(item,index) in (pageDataObj.star_rank_per_phase)" :key="index"></template> -->
                    <block name="rank_list_item_view" v-if="false" v-for="(item, index) in pageDataObj.star_rank_per_phase" :key="index">
                        <view class="act_rank_list_item" :style="showLine && index == 0 ? 'margin-top: 0rpx;' : ''">
                            <view class="act_rank_list_item_line" v-if="showLine && index != 0"></view>
                            <view class="act_rank_list_item_msg">
                                <view
                                    :class="
                                        'act_rank_list_item_rank ' +
                                        (item.rank == 1
                                            ? 'act_rank_list_item_rank_one'
                                            : item.rank == 2
                                            ? 'act_rank_list_item_rank_two'
                                            : item.rank == 3
                                            ? 'act_rank_list_item_rank_stree'
                                            : '')
                                    "
                                    :style="
                                        item.rank == -1 || item.rank == '未上榜'
                                            ? 'color: #333333;line-height:30rpx;margin-top:-40rpx'
                                            : item.rank < 4
                                            ? 'color: #ffffff;'
                                            : 'color: #333333;'
                                    "
                                >
                                    {{ pageDataObj.single_user_rank_info_all_time.rank == -1 ? '未上榜' : pageDataObj.single_user_rank_info_all_time.rank }}
                                </view>
                                <view :class="showStar ? 'act_rank_list_item_head' : 'act_rank_list_item_head'">
                                    <image
                                        @tap="gotoDressRoom"
                                        class="act_rank_list_item_head_image"
                                        :data-userid="item.userinfo._id"
                                        :src="item.userinfo.image.middle_pic"
                                        v-if="showStar"
                                    ></image>
                                    <image class="act_rank_list_item_head_image" :src="item.starinfo.logo_img ? item.starinfo.logo_img : item.starball.logo_url" v-else></image>
                                </view>
                                <view class="act_rank_list_item_column" style="flex: 1">
                                    <view class="act_rank_list_item_name">
                                        {{
                                            true
                                                ? pageDataObj.single_user_rank_info_all_time.userinfo.nickname
                                                : pageDataObj.single_user_rank_info_all_time.starinfo
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.name
                                                : pageDataObj.single_user_rank_info_all_time.starball.name
                                        }}
                                    </view>
                                    <view class="act_rank_list_item_soce">{{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}</view>
                                </view>
                                <view class="act_rank_list_item_column" style="align-items: center; margin-right: 22rpx">
                                    <view class="act_rank_list_item_soce" v-if="showStar">
                                        <image class="act_rank_list_item_head" :src="item.max_starinfo.logo_img"></image>
                                    </view>
                                    <view class="act_rank_list_item_soce" v-else-if="showScore">{{ pageDataObj.single_user_rank_info_all_time.score }}积分</view>
                                    <block v-else>
                                        <view
                                            @tap="sendLeaderBoardClick"
                                            class="act_rank_list_item_tuan"
                                            :data-item="item"
                                            :data-zoneinfo="pageDataObj.bp_time_arr[pageDataObj.phase]"
                                            v-if="true"
                                        ></view>
                                    </block>
                                </view>
                            </view>

                            <view class="star_buff_box" v-if="item.buff_list">
                                <block v-if="item.buff_list.length > 0">
                                    <view class="star_buff_inner_box" @tap="getStarBuffTeamList" :data-starid="item.starid">
                                        <image
                                            style="width: 177rpx; height: 24rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/7075d915e89af609564323cfbf20e8cc.png"
                                        ></image>
                                        <view class="star_buff_item" v-for="(b_item, index) in item.buff_list" :key="index">{{ b_item }}</view>
                                    </view>
                                    <view class="star_buff_button" :style="item.buff_list.length > 4 ? 'height:72rpx' : ''" @tap="getStarBuffTeamList" :data-starid="item.starid">
                                        <image
                                            style="width: 27rpx; height: 51rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/2aaedafaef55928f5d223f71aa120999.png"
                                        ></image>
                                    </view>
                                </block>
                                <block v-else-if="">
                                    <image
                                        style="width: 100%; height: 55rpx"
                                        @tap="gotoTreeRule"
                                        data-type="2"
                                        src="https://star-img.xingxiaoculture.com/2023/05/15/c1360019d293eaaa2ccbcfc9754250d9.png"
                                    ></image>
                                </block>
                            </view>

                            <view class="flex_row flex_center" style="flex-wrap: wrap; width: 100%; margin-top: 20rpx" v-if="item.card_list">
                                <text style="font-size: 11px; color: #333333; font-weight: 600; width: 150rpx">
                                    获得卡片
                                    <text style="color: #ff5d26">{{ pageDataObj.single_user_rank_info_all_time.card_num }}</text>
                                    /8
                                </text>
                                <text style="font-size: 10px; color: #333333; width: 484rpx; text-align: right">
                                    每张获得的卡片已为星球增加
                                    <text style="color: #ff5d26">200万</text>
                                    {{ tool_b_name }}
                                </text>
                                <view class="fengzhen_item" style="width: 148rpx" v-for="(c_item, index) in item.card_list" :key="index">
                                    <image class="fengzhen_item_image" style="width: 148rpx; height: 190rpx" :src="c_item.card_img"></image>

                                    <view
                                        v-if="!c_item.userinfo"
                                        style="z-index: 1; width: 148rpx; height: 190rpx; background-color: rgba(0, 0, 0, 0.6); margin-top: -190rpx; border-radius: 10rpx"
                                    ></view>

                                    <view
                                        class="flex_column flex_center"
                                        v-if="c_item.userinfo"
                                        style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx; background: linear-gradient(180deg, #dbf2ff, #b3d4ff)"
                                        @tap="gotoDressRoom"
                                        :data-userid="c_item.userinfo._id"
                                    >
                                        <image class="fengzhen_item_user_image" :src="c_item.userinfo.image.thumbnail_pic"></image>
                                        <view class="fengzhen_item_star" style="margin-top: 0rpx; color: #333333">{{ c_item.userinfo.nickname }}</view>
                                    </view>

                                    <view class="flex_column flex_center" v-else-if="" style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx">
                                        <view class="fengzhen_item_ding" @tap="tabItemClick" data-index="1" style="font-size: 9px" :data-item="item">帮星球得卡片</view>
                                    </view>
                                </view>
                            </view>

                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; margin-top: 20rpx; width: 632rpx; border-radius: 20rpx; padding: 8rpx; background-color: #77b3f2"
                                v-if="item.top3group"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的公会</view>
                                    <view @tap="gotoGonghuiList" class="act_rank_list_more" :data-starid="item.starid" style="margin: 0; text-align: right; color: #ffffff">
                                        查看更多>
                                    </view>
                                </view>
                                <view
                                    class="flex_row flex_center"
                                    style="width: 622rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 10rpx; margin-top: 10rpx"
                                    v-for="(g_item, index) in item.top3group"
                                    :key="index"
                                >
                                    <view style="width: 38%; height: 64rpx" v-if="g_item.top3user">
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="gu_item.userinfo._id"
                                            :src="gu_item.userinfo.image.thumbnail_pic"
                                            style="width: 64rpx; height: 64rpx; border-radius: 50%; margin: 2rpx"
                                            v-for="(gu_item, index1) in g_item.top3user"
                                            :key="index1"
                                        ></image>
                                    </view>

                                    <view class="act_rank_list_item_soce" style="width: 30%; color: #5f758d">{{ g_item.group_name }}</view>

                                    <view class="act_rank_list_item_soce" style="width: 25%; text-align: right; color: #5f758d">{{ g_item.votetimes }}</view>
                                </view>
                                <view style="height: 25rpx"></view>
                            </view>
                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; width: 632rpx; border-radius: 20rpx; padding: 8rpx; margin-top: 20rpx; background-color: #77b3f2"
                                v-if="item.top3user"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的用户</view>
                                    <view @tap="gotoPersonList" class="act_rank_list_more" :data-starid="item.starid" style="margin: 0; text-align: right; color: #ffffff">
                                        查看更多>
                                    </view>
                                </view>
                                <view class="flex_row flex_center">
                                    <view
                                        class="flex_column flex_center"
                                        style="width: 186rpx; height: 146rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 20rpx; margin: 12rpx"
                                        v-for="(u_item, index) in item.top3user"
                                        :key="index"
                                    >
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="u_item.userinfo._id"
                                            :src="u_item.userinfo.image.thumbnail_pic"
                                            style="width: 70rpx; height: 70rpx; border-radius: 50%; margin: 2rpx"
                                        ></image>

                                        <view class="act_rank_list_item_soce" style="width: 150rpx; overflow: hidden; white-space: nowrap; text-align: center; color: #5e758f">
                                            {{ u_item.userinfo.nickname }}
                                        </view>

                                        <view class="act_rank_list_item_soce" style="color: #5e758f">{{ u_item.votetimes }}</view>
                                    </view>
                                </view>
                            </view>
                            <text class="act_rank_list_item_text" v-if="item.text">{{ pageDataObj.single_user_rank_info_all_time.text }}</text>
                            <view style="height: 25rpx" v-else></view>
                        </view>
                    </block>
                    <view @tap="lookMoreClick" class="act_rank_list_more" :data-type="0" v-if="pageDataObj.star_rank_per_phase.length >= 5">查看更多></view>
                    <!-- parse <template is="rank_list_empty_view" :data="text:'暂无排名信息，快去为TA送'+tool_b_name+'吧！',showBtn:true" v-if="(!pageDataObj.star_rank_per_phase||pageDataObj.star_rank_per_phase.length==0)"></template> -->
                    <block name="rank_list_empty_view" v-if="!pageDataObj.star_rank_per_phase || pageDataObj.star_rank_per_phase.length == 0">
                        <view class="act_rank_list_empty">
                            <image class="act_rank_list_empty_icon" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/12/29/rjmDKGfwX31609236901495.png"></image>
                            <view class="act_rank_list_empty_text">{{ '暂无排名信息，快去为TA送' + tool_b_name + '吧！' }}</view>
                        </view>
                    </block>
                </view>
                <view class="act_rank_list_view tabPage4" id="top-3">
                    <image class="act_tb_head_image" src=""></image>
                    <image class="act_rank_list_image" src="https://star-img.xingxiaoculture.com/2023/05/15/453884aee34996795b1bef67775010f5.png"></image>
                    <text class="act_rank_list_tip4">根据公会 05.20 11：'00' ~ 05.31 22：'00' 累计为星球赠送的 {{ tool_b_name }}数量排名，数据每3分钟更新一次</text>
                    <image @tap="activityrule" class="act_rank_list_banner2" :data-type="5" :src="pageDataObj.group_rank_banner"></image>
                    <view>
                        <view class="group-data-list-content">
                            <view v-if="pageDataObj.group_rank.user_group.length > 0">
                                <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                                    <view class="group_pk_prizes_list_line"></view>
                                    <view class="group_pk_prizes_list_text2">我的公会</view>
                                </view>
                                <view class="group-frame" v-for="(item, index) in pageDataObj.group_rank.user_group" :key="index">
                                    <view class="group-data-item">
                                        <view class="group-data-content">
                                            <view class="group-data-left">
                                                <text
                                                    :class="[
                                                        'group-num',
                                                        item.rank === 1 ? 'group-num_01' : '',
                                                        item.rank === 2 ? 'group-num_02' : '',
                                                        item.rank === 3 ? 'group-num_03' : ''
                                                    ]"
                                                    v-if="item.rank <= 10 && item.rank > 0"
                                                >
                                                    {{ item.rank }}
                                                </text>
                                                <text class="group-data-level-not" v-else>未上榜</text>
                                            </view>
                                            <view class="group-data-info">
                                                <view class="group-title">{{ item.group_info.admin_name }}</view>
                                                <view class="group-num-info">{{ item.votetimes }}{{ tool_b_unit }}</view>
                                                <view class="group-data-avatar">
                                                    <image
                                                        @tap="gotoDressRoom"
                                                        :class="[
                                                            idx === 0 ? 'group-data-avatar_first' : '',
                                                            idx === 1 ? 'group-data-avatar_content' : '',
                                                            idx === 2 ? 'group-data-avatar_last' : ''
                                                        ]"
                                                        :data-userid="itemImg.userinfo._id"
                                                        :src="itemImg.userinfo.image.thumbnail_pic"
                                                        v-if="3 > idx"
                                                        v-for="(itemImg, idx) in item.user_list"
                                                        :key="idx"
                                                    ></image>
                                                </view>
                                            </view>
                                            <view class="group-data-btn">
                                                <image
                                                    :src="item.group_info.group_emblem_img ? item.group_info.group_emblem_img : item.star_ball.logo_url"
                                                    style="width: 104rpx; height: 104rpx"
                                                ></image>
                                            </view>
                                        </view>
                                        <view class="act_rank_list_item_text" style="margin: 10rpx; width: 560rpx">
                                            <view class="group_level_garw_item">{{ pageDataObj.single_user_rank_info_all_time.text }}</view>
                                        </view>
                                    </view>

                                    <image :src="item.group_info.group_frame.icon" style="width: 100%; height: 100%; position: absolute" v-if="item.group_info.group_frame"></image>
                                </view>
                            </view>
                            <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                                <view class="group_pk_prizes_list_line"></view>
                                <view class="group_pk_prizes_list_text2">公会总排名</view>
                            </view>
                            <view v-if="pageDataObj.group_rank.list.length > 0">
                                <view class="group-frame" v-for="(item, index) in pageDataObj.group_rank.list" :key="index">
                                    <view class="group-data-item">
                                        <view class="group-data-content">
                                            <view class="group-data-left">
                                                <text
                                                    :class="['group-num', index === 0 ? 'group-num_01' : '', index === 1 ? 'group-num_02' : '', index === 2 ? 'group-num_03' : '']"
                                                >
                                                    {{ index + 1 }}
                                                </text>
                                            </view>
                                            <view class="group-data-info">
                                                <view class="group-title">{{ item.group_info.admin_name }}</view>
                                                <view class="group-num-info">{{ item.votetimes }}{{ tool_b_unit }}</view>
                                                <view class="group-data-avatar">
                                                    <image
                                                        @tap="gotoDressRoom"
                                                        :class="[
                                                            idx === 0 ? 'group-data-avatar_first' : '',
                                                            idx === 1 ? 'group-data-avatar_content' : '',
                                                            idx === 2 ? 'group-data-avatar_last' : ''
                                                        ]"
                                                        :data-userid="itemImg.userinfo._id"
                                                        :src="itemImg.userinfo.image.thumbnail_pic"
                                                        v-if="3 > idx"
                                                        v-for="(itemImg, idx) in item.user_list"
                                                        :key="idx"
                                                    ></image>
                                                </view>
                                            </view>
                                            <view class="group-data-btn">
                                                <image
                                                    :src="item.group_info.group_emblem_img ? item.group_info.group_emblem_img : item.star_ball.logo_url"
                                                    style="width: 104rpx; height: 104rpx"
                                                ></image>
                                            </view>
                                        </view>
                                        <view class="act_rank_list_item_text" style="margin: 10rpx; width: 560rpx" v-if="item.text">
                                            <view class="group_level_garw_item">{{ pageDataObj.single_user_rank_info_all_time.text }}</view>
                                        </view>
                                    </view>

                                    <image :src="item.group_info.group_frame.icon" style="width: 100%; height: 100%; position: absolute" v-if="item.group_info.group_frame"></image>
                                </view>
                            </view>
                            <view class="rankFlex_move_not" style="margin: 20rpx 0" v-else>
                                <image
                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2020/12/29/rjmDKGfwX31609236901495.png"
                                    style="width: 93rpx; height: 66rpx"
                                ></image>
                                <view class="rankFlex_move_not_text">暂无排名信息，请等待数据更新！</view>
                            </view>
                            <view @tap="handleFansMove" class="rankFlex_move" v-if="pageDataObj.group_rank.list.length >= 10">查看更多></view>
                        </view>
                    </view>
                </view>
                <!-- 个人总榜 -->
                <view class="act_rank_list_view tabPage5">
                    <image class="act_tb_head_image" src=""></image>
                    <image class="act_rank_list_image" src="https://star-img.xingxiaoculture.com/2023/05/15/45ca0730baec5baee21d6d93fca3f8e1.png"></image>
                    <text class="act_rank_list_tip4">根据个人 05.20 11：'00' ~ 05.31 22：'00' 累计为 星球赠送的{{ tool_b_name }}数量排名，数据每3分钟更新</text>
                    <image @tap="activityrule" class="act_rank_list_banner3" :data-type="6" :src="pageDataObj.user_rank_banner"></image>
                    <view
                        class="group_pk_prizes_list2"
                        style="margin: 30rpx auto"
                        v-if="is_login && pageDataObj.single_user_rank_info_all_time.max_starid != 0 && pageDataObj.user_rank_all_time.length > 0"
                    >
                        <view class="group_pk_prizes_list_line"></view>
                        <view class="group_pk_prizes_list_text2">我的个人排名</view>
                    </view>
                    <!-- parse <template is="rank_list_item_view" :data="item:pageDataObj.single_user_rank_info_all_time,index:0,showStar:true,tool_b_unit:tool_b_unit" v-if="is_login&&pageDataObj.single_user_rank_info_all_time.max_starid!=0&&pageDataObj.user_rank_all_time.length>0"></template> -->
                    <block
                        name="rank_list_item_view"
                        v-if="false"
                        v-if="is_login && pageDataObj.single_user_rank_info_all_time.max_starid != 0 && pageDataObj.user_rank_all_time.length > 0"
                    >
                        <view class="act_rank_list_item" :style="showLine && 0 == 0 ? 'margin-top: 0rpx;' : ''">
                            <view class="act_rank_list_item_line" v-if="showLine && 0 != 0"></view>
                            <view class="act_rank_list_item_msg">
                                <view
                                    :class="
                                        'act_rank_list_item_rank ' +
                                        (pageDataObj.single_user_rank_info_all_time.rank == 1
                                            ? 'act_rank_list_item_rank_one'
                                            : pageDataObj.single_user_rank_info_all_time.rank == 2
                                            ? 'act_rank_list_item_rank_two'
                                            : pageDataObj.single_user_rank_info_all_time.rank == 3
                                            ? 'act_rank_list_item_rank_stree'
                                            : '')
                                    "
                                    :style="
                                        pageDataObj.single_user_rank_info_all_time.rank == -1 || pageDataObj.single_user_rank_info_all_time.rank == '未上榜'
                                            ? 'color: #333333;line-height:30rpx;margin-top:-40rpx'
                                            : pageDataObj.single_user_rank_info_all_time.rank < 4
                                            ? 'color: #ffffff;'
                                            : 'color: #333333;'
                                    "
                                >
                                    {{ pageDataObj.single_user_rank_info_all_time.rank == -1 ? '未上榜' : pageDataObj.single_user_rank_info_all_time.rank }}
                                </view>
                                <view :class="true ? 'act_rank_list_item_head' : 'act_rank_list_item_head'">
                                    <image
                                        @tap="gotoDressRoom"
                                        class="act_rank_list_item_head_image"
                                        :data-userid="pageDataObj.single_user_rank_info_all_time.userinfo._id"
                                        :src="pageDataObj.single_user_rank_info_all_time.userinfo.image.middle_pic"
                                        v-if="true"
                                    ></image>
                                    <image
                                        class="act_rank_list_item_head_image"
                                        :src="
                                            pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                                : pageDataObj.single_user_rank_info_all_time.starball.logo_url
                                        "
                                        v-else
                                    ></image>
                                </view>
                                <view class="act_rank_list_item_column" style="flex: 1">
                                    <view class="act_rank_list_item_name">
                                        {{
                                            true
                                                ? pageDataObj.single_user_rank_info_all_time.userinfo.nickname
                                                : pageDataObj.single_user_rank_info_all_time.starinfo
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.name
                                                : pageDataObj.single_user_rank_info_all_time.starball.name
                                        }}
                                    </view>
                                    <view class="act_rank_list_item_soce">{{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}</view>
                                </view>
                                <view class="act_rank_list_item_column" style="align-items: center; margin-right: 22rpx">
                                    <view class="act_rank_list_item_soce" v-if="true">
                                        <image class="act_rank_list_item_head" :src="pageDataObj.single_user_rank_info_all_time.max_starinfo.logo_img"></image>
                                    </view>
                                    <view class="act_rank_list_item_soce" v-else-if="showScore">{{ pageDataObj.single_user_rank_info_all_time.score }}积分</view>
                                    <block v-else>
                                        <view
                                            @tap="sendLeaderBoardClick"
                                            class="act_rank_list_item_tuan"
                                            :data-item="pageDataObj.single_user_rank_info_all_time"
                                            :data-zoneinfo="pageDataObj.bp_time_arr[pageDataObj.phase]"
                                            v-if="true"
                                        ></view>
                                    </block>
                                </view>
                            </view>

                            <view class="star_buff_box" v-if="pageDataObj.single_user_rank_info_all_time.buff_list">
                                <block v-if="pageDataObj.single_user_rank_info_all_time.buff_list.length > 0">
                                    <view class="star_buff_inner_box" @tap="getStarBuffTeamList" :data-starid="pageDataObj.single_user_rank_info_all_time.starid">
                                        <image
                                            style="width: 177rpx; height: 24rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/7075d915e89af609564323cfbf20e8cc.png"
                                        ></image>
                                        <view class="star_buff_item" v-for="(b_item,0) in (pageDataObj.single_user_rank_info_all_time.buff_list)" :key="0">{{ b_item }}</view>
                                    </view>
                                    <view
                                        class="star_buff_button"
                                        :style="pageDataObj.single_user_rank_info_all_time.buff_list.length > 4 ? 'height:72rpx' : ''"
                                        @tap="getStarBuffTeamList"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                    >
                                        <image
                                            style="width: 27rpx; height: 51rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/2aaedafaef55928f5d223f71aa120999.png"
                                        ></image>
                                    </view>
                                </block>
                                <block v-else-if="">
                                    <image
                                        style="width: 100%; height: 55rpx"
                                        @tap="gotoTreeRule"
                                        data-type="2"
                                        src="https://star-img.xingxiaoculture.com/2023/05/15/c1360019d293eaaa2ccbcfc9754250d9.png"
                                    ></image>
                                </block>
                            </view>

                            <view class="flex_row flex_center" style="flex-wrap: wrap; width: 100%; margin-top: 20rpx" v-if="pageDataObj.single_user_rank_info_all_time.card_list">
                                <text style="font-size: 11px; color: #333333; font-weight: 600; width: 150rpx">
                                    获得卡片
                                    <text style="color: #ff5d26">{{ pageDataObj.single_user_rank_info_all_time.card_num }}</text>
                                    /8
                                </text>
                                <text style="font-size: 10px; color: #333333; width: 484rpx; text-align: right">
                                    每张获得的卡片已为星球增加
                                    <text style="color: #ff5d26">200万</text>
                                    {{ tool_b_name }}
                                </text>
                                <view class="fengzhen_item" style="width: 148rpx" v-for="(c_item,0) in (pageDataObj.single_user_rank_info_all_time.card_list)" :key="0">
                                    <image class="fengzhen_item_image" style="width: 148rpx; height: 190rpx" :src="c_item.card_img"></image>

                                    <view
                                        v-if="!c_item.userinfo"
                                        style="z-index: 1; width: 148rpx; height: 190rpx; background-color: rgba(0, 0, 0, 0.6); margin-top: -190rpx; border-radius: 10rpx"
                                    ></view>

                                    <view
                                        class="flex_column flex_center"
                                        v-if="c_item.userinfo"
                                        style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx; background: linear-gradient(180deg, #dbf2ff, #b3d4ff)"
                                        @tap="gotoDressRoom"
                                        :data-userid="c_item.userinfo._id"
                                    >
                                        <image class="fengzhen_item_user_image" :src="c_item.userinfo.image.thumbnail_pic"></image>
                                        <view class="fengzhen_item_star" style="margin-top: 0rpx; color: #333333">{{ c_item.userinfo.nickname }}</view>
                                    </view>

                                    <view class="flex_column flex_center" v-else-if="" style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx">
                                        <view
                                            class="fengzhen_item_ding"
                                            @tap="tabItemClick"
                                            data-index="1"
                                            style="font-size: 9px"
                                            :data-item="pageDataObj.single_user_rank_info_all_time"
                                        >
                                            帮星球得卡片
                                        </view>
                                    </view>
                                </view>
                            </view>

                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; margin-top: 20rpx; width: 632rpx; border-radius: 20rpx; padding: 8rpx; background-color: #77b3f2"
                                v-if="pageDataObj.single_user_rank_info_all_time.top3group"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的公会</view>
                                    <view
                                        @tap="gotoGonghuiList"
                                        class="act_rank_list_more"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                        style="margin: 0; text-align: right; color: #ffffff"
                                    >
                                        查看更多>
                                    </view>
                                </view>
                                <view
                                    class="flex_row flex_center"
                                    style="width: 622rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 10rpx; margin-top: 10rpx"
                                    v-for="(g_item,0) in (pageDataObj.single_user_rank_info_all_time.top3group)"
                                    :key="0"
                                >
                                    <view style="width: 38%; height: 64rpx" v-if="g_item.top3user">
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="gu_item.userinfo._id"
                                            :src="gu_item.userinfo.image.thumbnail_pic"
                                            style="width: 64rpx; height: 64rpx; border-radius: 50%; margin: 2rpx"
                                            v-for="(gu_item, index1) in g_item.top3user"
                                            :key="index1"
                                        ></image>
                                    </view>

                                    <view class="act_rank_list_item_soce" style="width: 30%; color: #5f758d">{{ g_item.group_name }}</view>

                                    <view class="act_rank_list_item_soce" style="width: 25%; text-align: right; color: #5f758d">{{ g_item.votetimes }}</view>
                                </view>
                                <view style="height: 25rpx"></view>
                            </view>
                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; width: 632rpx; border-radius: 20rpx; padding: 8rpx; margin-top: 20rpx; background-color: #77b3f2"
                                v-if="pageDataObj.single_user_rank_info_all_time.top3user"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的用户</view>
                                    <view
                                        @tap="gotoPersonList"
                                        class="act_rank_list_more"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                        style="margin: 0; text-align: right; color: #ffffff"
                                    >
                                        查看更多>
                                    </view>
                                </view>
                                <view class="flex_row flex_center">
                                    <view
                                        class="flex_column flex_center"
                                        style="width: 186rpx; height: 146rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 20rpx; margin: 12rpx"
                                        v-for="(u_item,0) in (pageDataObj.single_user_rank_info_all_time.top3user)"
                                        :key="0"
                                    >
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="u_item.userinfo._id"
                                            :src="u_item.userinfo.image.thumbnail_pic"
                                            style="width: 70rpx; height: 70rpx; border-radius: 50%; margin: 2rpx"
                                        ></image>

                                        <view class="act_rank_list_item_soce" style="width: 150rpx; overflow: hidden; white-space: nowrap; text-align: center; color: #5e758f">
                                            {{ u_item.userinfo.nickname }}
                                        </view>

                                        <view class="act_rank_list_item_soce" style="color: #5e758f">{{ u_item.votetimes }}</view>
                                    </view>
                                </view>
                            </view>
                            <text class="act_rank_list_item_text" v-if="pageDataObj.single_user_rank_info_all_time.text">
                                {{ pageDataObj.single_user_rank_info_all_time.text }}
                            </text>
                            <view style="height: 25rpx" v-else></view>
                        </view>
                    </block>
                    <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                        <view class="group_pk_prizes_list_line"></view>
                        <view class="group_pk_prizes_list_text2">个人总排名</view>
                    </view>
                    <!-- parse <template is="rank_list_item_view" :data="item:item,index:0,showStar:true,tool_b_unit:tool_b_unit" v-for="(item,index) in (pageDataObj.user_rank_all_time)" :key="index"></template> -->
                    <block name="rank_list_item_view" v-if="false" v-for="(item, index) in pageDataObj.user_rank_all_time" :key="index">
                        <view class="act_rank_list_item" :style="showLine && 0 == 0 ? 'margin-top: 0rpx;' : ''">
                            <view class="act_rank_list_item_line" v-if="showLine && 0 != 0"></view>
                            <view class="act_rank_list_item_msg">
                                <view
                                    :class="
                                        'act_rank_list_item_rank ' +
                                        (pageDataObj.single_user_rank_info_all_time.rank == 1
                                            ? 'act_rank_list_item_rank_one'
                                            : pageDataObj.single_user_rank_info_all_time.rank == 2
                                            ? 'act_rank_list_item_rank_two'
                                            : pageDataObj.single_user_rank_info_all_time.rank == 3
                                            ? 'act_rank_list_item_rank_stree'
                                            : '')
                                    "
                                    :style="
                                        pageDataObj.single_user_rank_info_all_time.rank == -1 || pageDataObj.single_user_rank_info_all_time.rank == '未上榜'
                                            ? 'color: #333333;line-height:30rpx;margin-top:-40rpx'
                                            : pageDataObj.single_user_rank_info_all_time.rank < 4
                                            ? 'color: #ffffff;'
                                            : 'color: #333333;'
                                    "
                                >
                                    {{ pageDataObj.single_user_rank_info_all_time.rank == -1 ? '未上榜' : pageDataObj.single_user_rank_info_all_time.rank }}
                                </view>
                                <view :class="true ? 'act_rank_list_item_head' : 'act_rank_list_item_head'">
                                    <image
                                        @tap="gotoDressRoom"
                                        class="act_rank_list_item_head_image"
                                        :data-userid="pageDataObj.single_user_rank_info_all_time.userinfo._id"
                                        :src="pageDataObj.single_user_rank_info_all_time.userinfo.image.middle_pic"
                                        v-if="true"
                                    ></image>
                                    <image
                                        class="act_rank_list_item_head_image"
                                        :src="
                                            pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                                : pageDataObj.single_user_rank_info_all_time.starball.logo_url
                                        "
                                        v-else
                                    ></image>
                                </view>
                                <view class="act_rank_list_item_column" style="flex: 1">
                                    <view class="act_rank_list_item_name">
                                        {{
                                            true
                                                ? pageDataObj.single_user_rank_info_all_time.userinfo.nickname
                                                : pageDataObj.single_user_rank_info_all_time.starinfo
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.name
                                                : pageDataObj.single_user_rank_info_all_time.starball.name
                                        }}
                                    </view>
                                    <view class="act_rank_list_item_soce">{{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}</view>
                                </view>
                                <view class="act_rank_list_item_column" style="align-items: center; margin-right: 22rpx">
                                    <view class="act_rank_list_item_soce" v-if="true">
                                        <image class="act_rank_list_item_head" :src="pageDataObj.single_user_rank_info_all_time.max_starinfo.logo_img"></image>
                                    </view>
                                    <view class="act_rank_list_item_soce" v-else-if="showScore">{{ pageDataObj.single_user_rank_info_all_time.score }}积分</view>
                                    <block v-else>
                                        <view
                                            @tap="sendLeaderBoardClick"
                                            class="act_rank_list_item_tuan"
                                            :data-item="pageDataObj.single_user_rank_info_all_time"
                                            :data-zoneinfo="pageDataObj.bp_time_arr[pageDataObj.phase]"
                                            v-if="true"
                                        ></view>
                                    </block>
                                </view>
                            </view>

                            <view class="star_buff_box" v-if="pageDataObj.single_user_rank_info_all_time.buff_list">
                                <block v-if="pageDataObj.single_user_rank_info_all_time.buff_list.length > 0">
                                    <view class="star_buff_inner_box" @tap="getStarBuffTeamList" :data-starid="pageDataObj.single_user_rank_info_all_time.starid">
                                        <image
                                            style="width: 177rpx; height: 24rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/7075d915e89af609564323cfbf20e8cc.png"
                                        ></image>
                                        <view class="star_buff_item" v-for="(b_item,0) in (pageDataObj.single_user_rank_info_all_time.buff_list)" :key="0">{{ b_item }}</view>
                                    </view>
                                    <view
                                        class="star_buff_button"
                                        :style="pageDataObj.single_user_rank_info_all_time.buff_list.length > 4 ? 'height:72rpx' : ''"
                                        @tap="getStarBuffTeamList"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                    >
                                        <image
                                            style="width: 27rpx; height: 51rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/2aaedafaef55928f5d223f71aa120999.png"
                                        ></image>
                                    </view>
                                </block>
                                <block v-else-if="">
                                    <image
                                        style="width: 100%; height: 55rpx"
                                        @tap="gotoTreeRule"
                                        data-type="2"
                                        src="https://star-img.xingxiaoculture.com/2023/05/15/c1360019d293eaaa2ccbcfc9754250d9.png"
                                    ></image>
                                </block>
                            </view>

                            <view class="flex_row flex_center" style="flex-wrap: wrap; width: 100%; margin-top: 20rpx" v-if="pageDataObj.single_user_rank_info_all_time.card_list">
                                <text style="font-size: 11px; color: #333333; font-weight: 600; width: 150rpx">
                                    获得卡片
                                    <text style="color: #ff5d26">{{ pageDataObj.single_user_rank_info_all_time.card_num }}</text>
                                    /8
                                </text>
                                <text style="font-size: 10px; color: #333333; width: 484rpx; text-align: right">
                                    每张获得的卡片已为星球增加
                                    <text style="color: #ff5d26">200万</text>
                                    {{ tool_b_name }}
                                </text>
                                <view class="fengzhen_item" style="width: 148rpx" v-for="(c_item,0) in (pageDataObj.single_user_rank_info_all_time.card_list)" :key="0">
                                    <image class="fengzhen_item_image" style="width: 148rpx; height: 190rpx" :src="c_item.card_img"></image>

                                    <view
                                        v-if="!c_item.userinfo"
                                        style="z-index: 1; width: 148rpx; height: 190rpx; background-color: rgba(0, 0, 0, 0.6); margin-top: -190rpx; border-radius: 10rpx"
                                    ></view>

                                    <view
                                        class="flex_column flex_center"
                                        v-if="c_item.userinfo"
                                        style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx; background: linear-gradient(180deg, #dbf2ff, #b3d4ff)"
                                        @tap="gotoDressRoom"
                                        :data-userid="c_item.userinfo._id"
                                    >
                                        <image class="fengzhen_item_user_image" :src="c_item.userinfo.image.thumbnail_pic"></image>
                                        <view class="fengzhen_item_star" style="margin-top: 0rpx; color: #333333">{{ c_item.userinfo.nickname }}</view>
                                    </view>

                                    <view class="flex_column flex_center" v-else-if="" style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx">
                                        <view
                                            class="fengzhen_item_ding"
                                            @tap="tabItemClick"
                                            data-index="1"
                                            style="font-size: 9px"
                                            :data-item="pageDataObj.single_user_rank_info_all_time"
                                        >
                                            帮星球得卡片
                                        </view>
                                    </view>
                                </view>
                            </view>

                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; margin-top: 20rpx; width: 632rpx; border-radius: 20rpx; padding: 8rpx; background-color: #77b3f2"
                                v-if="pageDataObj.single_user_rank_info_all_time.top3group"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的公会</view>
                                    <view
                                        @tap="gotoGonghuiList"
                                        class="act_rank_list_more"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                        style="margin: 0; text-align: right; color: #ffffff"
                                    >
                                        查看更多>
                                    </view>
                                </view>
                                <view
                                    class="flex_row flex_center"
                                    style="width: 622rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 10rpx; margin-top: 10rpx"
                                    v-for="(g_item,0) in (pageDataObj.single_user_rank_info_all_time.top3group)"
                                    :key="0"
                                >
                                    <view style="width: 38%; height: 64rpx" v-if="g_item.top3user">
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="gu_item.userinfo._id"
                                            :src="gu_item.userinfo.image.thumbnail_pic"
                                            style="width: 64rpx; height: 64rpx; border-radius: 50%; margin: 2rpx"
                                            v-for="(gu_item, index1) in g_item.top3user"
                                            :key="index1"
                                        ></image>
                                    </view>

                                    <view class="act_rank_list_item_soce" style="width: 30%; color: #5f758d">{{ g_item.group_name }}</view>

                                    <view class="act_rank_list_item_soce" style="width: 25%; text-align: right; color: #5f758d">{{ g_item.votetimes }}</view>
                                </view>
                                <view style="height: 25rpx"></view>
                            </view>
                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; width: 632rpx; border-radius: 20rpx; padding: 8rpx; margin-top: 20rpx; background-color: #77b3f2"
                                v-if="pageDataObj.single_user_rank_info_all_time.top3user"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的用户</view>
                                    <view
                                        @tap="gotoPersonList"
                                        class="act_rank_list_more"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                        style="margin: 0; text-align: right; color: #ffffff"
                                    >
                                        查看更多>
                                    </view>
                                </view>
                                <view class="flex_row flex_center">
                                    <view
                                        class="flex_column flex_center"
                                        style="width: 186rpx; height: 146rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 20rpx; margin: 12rpx"
                                        v-for="(u_item,0) in (pageDataObj.single_user_rank_info_all_time.top3user)"
                                        :key="0"
                                    >
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="u_item.userinfo._id"
                                            :src="u_item.userinfo.image.thumbnail_pic"
                                            style="width: 70rpx; height: 70rpx; border-radius: 50%; margin: 2rpx"
                                        ></image>

                                        <view class="act_rank_list_item_soce" style="width: 150rpx; overflow: hidden; white-space: nowrap; text-align: center; color: #5e758f">
                                            {{ u_item.userinfo.nickname }}
                                        </view>

                                        <view class="act_rank_list_item_soce" style="color: #5e758f">{{ u_item.votetimes }}</view>
                                    </view>
                                </view>
                            </view>
                            <text class="act_rank_list_item_text" v-if="pageDataObj.single_user_rank_info_all_time.text">
                                {{ pageDataObj.single_user_rank_info_all_time.text }}
                            </text>
                            <view style="height: 25rpx" v-else></view>
                        </view>
                    </block>
                    <view @tap="lookMoreClick" class="act_rank_list_more" :data-type="2" v-if="pageDataObj.user_rank_all_time.length >= 5">查看更多></view>
                    <!-- parse <template is="rank_list_empty_view" :data="text:'暂无排名信息，请等待数据更新~'" v-if="(!pageDataObj.user_rank_all_time||pageDataObj.user_rank_all_time.length==0)"></template> -->
                    <block name="rank_list_empty_view" v-if="false" v-if="!pageDataObj.user_rank_all_time || pageDataObj.user_rank_all_time.length == 0">
                        <view class="act_rank_list_empty">
                            <image class="act_rank_list_empty_icon" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/12/29/rjmDKGfwX31609236901495.png"></image>
                            <view class="act_rank_list_empty_text">{{ '暂无排名信息，快去为TA送' + tool_b_name + '吧！' }}</view>
                        </view>
                    </block>
                    <image class="act_tb_head_image" src=""></image>
                </view>
                <!-- 星球下个人榜 -->
                <view class="act_rank_list_view tabPage6">
                    <image class="act_tb_head_image" src=""></image>
                    <image class="act_rank_list_image" src="https://star-img.xingxiaoculture.com/2023/05/16/33003b386af031d6580da7407310b403.png"></image>
                    <text class="act_rank_list_tip4">根据个人 05.20 11：'00' ~ 05.31 22：'00' 累计为\n星球赠送的{{ tool_b_name }}数量排名，数据每3分钟更新</text>
                    <view style="height: 39rpx; display: flex; align-items: center; justify-content: center; position: relative; margin-top: 20rpx">
                        <text style="font-size: 38rpx; color: #f58609; font-weight: 900">{{ userRankInStar.starball.name }}</text>
                        <image
                            style="width: 359rpx; height: 27rpx; position: absolute; top: 10rpx"
                            src="https://star-img.xingxiaoculture.com/2023/05/17/a8cbd58e8866c8f3a467eb95d9ab1c19.png"
                        ></image>
                        <view
                            style="
                                width: 102rpx;
                                height: 46rpx;
                                background-color: #f8a409;
                                border-radius: 14rpx;
                                text-align: center;
                                position: absolute;
                                color: #ffffff;
                                right: -250rpx;
                            "
                            @tap="showDialogClick"
                            data-select="13"
                        >
                            切换
                        </view>
                    </view>
                    <view style="font-size: 10px; margin-top: 10rpx">
                        个人贡献前
                        <text style="color: #ff7b1e">30</text>
                        名奖励
                    </view>
                    <image
                        style="width: 636rpx; height: 650rpx; border-radius: 22rpx; margin-top: 20rpx"
                        :src="
                            userRankInStarImg[userRankInStarSelectStarid ? userRankInStarSelectStarid : pageDataObj.user_select_starid]
                                ? userRankInStarImg[userRankInStarSelectStarid ? userRankInStarSelectStarid : pageDataObj.user_select_starid]
                                : userRankInStarImg[0]
                        "
                    ></image>
                    <view style="font-size: 10px; margin-top: 10rpx; width: 90%">
                        上个月大型活动，前四名的星球（啵啵星球、小笼包星球、小飞侠星球、嘉人星 球）为实体定制周边礼包奖励（不包邮)，其他星球为平台虚拟道具礼包奖励。
                    </view>
                    <image v-if="userRankInStar.user_rank_banner" @tap="activityrule" class="act_rank_list_banner3" :data-type="7" :src="userRankInStar.user_rank_banner"></image>
                    <view
                        class="group_pk_prizes_list2"
                        style="margin: 30rpx auto"
                        v-if="is_login && pageDataObj.single_user_rank_info_all_time.max_starid != 0 && userRankInStar.user"
                    >
                        <view class="group_pk_prizes_list_line"></view>
                        <view class="group_pk_prizes_list_text2">我的个人排名</view>
                    </view>
                    <!-- parse <template is="rank_list_item_view" :data="item:userRankInStar.user,index:0,showStar:true,tool_b_unit:tool_b_unit" v-if="is_login&&pageDataObj.single_user_rank_info_all_time.max_starid!=0&&userRankInStar.list.length>0"></template> -->
                    <block name="rank_list_item_view" v-if="false" v-if="is_login && pageDataObj.single_user_rank_info_all_time.max_starid != 0 && userRankInStar.list.length > 0">
                        <view class="act_rank_list_item" :style="showLine && 0 == 0 ? 'margin-top: 0rpx;' : ''">
                            <view class="act_rank_list_item_line" v-if="showLine && 0 != 0"></view>
                            <view class="act_rank_list_item_msg">
                                <view
                                    :class="
                                        'act_rank_list_item_rank ' +
                                        (pageDataObj.single_user_rank_info_all_time.rank == 1
                                            ? 'act_rank_list_item_rank_one'
                                            : pageDataObj.single_user_rank_info_all_time.rank == 2
                                            ? 'act_rank_list_item_rank_two'
                                            : pageDataObj.single_user_rank_info_all_time.rank == 3
                                            ? 'act_rank_list_item_rank_stree'
                                            : '')
                                    "
                                    :style="
                                        pageDataObj.single_user_rank_info_all_time.rank == -1 || pageDataObj.single_user_rank_info_all_time.rank == '未上榜'
                                            ? 'color: #333333;line-height:30rpx;margin-top:-40rpx'
                                            : pageDataObj.single_user_rank_info_all_time.rank < 4
                                            ? 'color: #ffffff;'
                                            : 'color: #333333;'
                                    "
                                >
                                    {{ pageDataObj.single_user_rank_info_all_time.rank == -1 ? '未上榜' : pageDataObj.single_user_rank_info_all_time.rank }}
                                </view>
                                <view :class="true ? 'act_rank_list_item_head' : 'act_rank_list_item_head'">
                                    <image
                                        @tap="gotoDressRoom"
                                        class="act_rank_list_item_head_image"
                                        :data-userid="pageDataObj.single_user_rank_info_all_time.userinfo._id"
                                        :src="pageDataObj.single_user_rank_info_all_time.userinfo.image.middle_pic"
                                        v-if="true"
                                    ></image>
                                    <image
                                        class="act_rank_list_item_head_image"
                                        :src="
                                            pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                                : pageDataObj.single_user_rank_info_all_time.starball.logo_url
                                        "
                                        v-else
                                    ></image>
                                </view>
                                <view class="act_rank_list_item_column" style="flex: 1">
                                    <view class="act_rank_list_item_name">
                                        {{
                                            true
                                                ? pageDataObj.single_user_rank_info_all_time.userinfo.nickname
                                                : pageDataObj.single_user_rank_info_all_time.starinfo
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.name
                                                : pageDataObj.single_user_rank_info_all_time.starball.name
                                        }}
                                    </view>
                                    <view class="act_rank_list_item_soce">{{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}</view>
                                </view>
                                <view class="act_rank_list_item_column" style="align-items: center; margin-right: 22rpx">
                                    <view class="act_rank_list_item_soce" v-if="true">
                                        <image class="act_rank_list_item_head" :src="pageDataObj.single_user_rank_info_all_time.max_starinfo.logo_img"></image>
                                    </view>
                                    <view class="act_rank_list_item_soce" v-else-if="showScore">{{ pageDataObj.single_user_rank_info_all_time.score }}积分</view>
                                    <block v-else>
                                        <view
                                            @tap="sendLeaderBoardClick"
                                            class="act_rank_list_item_tuan"
                                            :data-item="pageDataObj.single_user_rank_info_all_time"
                                            :data-zoneinfo="pageDataObj.bp_time_arr[pageDataObj.phase]"
                                            v-if="true"
                                        ></view>
                                    </block>
                                </view>
                            </view>

                            <view class="star_buff_box" v-if="pageDataObj.single_user_rank_info_all_time.buff_list">
                                <block v-if="pageDataObj.single_user_rank_info_all_time.buff_list.length > 0">
                                    <view class="star_buff_inner_box" @tap="getStarBuffTeamList" :data-starid="pageDataObj.single_user_rank_info_all_time.starid">
                                        <image
                                            style="width: 177rpx; height: 24rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/7075d915e89af609564323cfbf20e8cc.png"
                                        ></image>
                                        <view class="star_buff_item" v-for="(b_item,0) in (pageDataObj.single_user_rank_info_all_time.buff_list)" :key="0">{{ b_item }}</view>
                                    </view>
                                    <view
                                        class="star_buff_button"
                                        :style="pageDataObj.single_user_rank_info_all_time.buff_list.length > 4 ? 'height:72rpx' : ''"
                                        @tap="getStarBuffTeamList"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                    >
                                        <image
                                            style="width: 27rpx; height: 51rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/2aaedafaef55928f5d223f71aa120999.png"
                                        ></image>
                                    </view>
                                </block>
                                <block v-else-if="">
                                    <image
                                        style="width: 100%; height: 55rpx"
                                        @tap="gotoTreeRule"
                                        data-type="2"
                                        src="https://star-img.xingxiaoculture.com/2023/05/15/c1360019d293eaaa2ccbcfc9754250d9.png"
                                    ></image>
                                </block>
                            </view>

                            <view class="flex_row flex_center" style="flex-wrap: wrap; width: 100%; margin-top: 20rpx" v-if="pageDataObj.single_user_rank_info_all_time.card_list">
                                <text style="font-size: 11px; color: #333333; font-weight: 600; width: 150rpx">
                                    获得卡片
                                    <text style="color: #ff5d26">{{ pageDataObj.single_user_rank_info_all_time.card_num }}</text>
                                    /8
                                </text>
                                <text style="font-size: 10px; color: #333333; width: 484rpx; text-align: right">
                                    每张获得的卡片已为星球增加
                                    <text style="color: #ff5d26">200万</text>
                                    {{ tool_b_name }}
                                </text>
                                <view class="fengzhen_item" style="width: 148rpx" v-for="(c_item,0) in (pageDataObj.single_user_rank_info_all_time.card_list)" :key="0">
                                    <image class="fengzhen_item_image" style="width: 148rpx; height: 190rpx" :src="c_item.card_img"></image>

                                    <view
                                        v-if="!c_item.userinfo"
                                        style="z-index: 1; width: 148rpx; height: 190rpx; background-color: rgba(0, 0, 0, 0.6); margin-top: -190rpx; border-radius: 10rpx"
                                    ></view>

                                    <view
                                        class="flex_column flex_center"
                                        v-if="c_item.userinfo"
                                        style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx; background: linear-gradient(180deg, #dbf2ff, #b3d4ff)"
                                        @tap="gotoDressRoom"
                                        :data-userid="c_item.userinfo._id"
                                    >
                                        <image class="fengzhen_item_user_image" :src="c_item.userinfo.image.thumbnail_pic"></image>
                                        <view class="fengzhen_item_star" style="margin-top: 0rpx; color: #333333">{{ c_item.userinfo.nickname }}</view>
                                    </view>

                                    <view class="flex_column flex_center" v-else-if="" style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx">
                                        <view
                                            class="fengzhen_item_ding"
                                            @tap="tabItemClick"
                                            data-index="1"
                                            style="font-size: 9px"
                                            :data-item="pageDataObj.single_user_rank_info_all_time"
                                        >
                                            帮星球得卡片
                                        </view>
                                    </view>
                                </view>
                            </view>

                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; margin-top: 20rpx; width: 632rpx; border-radius: 20rpx; padding: 8rpx; background-color: #77b3f2"
                                v-if="pageDataObj.single_user_rank_info_all_time.top3group"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的公会</view>
                                    <view
                                        @tap="gotoGonghuiList"
                                        class="act_rank_list_more"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                        style="margin: 0; text-align: right; color: #ffffff"
                                    >
                                        查看更多>
                                    </view>
                                </view>
                                <view
                                    class="flex_row flex_center"
                                    style="width: 622rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 10rpx; margin-top: 10rpx"
                                    v-for="(g_item,0) in (pageDataObj.single_user_rank_info_all_time.top3group)"
                                    :key="0"
                                >
                                    <view style="width: 38%; height: 64rpx" v-if="g_item.top3user">
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="gu_item.userinfo._id"
                                            :src="gu_item.userinfo.image.thumbnail_pic"
                                            style="width: 64rpx; height: 64rpx; border-radius: 50%; margin: 2rpx"
                                            v-for="(gu_item, index1) in g_item.top3user"
                                            :key="index1"
                                        ></image>
                                    </view>

                                    <view class="act_rank_list_item_soce" style="width: 30%; color: #5f758d">{{ g_item.group_name }}</view>

                                    <view class="act_rank_list_item_soce" style="width: 25%; text-align: right; color: #5f758d">{{ g_item.votetimes }}</view>
                                </view>
                                <view style="height: 25rpx"></view>
                            </view>
                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; width: 632rpx; border-radius: 20rpx; padding: 8rpx; margin-top: 20rpx; background-color: #77b3f2"
                                v-if="pageDataObj.single_user_rank_info_all_time.top3user"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的用户</view>
                                    <view
                                        @tap="gotoPersonList"
                                        class="act_rank_list_more"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                        style="margin: 0; text-align: right; color: #ffffff"
                                    >
                                        查看更多>
                                    </view>
                                </view>
                                <view class="flex_row flex_center">
                                    <view
                                        class="flex_column flex_center"
                                        style="width: 186rpx; height: 146rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 20rpx; margin: 12rpx"
                                        v-for="(u_item,0) in (pageDataObj.single_user_rank_info_all_time.top3user)"
                                        :key="0"
                                    >
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="u_item.userinfo._id"
                                            :src="u_item.userinfo.image.thumbnail_pic"
                                            style="width: 70rpx; height: 70rpx; border-radius: 50%; margin: 2rpx"
                                        ></image>

                                        <view class="act_rank_list_item_soce" style="width: 150rpx; overflow: hidden; white-space: nowrap; text-align: center; color: #5e758f">
                                            {{ u_item.userinfo.nickname }}
                                        </view>

                                        <view class="act_rank_list_item_soce" style="color: #5e758f">{{ u_item.votetimes }}</view>
                                    </view>
                                </view>
                            </view>
                            <text class="act_rank_list_item_text" v-if="pageDataObj.single_user_rank_info_all_time.text">
                                {{ pageDataObj.single_user_rank_info_all_time.text }}
                            </text>
                            <view style="height: 25rpx" v-else></view>
                        </view>
                    </block>
                    <view class="group_pk_prizes_list2" style="margin: 30rpx auto">
                        <view class="group_pk_prizes_list_line"></view>
                        <view class="group_pk_prizes_list_text2">个人星球下总排名</view>
                    </view>
                    <!-- parse <template is="rank_list_item_view" :data="item:item,index:0,showStar:true,tool_b_unit:tool_b_unit" v-if="index < 10" v-for="(item,index) in (userRankInStar.list)" :key="index"></template> -->
                    <block name="rank_list_item_view" v-if="false" v-if="index < 10" v-for="(item, index) in userRankInStar.list" :key="index">
                        <view class="act_rank_list_item" :style="showLine && 0 == 0 ? 'margin-top: 0rpx;' : ''">
                            <view class="act_rank_list_item_line" v-if="showLine && 0 != 0"></view>
                            <view class="act_rank_list_item_msg">
                                <view
                                    :class="
                                        'act_rank_list_item_rank ' +
                                        (pageDataObj.single_user_rank_info_all_time.rank == 1
                                            ? 'act_rank_list_item_rank_one'
                                            : pageDataObj.single_user_rank_info_all_time.rank == 2
                                            ? 'act_rank_list_item_rank_two'
                                            : pageDataObj.single_user_rank_info_all_time.rank == 3
                                            ? 'act_rank_list_item_rank_stree'
                                            : '')
                                    "
                                    :style="
                                        pageDataObj.single_user_rank_info_all_time.rank == -1 || pageDataObj.single_user_rank_info_all_time.rank == '未上榜'
                                            ? 'color: #333333;line-height:30rpx;margin-top:-40rpx'
                                            : pageDataObj.single_user_rank_info_all_time.rank < 4
                                            ? 'color: #ffffff;'
                                            : 'color: #333333;'
                                    "
                                >
                                    {{ pageDataObj.single_user_rank_info_all_time.rank == -1 ? '未上榜' : pageDataObj.single_user_rank_info_all_time.rank }}
                                </view>
                                <view :class="true ? 'act_rank_list_item_head' : 'act_rank_list_item_head'">
                                    <image
                                        @tap="gotoDressRoom"
                                        class="act_rank_list_item_head_image"
                                        :data-userid="pageDataObj.single_user_rank_info_all_time.userinfo._id"
                                        :src="pageDataObj.single_user_rank_info_all_time.userinfo.image.middle_pic"
                                        v-if="true"
                                    ></image>
                                    <image
                                        class="act_rank_list_item_head_image"
                                        :src="
                                            pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.logo_img
                                                : pageDataObj.single_user_rank_info_all_time.starball.logo_url
                                        "
                                        v-else
                                    ></image>
                                </view>
                                <view class="act_rank_list_item_column" style="flex: 1">
                                    <view class="act_rank_list_item_name">
                                        {{
                                            true
                                                ? pageDataObj.single_user_rank_info_all_time.userinfo.nickname
                                                : pageDataObj.single_user_rank_info_all_time.starinfo
                                                ? pageDataObj.single_user_rank_info_all_time.starinfo.name
                                                : pageDataObj.single_user_rank_info_all_time.starball.name
                                        }}
                                    </view>
                                    <view class="act_rank_list_item_soce">{{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}</view>
                                </view>
                                <view class="act_rank_list_item_column" style="align-items: center; margin-right: 22rpx">
                                    <view class="act_rank_list_item_soce" v-if="true">
                                        <image class="act_rank_list_item_head" :src="pageDataObj.single_user_rank_info_all_time.max_starinfo.logo_img"></image>
                                    </view>
                                    <view class="act_rank_list_item_soce" v-else-if="showScore">{{ pageDataObj.single_user_rank_info_all_time.score }}积分</view>
                                    <block v-else>
                                        <view
                                            @tap="sendLeaderBoardClick"
                                            class="act_rank_list_item_tuan"
                                            :data-item="pageDataObj.single_user_rank_info_all_time"
                                            :data-zoneinfo="pageDataObj.bp_time_arr[pageDataObj.phase]"
                                            v-if="true"
                                        ></view>
                                    </block>
                                </view>
                            </view>

                            <view class="star_buff_box" v-if="pageDataObj.single_user_rank_info_all_time.buff_list">
                                <block v-if="pageDataObj.single_user_rank_info_all_time.buff_list.length > 0">
                                    <view class="star_buff_inner_box" @tap="getStarBuffTeamList" :data-starid="pageDataObj.single_user_rank_info_all_time.starid">
                                        <image
                                            style="width: 177rpx; height: 24rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/7075d915e89af609564323cfbf20e8cc.png"
                                        ></image>
                                        <view class="star_buff_item" v-for="(b_item,0) in (pageDataObj.single_user_rank_info_all_time.buff_list)" :key="0">{{ b_item }}</view>
                                    </view>
                                    <view
                                        class="star_buff_button"
                                        :style="pageDataObj.single_user_rank_info_all_time.buff_list.length > 4 ? 'height:72rpx' : ''"
                                        @tap="getStarBuffTeamList"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                    >
                                        <image
                                            style="width: 27rpx; height: 51rpx"
                                            src="https://star-img.xingxiaoculture.com/2023/03/16/2aaedafaef55928f5d223f71aa120999.png"
                                        ></image>
                                    </view>
                                </block>
                                <block v-else-if="">
                                    <image
                                        style="width: 100%; height: 55rpx"
                                        @tap="gotoTreeRule"
                                        data-type="2"
                                        src="https://star-img.xingxiaoculture.com/2023/05/15/c1360019d293eaaa2ccbcfc9754250d9.png"
                                    ></image>
                                </block>
                            </view>

                            <view class="flex_row flex_center" style="flex-wrap: wrap; width: 100%; margin-top: 20rpx" v-if="pageDataObj.single_user_rank_info_all_time.card_list">
                                <text style="font-size: 11px; color: #333333; font-weight: 600; width: 150rpx">
                                    获得卡片
                                    <text style="color: #ff5d26">{{ pageDataObj.single_user_rank_info_all_time.card_num }}</text>
                                    /8
                                </text>
                                <text style="font-size: 10px; color: #333333; width: 484rpx; text-align: right">
                                    每张获得的卡片已为星球增加
                                    <text style="color: #ff5d26">200万</text>
                                    {{ tool_b_name }}
                                </text>
                                <view class="fengzhen_item" style="width: 148rpx" v-for="(c_item,0) in (pageDataObj.single_user_rank_info_all_time.card_list)" :key="0">
                                    <image class="fengzhen_item_image" style="width: 148rpx; height: 190rpx" :src="c_item.card_img"></image>

                                    <view
                                        v-if="!c_item.userinfo"
                                        style="z-index: 1; width: 148rpx; height: 190rpx; background-color: rgba(0, 0, 0, 0.6); margin-top: -190rpx; border-radius: 10rpx"
                                    ></view>

                                    <view
                                        class="flex_column flex_center"
                                        v-if="c_item.userinfo"
                                        style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx; background: linear-gradient(180deg, #dbf2ff, #b3d4ff)"
                                        @tap="gotoDressRoom"
                                        :data-userid="c_item.userinfo._id"
                                    >
                                        <image class="fengzhen_item_user_image" :src="c_item.userinfo.image.thumbnail_pic"></image>
                                        <view class="fengzhen_item_star" style="margin-top: 0rpx; color: #333333">{{ c_item.userinfo.nickname }}</view>
                                    </view>

                                    <view class="flex_column flex_center" v-else-if="" style="margin-top: 10rpx; width: 148rpx; height: 64rpx; border-radius: 10rpx">
                                        <view
                                            class="fengzhen_item_ding"
                                            @tap="tabItemClick"
                                            data-index="1"
                                            style="font-size: 9px"
                                            :data-item="pageDataObj.single_user_rank_info_all_time"
                                        >
                                            帮星球得卡片
                                        </view>
                                    </view>
                                </view>
                            </view>

                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; margin-top: 20rpx; width: 632rpx; border-radius: 20rpx; padding: 8rpx; background-color: #77b3f2"
                                v-if="pageDataObj.single_user_rank_info_all_time.top3group"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的公会</view>
                                    <view
                                        @tap="gotoGonghuiList"
                                        class="act_rank_list_more"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                        style="margin: 0; text-align: right; color: #ffffff"
                                    >
                                        查看更多>
                                    </view>
                                </view>
                                <view
                                    class="flex_row flex_center"
                                    style="width: 622rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 10rpx; margin-top: 10rpx"
                                    v-for="(g_item,0) in (pageDataObj.single_user_rank_info_all_time.top3group)"
                                    :key="0"
                                >
                                    <view style="width: 38%; height: 64rpx" v-if="g_item.top3user">
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="gu_item.userinfo._id"
                                            :src="gu_item.userinfo.image.thumbnail_pic"
                                            style="width: 64rpx; height: 64rpx; border-radius: 50%; margin: 2rpx"
                                            v-for="(gu_item, index1) in g_item.top3user"
                                            :key="index1"
                                        ></image>
                                    </view>

                                    <view class="act_rank_list_item_soce" style="width: 30%; color: #5f758d">{{ g_item.group_name }}</view>

                                    <view class="act_rank_list_item_soce" style="width: 25%; text-align: right; color: #5f758d">{{ g_item.votetimes }}</view>
                                </view>
                                <view style="height: 25rpx"></view>
                            </view>
                            <view
                                class="act_rank_list_item_column"
                                style="margin-left: 0; width: 632rpx; border-radius: 20rpx; padding: 8rpx; margin-top: 20rpx; background-color: #77b3f2"
                                v-if="pageDataObj.single_user_rank_info_all_time.top3user"
                            >
                                <view class="flex_row flex_center" style="margin-top: 15rpx">
                                    <view class="act_rank_list_item_name" style="width: 320rpx; font-weight: 600; color: #ffffff">星球下排名前三的用户</view>
                                    <view
                                        @tap="gotoPersonList"
                                        class="act_rank_list_more"
                                        :data-starid="pageDataObj.single_user_rank_info_all_time.starid"
                                        style="margin: 0; text-align: right; color: #ffffff"
                                    >
                                        查看更多>
                                    </view>
                                </view>
                                <view class="flex_row flex_center">
                                    <view
                                        class="flex_column flex_center"
                                        style="width: 186rpx; height: 146rpx; padding: 10rpx 2rpx 10rpx 2rpx; background-color: #e4eef8; border-radius: 20rpx; margin: 12rpx"
                                        v-for="(u_item,0) in (pageDataObj.single_user_rank_info_all_time.top3user)"
                                        :key="0"
                                    >
                                        <image
                                            @tap="gotoDressRoom"
                                            :data-userid="u_item.userinfo._id"
                                            :src="u_item.userinfo.image.thumbnail_pic"
                                            style="width: 70rpx; height: 70rpx; border-radius: 50%; margin: 2rpx"
                                        ></image>

                                        <view class="act_rank_list_item_soce" style="width: 150rpx; overflow: hidden; white-space: nowrap; text-align: center; color: #5e758f">
                                            {{ u_item.userinfo.nickname }}
                                        </view>

                                        <view class="act_rank_list_item_soce" style="color: #5e758f">{{ u_item.votetimes }}</view>
                                    </view>
                                </view>
                            </view>
                            <text class="act_rank_list_item_text" v-if="pageDataObj.single_user_rank_info_all_time.text">
                                {{ pageDataObj.single_user_rank_info_all_time.text }}
                            </text>
                            <view style="height: 25rpx" v-else></view>
                        </view>
                    </block>
                    <view
                        @tap="lookMoreClick"
                        class="act_rank_list_more"
                        :data-type="2"
                        :data-starid="userRankInStarSelectStarid ? userRankInStarSelectStarid : pageDataObj.user_select_starid"
                        v-if="userRankInStar.list.length >= 5"
                    >
                        查看更多>
                    </view>
                    <!-- parse <template is="rank_list_empty_view" :data="text:'暂无排名信息，请等待数据更新~'" v-if="(!userRankInStar.list||userRankInStar.list.length==0)"></template> -->
                    <block name="rank_list_empty_view" v-if="false" v-if="!userRankInStar.list || userRankInStar.list.length == 0">
                        <view class="act_rank_list_empty">
                            <image class="act_rank_list_empty_icon" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/12/29/rjmDKGfwX31609236901495.png"></image>
                            <view class="act_rank_list_empty_text">{{ '暂无排名信息，快去为TA送' + tool_b_name + '吧！' }}</view>
                        </view>
                    </block>
                    <image class="act_tb_head_image" src=""></image>
                </view>
                <!-- 个人解锁 -->
                <view class="act_rank_list_view tabPage7" style="margin-bottom: 226rpx">
                    <image class="act_tb_head_image" src=""></image>
                    <image class="act_rank_list_image" src="https://star-img.xingxiaoculture.com/2023/05/15/99ec63e2b8b29aa86182fd6f5ffa65a5.png"></image>
                    <text class="act_rank_list_tip4">根据个人 05.20 11：'00' ~ 05.31 22：'00' 累计为 星球赠送的{{ tool_b_name }}数量解锁，数据每3分钟更新</text>
                    <text class="act_rank_list_tip4" style="font-size: 10px; background-color: #bdd7ff; width: 412rpx; line-height: 32rpx">
                        星球获得{{ tool_b_name }}加成后，解锁更容易
                    </text>
                    <view class="group_pk_prizes_guardian_value">
                        <view class="group_pk_prizes_guardian_value_text" style="margin-right: 10rpx">已赠送</view>
                        <view class="group_pk_prizes_guardian_value_count" v-for="(item, index) in pageDataObj.vote_times_array" :key="index">{{ item }}</view>
                        <view class="group_pk_prizes_guardian_value_text" style="margin-left: 10rpx">{{ tool_b_unit }}</view>
                    </view>
                    <view class="group_pk_prizes_tab_view"></view>
                    <view class="group_pk_prizes_list_item" v-for="(item, index) in pageDataObj.unblock_reward_arr" :key="index">
                        <view class="group_pk_prizes_list_text" :style="'color: ' + (item.status == 1 ? '#3974D2' : '#99B4E1') + ';text-align: right;width:160rpx'">
                            {{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}
                        </view>

                        <view class="group_pk_prizes_list_status">
                            <view
                                class="group_pk_prizes_list_status_circle1"
                                :style="item.status == 1 ? 'background: #ffffff;border:solid 3rpx #3571D5' : 'background: #99B4E1;'"
                            ></view>
                            <block v-if="index != pageDataObj.unblock_reward_arr.length - 1">
                                <view
                                    class="group_pk_prizes_list_status_circle2"
                                    :style="'background: ' + (item.status == 1 ? '#3571D5' : '#99B4E1') + ';'"
                                    v-for="(circleitem, circleindex) in [1, 1, 1, 1, 1, 1]"
                                    :key="circleindex"
                                ></view>
                            </block>
                        </view>

                        <view class="group_pk_prizes_list_text" :style="'color: ' + (item.status == 1 ? '#3974D2' : '#99B4E1') + ';width:360rpx'">{{ item.reward }}</view>
                    </view>
                    <text class="group_pk_prizes_tip_text">以上奖励可累计解锁且全部解锁后不可重复解锁 个人解锁奖励达成后10分钟内发放</text>
                    <image class="act_tb_head_image" src=""></image>
                </view>
                <view class="user_msg_view">
                    <view class="user_msg_view_box_upper">
                        <image
                            class="user_msg_head"
                            :src="is_login ? user_info.image.origin_pic : 'https://star-img.xingxiaoculture.com/search/topics/images/2021/1/26/8nYDdtxeHZ1611646272807.png'"
                        ></image>
                        <view class="user_msg_inner">
                            <view class="user_msg_text_nickname" style="font-weight: 700">{{ is_login ? user_info.nickname : '未登录' }}</view>
                        </view>
                        <view class="user_msg_text">{{ tool_s_name }}：{{ is_login ? goodwillNumLittle + '' : '- -' }}</view>
                        <view class="user_msg_text" style="margin-right: 10rpx">{{ tool_b_name }}：{{ is_login ? goodwillNum + '' : '- -' }}</view>
                    </view>
                    <view class="user_msg_view_box_upper">
                        <view @tap="showDialogClick" class="user_msg_btn" :data-select="6">获取{{ tool_s_name }}</view>
                        <view @tap="showDialogClick" class="user_msg_btn" :data-select="7">获取{{ tool_b_name }}</view>
                        <view
                            @tap="bottomViewClick"
                            class="user_msg_btn"
                            style="background-image: url(https://star-img.xingxiaoculture.com/2023/05/15/bb8a48639d9b5287e5eda69f3882f06f.png)"
                        >
                            {{ !is_login ? '去登录' : pageDataObj.now_time > pageDataObj.end_time ? '活动已结束' : '赠送' + tool_b_name }}
                        </view>
                    </view>
                </view>
            </view>
            <view class="dialog_view" v-if="isShowDialog">
                <view class="dialog_main_view" :style="(showDialogType == 3 ? 'background-color: #4C3289;' : '') + (showDialogType == 8 ? 'width: 670rpx;' : '')">
                    <view class="dialog_close" v-if="showDialogType != 5">
                        <image @tap="dialogCloseClick" class="dialog_close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"></image>
                    </view>
                    <view class="dialog_main_stop_data" v-if="showDialogType == 1">今日为休整日期，不可使用{{ tool_b_name }}～</view>
                    <view class="dialog_main_stop_data" style="margin: 76rpx 0rpx 56rpx; text-align: center" v-if="showDialogType == 2">活动已结束,感谢您的参与</view>
                    <scroll-view :scrollY="true" style="max-height: 800rpx; margin-top: 22rpx" v-if="showDialogType == 3">
                        <view class="dialog_main_scroll_view">
                            <view class="act_rank_list_tip5">【{{ bpTimeCombine[clickIndex].title }} 预告】</view>
                            <view class="act_rank_list_tip6">上半场</view>
                            <view class="act_rank_list_caption" style="width: 542rpx">
                                <view class="act_rank_list_tip2">{{ bpTimeCombine[clickIndex].phase_arr[0].phase_text }}</view>
                                <view class="act_rank_list_caption_inner">
                                    <text class="act_rank_list_caption_text" style="width: 216rpx; font-size: 26rpx">{{ bpTimeCombine[clickIndex].phase_arr[0].phase_from }}</text>
                                    至
                                    <text class="act_rank_list_caption_text" style="width: 216rpx; font-size: 26rpx">{{ bpTimeCombine[clickIndex].phase_arr[0].phase_to }}</text>
                                </view>
                                <view class="act_rank_list_caption_tip">排名TOP1的获得大屏奖励，前五都有奖励</view>
                            </view>
                            <block v-for="(item, index) in getBenefitsObj.reward_img_arr[0]" :key="index">
                                <view class="dialog_main_rank_one">第{{ index + 1 }}名</view>

                                <image class="dialog_main_rank_one_image" :src="item.img" :style="index == 4 ? 'height:903rpx;' : ''"></image>

                                <view class="dialog_main_rank_one_text">{{ pageDataObj.single_user_rank_info_all_time.text }}</view>
                            </block>
                            <view class="act_rank_list_tip6">下半场</view>
                            <view class="act_rank_list_caption" style="width: 542rpx">
                                <view class="act_rank_list_tip2">{{ bpTimeCombine[clickIndex].phase_arr[1].phase_text }}</view>
                                <view class="act_rank_list_caption_inner">
                                    <text class="act_rank_list_caption_text" style="width: 216rpx; font-size: 26rpx">{{ bpTimeCombine[clickIndex].phase_arr[1].phase_from }}</text>
                                    至
                                    <text class="act_rank_list_caption_text" style="width: 216rpx; font-size: 26rpx">{{ bpTimeCombine[clickIndex].phase_arr[1].phase_to }}</text>
                                </view>
                                <view class="act_rank_list_caption_tip">排名TOP1的获得大屏奖励，前五都有奖励</view>
                            </view>
                            <block v-for="(item, index) in getBenefitsObj.reward_img_arr[1]" :key="index">
                                <view class="dialog_main_rank_one">第{{ index + 1 }}名</view>

                                <image
                                    class="dialog_main_rank_one_image"
                                    :src="item.img"
                                    :style="index == 2 ? 'height:903rpx;' : index == 3 || index == 4 ? 'height:491rpx;' : ''"
                                ></image>

                                <view class="dialog_main_rank_one_text">{{ pageDataObj.single_user_rank_info_all_time.text }}</view>
                            </block>
                        </view>
                    </scroll-view>
                    <scroll-view :scrollY="true" style="max-height: 800rpx; margin-top: 22rpx" v-if="showDialogType == 4">
                        <view class="act_rank_list_tip5" style="margin-left: 116rpx">{{ getMakeKnownObj.title }}</view>
                        <view class="dialog_main_scroll_view" v-for="(item, index) in getMakeKnownObj.star_rank_per_phase_bom" :key="index">
                            <view class="dialog_main_chuxi_rank">第{{ index + 1 }}名</view>

                            <view class="dialog_main_chuxi_head_view">
                                <image class="dialog_main_chuxi_head" :src="item.starinfo.boat"></image>
                            </view>

                            <view class="dialog_main_chuxi_name">{{ myGuardStarItem.starinfo.name }}</view>

                            <view class="dialog_main_chuxi_soce">{{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}</view>

                            <text class="dialog_main_chuxi_text">{{ item.reward_text }}</text>
                        </view>
                        <view style="display: flex; align-items: center; margin-top: 52rpx; justify-content: center">
                            <view class="dialog_main_chuxi_line_text">补给站获奖详情</view>
                        </view>
                        <view class="table" style="width: 18rem; justify-content: center; margin-left: 10rpx; align-items: center">
                            <view class="tr" style="width: 18rem">
                                <view class="th t1">补给时间</view>
                                <view class="th t2">获得奖励</view>
                                <view class="th t3">获得星球</view>
                            </view>
                            <view class="tr" style="width: 18rem" v-for="(item, index) in getMakeKnownObj.fill_station_list" :key="index">
                                <view class="td t1">{{ item.time }}</view>

                                <view class="td t2" v-if="item.prize">
                                    <text>{{ item.prize + '\n+' + item.buff }}</text>
                                </view>

                                <view @tap="activityrule" class="td t2" :data-type="4" style="text-decoration: underline" v-else>查看补给奖励</view>

                                <view class="td t3" v-if="item.prize">
                                    <text>{{ item.starball + '\n' + item.votetimes }}</text>
                                </view>

                                <view class="td t3" v-else></view>
                            </view>
                        </view>
                        <view style="display: flex; align-items: center; margin-top: 52rpx; justify-content: center">
                            <view class="dialog_main_chuxi_line"></view>
                            <view class="dialog_main_chuxi_line_text">本场排名TOP-10</view>
                            <view class="dialog_main_chuxi_line"></view>
                        </view>
                        <view class="dialog_main_chuxi_data">
                            数据统计：2022年{{ pageDataObj.bp_time_arr[clickIndex].phase_from }}-{{ pageDataObj.bp_time_arr[clickIndex].phase_to }}
                        </view>
                        <view class="dialog_main_chuxi_item" v-for="(item, index) in getMakeKnownObj.star_rank_per_phase" :key="index">
                            <view
                                class="dialog_main_chuxi_item_rank"
                                :style="
                                    'background: ' +
                                    (index == 0 ? '#f7ed93' : index == 1 ? '#D7DCDE' : index == 2 ? '#FFA600' : '#FFFFFF') +
                                    '; color: ' +
                                    (index < 3 ? '#502a00' : '#FF7689')
                                "
                            >
                                {{ index + 1 }}
                            </view>

                            <image
                                class="dialog_main_chuxi_item_head"
                                :src="item.starinfo.logo_img"
                                :style="'border: 4rpx solid ' + (index == 0 ? '#e9d293' : index == 1 ? '#D7DCDE' : '#E77B1D') + ';'"
                            ></image>

                            <view class="dialog_main_chuxi_item_name">{{ myGuardStarItem.starinfo.name }}</view>

                            <view class="dialog_main_chuxi_item_count">{{ pageDataObj.single_user_rank_info_all_time.vote_times }}{{ tool_b_unit }}</view>
                        </view>
                    </scroll-view>
                    <block v-if="showDialogType == 5">
                        <text class="dialog_main_stop_data" style="text-align: center">{{ getResultMsg }}</text>
                        <view @tap="refreshPageData" class="dialog_main_stop_btn">刷新页面</view>
                    </block>
                    <block v-if="showDialogType == 6">
                        <image src="https://star-img.xingxiaoculture.com/2023/05/19/5b2522f72b992ac0ad9b79466091d34d.png" style="width: 600rpx; height: 192rpx"></image>
                        <scroll-view :scrollY="true">
                            <view class="get_item_box">
                                <view class="dialog_text01">去偷星</view>
                                <view class="dialog_text02">偷星成功后，可以随机掉落{{ tool_s_name }}，更有偷星机器人加入，更多玩法等你来~</view>
                                <view @tap="dialogGetBookClick" class="dialog_text_btn" :data-type="0">立即偷星</view>
                                <view class="dialog_text01">去送星</view>
                                <view class="dialog_text02">单次送星达到300颗，即可获得1{{ tool_s_unit }}{{ tool_s_name }}，可以累加，快去为星球送守护星获得吧～</view>
                                <view @tap="dialogGetBookClick" class="dialog_text_btn" :data-type="1">立即送星</view>
                                <!-- <view class="dialog_text01">公会币购买</view>
                        <view class="dialog_text02">使用公会币直接购买{{tool_s_name}}</view>
                        <view bindtap="dialogGetBookClick" class="dialog_text_btn" data-exchange_type="sugar" data-type="{{5}}">去购买</view> -->
                            </view>
                        </scroll-view>
                    </block>
                    <block v-if="showDialogType == 7">
                        <image src="https://star-img.xingxiaoculture.com/2023/05/19/5b2522f72b992ac0ad9b79466091d34d.png" style="width: 600rpx; height: 192rpx"></image>
                        <scroll-view :scrollY="true">
                            <view class="get_item_box">
                                <view class="dialog_text01">水晶海之战</view>
                                <view class="dialog_text02">去跟好友组成队伍，一起去水晶海之战，获得队伍排名奖励~</view>
                                <view @tap="tabItemClick" class="dialog_text_btn" :data-index="0">水晶海之战</view>
                                <view class="dialog_text01">召唤海神雨</view>
                                <view class="dialog_text02">去召唤海神雨，个人可获得超值{{ tool_b_name }}，还可为星球争夺卡片并增加大量{{ tool_b_name }}哦~</view>
                                <view @tap="tabItemClick" class="dialog_text_btn" :data-index="1">召唤海神雨</view>
                                <view class="dialog_text01">去偷星</view>
                                <view class="dialog_text02">偷星成功后，可以随机掉落{{ tool_b_name }}，更有偷星机器人加入，更多玩法等你来~</view>
                                <view @tap="dialogGetBookClick" class="dialog_text_btn" :data-type="0">立即偷星</view>
                                <view class="dialog_text01">去送星</view>
                                <view class="dialog_text02">单次送星达到300颗，即可获得1{{ tool_b_unit }}{{ tool_b_name }}，可以累加，快去为星球送守护星获得吧～</view>
                                <view @tap="dialogGetBookClick" class="dialog_text_btn" :data-type="1">立即送星</view>
                                <view class="dialog_text01">公会币购买</view>
                                <view class="dialog_text02">使用公会币直接购买{{ tool_b_name }}</view>
                                <view @tap="dialogGetBookClick" class="dialog_text_btn" data-exchange_type="heart_love_letter" :data-type="5">去购买</view>
                            </view>
                        </scroll-view>
                    </block>
                    <block v-else-if="showDialogType == 8">
                        <view class="dialog_text01">成功加入队伍</view>
                        <text class="dialog_main_stop_data">成功加入{{ joinTeamInfo.team_name }}，快去参加水晶海之战</text>
                        <text class="dialog_main_stop_data" style="text-align: center; margin-top: 0rpx">
                            <text>
                                获得组队奖励：
                                <text style="color: #ff881a">{{ tool_s_name }}*200</text>
                            </text>
                        </text>
                        <view class="group-data-item dialog_teaminfo" v-if="joinTeamInfo">
                            <view class="group-data-content" style="width: 100%">
                                <view class="group-data-left">
                                    <image class="tree_img" :src="joinTeamInfo.star_tree"></image>
                                    <view style="width: 300rpx; margin-left: 10rpx; display: flex; align-items: center">
                                        <image
                                            @tap="gotoDressRoom"
                                            class="team_info_user_img"
                                            :data-userid="mitem.userinfo._id"
                                            :src="mitem.userinfo.image.thumbnail_pic"
                                            v-for="(mitem, index) in joinTeamInfo.members"
                                            :key="index"
                                        ></image>
                                    </view>
                                </view>
                                <view class="group-data-info">
                                    <view class="group-title1">{{ joinTeamInfo.team_name }}</view>
                                </view>
                            </view>
                        </view>
                        <view @tap="dialogCloseClick" class="dialog_text_btn">知道了</view>
                    </block>
                    <block v-else-if="showDialogType == 9">
                        <view class="dialog_text01">申请加入队伍</view>
                        <text class="dialog_main_stop_data">每场水晶海之战只能加入三个队伍，请谨慎申请</text>
                        <view class="group-data-item dialog_teaminfo" v-if="joinTeamInfo">
                            <view class="group-data-content">
                                <view class="group-data-left">
                                    <view class="group-data-avatar" style="width: 160rpx; margin-left: 10rpx">
                                        <image
                                            @tap="gotoDressRoom"
                                            :class="[
                                                idx === 0 ? 'group-data-avatar_first' : '',
                                                idx === 1 ? 'group-data-avatar_content' : '',
                                                idx === 2 ? 'group-data-avatar_last' : ''
                                            ]"
                                            :data-userid="joinTeamInfo.members[0].userinfo._id"
                                            :src="
                                                joinTeamInfo.members[0].userinfo.image.thumbnail_pic
                                                    ? joinTeamInfo.members[0].userinfo.image.thumbnail_pic
                                                    : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                            "
                                        ></image>
                                        <image
                                            @tap="gotoDressRoom"
                                            :class="[
                                                idx === 0 ? 'group-data-avatar_first' : '',
                                                idx === 1 ? 'group-data-avatar_content' : '',
                                                idx === 2 ? 'group-data-avatar_last' : ''
                                            ]"
                                            :data-userid="joinTeamInfo.members[1].userinfo._id"
                                            :src="
                                                joinTeamInfo.members[1].userinfo.image.thumbnail_pic
                                                    ? joinTeamInfo.members[1].userinfo.image.thumbnail_pic
                                                    : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                            "
                                        ></image>
                                        <image
                                            @tap="gotoDressRoom"
                                            :class="[
                                                idx === 0 ? 'group-data-avatar_first' : '',
                                                idx === 1 ? 'group-data-avatar_content' : '',
                                                idx === 2 ? 'group-data-avatar_last' : ''
                                            ]"
                                            :data-userid="joinTeamInfo.members[2].userinfo._id"
                                            :src="
                                                joinTeamInfo.members[2].userinfo.image.thumbnail_pic
                                                    ? joinTeamInfo.members[2].userinfo.image.thumbnail_pic
                                                    : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                            "
                                        ></image>
                                        <image
                                            @tap="gotoDressRoom"
                                            :class="[
                                                idx === 0 ? 'group-data-avatar_first' : '',
                                                idx === 1 ? 'group-data-avatar_content' : '',
                                                idx === 2 ? 'group-data-avatar_last' : ''
                                            ]"
                                            :data-userid="joinTeamInfo.members[3].userinfo._id"
                                            :src="
                                                joinTeamInfo.members[3].userinfo.image.thumbnail_pic
                                                    ? joinTeamInfo.members[3].userinfo.image.thumbnail_pic
                                                    : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                            "
                                        ></image>
                                        <image
                                            @tap="gotoDressRoom"
                                            :class="[
                                                idx === 0 ? 'group-data-avatar_first' : '',
                                                idx === 1 ? 'group-data-avatar_content' : '',
                                                idx === 2 ? 'group-data-avatar_last' : ''
                                            ]"
                                            :data-userid="joinTeamInfo.members[4].userinfo._id"
                                            :src="
                                                joinTeamInfo.members[4].userinfo.image.thumbnail_pic
                                                    ? joinTeamInfo.members[4].userinfo.image.thumbnail_pic
                                                    : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                            "
                                        ></image>
                                    </view>
                                </view>
                                <view class="group-data-info">
                                    <view class="group-title1">{{ joinTeamInfo.team_name }}</view>
                                    <view class="group-num-info1">LV.{{ joinTeamInfo.level }}</view>
                                </view>
                            </view>
                        </view>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_cancel_btn">取消</view>
                            <view @tap="joinTeam" class="dialog_main_confirm_btn">确认申请</view>
                        </view>
                    </block>
                    <block v-else-if="showDialogType == 10">
                        <view class="dialog_text01">每日召唤拿{{ tool_s_name }}</view>
                        <text class="dialog_main_stop_data" style="text-align: center">
                            召唤公会内的小伙伴一起来拿{{ tool_s_name }}
                            <text style="color: #8466ff">*200</text>
                            {{ tool_s_unit }}奖励
                        </text>
                        <view class="dialog_text_btn">
                            召唤
                            <button class="tree_team_join_button" data-type="share_award" openType="share" style="width: 468rpx; height: 66rpx">+</button>
                        </view>
                    </block>
                    <block v-else-if="showDialogType == 11">
                        <view class="dialog_text01">每日召唤拿{{ tool_s_name }}</view>
                        <text class="dialog_main_stop_data" style="text-align: center">{{ daySharePrizeWord }}</text>
                        <view @tap="dialogCloseClick" class="dialog_text_btn">知道了</view>
                    </block>
                    <block v-else-if="showDialogType == 12">
                        <view class="dialog_text01">默认组队规则</view>
                        <text class="dialog_main_stop_data" style="font-size: 13px; line-height: 54rpx; margin-top: 0rpx">
                            1、我们会在每个制作{{ tool_b_name }}开放日自动帮您和公会内的成员组成一个默认队伍； 2、按照公会内贡献值的数值排序默认组成伍：
                            排名1~5名的用户一个队伍，6~10名的用户一个队伍，以此类推，1~5名的队伍，第1名为队长，6~10名的队伍，第6名为队长； 3、组成默认队伍后可以在制作{{
                                tool_b_name
                            }}未开始之前自行退出该队伍或者修改该队伍的信息噢~
                        </text>
                        <view @tap="dialogCloseClick" class="dialog_text_btn">知道了</view>
                    </block>
                    <block v-else-if="showDialogType == 13">
                        <view class="dialog_text01">我的星球</view>
                        <view style="display: flex; flex-direction: column; align-items: center">
                            <scroll-view :scrollY="true" style="height: 900rpx">
                                <view class="content_selecttips_line" v-for="(item, index) in pageDataObj.my_starball_list" :key="index">
                                    <image class="content_starscore_head" :src="item.starball.logo_url"></image>

                                    <view class="content_selecttips_name">
                                        {{ item.starball.name }}
                                    </view>

                                    <view @tap="selectStar2" class="content_selecttips_button" :data-starid="item.starball.starid">选择</view>
                                </view>
                            </scroll-view>
                        </view>
                    </block>
                    <block v-else-if="showDialogType == 14">
                        <image src="https://star-img.xingxiaoculture.com/2022/11/14/337d6840a46feabb84a3ba28d8017f4d.png" style="width: 600rpx; height: 192rpx"></image>
                        <scroll-view :scrollY="true">
                            <view class="get_item_box">
                                <view class="dialog_text01">竞猜玩法简介</view>
                                <view class="dialog_text02">1.竞猜玩法在本活动当中11.21日上线；</view>
                                <view class="dialog_text02">2.猜猜哪个队伍获胜，还能获道具奖励~</view>
                                <view @tap="dialogCloseClick" class="dialog_text_btn">我知道了</view>
                            </view>
                        </scroll-view>
                        <view style="margin-bottom: 20rpx"></view>
                    </block>
                    <block v-else-if="showDialogType == 15">
                        <image
                            src="https://star-img.xingxiaoculture.com/2022/12/12/dda35b79c8482b92cf255ab3e0fd301c.png"
                            style="width: 305rpx; height: 80rpx; margin-top: -40rpx; z-index: 1"
                        ></image>
                        <image
                            src="https://star-img.xingxiaoculture.com/2022/12/12/05302d3588354c5100dcd1e4c2969652.png"
                            style="width: 600rpx; height: 1104rpx; margin-top: -40rpx"
                        ></image>
                    </block>
                    <block v-else-if="showDialogType == 16">
                        <view class="dialog_text01">恭喜获得卡片</view>
                        <view class="dialog_text02" style="margin-top: 20rpx; text-align: center">
                            <text>
                                {{ thisHasCard.time }}成功为{{ thisHasCard.starball.name }}获得
                                <text style="color: #ff5d26">{{ thisHasCard.card_name }}</text>
                                卡片一张。为星球增加了
                                <text style="color: #ff5d26">200万</text>
                                的{{ tool_b_name }}，快去告诉大家吧~
                            </text>
                        </view>
                        <image style="width: 327rpx; height: 327rpx" :src="thisHasCard.card_img"></image>
                        <button class="dialog_text_btn" openType="share">快去告诉Ta们</button>
                        <view @tap="dialogCloseClick" class="dialog_text_btn" style="background: none; color: #4fcaff; border: 1rpx solid #4fcaff">我知道了</view>
                        <view class="dialog_text02" style="margin-top: 20rpx; text-align: center"><text>(数值将在获得时间后的3分钟内统计)</text></view>
                    </block>
                    <image class="dialog_main_image_bg" src="" v-if="showDialogType != 15"></image>
                </view>
            </view>
            <view class="dialog_view" v-if="bTipsWindowShow">
                <view class="dialog_main_view" :style="bTipsWindowShowType == 14 ? 'width:680rpx' : ''">
                    <view class="dialog_close">
                        <image @tap="dialogCloseClick" class="dialog_close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"></image>
                    </view>
                    <block v-if="bTipsWindowShowType == 0">
                        <view class="dialog_text01">申请加入队伍</view>
                        <text class="dialog_main_stop_data" style="text-align: center">每场水晶海之战只能加入三个队伍，请谨慎申请</text>
                        <view class="dialog_teaminfo" v-if="joinTeamInfo">
                            <view
                                :class="
                                    'scrollOrder ' +
                                    (joinTeamInfo.rank == -1
                                        ? 'scrollNo'
                                        : joinTeamInfo.rank == 1
                                        ? 'scrollOne'
                                        : joinTeamInfo.rank == 2
                                        ? 'scrollTwo'
                                        : joinTeamInfo.rank == 3
                                        ? 'scrollThree'
                                        : '')
                                "
                            >
                                {{ joinTeamInfo.rank == -1 ? '未上榜' : joinTeamInfo.rank }}
                            </view>
                            <view class="m11" style="width: 200rpx; margin-left: 20rpx">
                                <view>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="joinTeamInfo.members[0].userinfo._id"
                                        :src="
                                            joinTeamInfo.members[0].userinfo.image.thumbnail_pic
                                                ? joinTeamInfo.members[0].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="joinTeamInfo.members[1].userinfo._id"
                                        :src="
                                            joinTeamInfo.members[1].userinfo.image.thumbnail_pic
                                                ? joinTeamInfo.members[1].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="joinTeamInfo.members[2].userinfo._id"
                                        :src="
                                            joinTeamInfo.members[2].userinfo.image.thumbnail_pic
                                                ? joinTeamInfo.members[2].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="joinTeamInfo.members[3].userinfo._id"
                                        :src="
                                            joinTeamInfo.members[3].userinfo.image.thumbnail_pic
                                                ? joinTeamInfo.members[3].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="joinTeamInfo.members[4].userinfo._id"
                                        :src="
                                            joinTeamInfo.members[4].userinfo.image.thumbnail_pic
                                                ? joinTeamInfo.members[4].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                </view>
                            </view>
                            <view class="group-data-info">
                                <view class="group-num-info1">LV.{{ joinTeamInfo.level }}</view>
                                <view class="group-title1">{{ joinTeamInfo.team_name }}</view>
                            </view>
                            <view class="group-data-info">
                                <view class="group-num-info3">{{ tool_b_name }}数量{{ joinTeamInfo.total_time }}</view>
                            </view>
                        </view>
                        <text class="dialog_main_stop_data" style="color: #000000; font-size: 24rpx">
                            申请加入当前队伍需要
                            <text style="color: #e1621b">242424果肥</text>
                            ，入队成功后扣除对应果肥数量，到达种植时间后将自动投入
                        </text>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_cancel_btn">取消</view>
                            <view @tap="joinTeam" class="dialog_main_confirm_btn">确认申请</view>
                        </view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 1">
                        <view class="dialog_text01">同意当前用户加入队伍</view>
                        <text class="dialog_main_stop_data">制作{{ tool_b_name }}开始后则不可踢出或退出，请谨慎通过</text>
                        <view class="dialog_teaminfo" v-if="applyUserinfo">
                            <view class="group_user_item">
                                <image class="group_user_item_image" :src="applyUserinfo.userinfo.image.middle_pic"></image>
                                <view class="group_user_item_title" style="width: 180rpx; color: #000000">{{ applyUserinfo.userinfo.nickname }}</view>
                                <view class="group_user_item_level" style="font-size: 12px; color: #000000">已赠送{{ applyUserinfo.user_votetimes_num }}{{ tool_b_unit }}</view>
                            </view>
                        </view>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_cancel_btn">取消</view>
                            <view @tap="agreeJoin" class="dialog_main_confirm_btn">同意</view>
                        </view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 2">
                        <view class="dialog_text01">加入队伍成功</view>
                        <text class="dialog_main_stop_data" style="text-align: center">成功加入{{ myBalloonTeamCurrent.team_name }}，快去制作{{ tool_b_name }}获得福利吧~</text>
                        <text class="dialog_main_stop_data" style="text-align: center; margin-top: 0rpx">
                            <text>
                                获得组队奖励：
                                <text style="color: #ff5d5d">{{ tool_s_name }}*200（将在制作{{ tool_b_name }}开始以后自动帮您制作）</text>
                            </text>
                        </text>
                        <view class="dialog_teaminfo" v-if="myBalloonTeamCurrentId">
                            <image class="tree_img" :src="myBalloonTeamCurrent.star_tree"></image>
                            <view class="m11" style="width: 280rpx; margin-left: 20rpx">
                                <view>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="myBalloonTeamCurrent.members[0].userinfo._id"
                                        :src="
                                            myBalloonTeamCurrent.members[0].userinfo.image.thumbnail_pic
                                                ? myBalloonTeamCurrent.members[0].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="myBalloonTeamCurrent.members[1].userinfo._id"
                                        :src="
                                            myBalloonTeamCurrent.members[1].userinfo.image.thumbnail_pic
                                                ? myBalloonTeamCurrent.members[1].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="myBalloonTeamCurrent.members[2].userinfo._id"
                                        :src="
                                            myBalloonTeamCurrent.members[2].userinfo.image.thumbnail_pic
                                                ? myBalloonTeamCurrent.members[2].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="myBalloonTeamCurrent.members[3].userinfo._id"
                                        :src="
                                            myBalloonTeamCurrent.members[3].userinfo.image.thumbnail_pic
                                                ? myBalloonTeamCurrent.members[3].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                    <image
                                        @tap="gotoDressRoom"
                                        :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                        :data-userid="myBalloonTeamCurrent.members[4].userinfo._id"
                                        :src="
                                            myBalloonTeamCurrent.members[4].userinfo.image.thumbnail_pic
                                                ? myBalloonTeamCurrent.members[4].userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                        "
                                    ></image>
                                </view>
                            </view>
                            <view class="group-data-info">
                                <view class="group-title1">{{ myBalloonTeamCurrent.team_name }}</view>
                            </view>
                        </view>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_confirm_btn2">知道了</view>
                        </view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 3">
                        <view class="dialog_text01">创建队伍成功</view>
                        <text class="dialog_main_stop_data2" style="font-size: 12px">
                            <text v-if="bTipsWindowShowType3ShowPrice">
                                获得组队奖励：
                                <text style="color: #ff881a">{{ tool_s_name }}*200</text>
                            </text>
                            <text v-else>当日获得创建奖励已达上限 无法再获得奖励</text>
                        </text>
                        <text class="dialog_main_stop_data2" style="margin-top: 20rpx">快去邀请好友一起水晶海之战吧~</text>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_confirm_btn2">知道了</view>
                        </view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 4">
                        <view class="dialog_text01">切换队伍</view>
                        <text class="dialog_main_stop_data2">只展示未开始和进行中的队伍</text>
                        <view @tap="selectTeam" class="team_select_item" :data-team_id="item.my_team.team_id" v-for="(item, index) in myBalloonTeam" :key="index">
                            <view class="team_select_item_icon_selected'" v-if="item.my_team.team_id == myBalloonTeamCurrentId">√</view>

                            <view class="team_select_item_icon'" v-else></view>

                            <view class="team_select_item_name">{{ item.my_team.team_name }}</view>

                            <view class="dialog_tree_info_captain_icon">{{ item.my_team.term_id }}</view>
                        </view>
                        <view @tap="selectTeam" class="team_select_item" :data-team_id="item.my_team.team_id" v-for="(item, index) in myBalloonTeamNext" :key="index">
                            <view class="team_select_item_icon_selected'" v-if="item.my_team.team_id == myBalloonTeamCurrentId">√</view>

                            <view class="team_select_item_icon'" v-else></view>

                            <view class="team_select_item_name">{{ item.my_team.team_name }}</view>

                            <view class="dialog_tree_info_captain_icon">{{ item.my_team.term_id }}</view>
                        </view>
                        <view
                            @tap="showBalloonCreateTeamPanel"
                            class="team_select_item_create_team"
                            v-if="
                                (arenaInfo.status != 'is_end' && myBalloonTeam.length < teamNumMax) ||
                                (arenaInfo.status == 'is_end' && arenaInfo.next_term_id && myBalloonTeamNext.length < teamNumMax)
                            "
                        >
                            +创建队伍
                        </view>
                        <view class="dialog_button_box">
                            <view @tap="confirmSelectTeam" class="dialog_main_confirm_btn2">确定</view>
                        </view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 5">
                        <view class="dialog_text01">是否解散当前队伍</view>
                        <text class="dialog_main_stop_data2">解散后队伍内成员将退出当前队伍 请问是否解散队伍？</text>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_cancel_btn">取消</view>
                            <view @tap="dismissTeam" class="dialog_main_confirm_btn">同意</view>
                        </view>
                        <view style="margin-bottom: 20rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 6">
                        <view class="dialog_text01">是否退出当前队伍</view>
                        <text class="dialog_main_stop_data2">退出队伍后将无法撤回该操作 请问是否退出当前队伍？</text>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_cancel_btn">取消</view>
                            <view @tap="quitTeam" class="dialog_main_confirm_btn">同意</view>
                        </view>
                        <view style="margin-bottom: 20rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 7">
                        <view class="dialog_text01">是否踢出当前成员</view>
                        <view class="dialog_teaminfo" style="flex-direction: column; margin-top: 20rpx; width: 516rpx" v-if="getoutMember">
                            <view class="group_user_item">
                                <image class="group_user_item_image" :src="getoutMember.userinfo.image.middle_pic"></image>
                                <view class="group_user_item_title" style="width: 180rpx">{{ getoutMember.userinfo.nickname }}</view>
                                <view class="group_user_item_level" style="font-size: 12px">
                                    已送出{{ getoutMember.user_votetimes_num ? getoutMember.user_votetimes_num : 0 }}{{ tool_b_unit }}
                                </view>
                            </view>
                        </view>
                        <text class="dialog_main_stop_data2">踢出该成员后将无法撤回该操作 请问是否踢出当前成员</text>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_cancel_btn">取消</view>
                            <view @tap="getoutTeam" class="dialog_main_confirm_btn">确定</view>
                        </view>
                        <view style="margin-bottom: 20rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 8">
                        <view class="dialog_text01">BOSS冻结了您的果树产果</view>
                        <text class="dialog_main_stop_data2">需要将果树再提升{{ bossInfo.beat_boss_need_level }}级，即可击败BOSS</text>
                        <view class="dialog_boss_gift_item">
                            <text class="dialog_boss_gift_detail_title">击败BOSS奖励：</text>
                            <image class="dialog_boss_gift_detail_img" src="https://star-img.xingxiaoculture.com/2022/09/13/370103afd7f34487110284f3eecc7f83.png"></image>
                            <text class="dialog_boss_gift_detail_text">{{ bossInfo.beat_boss_drop_fruit }}</text>
                        </view>
                        <view class="dialog_boss_gift_item">
                            <text class="dialog_boss_gift_detail_title"></text>
                            <image class="dialog_boss_gift_detail_img" :src="bossInfo.beat_boss_drop_gift_img"></image>
                            <text class="dialog_boss_gift_detail_text">{{ bossInfo.beat_boss_drop_gift }}</text>
                        </view>
                        <view class="dialog_boss_gift_item">
                            <text class="dialog_boss_gift_detail_text" style="width: 100%; text-align: center">（队伍共同获得，通过个人{{ tool_b_name }}占比分配）</text>
                        </view>
                        <view class="dialog_button_box">
                            <view @tap="showVotePanel" class="dialog_main_confirm_btn2">制作{{ tool_b_name }}</view>
                        </view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 9">
                        <image src="https://star-img.idol001.com/2022/09/15/c9056ba3145f1d6f5de0a7f37263bb94.png" style="margin-top: 20rpx; width: 188rpx; height: 240rpx"></image>
                        <view class="dialog_text01">已成功击败BOSS！</view>
                        <text class="dialog_main_stop_data2">恭喜您成功击败BOSS，果树产果能力已恢复~</text>
                        <view class="dialog_boss_gift_item">
                            <text class="dialog_boss_gift_detail_title">获得BOSS奖励：</text>
                            <image class="dialog_boss_gift_detail_img" src="https://star-img.xingxiaoculture.com/2022/09/13/370103afd7f34487110284f3eecc7f83.png"></image>
                            <text class="dialog_boss_gift_detail_text">{{ bossInfo.beat_boss_drop_fruit }}</text>
                        </view>
                        <view class="dialog_boss_gift_item">
                            <text class="dialog_boss_gift_detail_title"></text>
                            <image class="dialog_boss_gift_detail_img" :src="bossInfo.beat_boss_drop_gift_img"></image>
                            <text class="dialog_boss_gift_detail_text">{{ bossInfo.beat_boss_drop_gift }}</text>
                        </view>
                        <view class="dialog_boss_gift_item">
                            <text class="dialog_boss_gift_detail_text" style="width: 100%; text-align: center">（队伍共同获得，通过个人{{ tool_b_name }}占比分配）</text>
                        </view>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_confirm_btn2">我知道了</view>
                        </view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 10">
                        <image :src="showTreeInfoObj.star_tree" style="margin-top: 20rpx; width: 235rpx; height: 240rpx"></image>
                        <text class="dialog_tree_info_teamname">{{ showTreeInfoObj.team_name }}</text>
                        <view class="dialog_tree_info_slogan">
                            <image
                                src="https://star-img.xingxiaoculture.com/2022/10/17/5412b2150f88c098ba891879f217096d.png"
                                style="width: 80rpx; height: 80rpx; position: absolute; left: 0; top: -30rpx"
                            ></image>
                            <image
                                @tap="showShortnameSet"
                                :data-item="showTreeInfoObj"
                                data-type="2"
                                src="https://star-img.xingxiaoculture.com/2022/10/17/ea41c097998ef380a4aeb40601cf3c4e.png"
                                style="width: 31rpx; height: 30rpx; position: absolute; right: 5rpx; top: 8rpx"
                                v-if="showTreeInfoObj.in_team"
                            ></image>
                            {{ showTreeInfoObj.slogan }}
                        </view>
                        <view class="dialog_tree_info_captain">
                            <image class="dialog_tree_info_user_img" :src="showTreeInfoObj.members[0].userinfo.image.middle_pic"></image>
                            <text class="dialog_tree_info_user_name">{{ showTreeInfoObj.members[0].userinfo.nickname }}</text>
                            <view class="dialog_tree_info_captain_icon">队长</view>
                        </view>
                        <view class="dialog_tree_info_member_box">
                            <view class="dialog_tree_info_member" v-if="index > 0" v-for="(item, index) in showTreeInfoObj.members" :key="index">
                                <image class="dialog_tree_info_user_img" :src="item.userinfo.image.middle_pic"></image>

                                <text class="dialog_tree_info_user_name" style="width: 140rpx">{{ myRankObject.userinfo.nickname }}</text>
                            </view>
                        </view>
                        <view @tap="grabCandy" class="dialog_main_confirm_btn2" :data-team_id="showTreeInfoObj.team_id" v-if="showTreeInfoObj.team_id != myBalloonTeamCurrentId">
                            抢夺{{ tool_b_name }}
                        </view>
                        <view style="margin-bottom: 20rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 11">
                        <image src="https://star-img.xingxiaoculture.com/2022/12/12/7762ec078126800779527b1bb0eb0951.png" style="width: 600rpx; height: 192rpx"></image>
                        <scroll-view :scrollY="true">
                            <view class="get_item_box">
                                <view class="dialog_text01">去偷星</view>
                                <view class="dialog_text02">偷星成功后，可以随机掉{{ tool_s_name }}，更有偷星机器人加入，更多玩法等你来~</view>
                                <view @tap="dialogGetBookClick" class="dialog_text_btn" :data-type="0">立即偷星</view>
                                <view class="dialog_text01">去送星</view>
                                <view class="dialog_text02">单次送星达到300颗，即可获得1{{ tool_s_unit }}{{ tool_s_name }}，可以累加，快去为星球送守护星获得吧～</view>
                                <view @tap="dialogGetBookClick" class="dialog_text_btn" :data-type="1">立即送星</view>
                                <view class="dialog_text01">公会币购买</view>
                                <view class="dialog_text02">使用公会币直接购买{{ tool_s_name }}</view>
                                <view @tap="gotoGradCandyPage" class="dialog_text_btn" data-open_type="sugar" :data-team_id="myBalloonTeamCurrentId">去购买</view>
                            </view>
                        </scroll-view>
                        <view style="margin-bottom: 20rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 12">
                        <view class="dialog_text01">抢夺成功</view>
                        <text class="dialog_main_stop_data2">
                            本次成功抢夺{{ grabResult.team_name }}免费{{ tool_b_name }}的
                            <text style="color: #8465ff">{{ grabResult.grab_candy_percent }} 共计{{ tool_b_name }}{{ grabResult.grab_candy_num }}{{ tool_b_unit }}</text>
                        </text>
                        <text class="dialog_main_stop_data2" style="margin-top: 20rpx">
                            该{{ tool_b_name }}将进入我方队伍的免费{{ tool_b_name }}中，也可以被其他队伍抢夺，本场制作{{ tool_b_name }}结束后到账
                        </text>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_confirm_btn2">知道了</view>
                        </view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 13">
                        <image src="https://star-img.xingxiaoculture.com/2022/12/12/7762ec078126800779527b1bb0eb0951.png" style="width: 670rpx; height: 212rpx"></image>
                        <view class="dialog_text01">{{ grabResult.title }}</view>
                        <text class="dialog_main_stop_data2" style="margin-top: 20rpx">{{ grabResult.content }}</text>
                        <view
                            @tap="gotoGradCandyPage"
                            class="dialog_main_confirm_btn2"
                            data-open_type="grab_times"
                            :data-team_id="myBalloonTeamCurrentId"
                            v-if="grabResult.code == 2"
                        >
                            获取抢夺次数
                        </view>
                        <view @tap="gotoGradCandyPage" class="dialog_main_confirm_btn2" data-open_type="magic" :data-team_id="myBalloonTeamCurrentId" v-if="grabResult.code == 3">
                            提升魔法攻击力
                        </view>
                        <view @tap="dialogCloseClick" class="dialog_main_cancel_btn2">知道了</view>
                        <view style="margin-bottom: 20rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 14">
                        <view class="team_info_item_top">
                            <view class="team_info_item_title">成员信息</view>
                        </view>
                        <!-- <view class="btn-group" style="margin-top: 0rpx;border-radius: 10rpx;width: 650rpx;">
                    <view bindtap="changeTabTeamInfo" class="{{teamInfoTab==0?'type-btn-on':'type-btn'}}" data-index="{{index}}" data-tab="0" data-team_type="{{type}}" style="{{teamInfoTab==0?'border-radius: 10rpx  0 0 10rpx;':''}}">
                        场上成员
                    </view>
                    <view bindtap="changeTabTeamInfo" class="{{teamInfoTab==1?'type-btn-on':'type-btn'}}" data-index="{{index}}" data-tab="1"  data-team_type="{{type}}" style="{{teamInfoTab==1?'border-radius: 0 10rpx 10rpx 0 ;':''}}">
                        所有成员
                    </view>
                </view> -->
                        <block v-if="teamInfoTab == 0">
                            <block
                                v-if="(arenaInfo.term_id == myBalloonTeamCurrent.term_id && arenaInfo.status == 'not_begin') || arenaInfo.term_id < myBalloonTeamCurrent.term_id"
                            >
                                <view class="team_info_line" style="width: 650rpx" v-if="member.userinfo" v-for="(member, index) in myBalloonTeamCurrent.members_pos" :key="index">
                                    <image class="team_info_line_area_image" :src="member.img"></image>

                                    <image
                                        class="team_info_line_user_image"
                                        :src="
                                            member.userinfo
                                                ? member.userinfo.image.thumbnail_pic
                                                : 'https://star-img.xingxiaoculture.com/2023/01/17/5f6b4ead93065f50ecf91a43cf0651ef.png'
                                        "
                                    ></image>

                                    <text class="team_info_line_text" style="width: 250rpx">{{ member.userinfo ? member.userinfo.nickname : '空缺中' }}</text>

                                    <view
                                        @tap="showGetoutTeam"
                                        class="team_info_line_right_button"
                                        :data-item="member"
                                        :data-team_id="myBalloonTeamCurrent.team_id"
                                        v-if="myBalloonTeamCurrent.is_captain && !member.is_captain"
                                    >
                                        踢出
                                    </view>
                                </view>
                            </block>
                            <scroll-view :enableFlex="true" :scrollY="true" style="display: flex; flex-direction: column; align-items: center; height: 900rpx" v-else>
                                <view class="team_info_item_hint" v-if="myBalloonTeamCurrent.expect_text">{{ myBalloonTeamCurrent.expect_text }}</view>
                                <view class="team_info_item" v-for="(member, index) in myBalloonTeamCurrent.members" :key="index">
                                    <view class="my_tree_team_head_box">
                                        <image class="my_tree_team_user_img" :src="member.userinfo.image.middle_pic"></image>
                                        <view class="my_tree_team_user_text" style="color: #ff7b1e; width: 30%">
                                            <!-- <text>{{member.userinfo.nickname}}<text style="font-size:11px;font-weight: 500;">
    {{tool_s_name}}收集:{{member.tools_by_votetimes}}{{tool_s_unit}}</text><text style="font-size:11px;font-weight: 500;">
        公会币拍摄收集:{{member.tools_by_tour}}{{tool_s_unit}}</text><text style="font-size:11px;font-weight: 500;">
        拼图解锁收集:{{member.tools_by_jigsaw}}{{tool_s_unit}}</text></text> -->
                                            <text>{{ member.userinfo.nickname }}</text>
                                        </view>
                                        <view class="my_tree_team_user_text" style="width: 28%" v-if="member.fruit_num">
                                            <text>对队伍产生贡献值\n < text style="color: #FF7B1E;">{{ member.fruit_num }}</text>
                                        </view>

                                        <view class="my_tree_team_user_text" style="color: #ff7b1e; width: 25%" v-if="member.level_percent">
                                            <text>占比{{ member.level_percent }}\n < text style="color: gray;">(展示2位小数)</text>
                                        </view>
                                    </view>

                                    <!-- <view class="team_info_item_hint2">当前场次共收集{{member.fruit_num}}点
                    <view class="flex_row" style="width: 600rpx;flex-wrap: wrap;">
                                    <view class="team_info_item_hint2_item" wx:key="pindex" wx:for="{{member.pos2score}}" wx:for-item="pos">{{pos.name}} {{pos.score}}</view>
                                </view>
                            </view> -->
                                </view>

                                <view class="group_pk_prizes_list2">
                                    <view class="group_pk_prizes_list_line"></view>
                                    <view class="group_pk_prizes_list_text2">贡献值计算规则</view>
                                </view>
                                <view class="my_team_rule_box">
                                    <view class="my_team_rule_line" style="background-color: #9cc8f6; border-radius: 10rpx 10rpx 0 0; height: 52rpx">
                                        <view class="my_team_rule_item" style="width: 20%; font-size: 10px">角色</view>
                                        <view class="my_team_rule_item" style="width: 40%; font-size: 10px">行为</view>
                                        <view class="my_team_rule_item" style="width: 40%; font-size: 10px">贡献值</view>
                                    </view>
                                    <view class="my_team_rule_line">
                                        <view class="my_team_rule_item" style="width: 20%; font-size: 11px">海洋女神</view>
                                        <view class="my_team_rule_item" style="width: 40%">以1.3倍消耗支援他人</view>
                                        <view class="my_team_rule_item" style="width: 40%">在支援位置算法的基础上*1.3</view>
                                    </view>
                                    <view class="my_team_rule_line">
                                        <view class="my_team_rule_item" style="width: 20%; font-size: 11px">星光能源师</view>
                                        <view class="my_team_rule_item" style="width: 40%">投入星能源</view>
                                        <view class="my_team_rule_item" style="width: 40%">每投入1点星能源=1贡献值</view>
                                    </view>
                                    <view class="my_team_rule_line">
                                        <view class="my_team_rule_item" style="width: 20%; font-size: 11px">星辉能源师</view>
                                        <view class="my_team_rule_item" style="width: 40%">投入星能源</view>
                                        <view class="my_team_rule_item" style="width: 40%">每投入1点星能源=1贡献值</view>
                                    </view>
                                    <view class="my_team_rule_line">
                                        <view class="my_team_rule_item" style="width: 20%; font-size: 11px">潮汐之翼</view>
                                        <view class="my_team_rule_item" style="width: 40%">加速船只</view>
                                        <view class="my_team_rule_item" style="width: 40%">每加速1米/分钟=50贡献值</view>
                                    </view>
                                    <view class="my_team_rule_line">
                                        <view class="my_team_rule_item" style="width: 20%; font-size: 11px">波涛咒语师</view>
                                        <view class="my_team_rule_item" style="width: 40%">降低前1名船只速度</view>
                                        <view class="my_team_rule_item" style="width: 40%">每降低1米/分钟=40贡献值</view>
                                    </view>
                                    <view class="my_team_rule_line">
                                        <view class="my_team_rule_item" style="width: 20%; font-size: 11px">助力成员</view>
                                        <view class="my_team_rule_item" style="width: 40%">投入星能源并被消耗</view>
                                        <view class="my_team_rule_item" style="width: 40%">每消耗1点星能源=1贡献值</view>
                                    </view>
                                </view>
                                <view class="group_pk_prizes_list2">
                                    <view class="group_pk_prizes_list_line"></view>
                                    <view class="group_pk_prizes_list_text2">助力成员信息</view>
                                </view>
                                <view class="team_info_my_info_table" style="margin-top: 20rpx">
                                    <view class="team_info_my_info_table_head">
                                        <view class="team_info_my_info_table_item" style="background-color: #c1dbf7; border: none">用户信息</view>
                                        <view class="team_info_my_info_table_item" style="background-color: #c1dbf7; border: none">贡献值</view>
                                        <view class="team_info_my_info_table_item" style="background-color: #c1dbf7; border: none">全队占比</view>
                                    </view>
                                    <block v-if="myBalloonTeamCurrent.user_list && myBalloonTeamCurrent.user_list.length > 0">
                                        <view
                                            class="team_info_my_info_table_line"
                                            style="background-color: #c1dbf7; height: 68rpx; border-top: 1rpx solid #ffffff; align-items: center"
                                            v-for="(item, index) in myBalloonTeamCurrent.user_list"
                                            :key="index"
                                        >
                                            <image class="my_tree_team_user_img" style="width: 52rpx; height: 52rpx" :src="item.userinfo.image.middle_pic"></image>

                                            <view class="my_tree_team_user_text" style="width: 25%">{{ myRankObject.userinfo.nickname }}</view>

                                            <view class="my_tree_team_user_text" @tap="showTeamUserInfoSingle" :data-item="item" style="width: 40%; color: #c1dbf7">
                                                {{ item.votetimes }}
                                            </view>

                                            <view class="my_tree_team_user_text" style="width: 20%" v-if="item.percent">{{ item.percent }}</view>
                                        </view>
                                    </block>
                                    <block v-else-if="">
                                        <text class="dialog_main_stop_data2">暂无数据</text>
                                    </block>
                                </view>
                            </scroll-view>
                        </block>
                        <block v-else-if="teamInfoTab == 1">
                            <scroll-view :enableFlex="true" :scrollY="true" style="display: flex; flex-direction: column; align-items: center; height: 900rpx">
                                <view style="font-size: 12px; width: 90%; text-align: center; margin-top: 10rpx">
                                    帮助队伍助力 有机会一起瓜分{{ tool_b_name }}奖励和获得活动专属头像框~
                                </view>
                                <view class="group_pk_prizes_list2">
                                    <view class="group_pk_prizes_list_line"></view>
                                    <view class="group_pk_prizes_list_text2">我的信息</view>
                                </view>
                                <view class="team_info_my_info">
                                    <image
                                        style="border-radius: 50%; height: 52rpx; width: 52rpx; margin-left: 5rpx"
                                        :src="myBalloonTeamCurrent.my_info.userinfo.image.thumbnail_pic"
                                    ></image>
                                    <text style="font-size: 12px; color: #333333; margin-left: 5rpx; width: 200rpx">{{ myBalloonTeamCurrent.my_info.userinfo.nickname }}</text>
                                    <text style="font-size: 11px; color: #333333; width: 260rpx">
                                        共收集魅力值
                                        <text style="color: #ff7b1e">{{ myBalloonTeamCurrent.my_info.votetimes }}</text>
                                    </text>
                                    <text style="font-size: 11px; color: #333333; width: 100rpx; text-align: center">
                                        全队占比
                                        <text style="color: #ff7b1e">{{ myBalloonTeamCurrent.my_info.percent }}</text>
                                    </text>
                                </view>
                                <!-- <view class="team_info_my_info_table">
                            <view class="team_info_my_info_table_head">
                                <view class="team_info_my_info_table_item" style="background-color: #FFD1ED;border:none">
                                    魅力类型
                                </view>
                                <view class="team_info_my_info_table_item" style="background-color: #FFD1ED;border:none">
                                    场下助力收集
                                </view>
                                <view class="team_info_my_info_table_item" style="background-color: #FFD1ED;border:none">
                                    场上收集
                                </view>
                                <view class="team_info_my_info_table_item" style="background-color: #FFD1ED;border:none">
                                    拼图解锁
                                </view>
                            </view>
                            <view class="team_info_my_info_table_line" wx:for="{{myBalloonTeamCurrent.my_info.pos_list}}" wx:if="{{item.help_num || item.playground_num || item.jigsaw_num}}" wx:key="pindex">
                                <view class="team_info_my_info_table_item" style="color:#F45F96">
                                    {{loactionArr[index]}}
                                </view>
                                <view class="team_info_my_info_table_item" style="{{!item.help_num?'color:#F45F96':''}}">
                                    {{item.help_num?item.help_num:'未助力'}}
                                </view>
                                <view class="team_info_my_info_table_item" style="{{!item.playground_num?'color:#F45F96':''}}">
                                    {{item.playground_num?item.playground_num:'未上场'}}
                                </view>
                                <view class="team_info_my_info_table_item" style="{{!item.jigsaw_num?'color:#F45F96':''}}">
                                    {{item.jigsaw_num?item.jigsaw_num:'未解锁'}}
                                </view>
                            </view>
                        </view> -->
                                <view class="team_info_my_info" v-if="myBalloonTeamCurrent.my_info.text">
                                    <text style="text-align: center">{{ myBalloonTeamCurrent.my_info.text }}</text>
                                </view>
                                <!-- <view class="act_rank_list_more" style="text-decoration: underline;">查看各个魅力类型详情 ></view> -->
                                <view class="group_pk_prizes_list2">
                                    <view class="group_pk_prizes_list_line"></view>
                                    <view class="group_pk_prizes_list_text2">助力成员信息</view>
                                </view>
                                <view class="team_info_my_info_table" style="margin-top: 20rpx">
                                    <view class="team_info_my_info_table_head">
                                        <view class="team_info_my_info_table_item" style="background-color: #c1dbf7; border: none">用户信息</view>
                                        <view class="team_info_my_info_table_item" style="background-color: #c1dbf7; border: none">贡献值</view>
                                        <view class="team_info_my_info_table_item" style="background-color: #c1dbf7; border: none">全队占比</view>
                                    </view>
                                    <block v-if="myBalloonTeamCurrent.user_list && myBalloonTeamCurrent.user_list.length > 0">
                                        <view
                                            class="team_info_my_info_table_line"
                                            style="background-color: #c1dbf7; height: 68rpx; border-top: 1rpx solid #ffffff; align-items: center"
                                            v-for="(item, index) in myBalloonTeamCurrent.user_list"
                                            :key="index"
                                        >
                                            <image class="my_tree_team_user_img" style="width: 52rpx; height: 52rpx" :src="item.userinfo.image.middle_pic"></image>

                                            <view class="my_tree_team_user_text" style="width: 20%">{{ myRankObject.userinfo.nickname }}</view>

                                            <view class="my_tree_team_user_text" @tap="showTeamUserInfoSingle" :data-item="item" style="width: 34%; text-align: center">
                                                {{ item.votetimes }}
                                            </view>

                                            <view class="my_tree_team_user_text" style="text-align: center; width: 34%" v-if="item.percent">{{ item.percent }}</view>
                                        </view>
                                    </block>
                                    <block v-else-if="">
                                        <text class="dialog_main_stop_data2">暂无数据</text>
                                    </block>
                                </view>
                            </scroll-view>
                        </block>
                        <view style="margin-bottom: 40rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 15">
                        <view class="team_info_item_top">
                            <view class="team_info_item_title">选择两个队内角色调换</view>
                        </view>
                        <view class="flex_row flex_center" v-for="(member, index) in myBalloonTeamCurrent.members_pos" :key="index">
                            <view @tap="locationTick" class="team_info_location_tick" :data-index="index">
                                <text v-if="member.tick">√</text>
                            </view>

                            <view class="team_info_line" style="width: 498rpx; margin-left: 20rpx; background-color: #ddebf3">
                                <view style="width: 176rpx; height: 100%; background-color: #2787fa; display: flex; align-items: center; border-radius: 14rpx 0 0 14rpx">
                                    <image class="team_info_line_area_image" :src="member.img"></image>
                                </view>
                                <image
                                    class="team_info_line_user_image"
                                    :src="
                                        member.userinfo
                                            ? member.userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2023/05/19/8a55e2a0f899e6fa7654ad32243a3ed7.png'
                                    "
                                ></image>
                                <text class="team_info_line_text" style="width: 220rpx; color: #333333">{{ member.userinfo ? member.userinfo.nickname : '空缺中' }}</text>
                            </view>
                        </view>
                        <!-- <view class="team_info_item_top" style="height: auto;">
                    <view class="team_info_item_title" style="font-size: 12px;color:#333333">调换后，对应的榜单数值不变</view>
                </view> -->
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_cancel_btn">取消</view>
                            <view @tap="changeLocationReq" class="dialog_main_confirm_btn">确定</view>
                        </view>
                        <view style="margin-bottom: 40rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 16">
                        <view class="team_info_item_top">
                            <view class="team_info_item_title">转让队长</view>
                        </view>
                        <view class="flex_row flex_center" v-if="!member.is_captain && member.userinfo" v-for="(member, index) in myBalloonTeamCurrent.members_pos" :key="index">
                            <view @tap="capTick" class="team_info_location_tick" :data-userid="member.userinfo._id" style="border-radius: 50%">
                                <text v-if="member.userinfo._id == changeCapUserid">√</text>
                            </view>

                            <view class="team_info_line" style="width: 498rpx; margin-left: 20rpx">
                                <image class="team_info_line_area_image" :src="member.img"></image>
                                <image
                                    class="team_info_line_user_image"
                                    :src="
                                        member.userinfo
                                            ? member.userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2023/01/17/5f6b4ead93065f50ecf91a43cf0651ef.png'
                                    "
                                ></image>
                                <text class="team_info_line_text" style="width: 250rpx">{{ member.userinfo ? member.userinfo.nickname : '空缺中' }}</text>
                            </view>
                        </view>
                        <view class="team_info_item_top" style="height: auto">
                            <view class="team_info_item_title" style="font-size: 12px">成功转让后将不可撤回该操作，请谨慎操作</view>
                        </view>
                        <view class="dialog_button_box">
                            <view @tap="dialogCloseClick" class="dialog_main_cancel_btn">取消</view>
                            <view @tap="changeCapReq" class="dialog_main_confirm_btn">确定</view>
                        </view>
                        <view style="margin-bottom: 40rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 17">
                        <view class="dialog_text01">更换成员上场</view>
                        <view class="dialog_main_confirm_btn2" style="position: relative">
                            邀请好友
                            <button
                                :data-team_id="myBalloonTeamCurrent.team_id"
                                openType="share"
                                style="width: 397rpx; height: 74rpx; top: 0; left: 0; position: absolute; opacity: 0"
                            ></button>
                        </view>
                        <text class="dialog_text02" style="font-weight: 600">邀请好友加入队伍，好友发起入队申请后，再去申请列表通过时选择需要更换的成员，即可换人上场</text>
                        <view @tap="handleTeamApply" class="dialog_main_confirm_btn2" :data-item="myBalloonTeamCurrent">去申请列表</view>
                        <text class="dialog_text02" style="font-weight: 600">去申请列表通过时选择需要更换的成员，即可换人上场</text>
                        <view style="height: 20rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 18">
                        <view class="dialog_text01">解锁拼图</view>
                        <text class="dialog_text02" style="font-weight: 600">解锁本块拼图可为当前魅力类型增加20000点魅力值</text>
                        <text class="dialog_text02" style="font-weight: 600">还有机会获得神秘道具</text>
                        <view class="flex_row" style="margin-top: 40rpx">
                            <text class="dialog_text02" style="font-weight: 600; width: 250rpx; text-align: center">解锁费用：\n < text style="color: #F45F96;">4000点体力点</text>
                            <text class="dialog_text02" style="font-weight: 600; width: 250rpx; text-align: center">
                                <text style="color: #f45f96">剩余</text>
                                体力点:\n{{ goodwillNumLittle }}
                            </text>
                        </view>
                        <view class="flex_row">
                            <view @tap="dialogCloseClick" class="dialog_main_confirm_btn" :data-item="myBalloonTeamCurrent">取消</view>
                            <view @tap="unlockJigsaw" class="dialog_main_confirm_btn" :data-item="myBalloonTeamCurrent">解锁当前拼图</view>
                        </view>
                        <text class="dialog_text02" style="font-weight: 600; color: #666666">每人每天在同个魅力类型最多可解锁2块拼图</text>
                        <view style="height: 20rpx"></view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 19">
                        <image
                            src="https://star-img.xingxiaoculture.com/2023/04/11/25233a2a15da713cb1db1ff9d089478a.png"
                            style="width: 180rpx; height: 180rpx; margin-top: -90rpx"
                        ></image>
                        <text class="dialog_text01" style="text-align: center; line-height: 50rpx">抱歉，当前体力点数量不足\n无法解锁</text>
                        <view @tap="handleGuihuaWindow" class="dialog_main_stop_btn" :data-type="0">获取体力点</view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 20">
                        <image
                            src="https://star-img.xingxiaoculture.com/2023/04/11/25233a2a15da713cb1db1ff9d089478a.png"
                            style="width: 180rpx; height: 180rpx; margin-top: -90rpx"
                        ></image>
                        <text class="dialog_text01" style="text-align: center; line-height: 50rpx">
                            抱歉， 每人每天在同个魅力类型最多可解锁2块拼图，您当前无法在这个魅力类型继续解锁
                        </text>
                        <view @tap="dialogCloseClick" class="dialog_main_stop_btn" :data-type="0">我知道了</view>
                    </block>
                    <block v-else-if="bTipsWindowShowType == 21">
                        <image
                            src="https://star-img.xingxiaoculture.com/2023/04/10/1b1e249f4b0ab718ff069298d15b02a5.png"
                            style="width: 180rpx; height: 180rpx; margin-top: -90rpx"
                        ></image>
                        <text class="dialog_text01" style="text-align: center; line-height: 50rpx">
                            成功解锁当前拼图，为当前魅力类型增加了
                            <text style="color: #f45f96">{{ unlockJigsawResult.charm_num }}点</text>
                            魅力值
                        </text>
                        <view
                            class="flex_row flex_center"
                            style="margin-top: 20rpx; background-color: #fbe7fc; border-radius: 19rpx; padding: 10rpx; margin-bottom: 20rpx"
                            v-if="unlockJigsawResult.tool_num"
                        >
                            <image
                                mode="aspectFit"
                                src="https://star-img.xingxiaoculture.com/2023/04/12/adfc5ea942f36bfea76b714e73860f82.png"
                                style="width: 96rpx; height: 96rpx"
                            ></image>
                            <view class="flex_column flex_start" style="width: 416rpx; font-size: 14px; margin-left: 10rpx; text-align: center">
                                <text style="color: #f45f96; font-size: 13px">{{ unlockJigsawResult.tool_num }}个手幅</text>
                                <text style="color: #333333; font-size: 12px">本次获得神秘道具</text>
                            </view>
                        </view>
                        <view @tap="dialogCloseClick" class="dialog_main_stop_btn" :data-type="0">我知道了</view>
                    </block>
                </view>
            </view>
        </block>
        <view v-else>
            <view class="mask">
                <view class="dialog1" style="padding: 28rpx">
                    <view class="dialog1-title">您还未设置主公会哦</view>
                    <text class="dialog1-txt" style="margin-top: 10rpx">本次比赛会以【公会】为主题，所以需要先将您加入的公会设置为主公会，才可正常参与比赛哦~</text>
                    <view class="dialog1-view1">
                        <view>如何设置主公会</view>
                        <view style="margin-top: 10rpx">在对应的聊天群内参与公会集结，按照提示操作，即可设置为主公会；</view>
                    </view>
                </view>
            </view>
        </view>
        <view class="dialog_view flex-c_center" v-if="isShowDialogAreaFuture">
            <view class="img-bg-tips-area2-scrollview">
                <scroll-view :scrollY="true" style="height: 900rpx">
                    <view class="img-bg-tips-area2">
                        <view class="group_pk_prizes_list2">
                            <view class="group_pk_prizes_list_line"></view>
                            <view class="group_pk_prizes_list_text2">{{ pageDataObj.bp_time_arr[dataPhase].title }}</view>
                        </view>
                        <text class="act_rank_list_tip4">{{ pageDataObj.bp_time_arr[dataPhase].phase_from_to }}</text>
                        <view class="content_area_future_box">
                            <view :class="'content_area_future_' + (index % 2 == 0 ? 'left' : 'right')" v-for="(item, index) in areaFutureList" :key="index">
                                <view class="content_area_future">
                                    <view class="content_area_future_title" :style="item.title_len > 3 ? 'font-size:18px;padding-top: 70rpx;' : ''">{{ item.title }}</view>
                                    <view class="content_area_future_score">{{ item.score }}积分</view>
                                </view>
                            </view>
                        </view>
                    </view>
                </scroll-view>
            </view>
        </view>
        <view class="dialog_view flex-c_center" v-if="isShowDialogShortName">
            <view class="img-bg-tips-area3">
                <view class="content_shortname_set_title">
                    {{ bShowDialogShortNameType == 1 ? '修改队伍名称' : bShowDialogShortNameType == 2 ? '修改队伍口号' : '公会简称设置' }}
                </view>
                <input
                    @input="inputShortName"
                    class="content_shortname_set_input"
                    maxlength="30"
                    :placeholder="
                        bShowDialogShortNameType == 1
                            ? '请输入队伍名称，6个字符以内'
                            : bShowDialogShortNameType == 2
                            ? '请输入队伍口号，20个字符以内'
                            : '请输入公会简称，2个字符以内'
                    "
                    placeholderStyle="color: #333333;"
                    type="text"
                    :value="
                        bShowDialogShortNameType == 1 ? myBalloonTeamCurrent.team_name : bShowDialogShortNameType == 2 ? myBalloonTeamCurrent.slogan : groupShortNameObj.shortname
                    "
                />
                <view class="content_shortname_set_button_box">
                    <view @tap="closeAreaInfo" class="content_shortname_set_button_cancel">取消</view>
                    <view @tap="updateShortnameSet" class="content_shortname_set_button_confirm">保存</view>
                </view>
                <view class="dialog_close">
                    <image @tap="closeAreaInfo" class="dialog_close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"></image>
                </view>
            </view>
        </view>
        <view class="dialog_view flex-c_center" v-if="isShowDialogGuihuaCompose">
            <view class="dialog_main_view">
                <view class="dialog_close" v-if="isShowDialogGuihuaType != 2">
                    <image @tap="dialogCloseClickGuihua" class="dialog_close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"></image>
                </view>
                <block v-if="isShowDialogGuihuaType == 0">
                    <view class="dialog_text01">{{ tool_s_name }}数量不足</view>
                    <text class="dialog_main_stop_data">当前剩余{{ goodwillNumLittle }}{{ tool_s_unit }}{{ tool_s_name }}</text>
                    <view @tap="handleGuihuaWindow" class="dialog_main_stop_btn" :data-type="0">前往获取</view>
                </block>
                <block v-else-if="isShowDialogGuihuaType == 1">
                    <view class="guihua_item">
                        <image class="guihua_image" mode="aspectFit" src="https://star-img.xingxiaoculture.com/2022/12/09/32f103335811747533a1d5cf951c4474.png"></image>
                        <view class="guihua_title_box">
                            <view class="guihua_title">{{ tool_b_name }}</view>
                            <view class="guihua_desc">使用{{ tool_s_name }}兑换{{ tool_b_name }}，给喜欢的TA赠送，最终会以{{ tool_b_name }}的数量进行排名</view>
                        </view>
                    </view>
                    <view class="guihua_text1">1{{ tool_b_unit }}{{ tool_b_name }}=3{{ tool_s_unit }}{{ tool_s_name }}</view>
                    <view class="guihua_text2">当前已有{{ tool_s_name }}{{ goodwillNumLittle }}{{ tool_s_unit }}</view>
                    <view class="guihua_text2">可兑换{{ tool_b_name }}{{ composeWinNum }}{{ tool_b_unit }}</view>
                    <view class="guihua_num_box">
                        <view class="guihua_title" style="width: 100%; font-size: 13px; text-align: center">
                            消耗{{ tool_s_name }}数量:{{ composeWinNum * 3 }}{{ tool_s_unit }}
                        </view>
                    </view>
                    <view @tap="handleGuihuaWindow" class="dialog_main_stop_btn" :data-type="1">兑换{{ tool_b_name }}</view>
                </block>
                <view
                    class="guihua_box"
                    style="background-image: url(https://star-img.idol001.com/2022/08/12/bc5f3e468db15381ccd63a3a53cebeea.png); background-size: 100% 100%"
                    v-else-if="isShowDialogGuihuaType == 2"
                >
                    <image class="guihua_gif" src="https://star-img.xingxiaoculture.com/2022/08/10/13eca019c2f3aa8c8ec9f97a32405bec.gif"></image>
                    <view class="guihua_title" style="font-size: 13px; margin-top: 58rpx">{{ tool_b_name }}制作中，请稍等，预计还有{{ timeoutLimitWine }}s完成</view>
                </view>
                <view class="guihua_box" v-else-if="isShowDialogGuihuaType == 3">
                    <image class="guihua_image2" mode="aspectFit" src="https://star-img.xingxiaoculture.com/2022/12/09/32f103335811747533a1d5cf951c4474.png"></image>
                    <view class="guihua_title">兑换成功</view>
                    <text class="dialog_main_stop_data" style="text-align: center">
                        消耗{{ tool_s_name }}
                        <text style="color: #ff5d5d">{{ genWineResult.flower_used_num }}</text>
                        {{ tool_s_unit }}， 成功制作
                        <text style="color: #ff5d5d">{{ genWineResult.wine_num }}</text>
                        {{ tool_b_unit }}{{ tool_b_name }}， 还剩余
                        <text style="color: #ff5d5d">{{ genWineResult.flower_left_num }}</text>
                        {{ tool_s_unit }}{{ tool_s_name }}
                    </text>
                    <view @tap="dialogCloseClickGuihua" class="dialog_main_stop_btn">知道了</view>
                </view>
            </view>
        </view>
        <view class="dialog_view flex-c_center" v-if="isShowDialogGift && pageDataObj.user_access_activity > 0">
            <view class="dialog_main_view" style="width: 670rpx">
                <view class="dialog_close">
                    <image @tap="dialogCloseClickGift" class="dialog_close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"></image>
                </view>
                <view class="dialog_text01">已自动为您打开海岛礼包*{{ pageDataObj.user_access_activity }}个</view>
                <text class="dialog_main_stop_data" style="font-size: 14px; margin-top: 20rpx">礼包内容明细：</text>
                <view class="flex_row flex_center" style="margin-top: 10rpx">
                    <image
                        mode="aspectFit"
                        src="https://star-img.xingxiaoculture.com/2023/05/18/c39de985438398246fa1b3ef85cb9ad3.png"
                        style="width: 114rpx; height: 114rpx; border-radius: 10rpx"
                    ></image>
                    <view class="flex_column flex_start" style="width: 416rpx; font-size: 14px; margin-left: 10rpx">
                        <text style="color: #333333">{{ tool_s_name }}*{{ pageDataObj.user_access_activity }}{{ tool_s_unit }}</text>
                        <text style="color: #333333">使用{{ tool_s_name }}收集艺人的魅力时刻获得大量奖励</text>
                    </view>
                </view>
                <view class="flex_row flex_center" style="margin-top: 10rpx">
                    <image
                        mode="aspectFit"
                        src="https://star-img.xingxiaoculture.com/2023/05/15/8a2a2a1de24d8c60ef1cfe8093d8696d.png"
                        style="width: 114rpx; height: 114rpx; border-radius: 10rpx"
                    ></image>
                    <view class="flex_column flex_start" style="width: 416rpx; font-size: 14px; margin-left: 10rpx">
                        <text style="color: #333333">{{ tool_b_name }}*{{ pageDataObj.user_access_activity }}{{ tool_b_unit }}</text>
                        <text style="color: #333333">{{ tool_b_name }}可以在本次活动中为星球赠送</text>
                    </view>
                </view>
                <text style="font-size: 14px; color: #ff6800; margin-top: 20rpx">后续获得的礼包将自动为您打开，并发放到账~</text>
                <view @tap="dialogCloseClickGift" class="dialog_main_stop_btn">知道了</view>
            </view>
        </view>
        <view
            class="dialog_view flex-c_center"
            style="background-color: rgba(0, 0, 0, 0.5); z-index: 199"
            v-if="isShowTopBanner && isShowTopBannerManager && pageDataObj.animation && pageDataObj.animation.animation"
        >
            <image class="tree_info_back" mode="aspectFit" :src="pageDataObj.animation.animation"></image>
            <view class="tree_info_back_banner" :style="'background: url(' + pageDataObj.animation.bg + ');background-size: 100% 100%;'">
                <image :src="pageDataObj.animation.starball_img" style="width: 80rpx; height: 80rpx; border-radius: 50%; margin-left: 20rpx"></image>
                <image :src="pageDataObj.animation.userinfo.image.thumbnail_pic" style="width: 80rpx; height: 80rpx; position: absolute; right: 10rpx; border-radius: 50%"></image>
                <text :style="'color:' + pageDataObj.animation.color + ';line-height: 40rpx;margin-left: 10rpx;width:450rpx;font-size: 13px;'">
                    <text style="color: #ff5d26">{{ pageDataObj.animation.userinfo.nickname }}</text>
                    为
                    <text style="color: #ff5d26">{{ pageDataObj.animation.starball_name }}</text>
                    获得卡片
                    <text style="color: #ff5d26">{{ pageDataObj.animation.card }}</text>
                    ，为星球增加了
                    <text style="color: #ff5d26">200万的{{ tool_b_name }}</text>
                </text>
            </view>
        </view>
        <view class="dialog_view flex-c_center" v-if="isShowDialogExceed && pageDataObj.notify_user_exceed">
            <view class="dialog_main_view" style="width: 670rpx">
                <view class="dialog_close">
                    <image @tap="dialogCloseClickExceed" class="dialog_close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"></image>
                </view>
                <image src="https://star-img.xingxiaoculture.com/2023/04/10/1b1e249f4b0ab718ff069298d15b02a5.png" style="width: 180rpx; height: 180rpx; margin-top: -90rpx"></image>
                <text class="dialog_text01" style="font-size: 14px; text-align: center; line-height: 40rpx">
                    您在{{ pageDataObj.notify_user_exceed.pos_name }}排名中成功反超登顶！ 获得以下奖励
                </text>
                <view class="flex_row flex_center" style="margin-top: 10rpx; background-color: #fbe7fc; border-radius: 19rpx; padding: 10rpx">
                    <image mode="aspectFit" src="https://star-img.xingxiaoculture.com/2023/02/02/5a928b32b289adafc116e719d120bd75.png" style="width: 96rpx; height: 96rpx"></image>
                    <view class="flex_column flex_start" style="width: 416rpx; font-size: 14px; margin-left: 10rpx; text-align: center">
                        <text style="color: #f45f96; font-size: 13px">{{ pageDataObj.notify_user_exceed.prize_num }}点魅力值</text>
                        <text style="color: #333333; font-size: 12px">
                            {{ pageDataObj.notify_user_exceed.order ? '该时段第' + pageDataObj.notify_user_exceed.order : '本' }}次反超奖励
                        </text>
                    </view>
                </view>
                <view @tap="dialogCloseClickExceed" class="dialog_main_stop_btn">知道了</view>
            </view>
        </view>
        <view class="dialog" v-if="exchangeXinyi">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <image @tap="closeExchange" class="dialog-close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"></image>
                <view class="dialog-content">
                    <view class="dialog-top">
                        <image class="goods-img" :src="goodsInfo.icon"></image>
                        <view class="goods-info">
                            <view class="goods-name">{{ goodsInfo.name }}</view>
                            <view class="goods-desc">{{ goodsInfo.description }}</view>
                        </view>
                    </view>
                    <view class="dialog-middle">
                        <view class="select-title">选择购买数量</view>
                        <view class="num-select-list">
                            <view
                                @tap="selectGood"
                                :class="'select-item ' + (item.num == selectedNum ? 'select-item-active' : '')"
                                :data-num="item.num"
                                v-for="(item, index) in goodsInfo.list"
                                :key="index"
                            >
                                <image class="select-img" :src="item.img"></image>

                                <view>
                                    <view class="select-num">{{ item.num_str }}</view>
                                    <view class="select-price">{{ item.diamond }}公会币</view>
                                </view>
                            </view>
                        </view>
                        <view class="custom-buy">
                            <view class="" style="width: 192rpx; display: block; box-sizing: border-box">自定义购买数量</view>
                            <input
                                @input="inputNum"
                                class="weui-input"
                                maxlength="12"
                                placeholder="输入购买数量"
                                placeholderStyle="color: #666666;"
                                style="width: 306rpx; height: 44rpx; display: block; box-sizing: border-box"
                                type="number"
                                :value="selectedNum"
                            />
                        </view>
                        <view class="money-item">
                            <view>余额：{{ userCoin }}公会币</view>
                            <view>
                                支付：
                                <text class="pink-word">{{ selectedNumCoin }}公会币</text>
                            </view>
                        </view>
                        <view @tap="buyIt" class="buy-btn">立即购买</view>
                    </view>
                </view>
            </view>
        </view>
        <view class="dialog" v-if="coinNoEnough">
            <view class="dialog-cover"></view>
            <view class="dialog-view">
                <image @tap="closeCoinExchange" class="dialog-close" src="https://star-img.xingxiaoculture.com/2022/10/11/4f68c1d6f8c2868ef11b16bd19c1e91f.png"></image>
                <view class="dialog-content">
                    <view class="goods-name">公会币不足</view>
                    <view class="goods-name">当前剩余{{ userCoin }}公会币</view>
                    <view @tap="coinExchange" class="buy-btn">前往获取</view>
                </view>
            </view>
        </view>
        <view class="dialog" v-if="isShowTeamUserInfoSingle">
            <view class="dialog-cover"></view>
            <view class="dialog-view" style="width: 650rpx; display: flex; align-items: center; flex-direction: column">
                <image @tap="hideTeamUserInfoSingle" class="dialog-close" src="https://star-img.xingxiaoculture.com/2022/10/11/4f68c1d6f8c2868ef11b16bd19c1e91f.png"></image>
                <text class="dialog_text01">成员贡献详情</text>
                <view class="team_info_my_info">
                    <image style="border-radius: 50%; height: 52rpx; width: 52rpx; margin-left: 5rpx" :src="thisTeamUser.userinfo.image.thumbnail_pic"></image>
                    <text style="font-size: 12px; color: #333333; margin-left: 5rpx; width: 200rpx">{{ thisTeamUser.userinfo.nickname }}</text>
                    <text style="font-size: 11px; color: #333333; width: 260rpx">
                        共收集魅力值
                        <text style="color: #f45f96">{{ thisTeamUser.votetimes }}</text>
                    </text>
                    <text style="font-size: 11px; color: #333333; width: 100rpx; text-align: center">
                        全队占
                        <text style="color: #f45f96">{{ thisTeamUser.percent }}</text>
                    </text>
                </view>
                <view class="team_info_my_info_table">
                    <view class="team_info_my_info_table_head">
                        <view class="team_info_my_info_table_item" style="background-color: #ffd1ed; border: none">魅力类型</view>
                        <view class="team_info_my_info_table_item" style="background-color: #ffd1ed; border: none">场下助力收集</view>
                        <view class="team_info_my_info_table_item" style="background-color: #ffd1ed; border: none">场上收集</view>
                        <view class="team_info_my_info_table_item" style="background-color: #ffd1ed; border: none">拼图解锁</view>
                    </view>
                    <view
                        class="team_info_my_info_table_line"
                        v-if="item.help_num || item.playground_num || item.jigsaw_num"
                        v-for="(item, index) in thisTeamUser.pos_list"
                        :key="index"
                    >
                        <view class="team_info_my_info_table_item" style="color: #f45f96">
                            <!-- <image style="width:100rpx;height:34rpx" src="{{item.img}}"></image> -->
                            {{ loactionArr[index] }}
                        </view>

                        <view class="team_info_my_info_table_item">
                            {{ item.help_num ? item.help_num : '未助力' }}
                        </view>

                        <view class="team_info_my_info_table_item">
                            {{ item.playground_num ? item.playground_num : '未上场' }}
                        </view>

                        <view class="team_info_my_info_table_item">
                            {{ item.jigsaw_num ? item.jigsaw_num : '未解锁' }}
                        </view>
                    </view>
                </view>
                <!-- <view class="act_rank_list_more" style="text-decoration: underline;">查看各个魅力类型详情 ></view> -->
            </view>
        </view>
        <view class="dialog" v-if="isShowBuffDetail">
            <view class="dialog-cover"></view>
            <view class="dialog-view" style="width: 650rpx; display: flex; align-items: center; flex-direction: column">
                <image @tap="hideBuffDetail" class="dialog-close" src="https://star-img.xingxiaoculture.com/2022/10/11/4f68c1d6f8c2868ef11b16bd19c1e91f.png"></image>
                <text class="dialog_text01">{{ tool_b_name }}加成说明</text>
                <text class="dialog_main_stop_data" style="font-size: 14px; margin-top: 20rpx">
                    当前星球总加成：
                    <text style="color: #ec4195">{{ teamBuffCountText }}</text>
                </text>
                <scroll-view style="width: 601rpx; max-height: 800rpx" :scroll-y="true">
                    <view class="team_buff_box" v-for="(item, index) in teamBuffList" :key="index">
                        <view class="team_buff_box_cornor">+{{ item.buff }}</view>

                        <text class="dialog_main_stop_data" style="margin: 0; margin-top: 40rpx">
                            <text style="color: #ec4195">本星球成员</text>
                            赠送{{ tool_b_name }}时 额外增加{{ item.buff }}{{ tool_b_name }}
                        </text>

                        <view class="team_buff_box_line">
                            <text style="width: 386rpx; text-align: center">为星球争夺该{{ tool_b_name }}加成的队伍</text>
                            <view class="team_buff_box_line_block">
                                <text>{{ item.day }}</text>
                                比赛
                            </view>
                        </view>

                        <text class="dialog_main_stop_data" style="margin: 0; margin-top: 20rpx; font-size: 13px; font-weight: 600">{{ item.team_name }}</text>

                        <view class="flex_center flex_row" style="margin-top: 10rpx">
                            <view
                                class="flex_column"
                                style="align-items: center; margin: 6rpx"
                                @tap="gotoDressRoom"
                                :data-userid="member._id"
                                v-for="(member, index1) in item.members"
                                :key="index1"
                            >
                                <image style="width: 68rpx; height: 68rpx; border-radius: 50%" :src="member.image.thumbnail_pic"></image>

                                <text style="font-size: 9px; color: #333333; text-align: center; width: 108rpx; margin-top: 5rpx">{{ member.nickname }}</text>
                            </view>
                        </view>
                    </view>
                </scroll-view>
                <view style="width: 142rpx; background-color: #f45f96; color: #ffffff; text-align: center; font-size: 11px">注意事项</view>
                <view style="color: #f45f96; margin-top: 10rpx; margin-bottom: 20rpx; text-align: center; font-size: 11px">
                    卡片为星球增加的{{ tool_b_name }}不能使用{{ tool_b_name }}加成
                </view>
            </view>
        </view>
        <!-- 左下角弹幕 -->
        <view class="left_damu_box" v-if="leftDamuShow && isShowTopDanmu">
            <view class="left_damu_line">
                <image class="left_damu_image" :src="leftDamuNow.userinfo.image.thumbnail_pic"></image>
                <text class="left_damu_nickname">{{ leftDamuNow.userinfo.nickname }}</text>
            </view>
            <view class="left_damu_text">给您助力了舞台魅力值：{{ leftDamuNow.text }}</view>
            <view class="left_damu_corner">+{{ leftDamuNow.num }}点</view>
        </view>
        <!-- 首次进入弹窗 -->
        <view class="dialog_view flex-c_center" v-if="isShowDialogEnter">
            <view class="dialog_main_view" style="width: 634rpx">
                <view class="dialog_close">
                    <image @tap="closeAreaInfo" class="dialog_close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png"></image>
                </view>
                <image src="https://star-img.xingxiaoculture.com/2023/05/19/5b2522f72b992ac0ad9b79466091d34d.png" style="width: 634rpx; height: 180rpx"></image>
                <view class="dialog_text01">背景故事</view>
                <view class="dialog_main_stop_data" style="font-size: 14px; margin-top: 20rpx">
                    在神秘的520那天，公会的成员们因为神秘力量的感召，突然漂流到了一个美丽的小岛上。
                </view>
                <view class="dialog_main_stop_data" style="font-size: 14px; margin-top: 20rpx">
                    在探索过程中，大家发现小岛上有着令人陶醉的音乐声，于是他们决定一起探寻这背后的秘密。
                </view>
                <view class="dialog_main_stop_data" style="font-size: 14px; margin-top: 20rpx">
                    经过不断的寻找，他们发现了一个隐藏在小岛上的音乐盛典——夏日畅想曲：海岛音乐节！在这里，大家可以尽情享受美妙的音乐、畅玩各种活动，并为自己的艺人争取丰厚的奖励。
                </view>
                <view class="dialog_main_stop_data" style="font-size: 14px; margin-top: 20rpx">同时，大家也可以一起建设这个美丽的小岛，让它变得更加美好！</view>

                <view @tap="closeAreaInfo" class="dialog_main_stop_btn">参加音乐节</view>
                <text style="font-size: 14px; color: #333333; margin-bottom: 50rpx; font-weight: 600" @tap="gotoIsland">进入小岛></text>
            </view>
        </view>

        <login_panel @closeViewEvent="closeLoginView" @loginCallback="loginCallback" v-if="bShowLoginPanel"></login_panel>
        <february_month_handsel
            @closeDialogClick="closeSendStatusDialog"
            @showVoteGuide="showVoteGuide"
            :dialogType="sendStatusDialogType"
            :starObject="starObject"
            :starRankType="starRankType"
            :zoneInfo="zoneInfo"
            v-if="showSendStatusDialog"
        ></february_month_handsel>
        <vote_panel
            :arenaInfo="arenaInfo"
            :zhuliPos="zhuliPos"
            @closeViewEvent="closeVoteView"
            @notEnoughEvent="notEnoughVote"
            @voteCallback="voteCallback"
            :myBalloonTeam="myBalloonTeamCurrent"
            :votePanelViewType="votePanelViewType"
            v-if="bShowVotePanel"
        ></vote_panel>
        <vote_panel_balloon_record
            @closeViewEvent="hideBalloonRecordPanel"
            :tabIndexSe="bShowBalloonRecordPanelIndexSe"
            :recordType="bShowBalloonRecordPanelType"
            v-if="bShowBalloonRecordPanel"
        ></vote_panel_balloon_record>
        <vote_panel_balloon_rule @closeViewEvent="hideBalloonRulePanel" v-if="bShowBalloonRulePanel"></vote_panel_balloon_rule>
        <vote_panel_balloon_star @chooseBalloonStar="selectStar" @closeViewEvent="hideBalloonStarPanel" v-if="bShowBalloonStarPanel"></vote_panel_balloon_star>
        <vote_panel_balloon_create_team
            :arenaInfo="arenaInfo"
            @closeViewEvent="hideBalloonCreateTeamPanel"
            @createTeamResultEvent="createTeamResult"
            :type="levelTab"
            v-if="bShowBalloonCreateTeamPanel"
        ></vote_panel_balloon_create_team>
        <vote_panel2
            v-if="bShowVotePanel2"
            :votePanelViewType="votePanelViewType2"
            :votePanelPlaceholder="votePanelPlaceholder"
            :votePanelStar="votePanelStar"
            :giftTermObj="giftTermObj"
            @voteCallback="voteCallback2"
            @closeViewEvent="closeVoteView2"
            @notEnoughEvent="notEnoughVote"
        ></vote_panel2>
        <vote_panel_balloon_location
            v-if="bShowBalloonLocationPanel"
            @chooseBalloonLocation="chooseBalloonLocation"
            @closeViewEvent="hideBalloonLocationPanel"
        ></vote_panel_balloon_location>
    </view>
</template>

<script>
import login_panel from '../../../component/login_panel/login_panel';
import february_month_handsel from '../../component/february_month/february_month_handsel';
import vote_panel from '../../component/6s/vote_panel';
import vote_panel_balloon_record from '../../component/6s/vote_panel_balloon_record';
import vote_panel_balloon_rule from '../../component/6s/vote_panel_balloon_rule';
import vote_panel_balloon_star from '../../component/6s/vote_panel_balloon_star';
import vote_panel_balloon_create_team from '../../component/6s/vote_panel_balloon_create_team';
import vote_panel2 from '../../component/6s/vote_panel2';
import vote_panel_balloon_location from '../../component/6s/vote_panel_balloon_location';
// subpage_v3/pages/january_month_s/january_month_formal.js
//1月s级正式 by-yuxianli
var net = require('../../../commonscript/common/netLoad.js');
var IDLoginManager = require('../../../commonscript/common/IDLoginManager.js');
var globalData = require('../../../commonscript/common/globalData.js');
var JumpManager = require('../../../commonscript/common/JumpManager.js');
var utils = require('../../../utils/util.js');
export default {
    components: {
        login_panel,
        february_month_handsel,
        vote_panel,
        vote_panel_balloon_record,
        vote_panel_balloon_rule,
        vote_panel_balloon_star,
        vote_panel_balloon_create_team,
        vote_panel2,
        vote_panel_balloon_location
    },
    //生命周期函数--监听页面加载
    data() {
        return {
            main_activity_id: '202305S',
            main_activity_id_api: '202305s',
            is_login: false,
            bShowLoginPanel: false,
            user_info: globalData.idolUserInfo,
            windowWidth: uni.getSystemInfoSync().windowWidth,
            tool_s_name: '贝币',
            tool_s_unit: '个',
            tool_b_name: '海洋之心',
            tool_b_unit: '个',
            selectDate: ['水晶海之战', '召唤海神雨', '星球分榜', '公会总榜', '个人总榜'],
            selectIndex: 0,
            selectStarIndex: 0,
            topViewHeight: 0,
            topViewBottom: 0,
            nowPageScrollValue: 0,
            pageViewBottom1: 0,
            pageViewBottom2: 0,
            pageViewBottom3: 0,
            pageViewBottom4: 0,
            pageViewBottom5: 0,
            scheduleActSelectIndex: 0,
            actCountdownTimeStr: '',
            actSupplyCountdownTimeStr: '',
            actArenaCountdownTimeStr: '',
            treeBossCountdownTimeStr: '',
            clickIndex: 0,
            isShowDialog: false,

            //其他提示弹窗
            showDialogType: 0,

            //0.犇犇团介绍  1.休整日期  2.活动结束  3.情人节霸屏预告  4.除夕霸屏公布  5.活动结算中  6.获得玫瑰途径
            isShowDialogArea: false,

            //区域弹窗
            isShowDialogAreaFuture: false,

            //未来分榜弹窗
            isShowDialogShortName: false,

            //简称设置
            bShowDialogShortNameType: 1,

            //1是队伍名称；2是队伍口号；3是公会简称
            isShowDialogEnter: false,

            //首次弹窗
            isShowDialogMeet: false,

            //遇见TA弹窗
            isShowDialogGift: true,

            //拆礼包弹窗
            isShowDialogExceed: true,

            //反超登顶弹窗
            isShowDialogGiftInfo: false,

            //礼包信息弹窗
            isShowDialogEnterFrame: false,

            //万圣小镇
            dialogTypeTitle: ['萌虎团介绍', '休整日期', '活动结束', '霸屏预告', '霸屏公布', '活动结算中'],

            bTipsWindowShow: false,
            bTipsWindowShowType: 21,
            bTipsWindowShowType3ShowPrice: false,
            bShowFulizhan: false,

            //福利站展开与收起

            getResultMsg: '',

            getBenefitsObj: {},

            getMakeKnownObj: {
                title: '',
                star_rank_per_phase_bom: [],
                fill_station_list: [],
                star_rank_per_phase: []
            },

            pageDataObj: {
                single_user_rank_info_all_time: {
                    rank: 0,

                    userinfo: {
                        _id: '',

                        image: {
                            middle_pic: ''
                        },

                        nickname: ''
                    },

                    starinfo: {
                        logo_img: false,
                        name: ''
                    },

                    starball: {
                        logo_url: '',
                        name: ''
                    },

                    vote_times: '',

                    max_starinfo: {
                        logo_img: ''
                    },

                    score: '',
                    buff_list: '',
                    starid: '',
                    card_list: '',
                    card_num: '',
                    top3group: '',
                    top3user: '',
                    text: '',
                    max_starid: 0
                },

                bp_time_arr: '',
                phase: '',
                exceed_msg_list: [],
                user_vote_stars_per_phase: [],
                star_rank_per_phase: [],
                group_rank_banner: '',

                group_rank: {
                    user_group: [],
                    list: []
                },

                user_rank_banner: '',
                user_rank_all_time: [],
                user_select_starid: '',
                vote_times_array: [],
                unblock_reward_arr: [],
                now_time: 0,
                end_time: 0,
                my_starball_list: [],
                user_access_activity: 0,

                animation: {
                    animation: '',
                    bg: '',
                    starball_img: '',

                    userinfo: {
                        image: {
                            thumbnail_pic: ''
                        },

                        nickname: ''
                    },

                    color: '',
                    starball_name: '',
                    card: ''
                },

                notify_user_exceed: {
                    pos_name: '',
                    prize_num: '',
                    order: false
                }
            },

            bgTimeCombineObj: {},
            isIphoneX: false,
            isGroupUser: false,

            //是否是官方群用户

            areaInfo: {},

            //弹窗地区信息模块
            areaList: [],

            //地区列表分块
            areaZoneInfo: {},

            //弹窗地区信息模块
            dataPhase: -1,

            //当前阶段
            areaFutureList: [],

            //未来阶段分榜
            groupShortNameObj: {
                shortname: ''
            },

            //公会简称设置对象
            eventMessageList: [],

            //事件消息
            danmuList: [],

            //弹幕数据
            danmuIndex: 0,

            //弹幕当前序号
            danmuOneTurnNum: 50,

            //一轮最多弹幕条数
            danmuOneTurnSecond: 30,

            //一轮时间
            guessMessageList: [],

            //竞猜消息时间
            messageBottom: '',

            //消息列表最后一个

            //点亮星球列表
            voteAllList: [],

            halfMsg: '',

            //赠送玫瑰部分
            spMedal: '',

            //加成铭牌
            goodwillNum: 0,

            //实际投票道具
            goodwillNumLittle: 0,

            //小道具
            userData: {
                user_fragment_num: 0,
                user_attack_num: 0,
                user_defend_num: 0,
                grab_times_num: 0
            },

            //用户道具信息汇总
            exchangeXinyi: false,

            // 心意情书兑换弹窗
            exchangeType: 'heart_love_letter',

            //购买类型：雪人(heart_love_letter), 雪(sugar), 魔法碎片(fragment), 抢夺次数(grab_times)
            showSendStatusDialog: false,

            //赠送窗口
            sendStatusDialogType: 0,

            starObject: {},
            zoneInfo: {},
            starRankType: '',
            isFirshLoad: true,
            starIdList: '',
            starNameList: '',
            xinYiNum: 0,

            // 心意情书数量
            goodsInfo: {
                icon: '',
                name: '',
                description: '',
                list: []
            },

            // 商品信息
            selectedNum: 0,

            // 当前购买数量
            selectedNumCoin: 0,

            // 当前购买数量需要的公会币
            coinRate: 1.2,

            //道具应援币兑换比例
            userCoin: 0,

            // 用户拥有的公会币
            coinNoEnough: false,

            // 公会币不足弹窗
            starList: [],

            //星球总榜
            allStarObject: {},

            // 星球总榜数据
            endText: '',

            // 活动结束提示语
            zhuliPos: -1,

            //助力的位置
            intervalTimeoutFengzhengStop: false,

            //暂停风筝计时
            teamInfoTab: 0,

            //tab
            isShowTeamUserInfoSingle: false,

            //用户单个信息弹窗
            thisTeamUser: {
                userinfo: {
                    image: {
                        thumbnail_pic: ''
                    },

                    nickname: ''
                },

                votetimes: '',
                percent: '',
                pos_list: []
            },

            //当前展示的用户数据
            thisHasCard: {
                card_name: '',
                card_img: ''
            },

            //展示当前获得卡数据
            teamBuffList: [],

            //buff详情展示
            teamBuffCountText: '',

            //计算展示
            isShowBuffDetail: false,

            //详情展示

            //放气球/点亮星球
            bShowVotePanel: false,

            //组队制作XX
            bShowVotePanel2: false,

            //放置/顶XX
            bShowBalloonStarPanel: false,

            bShowBalloonCreateTeamPanel: false,
            bShowBalloonLocationPanel: false,
            votePanelViewType: 1,

            //组队制作XX
            votePanelViewType2: 1,

            //放置/顶XX
            votePanelPlaceholder: {},

            votePanelStar: {},
            bShowBalloonRecordPanel: false,
            bShowBalloonRecordPanelIndexSe: 0,
            bShowBalloonRecordPanelType: 0,
            bShowBalloonRulePanel: false,
            transX: 1,

            //祝福语初始位置
            openCloud: false,

            //是不是进来就要打开编织云彩
            openAction: '',

            //是不是进来就要执行动作
            isCaptain: 0,

            //是否隊長
            notAgreeNum: 0,

            //未同意数量
            allBalloonteam: [],

            //气球小队列表
            myBalloonTeam: [],

            //我的气球小队
            myBalloonTeamHelp: [],

            //我的气球小队
            myBalloonTeamDivide: {
                1: 0,
                2: 0,
                3: 0
            },

            //我的气球小队分场次数量
            myBalloonTeamNext: [],

            //明天的队伍
            myBalloonTeamNextDivide: {
                1: 0,
                2: 0,
                3: 0
            },

            //我的气球下场小队分场次数量
            myBalloonTeamCurrent: {
                team_name: '',
                star_tree: '',
                members: '',
                term_id: '',
                members_pos: [],
                team_id: '',
                is_captain: '',
                expect_text: '',
                user_list: '',

                my_info: {
                    userinfo: {
                        image: {
                            thumbnail_pic: ''
                        },

                        nickname: ''
                    },

                    votetimes: '',
                    percent: '',
                    text: ''
                },

                slogan: ''
            },

            //当前的队伍信息
            arenaText: '',

            //擂台描述文字
            arenaTermClose: 0,

            //是否提前结束
            bShowBalloonDateSelect: false,

            //擂台日期选择
            balloonTermId: '',

            //用户切换的擂台id
            balloonTermDate: '',

            //用户切换的擂台日期
            arenaInfo: null,

            //当前时刻的擂台信息
            arenaList: [],

            //到当前所有擂台场次
            isShowDialogGuihuaCompose: false,

            //桂花合成系列弹窗
            isShowDialogGuihuaType: 0,

            //桂花合成系列弹窗类型
            composeWinNum: 0,

            //桂花合成
            timeoutLimitWine: 0,

            //酿酒倒计时
            genWineResult: null,

            //酿酒返回
            joinTeamid: null,

            //申请加入队伍id
            joinTeamInfo: null,

            //申请加入队伍信息
            treeBoss: null,

            //下一个或者当前的树BOSS
            daySharePrizeWord: '',

            //分享奖励文案
            allBalloonteamTree: [],

            //果园
            dismissTeamid: '',

            //解散队伍id
            quitTeamid: '',

            //退出队伍id
            getoutTeamid: '',

            //移除成员队伍弹id
            getoutMember: {
                userinfo: {
                    image: {
                        middle_pic: ''
                    },

                    nickname: ''
                },

                user_votetimes_num: false
            },

            //移除成员信息
            currentIndex: 0,

            //swiper index
            swipheight: [],

            //swiper 高度集合
            levelTab: 0,

            //1初级场；2中级场；3高级场
            teamNumMax: 3,

            //每人可加队伍数上限
            teamPeopleNumMax: 5,

            //每个队伍人数上限
            isShowTopBanner: true,

            //顶部横幅/动画展示完毕与否
            isShowTopBannerManager: false,

            //顶部动画用户人为开关
            topBannerArr: [{}, {}, {}],

            //首页标语
            topBannerIndex: 0,

            //顶部标语序号
            isShowTopDanmu: false,

            //顶部弹幕
            chrDarkImg: '',

            chrLightImg: '',
            chrLightALL: null,
            openGiftResult: {},

            //拆礼包结果
            giftDateList: [],

            //到当前所有放置礼物日期
            bShowGiftDateSelect: false,

            //放置礼物日期选择
            giftTermObj: null,

            //用户term数据
            giftPositionInfo: {},

            //点击的礼盒位置数据
            arenaBanner: '',

            //制作道具banner地址
            tabIndex: 0,

            //top3列表切换
            locaationTop3: [[], [], [], [], []],

            loactionArr: ['海洋女神', '星光能源师', '星辉能源师', '潮汐之翼', '波涛咒语师'],

            loactionDescArr: [
                [
                    {
                        hl: 0,
                        text: '可支援其他任何位置\n但消耗为'
                    },
                    {
                        hl: 1,
                        text: '1.3'
                    },
                    {
                        hl: 0,
                        text: '倍'
                    }
                ],
                [
                    {
                        hl: 0,
                        text: '负责投入'
                    },
                    {
                        hl: 1,
                        text: '星能源'
                    },
                    {
                        hl: 0,
                        text: '\n船只前进需要消耗能源'
                    }
                ],
                [
                    {
                        hl: 0,
                        text: '负责投入'
                    },
                    {
                        hl: 1,
                        text: '星能源'
                    },
                    {
                        hl: 0,
                        text: '\n船只前进需要消耗能源'
                    }
                ],
                [
                    {
                        hl: 0,
                        text: '负责提升'
                    },
                    {
                        hl: 1,
                        text: '行驶速度'
                    }
                ],
                [
                    {
                        hl: 0,
                        text: '可攻击前一名船只\n以降低其'
                    },
                    {
                        hl: 1,
                        text: '行驶速度'
                    }
                ]
            ],

            locationTickArr: [],

            //存打钩的数组
            changeCapUserid: '',

            //转让队长userid

            waterColumn: 0,

            waterRow: 0,

            areanStatusToNum: {
                not_begin: 0,
                first_half: 1,
                half_time: 2,
                second_half: 3,
                is_end: 4
            },

            forTest: ['', '', '', '', '', '', ''],

            unlockJigsawResult: {
                charm_num: '',
                tool_num: ''
            },

            unlockJigsawObj: {},

            //坐下助力弹幕
            leftDamuList: [],

            leftDamuNow: {
                userinfo: {
                    image: {
                        thumbnail_pic: ''
                    },

                    nickname: ''
                },

                text: '',
                num: ''
            },

            leftDamuShow: false,

            //星球下个人总排名
            userRankInStar: {
                starball: {
                    name: ''
                },

                user_rank_banner: '',
                user: '',
                list: []
            },

            userRankInStarSelectStarid: 0,

            userRankInStarImg: {
                68541: 'https://star-img.xingxiaoculture.com/2023/05/20/c31463d6dae0bad909635de21f79acf2.png',
                7460: 'https://star-img.xingxiaoculture.com/2023/05/20/dfbe3be455efb62a00dd949bc9da3985.png',
                7643: 'https://star-img.xingxiaoculture.com/2023/05/20/056d0a192416974bc2ba416b109d9347.png',
                6821: 'https://star-img.xingxiaoculture.com/2023/05/20/7399c147fac7c1f143289bcd86b97092.png',
                0: 'https://star-img.xingxiaoculture.com/2023/05/20/85cf12338fa282d58504389e2befe5aa.png'
            },

            tabClickable: false,

            //活动阶段时间
            timeoutLimit: 0,

            intervalTimeoutAct: null,

            //补给阶段时间
            timeoutLimitSupply: 0,

            intervalTimeoutActSupply: null,

            //树BOSS
            timeoutLimitTreeBoss: 0,

            intervalTimeoutTreeBoss: null,

            //擂台时间
            timeoutLimitArena: 0,

            intervalTimeoutActArena: null,

            //酿酒时间
            intervalTimeoutWine: null,

            //祝福语轮播
            intervalTimeoutWordRoll: null,

            //风筝整场时间
            timeoutLimitFengzhengAll: 0,

            intervalTimeoutActFengzhengAll: null,

            //风筝获得卡牌倒计时
            intervalTimeoutFengzheng: null,

            //弹幕
            intervalTimeoutDanmu: null,

            bpTimeCombine: '',
            actFengzhengAllCountdownTimeStr: '',
            showLine: '',
            showScore: '',
            b_item: '',

            c_item: {
                card_img: '',

                userinfo: {
                    _id: '',

                    image: {
                        thumbnail_pic: ''
                    },

                    nickname: ''
                }
            },

            g_item: {
                top3user: '',
                group_name: '',
                votetimes: ''
            },

            gu_item: {
                userinfo: {
                    _id: '',

                    image: {
                        thumbnail_pic: ''
                    }
                }
            },

            u_item: {
                userinfo: {
                    _id: '',

                    image: {
                        thumbnail_pic: ''
                    },

                    nickname: ''
                },

                votetimes: ''
            },

            member: {
                img: '',
                userinfo: false,
                is_captain: '',
                fruit_num: '',
                level_percent: '',
                tick: '',
                _id: '',

                image: {
                    thumbnail_pic: ''
                },

                nickname: ''
            },

            thumbnail_pic: '',

            titem: {
                hl: false,
                text: ''
            },

            tindex: 0,

            m_item: {
                userinfo: {
                    image: {
                        middle_pic: ''
                    },

                    nickname: ''
                },

                votetimes: '',
                time: '',
                percent: ''
            },

            p_item: {
                userid: ''
            },

            p_index: 0,
            slogan: '',
            in_team: '',
            middle_pic: '',

            people: {
                team_score_rank: 0,
                score: '',
                tools: '',
                diff: 0,

                userinfo: {
                    _id: '',
                    nickname: ''
                }
            },

            citem: {
                highlight: 0,
                text: ''
            },

            term_id: '',
            phase_title_img: '',
            phase_img: '',
            phase_from_to: '',
            showStar: false,
            circleindex: 0,
            origin_pic: '',
            title: '',
            phase_text: '',
            phase_arr: '',
            phase_from: '',
            phase_to: '',

            myGuardStarItem: {
                starinfo: {
                    name: ''
                }
            },

            mitem: {
                userinfo: {
                    _id: '',

                    image: {
                        thumbnail_pic: ''
                    }
                }
            },

            userinfo: {
                _id: '',

                image: {
                    thumbnail_pic: false,
                    middle_pic: ''
                },

                nickname: ''
            },

            applyUserinfo: {
                userinfo: {
                    image: {
                        middle_pic: ''
                    },

                    nickname: ''
                },

                user_votetimes_num: ''
            },

            myBalloonTeamCurrentId: '',

            bossInfo: {
                beat_boss_need_level: '',
                beat_boss_drop_fruit: '',
                beat_boss_drop_gift_img: '',
                beat_boss_drop_gift: ''
            },

            showTreeInfoObj: {
                star_tree: '',
                team_name: '',
                in_team: '',
                members: '',
                team_id: ''
            },

            myRankObject: {
                userinfo: {
                    nickname: ''
                }
            },

            grabResult: {
                grab_candy_percent: '',
                grab_candy_num: '',
                title: '',
                content: '',
                code: 0
            }
        };
    },
    onLoad: function (options) {
        uni.showShareMenu({
            withShareTicket: true
        });
        var that = this;
        uni.getSystemInfo({
            success: function (res) {
                console.log('getSystemInfo res.platform ==' + res.platform);
                if (res.model && res.model.match('iPhone X')) {
                    that.setData({
                        isIphoneX: true
                    });
                }
            }
        });
        IDLoginManager.stepLoginState();
        this.loginLogicStar();
        var date = new Date().getDate();
        if (!uni.getStorageSync(this.main_activity_id + '_topbannerclose_' + date)) {
            that.setData({
                isShowTopBannerManager: true
            });
        }
        // if(!wx.getStorageSync(this.data.main_activity_id+"_topdanmuclose_"+date)){
        //   oThis.setData({ isShowTopDanmu: true });
        // }
        this.getCountStatus();
        if (!uni.getStorageSync(this.main_activity_id + '_enter')) {
            that.showDialogEnter();
            uni.setStorageSync(this.main_activity_id + '_enter', 1);
        }

        // if(options.cloud){
        //   oThis.setData({ openCloud: options.cloud })
        // }
        if (options.action) {
            that.setData({
                openAction: options.action
            });
        }
    },
    //生命周期函数--监听页面初次渲染完成
    onReady: function () {
        this.initTabView();
    },
    //生命周期函数--监听页面显示
    onShow: function () {
        this.getUserCoin();
        this.initGoodwillNum();
        this.getArenaRank();
    },
    //生命周期函数--监听页面隐藏
    onHide: function () {},
    //生命周期函数--监听页面卸载
    onUnload: function () {
        clearInterval(this.intervalTimeoutAct);
        clearInterval(this.intervalTimeoutActSupply);
        clearInterval(this.intervalTimeoutActArena);
        clearInterval(this.intervalTimeoutWordRoll);
    },
    //页面相关事件处理函数--监听用户下拉动作
    onPullDownRefresh: function () {
        this.initPageData();
        this.getCountStatus();
    },
    //页面上拉触底事件的处理函数
    onReachBottom: function () {},
    //用户点击右上角分享
    onShareAppMessage: function (res) {
        if (res.target) {
            let team_id = res.target.dataset.team_id;
        } else {
        }
        if (res.target) {
            let action = res.target.dataset.action;
        } else {
            let action = '';
        }
        let pos = res.target ? res.target.dataset.pos : '';
        var p = {};
        if (res.from === 'button') {
            if (team_id) {
                if (action == 'zhuli') {
                    var path =
                        '/subpage_v3/pages/202305S/activity_formal?action=' +
                        encodeURIComponent(
                            JSON.stringify({
                                name: 'open_tool_panel',
                                num: team_id
                            })
                        );
                    console.log('sharePath', path);
                    p = {
                        title: '助力我的队伍，一起获得超值奖励~',
                        path: path,
                        imageUrl: 'https://star-img.xingxiaoculture.com/2023/05/18/770aa32fac270aa8ce277801d9dd4e18.png'
                    };
                } else {
                    p = {
                        title: '加入我的队伍，一起水晶海之战得超值福利~',
                        path: '/subpage_v3/pages/202305S/activity_team_joinlist?team_id=' + team_id + '&pos=' + pos,
                        imageUrl: 'https://star-img.xingxiaoculture.com/2023/05/18/770aa32fac270aa8ce277801d9dd4e18.png'
                    };
                }
            } else {
                //this.getDaySharePrize();
                if (this.thisHasCard) {
                    var thisHasCard = this.thisHasCard;
                    var title = '星球' + thisHasCard.starball.name + '获得了' + thisHasCard.card_name + '卡片，为星球增加了200万爱心';
                } else {
                    var title = '【海岛音乐节，夏日畅想曲】活动进行中~';
                }
                p = {
                    title: title,
                    path: '/subpage_v3/pages/202305S/activity_formal',
                    imageUrl: 'https://star-img.xingxiaoculture.com/2023/05/18/770aa32fac270aa8ce277801d9dd4e18.png'
                };
            }
        } else {
            p = {
                title: '【海岛音乐节，夏日畅想曲】活动进行中~',
                path: '/subpage_v3/pages/202305S/activity_formal',
                imageUrl: 'https://star-img.xingxiaoculture.com/2023/05/18/770aa32fac270aa8ce277801d9dd4e18.png'
            };
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
    onPageScroll: utils.debounce(function (res) {
        this.setData(
            {
                nowPageScrollValue: res[0].scrollTop
            },
            function () {
                //console.log('rich text scrollTop', this.data.nowPageScrollValue);
                if (this.tabClickable) {
                    this.tabClickable = false;
                    return;
                }
                var topHeight = this.nowPageScrollValue / 2 - this.topViewHeight * 6;
                console.log('rich topHeight', topHeight);
                if (/*topHeight >= this.data.pageViewBottom1 && */ topHeight < this.pageViewBottom2) {
                    this.setSelectIndex(0);
                } else if (topHeight >= this.pageViewBottom2 && topHeight < this.pageViewBottom3) {
                    this.setSelectIndex(1);
                } else if (topHeight >= this.pageViewBottom3 && topHeight < this.pageViewBottom4) {
                    this.setSelectIndex(2);
                } else if (topHeight >= this.pageViewBottom4 && topHeight < this.pageViewBottom5) {
                    this.setSelectIndex(3);
                } else if (topHeight >= this.pageViewBottom5) {
                    this.setSelectIndex(4);
                } else {
                    this.setSelectIndex(0);
                }
            }
        );
    }),
    methods: {
        /* 获取是否官方群 */
        requestGetGroupMainList() {
            var oThis = this;
            var url = 'https://data.idol001.com/api_activity_2021_08s.php?action=get_group_main_list';
            var params = {};
            net.fetchApi(url, params, 'GET').then((res) => {
                let { is_office_group, list } = res.data;
                if (is_office_group == 1 && list.length > 0) {
                    this.setData({
                        isGroupUser: true
                    });
                    // wx.setStorageSync("userGroupList", list);
                    this.getDanmuList();
                } else {
                    this.setData({
                        isGroupUser: false
                    });
                }
            });
        },

        initTabView: function () {
            var that = this;
            if (this.topViewBottom == 0) {
                uni.createSelectorQuery()
                    .in(uni)
                    .selectAll('.tab_view')
                    .boundingClientRect(function (rects) {
                        rects.forEach(function (rect) {
                            console.log('rich text tabViewBottom', rect.bottom);
                            that.setData({
                                topViewBottom: rect.bottom - 100,
                                topViewHeight: rect.height
                            });
                        });
                    })
                    .exec();
                uni.createSelectorQuery()
                    .in(uni)
                    .selectAll('.tabPage1')
                    .boundingClientRect(function (rects) {
                        rects.forEach(function (rect) {
                            console.log('rich text pageViewBottom1', rect.top);
                            that.setData({
                                pageViewBottom1: rect.top - 100
                            });
                        });
                    })
                    .exec();
                uni.createSelectorQuery()
                    .in(uni)
                    .selectAll('.tabPage2')
                    .boundingClientRect(function (rects) {
                        rects.forEach(function (rect) {
                            console.log('rich text pageViewBottom2', rect.top);
                            that.setData({
                                pageViewBottom2: rect.top - 100
                            });
                        });
                    })
                    .exec();
                uni.createSelectorQuery()
                    .in(uni)
                    .selectAll('.tabPage3')
                    .boundingClientRect(function (rects) {
                        rects.forEach(function (rect) {
                            console.log('rich text pageViewBottom3', rect.top);
                            that.setData({
                                pageViewBottom3: rect.top - 100
                            });
                        });
                    })
                    .exec();
                uni.createSelectorQuery()
                    .in(uni)
                    .selectAll('.tabPage4')
                    .boundingClientRect(function (rects) {
                        rects.forEach(function (rect) {
                            console.log('rich text pageViewBottom4', rect.top);
                            that.setData({
                                pageViewBottom4: rect.top - 100
                            });
                        });
                    })
                    .exec();
                uni.createSelectorQuery()
                    .in(uni)
                    .selectAll('.tabPage5')
                    .boundingClientRect(function (rects) {
                        rects.forEach(function (rect) {
                            console.log('rich text pageViewBottom5', rect.top);
                            that.setData({
                                pageViewBottom5: rect.top - 100
                            });
                        });
                    })
                    .exec();
            }

            //this.openExchangeXinyi();
            this.initGoodwillNum();
        },

        setSelectIndex: function (index) {
            this.setData({
                selectIndex: index
            });
        },

        tabItemClick: function (e) {
            this.tabClickable = true;
            this.setData(
                {
                    selectIndex: e.currentTarget.dataset.index
                },
                function () {
                    if (this.selectIndex == 0) {
                        this.moveViewClick('.tabPage1');
                    } else if (this.selectIndex == 1) {
                        this.moveViewClick('.tabPage2');
                    } else if (this.selectIndex == 2) {
                        this.moveViewClick('.tabPage3');
                    } else if (this.selectIndex == 3) {
                        this.moveViewClick('.tabPage4');
                    } else if (this.selectIndex == 4) {
                        this.moveViewClick('.tabPage5');
                    }
                    this.dialogCloseClick();
                }
            );
        },

        moveViewClick: function (cls) {
            var that = this;
            uni.createSelectorQuery()
                .in(uni)
                .selectAll(cls)
                .boundingClientRect(function (rects) {
                    rects.forEach(function (rect) {
                        console.log('rich text 1111', that.nowPageScrollValue);
                        console.log('rich text 2222', rect.top);
                        console.log('rich text 3333', that.topViewHeight);
                        uni.pageScrollTo({
                            scrollTop: that.nowPageScrollValue + rect.top - that.topViewHeight * 2,
                            duration: 300
                        });
                    });
                })
                .exec();
        },

        getCountStatus: function (refresh) {
            var that = this;
            var url = 'https://data.idol001.com/api_2021_01s.php?action=get_count_status';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data && resp.data.ok == 1) {
                    that.setData({
                        isShowDialog: true,
                        showDialogType: 5,
                        getResultMsg: resp.data.msg
                    });
                } else if (refresh) {
                    that.setData({
                        isShowDialog: false
                    });
                    that.initPageData();
                }
            });
        },

        refreshPageData: function () {
            this.getCountStatus(true);
        },

        initPageData: function (option) {
            console.log('初始化页面数据');
            clearInterval(this.intervalTimeoutAct);
            clearInterval(this.intervalTimeoutActSupply);
            //clearInterval(this.intervalTimeoutWordRoll);
            this.timeoutLimit = 0;
            this.timeoutLimitSupply = 0;
            var that = this;
            var url = 'https://data.idol001.com/api_2021_01s.php?action=get_data';
            var starid = uni.getStorageSync(this.main_activity_id + '_starid');
            var params = {
                starid: starid
            };
            if (this.dataPhase != -1) {
                params['phase'] = this.dataPhase;
            }
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    uni.stopPullDownRefresh();
                    console.log(resp);
                    if (resp && resp.data) {
                        if (this.dataPhase != -1) {
                            resp.data.phase = this.dataPhase;
                        }
                        resp.data.phase_big = Math.floor(resp.data.phase / 2);
                        resp.data.phase_small = resp.data.phase % 2;
                        if (resp.data.now_time >= resp.data.bp_time_arr[resp.data.phase].begin_time && resp.data.now_time <= resp.data.bp_time_arr[resp.data.phase].end_time) {
                            that.timeoutLimit = resp.data.bp_time_arr[resp.data.phase].end_time - resp.data.now_time + new Date().getTime() / 1000;
                            that.initActInterval();
                            var this_timeoutLimitSupply = 0;
                            if (resp.data.bp_time_arr[resp.data.phase].fill_station_list) {
                                for (var fi in resp.data.bp_time_arr[resp.data.phase].fill_station_list) {
                                    if (resp.data.now_time < resp.data.bp_time_arr[resp.data.phase].fill_station_list[fi].timestamp) {
                                        this_timeoutLimitSupply =
                                            resp.data.bp_time_arr[resp.data.phase].fill_station_list[fi].timestamp - resp.data.now_time + new Date().getTime() / 1000;
                                        break;
                                    }
                                }
                            }
                            that.timeoutLimitSupply = this_timeoutLimitSupply;
                            if (that.timeoutLimitSupply) {
                                that.initActSupplyInterval();
                            } else {
                                that.setData({
                                    actSupplyCountdownTimeStr: '福利站时间已到达，最终结果已更新'
                                });
                            }
                        } else if (resp.data.now_time < resp.data.bp_time_arr[resp.data.phase].begin_time) {
                            that.setData({
                                actCountdownTimeStr: '本轮活动未开始'
                            });
                            that.setData({
                                actSupplyCountdownTimeStr: ''
                            });
                        } else {
                            // if(resp.data.phase%2==0){
                            //   oThis.setData({ actCountdownTimeStr: '上半场活动已结束' })
                            // }else if(resp.data.phase==resp.data.bp_time_arr.length-1){
                            //   oThis.setData({ actCountdownTimeStr: '本场活动已结束' })
                            // }else{
                            that.setData({
                                actCountdownTimeStr: '本轮活动已结束'
                            });
                            that.setData({
                                actSupplyCountdownTimeStr: '福利站时间已到达，最终结果已更新'
                            });
                            //}
                        }

                        var index = 0;
                        for (var i = 0; i < resp.data.bp_time_arr.length; i++) {
                            if (resp.data.bp_time_arr[i].status == 1 || resp.data.bp_time_arr[i].status == 2 || resp.data.bp_time_arr[i].rest_status == 1) {
                                index = i;
                            }
                            resp.data.bp_time_arr[i].title_count = resp.data.bp_time_arr[i].title.split('').length;
                            resp.data.bp_time_arr[i].title_bar = resp.data.bp_time_arr[i].title.slice(0, 2) + '\n' + resp.data.bp_time_arr[i].title.slice(2);
                            console.log('title_count:' + resp.data.bp_time_arr[i].title_count);
                        }
                        resp.data['vote_times_array'] = resp.data.single_user_rank_info_all_time.vote_times
                            ? resp.data.single_user_rank_info_all_time.vote_times.toString().split('')
                            : [0];
                        //resp.data['robot_vote_times_array'] = resp.data['unlock_robot'].user_votetimes ? resp.data['unlock_robot'].user_votetimes.toString().split('') : [0];
                        //resp.data['robot_vote_times_percent'] = Math.floor(resp.data['unlock_robot'].user_votetimes/resp.data['unlock_robot'].robot_votetimes*100);
                        //resp.data['robot_vote_times_percent'] = resp.data['robot_vote_times_percent']>100?100:resp.data['robot_vote_times_percent'];

                        //组合上下半场数据
                        var bpTimeCombine = [];
                        if (resp.data.bp_time_arr) {
                            for (var bp in resp.data.bp_time_arr) {
                                var bpd = Math.floor(bp / 2);
                                console.log(bpd);
                                resp.data.bp_time_arr[bp].phase = bp;
                                if (!bpTimeCombine[bpd]) {
                                    bpTimeCombine[bpd] = Object.assign({}, resp.data.bp_time_arr[bp]);
                                    bpTimeCombine[bpd].phase_arr = [];
                                    bpTimeCombine[bpd].phase_arr[0] = Object.assign({}, resp.data.bp_time_arr[bp]);
                                    console.log(bpTimeCombine);
                                } else {
                                    bpTimeCombine[bpd].phase_arr[1] = Object.assign({}, resp.data.bp_time_arr[bp]);
                                    bpTimeCombine[bpd].rest = resp.data.bp_time_arr[bp].rest;
                                    bpTimeCombine[bpd].rest_status = resp.data.bp_time_arr[bp].rest_status;
                                    bpTimeCombine[bpd].rest_time = resp.data.bp_time_arr[bp].rest_time;
                                    if (resp.data.now_time >= bpTimeCombine[bpd].phase_arr[0].begin_time && resp.data.now_time < bpTimeCombine[bpd].phase_arr[1].end_time) {
                                        bpTimeCombine[bpd].status = 1;
                                        bpTimeCombine[bpd].text = '进行中';
                                    }
                                }
                            }
                        }
                        //判断福利解锁分艺人的选中状态
                        if (resp.data.user_select_starid && resp.data.my_starball_list.length > 0) {
                            for (var msl in resp.data.my_starball_list) {
                                if (resp.data.user_select_starid == resp.data.my_starball_list[msl].starball.starid) {
                                    that.setData({
                                        selectStarIndex: msl
                                    });
                                }
                            }
                        }
                        // if(oThis.data.dataPhase==-1){
                        //   oThis.setData({
                        //     scheduleActSelectIndex: Math.floor(index/2) != 0 ? Math.floor(index/2)-1 : Math.floor(index/2)
                        //   })
                        // }
                        //地区列表分块
                        var areaList = [];
                        if (resp.data.area_list) {
                            for (var al in resp.data.area_list) {
                                var mod = Math.floor(al / 8);
                                if (typeof areaList[mod] !== 'object') {
                                    areaList[mod] = [];
                                }
                                areaList[mod].push(resp.data.area_list[al]);
                            }
                        }
                        console.log('areaList', that.scheduleActSelectIndex);
                        that.setData(
                            {
                                pageDataObj: resp.data,
                                bpTimeCombine: bpTimeCombine,
                                currentIndex: that.currentIndex >= areaList.length ? 0 : that.currentIndex,
                                areaList: areaList,
                                messageBottom: 'messageBottom' //如果消息是单独接口，就在单独那处理
                            },
                            function () {
                                that.initTabView();
                                that.getPositionInfoList();
                                //oThis.getEventMessageList();
                                //oThis.getGuessMessageList();

                                that.getArenaInfo();
                                that.getTopBannerList();
                                that.getUserRankingInStar();
                                //oThis.showMeetTaWindow();
                                that.doOpenAction();
                                console.log('bpTimeCombine', bpTimeCombine);
                                if (that.scheduleActSelectIndex == 0) {
                                    that.setData({
                                        scheduleActSelectIndex: index
                                    });
                                }
                            }
                        );
                        if (option && option.open_animi) {
                            that.setData({
                                isShowTopBanner: true
                            });
                        }
                        //烟花延时关闭
                        if (that.isShowTopBannerManager) {
                            that.closeFireTimeOut();
                        }
                        //左下角弹幕
                        that.getLeftDamuList();
                    } else {
                        uni.showToast({
                            title: resp.data.msg ? resp.data.msg : '网络异常，请稍后再试',
                            icon: 'none'
                        });
                    }
                },
                function (resp) {
                    uni.stopPullDownRefresh();
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
        },

        initActInterval: function () {
            var that = this;
            var t = that.timeoutLimit - new Date().getTime() / 1000;
            var et = that.timeChange(t);
            that.setData({
                actCountdownTimeStr: '本轮结束剩余' + et
            });
            that.intervalTimeoutAct = setInterval(function () {
                var t = that.timeoutLimit - new Date().getTime() / 1000;
                if (t >= 0) {
                    var et = that.timeChange(t);
                    that.setData({
                        actCountdownTimeStr: '本轮结束剩余' + et
                    });
                }
                if (that.timeoutLimit != 0 && new Date().getTime() / 1000 >= that.timeoutLimit) {
                    //oThis.setData({isShowDialog: true, showDialogType: 5})
                    that.getCountStatus();
                    that.initPageData();
                }
            }, 998);
        },

        initActSupplyInterval: function () {
            var that = this;
            var t = that.timeoutLimitSupply - new Date().getTime() / 1000;
            var et = that.timeChange(t);
            that.setData({
                actSupplyCountdownTimeStr: '距离福利站到达剩余' + et
            });
            that.intervalTimeoutActSupply = setInterval(function () {
                var t = that.timeoutLimitSupply - new Date().getTime() / 1000;
                if (t >= 0) {
                    var et = that.timeChange(t);
                    that.setData({
                        actSupplyCountdownTimeStr: '距离福利站到达剩余' + et
                    });
                }
                if (that.timeoutLimitSupply != 0 && new Date().getTime() / 1000 >= that.timeoutLimitSupply) {
                    //oThis.setData({isShowDialog: true, showDialogType: 5})
                    that.getCountStatus();
                    that.initPageData();
                }
            }, 998);
        },

        initTreeBossInterval: function () {
            var that = this;
            var t = that.timeoutLimitTreeBoss - new Date().getTime() / 1000;
            var et = that.timeChange(t);
            if (that.arenaInfo.current_time < that.treeBoss.begin_time) {
                that.setData({
                    treeBossCountdownTimeStr: et
                });
            } else {
                that.setData({
                    treeBossCountdownTimeStr: '进行中'
                });
            }
            that.intervalTimeoutTreeBoss = setInterval(function () {
                var t = that.timeoutLimitTreeBoss - new Date().getTime() / 1000;
                if (t >= 0) {
                    var et = that.timeChange(t);
                    if (that.arenaInfo.current_time < that.treeBoss.begin_time) {
                        that.setData({
                            treeBossCountdownTimeStr: et
                        });
                    } else {
                        that.setData({
                            treeBossCountdownTimeStr: '进行中'
                        });
                    }
                }
                if (that.timeoutLimitTreeBoss != 0 && new Date().getTime() / 1000 >= that.timeoutLimitTreeBoss) {
                    that.getArenaInfo();
                }
            }, 998);
        },

        initActArenaInterval: function () {
            var that = this;
            var t = that.timeoutLimitArena - new Date().getTime() / 1000;
            var et = that.timeChange(t);
            if (that.arenaInfo.status == 'not_begin') {
                that.setData({
                    actArenaCountdownTimeStr: '还剩' + et + '开始'
                });
            } else if (that.arenaInfo.status == 'first_half') {
                that.setData({
                    actArenaCountdownTimeStr: '还剩' + et + '上半场结束'
                });
            } else if (that.arenaInfo.status == 'half_time') {
                that.setData({
                    actArenaCountdownTimeStr: '还剩' + et + '下半场开始'
                });
            } else {
                that.setData({
                    actArenaCountdownTimeStr: '还剩' + et + '全场结束'
                });
            }
            that.intervalTimeoutActArena = setInterval(function () {
                var t = that.timeoutLimitArena - new Date().getTime() / 1000;
                if (t >= 0) {
                    if (!that.balloonTermId || that.balloonTermId == that.arenaInfo.term_id) {
                        var et = that.timeChange(t);
                        if (that.arenaInfo.status == 'not_begin') {
                            that.setData({
                                actArenaCountdownTimeStr: '还剩' + et + '开始'
                            });
                        } else if (that.arenaInfo.status == 'first_half') {
                            that.setData({
                                actArenaCountdownTimeStr: '还剩' + et + '上半场结束'
                            });
                        } else if (that.arenaInfo.status == 'half_time') {
                            that.setData({
                                actArenaCountdownTimeStr: '还剩' + et + '下半场开始'
                            });
                        } else {
                            that.setData({
                                actArenaCountdownTimeStr: '还剩' + et + '全场结束'
                            });
                        }
                    } else {
                        that.setData({
                            actArenaCountdownTimeStr: '本场次已结束'
                        });
                    }
                }
                if (that.timeoutLimitArena != 0 && new Date().getTime() / 1000 >= that.timeoutLimitArena) {
                    //oThis.setData({isShowDialog: true, showDialogType: 5})
                    that.getCountStatus();
                    that.getArenaInfo();
                }
            }, 998);
        },

        initWineInterval: function () {
            var that = this;
            this.setData({
                timeoutLimitWine: 3
            });
            if (!that.intervalTimeoutWine) {
                that.intervalTimeoutWine = setInterval(function () {
                    var t = that.timeoutLimitWine - 1;
                    if (t >= 0) {
                        if (t == 0 && that.timeoutLimitWine != 0) {
                            that.setData({
                                isShowDialogGuihuaType: 3
                            });
                        }
                        that.setData({
                            timeoutLimitWine: t
                        });
                        console.log(t);
                    }
                }, 998);
            }
        },

        initWordRollInterval: function () {
            var that = this;
            var t = that.transX;
            var i = 0;
            that.intervalTimeoutWordRoll = setInterval(function () {
                //  var t = oThis.data.transX-0.01;
                //  if(t<-1){
                //    var t = 1;
                //    oThis.setData({
                //      topBannerIndex: oThis.data.topBannerIndex+1>=oThis.data.topBannerArr.length?0: oThis.data.topBannerIndex+1
                //     })
                //  }
                //  oThis.setData({transX: t})

                if (that.leftDamuList.length > 0) {
                    that.setData({
                        leftDamuNow: that.leftDamuList[i],
                        leftDamuShow: true
                    });
                    setTimeout(() => {
                        that.setData({
                            leftDamuShow: false
                        });
                    }, 8000);
                    if (i >= that.leftDamuList.length) {
                        i = 0;
                    } else {
                        i++;
                    }
                }
            }, 10000);
        },

        initActFengzhengAllInterval: function () {
            var that = this;
            var t = that.timeoutLimitFengzhengAll - new Date().getTime() / 1000;
            var et = that.timeChange(t);
            if (t <= 0) {
                that.setData({
                    actFengzhengAllCountdownTimeStr: '本场次已结束'
                });
            } else {
                that.setData({
                    actFengzhengAllCountdownTimeStr: '当前场次还剩' + et + '结束'
                });
            }
            that.intervalTimeoutActFengzhengAll = setInterval(function () {
                var t = that.timeoutLimitFengzhengAll - new Date().getTime() / 1000;
                if (t >= 0) {
                    var et = that.timeChange(t);
                    that.setData({
                        actFengzhengAllCountdownTimeStr: '当前场次还剩' + et + '结束'
                    });
                } else {
                    that.setData({
                        actFengzhengAllCountdownTimeStr: '本场次已结束'
                    });
                }
            }, 998);
        },

        initFengzhengInterval: function () {
            var that = this;
            that.intervalTimeoutFengzheng = setInterval(function () {
                if (!that.intervalTimeoutFengzhengStop) {
                    var list = that.voteAllList;
                    for (var i in list) {
                        if (!list[i].data.card && list[i].data.userid && typeof list[i].data.prize_left_time != 'undefined') {
                            if (list[i].data.prize_left_time > 0) {
                                list[i].data.prize_left_time--;
                                list[i].data.left_time_text = that.timeChange(list[i].data.prize_left_time, 'mm:ss');
                            } else {
                                that.setData({
                                    intervalTimeoutFengzhengStop: true
                                });
                                that.getPositionInfoList();
                                break;
                            }
                        }
                    }
                    that.setData({
                        voteAllList: list
                    });
                }
            }, 1000);
        },

        timeChange: function (timestamp, format) {
            var d;
            var h;
            var m;
            var s;
            d = Math.floor(timestamp / 60 / 60 / 24);
            h = Math.floor((timestamp / 60 / 60) % 24);
            m = Math.floor((timestamp / 60) % 60);
            s = Math.floor(timestamp % 60);
            if (h < 10) {
                h = '0' + h;
            }
            if (m < 10) {
                m = '0' + m;
            }
            if (s < 10) {
                s = '0' + s;
            }
            if (format == 'mm:ss') {
                var last_time = (h != '00' ? h + ':' : '') + m + ':' + s;
            } else {
                var last_time = d != 0 ? d + '天' : '';
                last_time += h + '时' + m + '分' + s + '秒';
            }
            return last_time;
        },

        dateFormat(timestamp, format) {
            if (String(timestamp).length === 10) {
                timestamp = timestamp * 1000;
            }
            var date = new Date(timestamp);
            var Y = date.getFullYear();
            var M = date.getMonth() + 1;
            var D = date.getDate();
            var hour = date.getHours();
            var min = date.getMinutes();
            var sec = date.getSeconds();
            if (format === 'YYYY') {
                return Y; // 2021
            } else if (format === 'YYYY-MM') {
                // 2021-07
                return Y + '-' + (M < 10 ? '0' + M : M);
            } else if (format === 'YYYY-MM-DD') {
                // 2021-07-12
                return Y + '-' + (M < 10 ? '0' + M : M) + '-' + (D < 10 ? '0' + D : D);
            } else if (format === 'HH:mm:ss') {
                // 10:20:35
                return (hour < 10 ? '0' + hour : hour) + ':' + (min < 10 ? '0' + min : min) + ':' + (sec < 10 ? '0' + sec : sec);
            } else if (format === 'HH:mm') {
                // 10:20
                return (hour < 10 ? '0' + hour : hour) + ':' + (min < 10 ? '0' + min : min);
            } else if (format === 'mm:ss') {
                // 10:20
                return (min < 10 ? '0' + min : min) + ':' + (sec < 10 ? '0' + sec : sec);
            } else if (format === 'YYYY-MM-DD HH:mm') {
                // 2021-07-12 10:20
                return Y + '-' + (M < 10 ? '0' + M : M) + '-' + (D < 10 ? '0' + D : D) + ' ' + (hour < 10 ? '0' + hour : hour) + ':' + (min < 10 ? '0' + min : min);
            } else if (format === 'YYYY-MM-DD HH:mm:ss') {
                // 2021-07-12 10:20:35
                return (
                    Y +
                    '-' +
                    (M < 10 ? '0' + M : M) +
                    '-' +
                    (D < 10 ? '0' + D : D) +
                    ' ' +
                    (hour < 10 ? '0' + hour : hour) +
                    ':' +
                    (min < 10 ? '0' + min : min) +
                    ':' +
                    (sec < 10 ? '0' + sec : sec)
                );
            } else if (format === 'free_style') {
                // 10:20:35
                return (
                    '<text class="tree_boss_text3">' +
                    (hour < 10 ? '0' + hour : hour) +
                    '</text>时<text class="tree_boss_text3">' +
                    (min < 10 ? '0' + min : min) +
                    '</text>分<text class="tree_boss_text3">' +
                    (sec < 10 ? '0' + sec : sec) +
                    '</text>秒'
                );
            } else {
                return '--';
            }
        },

        activityrule: function (e) {
            uni.navigateTo({
                url: 'activity_rule?type=' + e.currentTarget.dataset.type
            });
        },

        scheduleTabClick: function (e) {
            var index = e.currentTarget.dataset.index;
            var that = this;
            console.log(index);
            if (this.pageDataObj.bp_time_arr[index].status != 0) {
                // this.showDialogMakeKnown(index);
                if (this.pageDataObj.bp_time_arr[index].status == 1) {
                    this.setData({
                        dataPhase: -1
                    });
                    this.getCountStatus();
                } else {
                    this.setData({
                        dataPhase: this.pageDataObj.bp_time_arr[index].phase
                    });
                }
                this.initPageData();
            } else {
                //this.showDialogBenefits(index);
                // var url = "https://data.xingxiaoculture.com/api_activity_'+this.data.main_activity_id_api+'.php?action=get_area_list";
                // var params = {
                //   'phase': index
                // };
                // net.fetchApi(url, params, 'GET').then(resp => {
                //   if (resp && resp.data) {
                //     oThis.setData({
                //       areaFutureList: resp.data,
                //       isShowDialogAreaFuture:true
                //     })
                //   }
                // }, function (resp) {
                //   wx.showToast({ title: '网络似乎不太顺畅哦', icon: 'none' });
                // });
            }
        },

        showDialogBenefits: function (index) {
            var that = this;
            var url = 'https://data.idol001.com/api_2021_01s.php?action=get_reward_info';
            var params = {
                phase: index
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        var end = resp.data.reward_img_arr.length;
                        resp.data['reward_img_arr_bom'] = resp.data.reward_img_arr.slice(1, end);
                        that.setData({
                            isShowDialog: true,
                            showDialogType: 3,
                            getBenefitsObj: resp.data,
                            clickIndex: index
                        });
                    }
                },
                function (resp) {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
        },

        showDialogMakeKnown: function (index) {
            var that = this;
            var url = 'https://data.idol001.com/api_2021_01s.php?action=get_bp_result_index';
            var params = {
                phase: index
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        for (var dd in resp.data.star_rank_per_phase) {
                            if (!resp.data.star_rank_per_phase[dd].reward_text || resp.data.star_rank_per_phase[dd].reward_text == '') {
                                dd--;
                                break;
                            }
                        }
                        resp.data['star_rank_per_phase_bom'] = resp.data.star_rank_per_phase.slice(0, dd + 1);
                        that.setData({
                            isShowDialog: true,
                            showDialogType: 4,
                            getMakeKnownObj: resp.data,
                            clickIndex: index
                        });
                    }
                },
                function (resp) {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
        },

        goToCouponPrizes: function () {
            if (!this.is_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            uni.navigateTo({
                url: '/subpage_v3/pages/january_month_s/january_month_s_final'
            });
        },

        stopDayTabClick: function (e) {
            if (this.pageDataObj.now_time >= this.pageDataObj.end_time) {
                this.setData({
                    isShowDialog: true,
                    showDialogType: 2
                });
                return;
            }
            // var index = e.currentTarget.dataset.index;
            // if (this.data.pageDataObj.bp_time_arr[index].rest_status==0) {
            //   return;
            // }
            this.setData({
                isShowDialog: true,
                showDialogType: 1
            });
        },

        lookMoreClick: function (e) {
            if (!this.is_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var type = e.currentTarget.dataset.type;
            var starid = e.currentTarget.dataset.starid;
            if (type == 0) {
                //霸屏排名
                uni.navigateTo({
                    url: '/subpage_v3/pages/star_rank_list/star_rank_list?phase=' + this.pageDataObj.phase
                });
            } else if (type == 1) {
                //排行总排行
                uni.navigateTo({
                    url: '/subpage_v3/pages/star_rank_all_list/star_rank_all_list'
                });
            } else if (type == 2) {
                //成员排名总排行
                uni.navigateTo({
                    url: '/subpage_v3/pages/star_fans_all_list/star_fans_all_list?' + (starid ? 'starid=' + starid : '')
                });
            } else if (type == 3) {
                //展示每个霸屏阶段我贡献的
                uni.navigateTo({
                    url: '/subpage_v3/pages/my_guard_list/my_guard_list?phase=' + this.pageDataObj.phase
                });
            } else if (type == 4) {
                //先锋队页面
                uni.navigateTo({
                    url: 'pioneer_list?starid=' + e.currentTarget.dataset.starid
                });
            } else if (type == 5) {
                //积分总榜
                uni.navigateTo({
                    url: 'star_rank_list'
                });
            }
        },

        goToDaBang: function (e) {
            if (!this.is_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            if (this.pageDataObj.now_time >= this.pageDataObj.end_time) {
                uni.showToast({
                    title: '霸屏活动已全部结束',
                    icon: 'none'
                });
                return;
            }
            var starId = e.currentTarget.dataset.starid;
            // if (!starId || starId == 0) {
            //   wx.switchTab({
            //     url: '/pages/idol_home/idol_home',
            //   })
            //   return
            // }
            // wx.navigateTo({
            //   url: '/pages/subpages_v2/pages/support_list/support_list_new?sid=' + starId
            // })
            uni.switchTab({
                url: '/pages/idol_home/idol_home'
            });
        },

        goToBengBengTuan: function (e) {
            if (!this.is_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var starId = e.currentTarget.dataset.starid;
            if (!starId || starId == 0) {
                return;
            }
            uni.navigateTo({
                url: '/subpage_v3/pages/spring_festival_act/benben_detail?sid=' + starId
            });
        },

        showDialogClick: function (e) {
            this.setData({
                isShowDialog: true,
                showDialogType: e.currentTarget.dataset.select
            });
        },

        showVoteGuide: function () {
            this.closeSendStatusDialog();
            this.setData({
                isShowDialog: true,
                showDialogType: 7,
                isShowDialogArea: false
            });
        },

        dialogCloseClick: function () {
            this.setData({
                isShowDialog: false,
                bTipsWindowShow: false
            });
        },

        loginLogicStar: function () {
            var that = this;
            console.log('登录状态', IDLoginManager.loginState());
            console.log('是否已经登录', IDLoginManager.is_has_login);
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                IDLoginManager.checkIsLogin(function (v) {
                    console.log(v);
                    if (v == IDLoginManager.is_not_login) {
                        console.log('未登录');
                        that.setData({
                            is_login: false,
                            bShowLoginPanel: true
                        });
                        that.initPageData();
                    } else {
                        console.log('已经登录');
                        that.setData({
                            is_login: true,
                            user_info: globalData.idolUserInfo
                        });
                        that.requestGetGroupMainList();
                        that.initPageData();
                    }
                });
            } else {
                that.setData({
                    is_login: true,
                    user_info: globalData.idolUserInfo
                });
                that.requestGetGroupMainList();
                that.initPageData();
            }
        },

        loginCallback: function (e) {
            var that = this;
            if (e.detail.success) {
                that.setData({
                    bShowLoginPanel: false
                });
                that.setData({
                    is_login: true,
                    user_info: globalData.idolUserInfo
                });
                that.initPageData();
            }
        },

        closeLoginView: function () {
            this.setData({
                bShowLoginPanel: false
            });
        },

        dialogGetBookClick: function (e) {
            var type = e.currentTarget.dataset.type;
            if (type == 0) {
                //this.postEvent({ schema_code: 'D001646Surxc099iwnddm1hn4v8qaqtm3' });
                this.setData({
                    isShowDialog: false
                });
                // wx.navigateTo({
                //   url: '/pages/subpages/pages/steal_supportStar_list/steal_supportStar_list'
                // })
                uni.navigateTo({
                    url: '/pages/subpages/pages/steal_supportStar_list/steal_supportStar_list'
                });
            } else if (type == 1) {
                //this.postEvent({ schema_code: 'D001646Szfjzxh4wsb97su32qrcc95sf5' });
                this.setData({
                    isShowDialog: false
                });
                // wx.switchTab({
                //   url: '/pages/rank_list/general_rank_list',
                // })
                uni.switchTab({
                    url: '/pages/idol_home/idol_home'
                });
            } else if (type == 2 || type == 4) {
                if (!this.is_login) {
                    this.setData({
                        bShowLoginPanel: true
                    });
                    return;
                }
                // if (type==2) { this.postEvent({ schema_code: 'D001646Sf8dhgdm73f0a2jiv3k49f9ls4' }); }
                // else if (type==4) { this.postEvent({ schema_code: 'D001646Sxqe9xk6vp1sfvwhaaxoeoe146' }); }
                this.setData({
                    isShowDialog: false
                });
                uni.navigateTo({
                    url: '/subpage_v3/pages/wish/wish?fsign=2'
                });
            } else if (type == 3) {
                // if (type==3) { this.postEvent({ schema_code: 'D001646Szwnfwt7danwszlnbv07s54dm2' }); }
                // else if (type==5) { this.postEvent({ schema_code: 'D001646Sezd9dqc5qt95pt6a1n2glgbz1' }); }
                // this.postEvent({ schema_code: 'D001646Szwnfwt7danwszlnbv07s54dm2' })
                //   this.postEvent({ schema_code: '' });
                // this.setData({ isShowDialog: true, showDialogType: 4 })
            } else if (type == 5) {
                //this.postEvent({ schema_code: 'D001646Sezd9dqc5qt95pt6a1n2glgbz1' })
                var exchangeType = e.currentTarget.dataset.exchange_type;
                this.openExchangeXinyi(exchangeType);
            }
        },

        //赠送玫瑰部分
        sendLeaderBoardClick: function (e) {
            if (this.pageDataObj.now_time > this.pageDataObj.bp_time_arr[this.pageDataObj.phase].end_time) {
                uni.showToast({
                    title: '该轮次已结束',
                    icon: 'none'
                });
                return;
            }
            var item = e.currentTarget.dataset.item;
            var innerItem = e.currentTarget.dataset.inneritem;
            var zoneInfo = e.currentTarget.dataset.zoneinfo;
            item['goodwill_num'] = item.vote_times; //zoneInfo.star_votetimes;
            console.log('zoneInfo', zoneInfo);

            // //地图分榜细列转换
            // item['starinfo'] = item.starball;
            // item['starinfo']['sid'] = item.starball.starid;
            // item['starinfo']['logo_img'] = item.starball.logo_url;
            //地图分榜细列转换

            zoneInfo['zone'] = zoneInfo.name; //之前用于地图分榜传值
            zoneInfo['zone_name'] = zoneInfo.title;
            zoneInfo['pic'] = zoneInfo.pic;
            this.setData({
                showSendStatusDialog: true,
                sendStatusDialogType: 0,
                starObject: item,
                starRankType: this.pageDataObj.phase,
                zoneInfo: zoneInfo
            });
        },

        closeSendStatusDialog: function () {
            this.setData({
                showSendStatusDialog: false
            });
            this.initGoodwillNum();
            //this.getUserSendAllStar();
        },

        openExchangeXinyi(exchangeType, selectedNum) {
            // 打开公会币兑换心意情书弹窗

            // wx.navigateTo({
            //   url: 'activity_vote_buy',
            // })
            // return;
            this.setData({
                selectedNum: selectedNum ? parseInt(selectedNum) : 0
            });
            if (exchangeType) {
                this.setData({
                    exchangeType: exchangeType
                });
            }
            var oThis = this;
            var url = 'https://data.idol001.com/api_2021_01s.php?action=get_heart_love_letter';
            var params = {
                type: this.exchangeType
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    // 获取心意情书列表
                    if (resp && resp.data) {
                        this.setData({
                            goodsInfo: resp.data.data
                        });
                        this.setData({
                            selectedNumCoin: Math.ceil(oThis.selectedNum * oThis.goodsInfo.diamond)
                        });
                    }
                },
                function (resp) {
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
            this.getUserCoin();
            this.setData({
                exchangeXinyi: true
            });
        },

        initGoodwillNum: function () {
            if (!this.is_login) {
                return;
            }
            var that = this;
            var url = 'https://data.idol001.com/api_2021_01s.php?action=get_user_votetimes_num';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data /*&& resp.data.user_votetimes_num*/) {
                    that.setData({
                        goodwillNum: resp.data.user_wine_num
                    });
                    that.setData({
                        goodwillNumLittle: resp.data.user_votetimes_num
                    });
                    that.setData({
                        userData: resp.data
                    });
                    //oThis.setData({ spMedal: resp.data.user_medal_img })
                } else {
                    that.setData({
                        goodwillNum: 0
                    });
                }
            });
        },

        getUserCoin() {
            // 获取用户拥有的公会币
            var oThis = this;
            const url = 'https://data.idol001.com/api_wxapp_list.php?action=user_rank_info';
            const params = {
                userid: globalData.idolUserInfo._id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        userCoin: resp.data.coin
                    });
                }
            });
        },

        selectGood(e) {
            // 选择商品
            let num = e.currentTarget.dataset.num;
            if (num) {
                this.setData({
                    selectedNum: num,
                    selectedNumCoin: Math.ceil(num * this.goodsInfo.diamond)
                });
            }
            console.log(this.userInfo);
        },

        inputNum(e) {
            // 自定义输入购买商品的数量
            let num = e.detail.value;
            this.setData({
                selectedNum: num ? parseInt(num) : ''
            });
            this.setData({
                selectedNumCoin: Math.ceil(this.selectedNum * this.goodsInfo.diamond)
            });
            console.log(this.selectedNum);
        },

        buyIt() {
            // 购买商品
            var oThis = this;
            if (oThis.selectedNum) {
                let needCoin = oThis.selectedNum * 15;
                // if(needCoin>oThis.data.userCoin){// 公会币不足
                //   console.log(oThis.data.userCoin);
                //       oThis.openCoinExchange();
                // }else{
                const url = 'https://data.idol001.com/api_2021_01s.php?action=buy_user_good_will_num';
                const params = {
                    num: oThis.selectedNum,
                    type: oThis.exchangeType
                };
                net.fetchApi(url, params, 'POST').then((resp) => {
                    if (resp.data) {
                        if (resp.data.ok == 200) {
                            uni.showToast({
                                title: '兑换成功',
                                icon: 'none'
                            });
                            this.getUserCoin();
                            this.initGoodwillNum();
                        } else if (resp.data.ok == 'no_enough') {
                            oThis.openCoinExchange();
                        } else {
                            uni.showToast({
                                title: resp.data.msg,
                                icon: 'none'
                            });
                        }
                    }
                });
                // }
            } else {
                uni.showToast({
                    title: '请选择或者输入需要兑换的数量',
                    icon: 'none'
                });
            }
        },

        closeExchange() {
            // 关闭公会币兑换心意情书弹窗
            this.setData({
                exchangeXinyi: false
            });
        },

        openCoinExchange() {
            // 公会币不足弹窗
            this.setData({
                coinNoEnough: true
            });
        },

        closeCoinExchange() {
            // 公会币不足弹窗
            this.setData({
                coinNoEnough: false
            });
        },

        coinExchange() {
            // 公会币兑换
            JumpManager.junmpToPayment();
            this.closeCoinExchange();
        },

        gotoPageClick: function (e) {
            if (!this.is_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var type = e.currentTarget.dataset.type;
            if (type == 0) {
                //前往许愿灯页面
                uni.navigateTo({
                    url: '/subpage_v3/pages/wish/wish'
                });
            } else if (type == 1) {
                //搜索艺人页面
                uni.navigateTo({
                    url: 'search_star_page'
                });
            } else if (type == 2) {
                //我守护的艺人页面
                this.postEvent({
                    schema_code: 'D001646Snq04rmcakmgghpzixpxnrv7d1'
                });
                uni.navigateTo({
                    url: '/subpage_v3/pages/february_month_formal/my_guard_star'
                });
            } else if (type == 3) {
                //个人贡献总榜页面
                this.postEvent({
                    schema_code: 'D001646Sixzg9eiqmw85f5t3esaosh4k0'
                });
                uni.navigateTo({
                    url: '/subpage_v3/pages/february_month_formal/user_rank_list'
                });
            } else if (type == 4) {
                //xxx榜详情页面
                var index = e.currentTarget.dataset.index;
                var name = e.currentTarget.dataset.name;
                var zone = e.currentTarget.dataset.zone;
                uni.navigateTo({
                    url: '/subpage_v3/pages/february_month_formal/star_rank_list?title=' + name + '&zone=' + zone
                });
            } else if (type == 5) {
                //星球总榜页面
                uni.navigateTo({
                    url: '/subpage_v3/pages/february_month_formal/all_star_rank'
                });
            }
        },

        bottomViewClick: function () {
            if (!this.is_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            if (this.pageDataObj.now_time >= this.pageDataObj.end_time) {
                this.setData({
                    showSendStatusDialog: true,
                    sendStatusDialogType: 3
                });
            } else {
                //this.postEvent({ schema_code: 'D001646Src6f0mhge96322of5zage9eo6' });
                if (this.goodwillNum > 0) {
                    uni.navigateTo({
                        url: 'search_star_page'
                    });
                } else {
                    //this.setData({ isShowDialog: true, showDialogType: 6 })
                    uni.showToast({
                        title: this.tool_b_name + '不足，请获取' + this.tool_b_name,
                        icon: 'none'
                    });
                }
            }
        },

        postEvent: function (params) {
            return;
            var url = 'https://data.idol001.com/api_chuanyun.php?action=post_event';
            if (IDLoginManager.loginState() == IDLoginManager.is_has_login) {
                params.F0000001 = new Date() - app.globalData.idolUserInfo.register_time > 86400 * 100 ? '2' : '1';
                params.F0000002 = app.globalData.idolUserInfo.idol_num;
                net.fetchApi(url, params, 'POST').then((resp) => {
                    console.log(resp.data);
                });
            } else {
                IDLoginManager.checkIsLogin(function (v) {
                    if (v == IDLoginManager.is_not_login) {
                        params.F0000001 = '3';
                    } else {
                        params.F0000001 = new Date() - app.globalData.idolUserInfo.register_time > 86400 * 100 ? '2' : '1';
                        params.F0000002 = app.globalData.idolUserInfo.idol_num;
                    }
                    net.fetchApi(url, params, 'POST').then((resp) => {
                        console.log(resp.data);
                    });
                });
            }
        },

        /* 点击更多粉丝群总榜 */
        handleFansMove: function (e) {
            const { groupStatue } = this;
            let param = `type=${groupStatue === '0' ? 1 : 2}`;
            console.log('点击更多粉丝群总榜', param);
            uni.navigateTo({
                url: `activity_xxfansqunR?${param}`
            });
        },

        /* 点击更多小队榜 */
        handleTeamMore: function (e) {
            let type = e.currentTarget.dataset.type ? e.currentTarget.dataset.type : 0;
            let action = e.currentTarget.dataset.action ? e.currentTarget.dataset.action : '';
            let team_id = e.currentTarget.dataset.team_id ? e.currentTarget.dataset.team_id : '';
            let term_id = e.currentTarget.dataset.term_id ? e.currentTarget.dataset.term_id : '';
            let loc = typeof e.currentTarget.dataset.loc != 'undefined' ? e.currentTarget.dataset.loc : -1;
            let level_tab = typeof e.currentTarget.dataset.level_tab != 'undefined' ? e.currentTarget.dataset.level_tab : 0;
            console.log('type', type);
            console.log('loc', loc);
            uni.navigateTo({
                url:
                    `activity_team?type=` +
                    type +
                    (this.balloonTermId && this.balloonTermId != this.arenaInfo.term_id ? '&term_id=' + this.balloonTermId : '') +
                    (action ? '&action=' + action : '') +
                    (team_id ? '&team_id=' + team_id : '') +
                    (loc > -1 ? '&location=' + loc : '') /*+(term_id?"&term_id="+term_id:"")*/ +
                    (level_tab ? '&level_tab=' + level_tab : '')
            });
        },

        /* 点击申请列表 */
        handleTeamApply: function (e) {
            let team_id = e.currentTarget.dataset.item.team_id;
            let team_name = e.currentTarget.dataset.item.team_name;
            uni.navigateTo({
                url: `activity_team_apply?team_id=` + team_id + '&team_name=' + team_name
            });
        },

        /* 点击加入其他队伍 */
        handleTeamJoinlist: function (e) {
            uni.navigateTo({
                url: `activity_team_joinlist`
            });
        },

        /* 点击竞猜列表 */
        handleTeamGuess: function (e) {
            // let team_id=e.currentTarget.dataset.item.team_id;
            // let team_name=e.currentTarget.dataset.item.team_name;
            uni.navigateTo({
                url: `activity_team_guess`
            });
        },

        showMedalToast: function (e) {
            uni.showToast({
                title: '【水神铭牌】划龙舟时可使划行距离*1.5倍',
                icon: 'none'
            });
        },

        showAreaInfo(e) {
            // 展示地区信息弹窗
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_area_info';
            var areaZoneInfo = e.currentTarget.dataset.areazoneinfo;
            this.setData({
                areaZoneInfo: areaZoneInfo
            });
            const params = {
                area: areaZoneInfo.name
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        areaInfo: resp.data,
                        isShowDialogArea: true
                    });
                }
            });
        },

        showDialogEnter() {
            this.setData({
                isShowDialogEnter: true
            });
        },

        closeAreaInfo() {
            this.setData({
                isShowDialogArea: false,
                isShowDialogAreaFuture: false,
                isShowDialogShortName: false,
                isShowDialogEnter: false,
                isShowDialogMeet: false
            });
        },

        gotoGonghuiList: function (e) {
            //var area = e.currentTarget.dataset.area;
            var phase = this.pageDataObj.phase;
            var starid = e.currentTarget.dataset.starid;
            uni.navigateTo({
                //url: 'activity_xxfansqunR_area?area=' + area+"&starid="+starid,
                url: 'activity_xxfansqunR_area?phase=' + phase + '&starid=' + starid
            });
        },

        gotoPersonList: function (e) {
            //var area = e.currentTarget.dataset.area;
            var phase = this.pageDataObj.phase;
            var starid = e.currentTarget.dataset.starid;
            uni.navigateTo({
                //url: 'activity_xxfansqunR_area?area=' + area+"&starid="+starid,
                url: 'activity_xxfansR_area?phase=' + phase + '&starid=' + starid
            });
        },

        gotoStarList: function (e) {
            var area = e.currentTarget.dataset.area;
            uni.navigateTo({
                url: 'star_rank_list_area?area=' + area
            });
        },

        selectStar: function (e) {
            //var starid = e.currentTarget.dataset.starid
            console.log('item', e.detail);
            var starid = e.detail.star_ball.starid;
            if (starid) {
                uni.setStorageSync(this.main_activity_id + '_starid', starid);
                this.initPageData();
                this.dialogCloseClick();
            }
        },

        selectStar2: function (e) {
            var starid = e.currentTarget.dataset.starid;
            //console.log("item",e.detail);
            //var starid = e.detail.star_ball.starid
            if (starid) {
                //wx.setStorageSync(this.data.main_activity_id+"_starid", starid);
                this.setData({
                    userRankInStarSelectStarid: starid
                });
                this.getUserRankingInStar();
                //this.initPageData();
                this.dialogCloseClick();
            }
        },

        showShortnameSet(e) {
            //设置公会简称弹窗
            var oThis = this;
            var type = e.currentTarget.dataset.type ? e.currentTarget.dataset.type : 1;
            if (type == 3) {
                const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=is_admin_user';
                var gourpInfo = e.currentTarget.dataset.group;
                var shortName = e.currentTarget.dataset.shortname;
                var groupShortNameObj = {
                    gid: gourpInfo.gid,
                    starid: gourpInfo.starid,
                    shortname: shortName ? shortName : ''
                };
                this.setData({
                    groupShortNameObj: groupShortNameObj
                });
                console.log(groupShortNameObj);
                const params = {
                    gid: gourpInfo.gid,
                    starid: gourpInfo.starid
                };
                net.fetchApi(url, params, 'GET').then((resp) => {
                    if (resp.data) {
                        if (!resp.data.is_admin) {
                            uni.showToast({
                                title: '只有本公会群管才有权限更改哦~',
                                icon: 'none'
                            });
                        } else {
                            oThis.setData({
                                isShowDialogShortName: true,
                                bShowDialogShortNameType: type
                            });
                        }
                    }
                });
            } else {
                console.log(e.currentTarget.dataset.item);
                oThis.setData({
                    isShowDialogShortName: true,
                    bShowDialogShortNameType: type,
                    myBalloonTeamCurrent: e.currentTarget.dataset.item
                });
            }
        },

        updateShortnameSet(e) {
            //设置信息
            var oThis = this;
            var myBalloonTeamCurrent = this.myBalloonTeamCurrent;
            if (this.bShowDialogShortNameType == 1) {
                var url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=set_team_name';
                var params = {
                    team_name: myBalloonTeamCurrent.team_name,
                    team_id: myBalloonTeamCurrent.team_id
                };
            } else if (this.bShowDialogShortNameType == 2) {
                var url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=set_team_slogan';
                var params = {
                    slogan: myBalloonTeamCurrent.slogan,
                    team_id: myBalloonTeamCurrent.team_id
                };
            } else {
                var url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=set_group_short_name';
                var groupShortNameObj = this.groupShortNameObj;
                var params = {
                    name: groupShortNameObj.shortname,
                    starid: groupShortNameObj.starid,
                    gid: groupShortNameObj.gid
                };
            }
            console.log(params);
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.code == 0) {
                        uni.showToast({
                            title: '设置成功',
                            icon: 'none'
                        });
                        if (oThis.bShowDialogShortNameType == 3) {
                            oThis.initPageData();
                        } else {
                            // oThis.setData({
                            //     transX:0.3
                            // });
                            oThis.getMyBalloonTeam();
                            //oThis.getAllBalloonTeamTree();
                            oThis.getTopBannerList();
                        }
                        oThis.closeAreaInfo();
                    } else {
                        uni.showToast({
                            title: '设置失败，请稍后重试',
                            icon: 'none'
                        });
                    }
                }
            });
        },

        inputShortName(e) {
            //设置名称
            let text = e.detail.value;
            const len = parseInt(text.length);
            let check_len = this.bShowDialogShortNameType == 1 ? 6 : this.bShowDialogShortNameType == 2 ? 20 : 2;
            if (len > check_len) {
                text = text.slice(0, check_len);
            }
            if (text != '') {
                if (this.bShowDialogShortNameType == 1 || this.bShowDialogShortNameType == 2) {
                    var myBalloonTeamCurrent = this.myBalloonTeamCurrent;
                    if (this.bShowDialogShortNameType == 1) {
                        myBalloonTeamCurrent['team_name'] = text;
                    } else {
                        myBalloonTeamCurrent['slogan'] = text;
                    }
                    this.setData({
                        myBalloonTeamCurrent: myBalloonTeamCurrent
                    });
                } else {
                    var groupShortNameObj = this.groupShortNameObj;
                    groupShortNameObj.shortname = text;
                    this.setData({
                        groupShortNameObj: groupShortNameObj
                    });
                }
            }
        },

        gotoMapPage() {
            uni.navigateTo({
                url: 'map?phase=' + this.pageDataObj.phase
            });
        },

        getEventMessageList() {
            //消息列表
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_msg_list';
            const params = {
                phase: this.pageDataObj.phase
                //term_id:this.data.balloonTermId
            };

            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        eventMessageList: resp.data,
                        messageBottom: 'messageBottom'
                    });
                    console.log('消息列表', oThis.eventMessageList.length);
                }
            });
        },

        showMeetTaWindow() {
            var key = this.main_activity_id + '_' + this.pageDataObj.starball_score_info.starball.starid + '_' + this.pageDataObj.phase + '_meet';
            if (this.pageDataObj.meet_ta_tips && this.pageDataObj.meet_ta_tips.text != '' && !this.isShowDialogEnter && !uni.getStorageSync(key)) {
                this.setData({
                    isShowDialogMeet: true
                });
                uni.setStorageSync(key, 1);
            }
        },

        //已下是放气球的操作
        /**
         * 获取首页星球信息列表
         */
        getPositionInfoList: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_idol_balloon.php?action=get_placeholder';
            var params = {
                activity_code: this.giftTermObj ? this.giftTermObj.activity_code : ''
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        let qiuList = resp.data.data;
                        let chrLightALL = null;
                        let chrCount = [];
                        for (let ql in qiuList) {
                            if (qiuList[ql].data.text) {
                                qiuList[ql].data.text_width = qiuList[ql].data.text.length * 12;
                            }
                            if (!qiuList[ql].data.starball.error) {
                                if (!chrCount[qiuList[ql].data.starball.starid]) {
                                    chrCount[qiuList[ql].data.starball.starid] = 1;
                                } else {
                                    chrCount[qiuList[ql].data.starball.starid]++;
                                }
                                console.log(chrCount[qiuList[ql].data.starball.starid]);
                                if (chrCount[qiuList[ql].data.starball.starid] >= 6) {
                                    chrLightALL = qiuList[ql].data;
                                }
                            }
                        }
                        //卡牌弹窗(错误逻辑，不该放这里)
                        // for(let ql in qiuList){
                        //   if(qiuList[ql].data.card &&
                        //     qiuList[ql].data.card.userid==this.data.user_info._id &&
                        //     !wx.getStorageSync(this.data.main_activity_id+"_fengzhencard_"+qiuList[ql].data.card.phase+"_"+qiuList[ql].tag_number+"_"+qiuList[ql].data.card.starid)){
                        //       var q_card = qiuList[ql].data.card;
                        //       q_card.starball = qiuList[ql].data.starball;
                        //       q_card.card_icon=qiuList[ql].data.card_icon;
                        //       wx.setStorageSync(this.data.main_activity_id+"_fengzhencard_"+qiuList[ql].data.card.phase+"_"+qiuList[ql].tag_number+"_"+qiuList[ql].data.card.starid,1);
                        //       oThis.setData({
                        //         thisHasCard:q_card,
                        //         isShowDialog: true,
                        //         showDialogType: 16
                        //       });
                        //       console.log('card',q_card);
                        //       break;
                        //   }
                        // }
                        console.log('chrLightALL', chrLightALL);
                        that.setData({
                            voteAllList: qiuList,
                            chrDarkImg: resp.data.dark,
                            chrLightImg: resp.data.light,
                            chrLightALL: chrLightALL,
                            giftDateList: resp.data.term_list,
                            intervalTimeoutFengzhengStop: false
                            //votePanelPlaceholder:qiuList.length>0?qiuList[0]:{}
                        });

                        if (!that.giftTermObj) {
                            that.setData({
                                giftTermObj: resp.data.term_list[0]
                            });
                        }
                        if (resp.data.now_time < resp.data.end_time) {
                            that.timeoutLimitFengzhengAll = resp.data.end_time - resp.data.now_time + new Date().getTime() / 1000;
                            if (!that.intervalTimeoutActFengzhengAll) {
                                that.initActFengzhengAllInterval();
                            }
                            that.getCardListThisPhase();
                        } else {
                            that.setData({
                                actFengzhengAllCountdownTimeStr: '本场次已结束'
                            });
                        }

                        // if(!oThis.intervalTimeoutFengzheng){
                        //   oThis.initFengzhengInterval();
                        // }

                        console.log('qiulist', that.giftDateList);
                    } else {
                        // oThis.setData({ goodwillNum: 0 })
                    }
                    // if(!oThis.intervalTimeoutWordRoll){
                    //   oThis.initWordRollInterval();
                    // }
                    if (that.openCloud) {
                        that.setData({
                            openCloud: false
                        });
                        that.showVotePanel({
                            currentTarget: {
                                dataset: {
                                    type: 2
                                }
                            }
                        });
                    }
                },
                function (resp) {
                    uni.stopPullDownRefresh();
                    uni.showToast({
                        title: '网络似乎不太顺畅哦',
                        icon: 'none'
                    });
                }
            );
        },

        // 放气球操作回调
        voteCallback: function (e) {
            var that = this;
            if (e.detail.success) {
                that.hideVotePanel();
            }
        },

        // 关闭放气球弹窗操作回调
        closeVoteView: function (e) {
            var that = this;
            var voteResult = e.detail;
            that.hideVotePanel();
            if (voteResult && voteResult.make_tool_extra_gift) {
                if (voteResult.make_tool_extra_gift == 'bullet_comment') {
                    that.getDanmuList();
                } else if (voteResult.make_tool_extra_gift == 'bullet_comment_and_animation') {
                    that.getDanmuList();
                    that.initPageData({
                        open_animi: true
                    });
                }
            }
        },

        // 显示组队制作XX
        showVotePanel: function (e) {
            // if(this.data.balloonTermId && this.data.balloonTermId!=this.data.arenaInfo.term_id){
            //   wx.showToast({ title: "不能抢占往期擂台", icon: 'none' });
            //   return
            // }

            var that = this;
            console.log('放气球数据', e.currentTarget.dataset);
            console.log('showVotePanel 显示放气球弹窗 e==' + JSON.stringify(e));
            let type = 1;
            let from = '';
            if (e.currentTarget.dataset && e.currentTarget.dataset.from) {
                console.log('showVotePanel 显示放气球弹窗 from==' + JSON.stringify(e.currentTarget.dataset.from));
                from = e.currentTarget.dataset.from;
            }
            if (e.currentTarget.dataset && e.currentTarget.dataset.type) {
                console.log('showVotePanel 显示放气球弹窗 type==' + JSON.stringify(e.currentTarget.dataset.type));
                type = e.currentTarget.dataset.type;
            }
            if (e.currentTarget.dataset && e.currentTarget.dataset.item) {
                console.log('showVotePanel 显示放气球弹窗 item==' + JSON.stringify(e.currentTarget.dataset.item));
            }
            if (e.currentTarget.dataset && typeof e.currentTarget.dataset.pos != 'undefined') {
                console.log('showVotePanel 助力位置 zhuliPos==' + JSON.stringify(e.currentTarget.dataset.pos));
                that.setData({
                    zhuliPos: e.currentTarget.dataset.pos
                });
            } else {
                that.setData({
                    zhuliPos: -1
                });
            }
            // 曝光位置放气球/顶气球
            type = 2; //这个月全部都是放气球
            if (type == 1) {
                console.log('曝光位置放气球/顶气球');
                that.setData({
                    votePanelViewType: type,
                    //votePanelPlaceholder: e.currentTarget.dataset.item,
                    myBalloonTeamCurrent: e.currentTarget.dataset.item,
                    votePanelStar: {},
                    bShowVotePanel: true
                });
            } else if (type == 2) {
                // 不顶，为艺人放气球
                console.log('不顶，为艺人放气球');
                if (from == 'adapt') {
                    console.log('不顶，为艺人放气球 - adapt');
                    var _votePanelStar = e.currentTarget.dataset.item;
                    _votePanelStar['starid'] = e.currentTarget.dataset.item.sid;
                    _votePanelStar['star_name'] = e.currentTarget.dataset.item.name;
                    _votePanelStar['star_thumbnail_pic'] = e.currentTarget.dataset.item.logo_img;
                    console.log('不顶，为艺人放气球 - adapt _votePanelStar==' + JSON.stringify(_votePanelStar));
                    that.setData({
                        votePanelViewType: type,
                        // votePanelPlaceholder: {},
                        // votePanelStar: _votePanelStar,
                        bShowVotePanel: true,
                        myBalloonTeamCurrent: e.currentTarget.dataset.item
                    });
                } else {
                    that.setData({
                        votePanelViewType: type,
                        // votePanelPlaceholder: {},
                        // votePanelStar: e.currentTarget.dataset.item,
                        bShowVotePanel: true,
                        myBalloonTeamCurrent: e.currentTarget.dataset.item
                    });
                }
            }
        },

        // 关闭放气球弹窗
        hideVotePanel: function () {
            console.log('showVotePanel 关闭放气球弹窗');
            var that = this;
            that.setData({
                bShowVotePanel: false
            });
            //oThis.initPageData();
            that.getMyBalloonTeam();
            //oThis.getAllBalloonTeamTree();
            that.initGoodwillNum();
        },

        // 显示气球记录弹窗
        showBalloonRecordPanel: function (e) {
            var tabIndexSe = e.currentTarget.dataset.tab_index_se;
            var type = e.currentTarget.dataset.type;
            console.log('showBalloonRecordPanel 显示气球记录弹窗');
            var that = this;
            that.setData({
                bShowBalloonRecordPanelIndexSe: tabIndexSe ? tabIndexSe : 0,
                bShowBalloonRecordPanelType: type ? type : 0,
                bShowBalloonRecordPanel: true
            });
            console.log(that.bShowBalloonRecordPanelType);
        },

        // 关闭气球记录弹窗
        hideBalloonRecordPanel: function () {
            console.log('hideBalloonRecordPanel 关闭气球记录弹窗');
            var that = this;
            that.setData({
                bShowBalloonRecordPanel: false
            });
        },

        // 显示放气球规则弹窗
        showBalloonRulePanel: function () {
            console.log('showBalloonRulePanel 显示放气球规则弹窗');
            var that = this;
            that.setData({
                bShowBalloonRulePanel: true
            });
        },

        // 隐藏放气球规则弹窗
        hideBalloonRulePanel: function () {
            console.log('hideBalloonRecordPanel 隐藏放气球规则弹窗');
            var that = this;
            that.setData({
                bShowBalloonRulePanel: false
            });
        },

        // 放气球操作回调
        voteCallback2: function (e) {
            var that = this;
            if (e.detail.success) {
                that.hideVotePanel2();
            }
        },

        // 关闭放气球弹窗操作回调
        closeVoteView2: function () {
            var that = this;
            that.hideVotePanel2();
        },

        // 显示放置/顶XX
        showVotePanel2: function (e) {
            var that = this;
            console.log('放气球数据', e.currentTarget.dataset);
            console.log('showVotePanel 显示放气球弹窗 e==' + JSON.stringify(e));
            let type = 1;
            let from = '';
            if (e.currentTarget.dataset && e.currentTarget.dataset.from) {
                console.log('showVotePanel 显示放气球弹窗 from==' + JSON.stringify(e.currentTarget.dataset.from));
                from = e.currentTarget.dataset.from;
            }
            if (e.currentTarget.dataset && e.currentTarget.dataset.type) {
                console.log('showVotePanel 显示放气球弹窗 type==' + JSON.stringify(e.currentTarget.dataset.type));
                type = e.currentTarget.dataset.type;
            }
            if (e.currentTarget.dataset && e.currentTarget.dataset.item) {
                console.log('showVotePanel 显示放气球弹窗 item==' + JSON.stringify(e.currentTarget.dataset.item));
            }
            // 曝光位置放气球/顶气球
            type = 1; //全是顶气球
            if (type == 1) {
                console.log('曝光位置放气球/顶气球');
                that.setData({
                    votePanelViewType2: type,
                    votePanelPlaceholder: e.currentTarget.dataset.item,
                    //myBalloonTeamCurrent:e.currentTarget.dataset.item,
                    votePanelStar: {},
                    bShowVotePanel2: true
                });
            } /*else if (type == 2) {// 不顶，为艺人放气球
      console.log("不顶，为艺人放气球");
      if (from == "adapt") {
        console.log("不顶，为艺人放气球 - adapt");
        var _votePanelStar = e.currentTarget.dataset.item;
        _votePanelStar['starid'] = e.currentTarget.dataset.item.sid;
        _votePanelStar['star_name'] = e.currentTarget.dataset.item.name;
        _votePanelStar['star_thumbnail_pic'] = e.currentTarget.dataset.item.logo_img;
        console.log("不顶，为艺人放气球 - adapt _votePanelStar==" + JSON.stringify(_votePanelStar));
        oThis.setData({
          votePanelViewType: type,
          votePanelPlaceholder: {},
          votePanelStar: _votePanelStar,
          bShowVotePanel: true,
          myBalloonTeamCurrent:e.currentTarget.dataset.item,
        })
      } else {
        oThis.setData({
          votePanelViewType: type,
          votePanelPlaceholder: {},
          votePanelStar: e.currentTarget.dataset.item,
          bShowVotePanel: true,
          myBalloonTeamCurrent:e.currentTarget.dataset.item,
        })
      }
      }*/
        },

        // 关闭放气球弹窗
        hideVotePanel2: function () {
            console.log('showVotePanel 关闭放气球弹窗');
            var that = this;
            that.setData({
                bShowVotePanel2: false
            });
            that.getPositionInfoList();
            that.initGoodwillNum();
        },

        showQueqiaoRule: function () {
            this.setData({
                isShowDialog: true,
                showDialogType: 8
            });
        },

        showHalfTips: function (e) {
            var item = e.currentTarget.dataset.item;
            console.log(e.currentTarget.dataset.item);
            if (item.status == 0) {
                this.setData({
                    isShowDialog: true,
                    showDialogType: 9,
                    halfMsg: '下半场开启时间为：' + item.phase_from + '至' + item.phase_to + '，开启后不继承上半场数值，下半场奖励可重新获得'
                });
            } else {
                if (item.status == 1) {
                    this.setData({
                        dataPhase: -1
                    });
                    this.getCountStatus();
                } else {
                    this.setData({
                        dataPhase: item.phase
                    });
                }
                this.initPageData();
            }
        },

        getArenaInfo() {
            //当前擂台信息
            var oThis = this;
            this.timeoutLimitArena = 0;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=during_challenge';
            const params = {
                //term_id: this.data.balloonTermId
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.begin_time) {
                        resp.data.begin_time_str = oThis.dateFormat(resp.data.begin_time, 'HH:mm');
                    }
                    if (resp.data.end_time) {
                        resp.data.end_time_str = oThis.dateFormat(resp.data.end_time, 'HH:mm') != '00:00' ? oThis.dateFormat(resp.data.end_time, 'HH:mm') : '24:00';
                    }
                    oThis.setData({
                        arenaList: resp.data.term_list,
                        balloonTermId: resp.data.term_list[0].term_id,
                        balloonTermDate: resp.data.term_list[0].datetime,
                        arenaInfo: resp.data
                    });
                    oThis.setData({
                        treeBoss: null
                    });
                    // for(var i=0;i<resp.data.boss_freeze_hours.length;i++){
                    //   if(resp.data.current_time < resp.data.boss_freeze_hours[i].end_time){
                    //     oThis.setData({
                    //       treeBoss:resp.data.boss_freeze_hours[i]
                    //     });
                    //     if(resp.data.current_time < resp.data.boss_freeze_hours[i].begin_time){
                    //       oThis.timeoutLimitTreeBoss = resp.data.boss_freeze_hours[i].begin_time - resp.data.current_time + (new Date()).getTime() / 1000;
                    //     }else{
                    //       oThis.timeoutLimitTreeBoss = resp.data.boss_freeze_hours[i].end_time - resp.data.current_time + (new Date()).getTime() / 1000;
                    //     }
                    //     if(!oThis.intervalTimeoutTreeBoss){
                    //       oThis.initTreeBossInterval();
                    //     }
                    //     break;
                    //   }
                    // }
                    // if(!oThis.data.treeBoss){
                    //   oThis.setData({
                    //     treeBossCountdownTimeStr:"当前场次已无BOSS"
                    //   });
                    // }

                    clearInterval(oThis.intervalTimeoutActArena);
                    //判断倒计时
                    if (resp.data.status == 'not_begin') {
                        oThis.timeoutLimitArena = resp.data.begin_time - resp.data.current_time + new Date().getTime() / 1000;
                        oThis.initActArenaInterval();
                    } else if (resp.data.status == 'first_half') {
                        oThis.timeoutLimitArena = resp.data.first_half_end_time - resp.data.current_time + new Date().getTime() / 1000;
                        oThis.initActArenaInterval();
                    } else if (resp.data.status == 'half_time') {
                        oThis.timeoutLimitArena = resp.data.half_end_time - resp.data.current_time + new Date().getTime() / 1000;
                        oThis.initActArenaInterval();
                    } else if (resp.data.status == 'second_half') {
                        oThis.timeoutLimitArena = resp.data.end_time - resp.data.current_time + new Date().getTime() / 1000;
                        oThis.initActArenaInterval();
                    } else {
                        oThis.setData({
                            actArenaCountdownTimeStr: '已结束'
                        });
                    }
                    oThis.getArenaRank();
                }
            });
        },

        getArenaRank() {
            this.getMyBalloonTeam();
            //this.getAllBalloonTeamTree();
            // this.getAllBalloonTeam();
            // this.getPositionInfoList();
            //this.getEventMessageList();
        },

        getMyBalloonTeam() {
            //我的小队
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_my_team';
            const params = {
                term_id: this.balloonTermId
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    var myBalloonTeamByTermDivide = {
                        1: 0,
                        2: 0,
                        3: 0
                    };
                    for (var i in resp.data.list) {
                        myBalloonTeamByTermDivide[resp.data.list[i].my_team.type]++;
                        resp.data.list[i].tab = 0;
                    }
                    if (resp.data.help_list) {
                        for (var i in resp.data.help_list) {
                            resp.data.help_list[i].tab = 0;
                        }
                    }
                    oThis.setData({
                        myBalloonTeam: resp.data.list ? resp.data.list : null,
                        myBalloonTeamHelp: resp.data.help_list ? resp.data.help_list : null,
                        myBalloonTeamDivide: myBalloonTeamByTermDivide
                        //isCaptain:resp.data.is_captain?resp.data.is_captain:0,
                        //notAgreeNum:resp.data.not_agree_num?resp.data.not_agree_num:0,
                    });
                    //加入队伍成功提示弹窗

                    for (var i = 0; i < oThis.myBalloonTeam.length; i++) {
                        if (!uni.getStorageSync(oThis.myBalloonTeam[i].my_team.team_id + '_jointeam')) {
                            if (!oThis.myBalloonTeam[i].is_captain) {
                                oThis.setData({
                                    joinTeamInfo: oThis.myBalloonTeam[i].my_team,
                                    isShowDialog: true,
                                    showDialogType: 8
                                });
                                uni.setStorageSync(oThis.myBalloonTeam[i].my_team.team_id + '_jointeam', 1);
                                break;
                            } else {
                                uni.setStorageSync(oThis.myBalloonTeam[i].my_team.team_id + '_jointeam', 1);
                            }
                        }
                        // if(!top_team || top_team.team_fruit<oThis.data.myBalloonTeam[i].my_team.team_fruit){
                        //   top_team = oThis.data.myBalloonTeam[i].my_team;
                        // }
                    }
                    // if(!oThis.data.levelTab){
                    //   oThis.setData({
                    //     levelTab:top_team?top_team.type:3
                    //   })
                    // }
                    //boss信息获取
                    if (oThis.treeBossCountdownTimeStr == '进行中') {
                        for (var i = 0; i < oThis.myBalloonTeam.length; i++) {
                            oThis.getBossInfo(oThis.myBalloonTeam[i].my_team.team_id);
                        }
                    }
                    if (oThis.arenaInfo.status == 'is_end' && oThis.arenaInfo.next_term_id) {
                        oThis.getMyBalloonTeamNext();
                    }
                    this.getAllBalloonTeamTree();
                }
            });
        },

        // getAllBalloonTeam(){//队伍排名
        //   var oThis = this;
        //   const url = 'https://data.xingxiaoculture.com/api_activity_'+this.data.main_activity_id_api+'.php?action=get_team_list';
        //   const params = {
        //     limit:3,
        //     term_id: this.data.balloonTermId,
        //     order_by:"team_time"
        //   };
        //   net.fetchApi(url, params, 'GET').then(resp => {
        //     if (resp.data) {
        //       oThis.setData({
        //         allBalloonteam: resp.data.list?resp.data.list:[],
        //         arenaText:resp.data.text?resp.data.text:"",
        //         arenaTermClose:resp.data.term_close?resp.data.term_close:0,
        //       });
        //     }
        //   })
        // },
        dingTeam(e) {
            //点赞
            if (this.balloonTermId && this.balloonTermId != this.arenaInfo.term_id) {
                uni.showToast({
                    title: '不能给往期队伍点赞',
                    icon: 'none'
                });
                return;
            }
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=digg_team';
            const params = {
                team_id: e.currentTarget.dataset.obj.team_id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.code == 0) {
                        oThis.getArenaRank();
                        oThis.initGoodwillNum();
                    }
                    if (resp.data.msg) {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
                        });
                    }
                }
            });
        },

        gotoDressRoom(e) {
            // 前往天使屋页面
            var userid = e.currentTarget.dataset.userid;
            if (userid) {
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/dress_room/dress_room?userID=' + userid
                });
            }
        },

        showArenaDateSelect: function () {
            this.setData({
                bShowBalloonDateSelect: !this.bShowBalloonDateSelect
            });
        },

        changeArenaRank: function (e) {
            this.setData({
                bShowBalloonDateSelect: false
            });
            this.setData({
                balloonTermId: e.currentTarget.dataset.item.term_id,
                balloonTermDate: e.currentTarget.dataset.item.datetime
            });
            this.getArenaRank();
        },

        showGuihuaWindow: function () {
            if (this.goodwillNumLittle < 3) {
                this.setData({
                    isShowDialogGuihuaCompose: true,
                    isShowDialogGuihuaType: 0
                });
            } else {
                this.setData({
                    isShowDialogGuihuaCompose: true,
                    isShowDialogGuihuaType: 1,
                    composeWinNum: Math.floor(this.goodwillNumLittle / 3)
                });
            }
        },

        handleGuihuaWindow: function (e) {
            let type = e.currentTarget.dataset.type;
            if (type == 0) {
                this.dialogCloseClickGuihua();
                this.dialogCloseClick();
                e.currentTarget.dataset.select = 6;
                this.showDialogClick(e);
            } else if (type == 1) {
                var oThis = this;
                const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=gen_wine';
                const params = {
                    //team_id: e.currentTarget.dataset.obj.team_id,
                };
                net.fetchApi(url, params, 'GET').then((resp) => {
                    if (resp.data) {
                        if (resp.data.code != 0) {
                            uni.showToast({
                                title: resp.data.msg,
                                icon: 'none'
                            });
                        } else {
                            oThis.setData({
                                isShowDialogGuihuaType: 2,
                                genWineResult: resp.data
                            });
                            //oThis.initWineInterval();
                            oThis.initGoodwillNum();
                            oThis.setData({
                                isShowDialogGuihuaType: 3
                            });
                        }
                    }
                });
            }
        },

        dialogCloseClickGuihua: function () {
            this.setData({
                isShowDialogGuihuaCompose: false
            });
        },

        joinTeamBoxShow(e) {
            if (this.balloonTermId && this.balloonTermId != this.arenaInfo.term_id) {
                uni.showToast({
                    title: '不能加入往期队伍',
                    icon: 'none'
                });
                return;
            }
            this.setData({
                joinTeamid: e.currentTarget.dataset.obj.team_id,
                joinTeamInfo: e.currentTarget.dataset.obj,
                isShowDialog: true,
                showDialogType: 9
            });
        },

        joinTeam() {
            //加入队伍
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=apply_join_team';
            const params = {
                team_id: this.joinTeamid
            };
            this.dialogCloseClick();
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.msg) {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
                        });
                    }
                }
            });
        },

        getBossInfo(team_id) {
            //我当前的小队
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php';
            const params = {
                action: 'get_boss_info',
                team_id: team_id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    var myBalloonTeam = oThis.myBalloonTeam;
                    for (var i = 0; i < myBalloonTeam.length; i++) {
                        if (myBalloonTeam[i].my_team.team_id == team_id) {
                            myBalloonTeam[i].bossInfo = resp.data;
                            break;
                        }
                    }
                    oThis.setData({
                        myBalloonTeam: myBalloonTeam
                    });
                }
            });
        },

        gotoGradCandyPage(e) {
            var team_id = e.currentTarget.dataset.team_id;
            var open_type = e.currentTarget.dataset.open_type;
            uni.navigateTo({
                url: 'activity_candy_grab?' + (team_id ? '&team_id=' + team_id : '') + (open_type ? '&open_type=' + open_type : '')
            });
        },

        dialogCloseClickGift: function () {
            this.setData({
                isShowDialogGift: false
            });
        },

        dialogCloseClickExceed: function () {
            this.setData({
                isShowDialogExceed: false
            });
        },

        dialogCloseClickEnterFrame: function () {
            this.setData({
                isShowDialogEnterFrame: false
            });
        },

        getDaySharePrize: function () {
            var that = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_day_share_prize';
            const params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    that.setData({
                        daySharePrizeWord: resp.data.msg,
                        showDialogType: 11,
                        isShowDialog: true
                    });
                    that.initGoodwillNum();
                }
            });
        },

        getAllBalloonTeamTree() {
            //队伍排名
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_team_fruit_ranking';
            const params = {
                term_id: this.balloonTermId,
                //order_by:"team_time",
                limit: 3
                //type:this.data.levelTab
            };

            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.team_list) {
                        var arr = resp.data.team_list;
                        var length = arr.length;
                        var randomIndex;
                        var temp;
                        var date = new Date().getDate();
                        if (length > 0) {
                            uni.setStorageSync(oThis.main_activity_id + '_enter_' + date, 1);
                        }
                        // while (length) {
                        //   randomIndex = Math.floor(Math.random() * (length--));
                        //   temp = arr[randomIndex];
                        //   arr[randomIndex] = arr[length];
                        //   arr[length] = temp
                        // }

                        oThis.setData({
                            allBalloonteamTree: arr.slice(0, 3),
                            arenaBanner: resp.data.banner
                        });
                        // if(!oThis.intervalTimeoutWordRoll){
                        //   oThis.initWordRollInterval();
                        // }
                        oThis.getLocationTop3();
                    } else {
                        oThis.setData({
                            allBalloonteamTree: []
                        });
                    }
                }
            });
        },

        getLocationTop3() {
            //取各位置TOP3列表
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_pos_rank&term_id';
            const params = {
                term_id: this.balloonTermId
                //order_by:"team_time",
                //limit:3,
                //type:this.data.levelTab
            };

            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.list) {
                        var arr = resp.data.list;
                        var locaationTop3 = [[], [], [], [], []];
                        for (var i in arr) {
                            locaationTop3[i] = arr[i];
                        }
                        oThis.setData({
                            locaationTop3: locaationTop3
                        });
                    }
                }
            });
        },

        // 显示设置艺人弹窗
        showBalloonStarPanel: function () {
            console.log('showBalloonStarPanel 显示设置艺人弹窗');
            var that = this;
            that.setData({
                bShowBalloonStarPanel: true
            });
        },

        // 隐藏设置艺人弹窗
        hideBalloonStarPanel: function () {
            console.log('hideBalloonStarPanel 隐藏设置艺人弹窗');
            var that = this;
            that.setData({
                bShowBalloonStarPanel: false
            });
        },

        // 显示创建队伍弹窗
        showBalloonCreateTeamPanel: function () {
            console.log('showBalloonCreateTeamPanel 显示队伍弹窗');
            var that = this;
            that.setData({
                bShowBalloonCreateTeamPanel: true
            });
        },

        // 隐藏创建队伍弹窗
        hideBalloonCreateTeamPanel: function () {
            console.log('hideBalloonCreateTeamPanel 隐藏创建队伍弹窗');
            var that = this;
            that.setData({
                bShowBalloonCreateTeamPanel: false
            });
        },

        createTeamResult: function (e) {
            let item = e.detail;
            console.log(item);
            this.hideBalloonCreateTeamPanel();
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 3,
                bTipsWindowShowType3ShowPrice: item.get_prize ? true : false
                //myBalloonTeamCurrentId:item.team_info.team_id
            });

            this.getMyBalloonTeam();
            this.initGoodwillNum();
        },

        showTeamInfo: function (e) {
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 14,
                myBalloonTeamCurrent: e.currentTarget.dataset.item,
                teamInfoTab: 0
            });
            this.getTeamAllInfo();
            if (e.currentTarget.dataset.tab == 1) {
                this.changeTabTeamInfo(e);
            }
        },

        showDismissTeam: function (e) {
            let team_id = e.currentTarget.dataset.team_id;
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 5,
                dismissTeamid: team_id
            });
        },

        dismissTeam() {
            //解散队伍
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=remove_team';
            const params = {
                team_id: this.dismissTeamid
            };
            this.dialogCloseClick();
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.msg) {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
                        });
                    }
                    oThis.getMyBalloonTeam();
                }
            });
        },

        showQuitTeam: function (e) {
            let team_id = e.currentTarget.dataset.team_id;
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 6,
                quitTeamid: team_id
            });
        },

        quitTeam() {
            //退出队伍
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=quit_team';
            const params = {
                team_id: this.quitTeamid
            };
            this.dialogCloseClick();
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.msg) {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
                        });
                    }
                    oThis.getMyBalloonTeam();
                }
            });
        },

        showGetoutTeam: function (e) {
            let team_id = e.currentTarget.dataset.team_id;
            let member = e.currentTarget.dataset.item;
            console.log(member);
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 7,
                getoutTeamid: team_id,
                getoutMember: member
            });
        },

        getoutTeam() {
            //将成员踢出队伍
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=remove_member';
            const params = {
                team_id: this.getoutTeamid,
                member_id: this.getoutMember.userinfo._id
            };
            this.dialogCloseClick();
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.msg) {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
                        });
                    }
                    oThis.getMyBalloonTeam();
                }
            });
        },

        notEnoughVote: function () {
            this.setData({
                isShowDialog: true,
                showDialogType: 6
            });
        },

        getMyBalloonTeamNext() {
            //我的小队
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_my_team';
            const params = {
                term_id: this.arenaInfo.next_term_id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    var myBalloonTeamByTermDivide = {
                        1: 0,
                        2: 0,
                        3: 0
                    };
                    for (var i in resp.data.list) {
                        myBalloonTeamByTermDivide[resp.data.list[i].my_team.type]++;
                    }
                    oThis.setData({
                        myBalloonTeamNext: resp.data.list,
                        myBalloonTeamNextDivide: myBalloonTeamByTermDivide
                    });
                    if (resp.data.list.length > 0) {
                        var arenaInfo = oThis.arenaInfo;
                        if (arenaInfo.term_list[0].term_id != arenaInfo.next_term_id) {
                            var datetime = arenaInfo.next_term_id.toString().replace('2023', '');
                            datetime = datetime.slice(0, 2) + '.' + datetime.slice(2);
                            arenaInfo.term_list.unshift({
                                term_id: arenaInfo.next_term_id,
                                datetime: datetime
                            });
                            oThis.setData({
                                arenaList: arenaInfo.term_list
                            });
                            console.log('arenaInfo', arenaInfo);
                        }
                    }
                    // if(!oThis.data.myBalloonTeamCurrentId && oThis.data.myBalloonTeamNext.length>0){
                    //   oThis.setData({
                    //     myBalloonTeamCurrentId:oThis.data.myBalloonTeamNext[0].my_team.team_id,
                    //   });
                    // }
                }
            });
        },

        onSlideChangeEnd: function (e) {
            var that = this;
            that.setData({
                currentIndex: e.detail.current
            });
        },

        gotoTreeRule: function (e) {
            let type = e.currentTarget.dataset.type ? e.currentTarget.dataset.type : 0;
            uni.navigateTo({
                url: 'activity_boss_rule?type=' + type /*+'&type_se='+(this.data.levelTab-1)*/
            });
        },

        doOpenAction() {
            if (this.openAction) {
                try {
                    var openAction = JSON.parse(decodeURIComponent(this.openAction));
                } catch (d) {
                    console.log('CatchClause', d);
                    console.log('CatchClause', d);
                    var openAction = decodeURIComponent(this.openAction);
                }
                console.log('openAction', openAction);
                if (typeof openAction == 'object') {
                    var action = openAction.name;
                    var num = openAction.num;
                } else if (typeof openAction == 'string') {
                    var action = openAction;
                    var num = 0;
                }
                if (action == 'arena') {
                    this.moveViewClick('.tabPage1');
                } else if (action == 'gift') {
                    this.moveViewClick('.tabPage2');
                } else if (action == 'exchange') {
                    this.showGuihuaWindow();
                } else if (action == 'buy_heart_love_letter') {
                    this.openExchangeXinyi('heart_love_letter');
                } else if (action == 'buy_sugar') {
                    this.openExchangeXinyi('sugar', num);
                } else if (action == 'open_tool_panel') {
                    this.getBalloonTeamInfo(num);
                }
            }
            this.setData({
                openAction: ''
            });
        },

        getGuessMessageList() {
            //消息列表
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_support_team_msg_list';
            const params = {
                term_id: this.balloonTermId
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        guessMessageList: resp.data,
                        messageBottom: 'messageBottom'
                    });
                }
            });
        },

        changeTab(e) {
            let tab = e.currentTarget.dataset;
            this.setData({
                tabIndex: tab.tab
            });
        },

        switchTopBanner() {
            console.log(this.isShowTopBanner);
            let isShowTopBanner = false;
            if (!this.isShowTopBannerManager) {
                isShowTopBanner = true;
            }
            let isShowTopBannerManager = !this.isShowTopBannerManager;
            this.setData({
                isShowTopBannerManager: isShowTopBannerManager,
                isShowTopBanner: isShowTopBanner
            });
            this.closeFireTimeOut();
            var date = new Date().getDate();
            uni.setStorageSync(this.main_activity_id + '_topbannerclose_' + date, !isShowTopBannerManager ? 1 : 0);
        },

        switchTopDanmu() {
            console.log(this.isShowTopDanmu);
            let isShowTopDanmu = !this.isShowTopDanmu;
            this.setData({
                isShowTopDanmu: isShowTopDanmu
            });
            var date = new Date().getDate();
            uni.setStorageSync(this.main_activity_id + '_topdanmuclose_' + date, !isShowTopDanmu ? 1 : 0);
        },

        getTopBannerList() {
            //消息列表
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_top1_team_list';
            const params = {
                //term_id:this.data.balloonTermId
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        topBannerArr: resp.data.list
                    });
                }
            });
        },

        getChrGift(e) {
            //拆礼物
            var oThis = this;
            var box_id = e.currentTarget.dataset.box_id;
            var box_left_num = e.currentTarget.dataset.box_left_num;
            if (box_left_num <= 0) {
                uni.showToast({
                    title: '该礼盒已被拆空',
                    icon: 'none'
                });
                return;
            }
            const url = 'https://data.xingxiaoculture.com/api_idol_balloon.php?action=open_box';
            const params = {
                box_id: box_id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        openGiftResult: resp.data,
                        isShowDialogGift: true
                    });
                    oThis.getPositionInfoList();
                }
            });
        },

        changeGiftDate: function (e) {
            this.setData({
                bShowGiftDateSelect: false
            });
            this.setData({
                giftTermObj: e.currentTarget.dataset.item
            });
            this.getPositionInfoList();
        },

        showGiftDateSelect: function () {
            this.setData({
                bShowGiftDateSelect: !this.bShowGiftDateSelect
            });
        },

        showGiftPositionInfo(e) {
            this.setData({
                isShowDialogGiftInfo: true,
                giftPositionInfo: e.currentTarget.dataset.item
            });
        },

        dialogCloseClickGiftInfo: function () {
            this.setData({
                isShowDialogGiftInfo: false
            });
        },

        showChangeLocation: function (e) {
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 15,
                myBalloonTeamCurrent: e.currentTarget.dataset.item,
                locationTickArr: []
            });
        },

        locationTick: function (e) {
            let location = e.currentTarget.dataset.index;
            console.log('location', location);
            let is_in = false;
            let locationTickArr = this.locationTickArr;
            let myBalloonTeamCurrent = this.myBalloonTeamCurrent;
            for (let i in this.locationTickArr) {
                if (locationTickArr[i] == location) {
                    locationTickArr.splice(i, 1);
                    is_in = true;
                    break;
                }
            }
            if (!is_in) {
                locationTickArr.push(location);
                if (locationTickArr.length > 2) {
                    locationTickArr.shift();
                }
            }
            console.log('locationTickArr', locationTickArr);
            for (let n in myBalloonTeamCurrent.members_pos) {
                if (locationTickArr.indexOf(parseInt(n)) >= 0) {
                    myBalloonTeamCurrent.members_pos[n].tick = true;
                } else {
                    myBalloonTeamCurrent.members_pos[n].tick = false;
                }
            }
            console.log('myBalloonTeamCurrent', myBalloonTeamCurrent);
            this.setData({
                locationTickArr: locationTickArr,
                myBalloonTeamCurrent: myBalloonTeamCurrent
            });
        },

        changeLocationReq() {
            var oThis = this;
            if (this.locationTickArr.length != 2) {
                uni.showToast({
                    title: '请选择2个队内角色进行交换',
                    icon: 'none'
                });
                return;
            }
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=change_member_pos';
            const params = {
                team_id: this.myBalloonTeamCurrent.team_id,
                pos1: this.locationTickArr[0],
                pos2: this.locationTickArr[1]
            };
            this.dialogCloseClick();
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.msg) {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
                        });
                    }
                    oThis.getMyBalloonTeam();
                }
            });
        },

        gotoMengHuList: function (e) {
            let starid = this.pageDataObj.user_select_starid;
            uni.navigateTo({
                url: 'menghu_list?starid=' + starid
            });
        },

        closeFireTimeOut: function () {
            var that = this;
            if (that.isShowTopBanner) {
                setTimeout(function () {
                    that.setData({
                        isShowTopBanner: false
                    });
                }, 3500);
            }
        },

        showChangeCap: function (e) {
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 16,
                myBalloonTeamCurrent: e.currentTarget.dataset.item,
                changeCapUserid: ''
            });
        },

        capTick: function (e) {
            let userid = e.currentTarget.dataset.userid;
            this.setData({
                changeCapUserid: userid
            });
        },

        changeCapReq() {
            var oThis = this;
            if (!this.changeCapUserid) {
                uni.showToast({
                    title: '请选择1名成员进行队长转让',
                    icon: 'none'
                });
                return;
            }
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=change_captain';
            const params = {
                team_id: this.myBalloonTeamCurrent.team_id,
                member_id: this.changeCapUserid
            };
            this.dialogCloseClick();
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.msg) {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
                        });
                    }
                    oThis.getMyBalloonTeam();
                }
            });
        },

        getDanmuList() {
            return;
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_bullet_comment_list';
            const params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data && resp.data.list) {
                    let damuList = [];
                    for (let i in resp.data.list) {
                        let damu = {
                            image: {
                                head: {
                                    src: resp.data.list[i].userinfo.image.thumbnail_pic,
                                    width: 20,
                                    height: 20
                                },
                                background: {
                                    src: resp.data.list[i].bg,
                                    width: 260,
                                    height: 30
                                },
                                gap: 0
                            },
                            content: resp.data.list[i].userinfo.nickname.substr(0, 6) + '：' + resp.data.list[i].text,
                            color: resp.data.list[i].color
                        };
                        damuList.push(damu);
                    }
                    oThis.setData({
                        danmuList: damuList,
                        danmuIndex: 0
                    });
                    oThis.initDanmu();
                }
            });
        },

        initDanmu() {
            var oThis = this;
            const barrageComp = this.zpSelectComponent('.barrage');
            this.barrage = barrageComp.getBarrageInstance({
                duration: 20,
                // 弹幕动画时长 (移动 2000px 所需时长)
                lineHeight: 3,
                // 弹幕行高
                padding: [0, 0, 0, 0],
                // 弹幕区四周留白
                alpha: 0.9,
                // 全局透明度
                font: '9px sans-serif',
                // 全局字体
                mode: 'separate',
                // 弹幕重叠 overlap  不重叠 separate
                range: [0, 1],
                // 弹幕显示的垂直范围，支持两个值。[0,1]表示弹幕整个随机分布，
                tunnelShow: false,
                // 显示轨道线
                tunnelMaxNum: 30,
                // 隧道最大缓冲长度
                maxLength: 52,
                // 弹幕最大字节长度，汉字算双字节
                safeGap: 20,
                // 发送时的安全间隔
                enableTap: false // 点击弹幕停止动画高亮显示
            });

            this.barrage.open();
            let isShowTopDanmu = oThis.isShowTopDanmu;
            if (!oThis.intervalTimeoutDanmu) {
                oThis.intervalTimeoutDanmu = setInterval(() => {
                    if (!isShowTopDanmu && oThis.isShowTopDanmu) {
                        oThis.barrage.open();
                    } else if (isShowTopDanmu && !oThis.isShowTopDanmu) {
                        oThis.barrage.close();
                    }
                    isShowTopDanmu = oThis.isShowTopDanmu;
                    if (oThis.isShowTopDanmu) {
                        let danmuList = oThis.danmuList;
                        if (danmuList.length > 0) {
                            let i = oThis.danmuIndex;
                            if (danmuList[i]) {
                                oThis.barrage.addData([danmuList[i]]);
                            }
                            if (i < oThis.danmuOneTurnNum - 1) {
                                i++;
                            } else {
                                i = 0;
                            }
                            oThis.setData({
                                danmuIndex: i
                            });
                        }
                    }
                }, (oThis.danmuOneTurnSecond / oThis.danmuOneTurnNum) * 1000);
            }
        },

        switchFulizhan() {
            this.setData({
                bShowFulizhan: !this.bShowFulizhan
            });
        },

        showSwitchMember(e) {
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 17,
                myBalloonTeamCurrent: e.currentTarget.dataset.item
            });
        },

        changeTabTeam(e) {
            let dataset = e.currentTarget.dataset;
            if (dataset.team_type == 'inteam') {
                var myBalloonTeam = this.myBalloonTeam;
                myBalloonTeam[dataset.index].tab = dataset.tab;
                this.setData({
                    myBalloonTeam: myBalloonTeam
                });
            } else {
                var myBalloonTeam = this.myBalloonTeamHelp;
                myBalloonTeam[dataset.index].tab = dataset.tab;
                this.setData({
                    myBalloonTeamHelp: myBalloonTeam
                });
            }
            if (dataset.tab == 1) {
                // console.log(myBalloonTeam[dataset.index]);
                this.getHelpInfo(myBalloonTeam[dataset.index].my_team.team_id, dataset.team_type);
            } else if (dataset.tab == 2) {
                this.getJigsaw(
                    myBalloonTeam[dataset.index].my_team.team_id,
                    myBalloonTeam[dataset.index].puzzle_pos ? myBalloonTeam[dataset.index].puzzle_pos : 0,
                    dataset.team_type
                );
            }
        },

        changeTabTeamInfo(e) {
            let dataset = e.currentTarget.dataset;
            this.setData({
                teamInfoTab: dataset.tab
            });
            if (dataset.tab == 1) {
                this.getTeamAllInfo();
            }
        },

        getTeamAllInfo() {
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_help_rank';
            const params = {
                //team_id:"64167e84f53fb1397b8b48af",
                team_id: this.myBalloonTeamCurrent.team_id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    var myBalloonTeamCurrent = this.myBalloonTeamCurrent;
                    myBalloonTeamCurrent.my_info = resp.data.user;
                    myBalloonTeamCurrent.user_list = resp.data.user_list;
                    oThis.setData({
                        myBalloonTeamCurrent: myBalloonTeamCurrent
                    });
                }
            });
        },

        getHelpInfo(team_id, team_type) {
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_help_list';
            const params = {
                //team_id:"64167e84f53fb1397b8b48af",
                team_id: team_id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    console.log(resp.data);
                    if (team_type == 'inteam') {
                        var myBalloonTeam = this.myBalloonTeam;
                        for (var i = 0; i < myBalloonTeam.length; i++) {
                            if (myBalloonTeam[i].my_team.team_id == team_id) {
                                myBalloonTeam[i].help_info = resp.data;
                                break;
                            }
                        }
                        this.setData({
                            myBalloonTeam: myBalloonTeam
                        });
                    } else {
                        var myBalloonTeam = this.myBalloonTeamHelp;
                        for (var i = 0; i < myBalloonTeam.length; i++) {
                            if (myBalloonTeam[i].my_team.team_id == team_id) {
                                myBalloonTeam[i].help_info = resp.data;
                                break;
                            }
                        }
                        this.setData({
                            myBalloonTeamHelp: myBalloonTeam
                        });
                    }
                }
            });
        },

        showTeamUserInfoSingle(e) {
            this.setData({
                thisTeamUser: e.currentTarget.dataset.item,
                isShowTeamUserInfoSingle: true
            });
        },

        hideTeamUserInfoSingle() {
            this.setData({
                isShowTeamUserInfoSingle: false
            });
        },

        getStarBuffTeamList(e) {
            var starid = e.currentTarget.dataset.starid;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_star_buff_team_list';
            const params = {
                starid: starid,
                phase: this.pageDataObj.phase
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data && resp.data.list && resp.data.list.length > 0) {
                    // var all_count = 0;
                    // var buff_list = [];
                    // for(var i in resp.data.list){
                    //   all_count+=resp.data.list[i].buff;
                    //   buff_list.push(resp.data.list[i].buff);
                    // }
                    // var text = buff_list.join("+")

                    this.setData({
                        teamBuffList: resp.data.list,
                        isShowBuffDetail: true,
                        teamBuffCountText: resp.data.buff_text
                    });
                } else {
                    this.setData({
                        teamBuffList: [],
                        teamBuffCountText: ''
                    });
                }
            });
        },

        hideBuffDetail() {
            this.setData({
                isShowBuffDetail: false
            });
        },

        //已下是放气球的操作
        /**
         * 获取首页星球信息列表
         */
        getCardListThisPhase: function () {
            var that = this;
            var url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_user_card_list';
            var params = {
                //activity_code:this.data.giftTermObj?this.data.giftTermObj.activity_code:""
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data && resp.data.list && resp.data.list.length > 0) {
                    var qiuList = resp.data.list[0];
                    if (qiuList._id && !uni.getStorageSync(this.main_activity_id + '_fengzhencard_' + qiuList._id)) {
                        var q_card = qiuList;
                        uni.setStorageSync(this.main_activity_id + '_fengzhencard_' + qiuList._id, 1);
                        that.setData({
                            thisHasCard: q_card,
                            isShowDialog: true,
                            showDialogType: 16
                        });
                    }
                }
            });
        },

        getJigsaw: function (team_id, locaiton, team_type) {
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_jigsaw';
            const params = {
                //team_id:"64167e84f53fb1397b8b48af",
                team_id: team_id,
                user_pos: locaiton
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    console.log(resp.data);
                    if (team_type == 'inteam') {
                        var myBalloonTeam = this.myBalloonTeam;
                        for (var i = 0; i < myBalloonTeam.length; i++) {
                            if (myBalloonTeam[i].my_team.team_id == team_id) {
                                myBalloonTeam[i].puzzle = resp.data;
                                myBalloonTeam[i].puzzle_pos = locaiton;
                                break;
                            }
                        }
                        this.setData({
                            myBalloonTeam: myBalloonTeam
                        });
                    } else {
                        var myBalloonTeam = this.myBalloonTeamHelp;
                        for (var i = 0; i < myBalloonTeam.length; i++) {
                            if (myBalloonTeam[i].my_team.team_id == team_id) {
                                myBalloonTeam[i].puzzle = resp.data;
                                myBalloonTeam[i].puzzle_pos = locaiton;
                                break;
                            }
                        }
                        this.setData({
                            myBalloonTeamHelp: myBalloonTeam
                        });
                    }
                }
            });
        },

        unlockJigsawClick(e) {
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 18,
                unlockJigsawObj: e.currentTarget.dataset
            });
        },

        unlockJigsaw: function () {
            var that = this;
            var dataset = this.unlockJigsawObj;
            console.log(dataset);
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=unlock_jigsaw';
            const params = {
                //team_id:"64167e84f53fb1397b8b48af",
                team_id: dataset.item.my_team.team_id,
                user_pos: dataset.item.puzzle_pos,
                piece: dataset.piece
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    console.log(resp.data);
                    if (resp.data.code == 0) {
                        this.setData({
                            bTipsWindowShow: true,
                            bTipsWindowShowType: 21,
                            unlockJigsawResult: resp.data
                        });
                        that.changeTabTeam({
                            currentTarget: {
                                dataset: dataset
                            }
                        });
                        this.initGoodwillNum();
                    } else if (resp.data.code == 2) {
                        this.setData({
                            bTipsWindowShow: true,
                            bTipsWindowShowType: 19,
                            unlockJigsawResult: resp.data
                        });
                    } else if (resp.data.code == 3) {
                        this.setData({
                            bTipsWindowShow: true,
                            bTipsWindowShowType: 20,
                            unlockJigsawResult: resp.data
                        });
                    } else {
                        uni.showToast({
                            title: resp.data.msg ? resp.data.msg : '网络异常，请稍后再试',
                            icon: 'none'
                        });
                    }
                }
            });
        },

        // 显示设置位置弹窗
        showBalloonLocationPanel: function (e) {
            var that = this;
            var item = e.currentTarget.dataset.item;
            item['team_type'] = e.currentTarget.dataset.team_type;
            that.setData({
                bShowBalloonLocationPanel: true,
                myBalloonTeamCurrent: e.currentTarget.dataset.item
            });
        },

        // 隐藏设置位置弹窗
        hideBalloonLocationPanel: function () {
            var that = this;
            that.setData({
                bShowBalloonLocationPanel: false
            });
        },

        //选择位置
        chooseBalloonLocation: function (e) {
            if (e.detail && e.detail.index >= 0) {
                var dataset = this.unlockJigsawObj;
                var team_id = this.myBalloonTeamCurrent.team_id;
                var team_type = this.myBalloonTeamCurrent.team_type;
                if (team_type == 'inteam') {
                    var myBalloonTeam = this.myBalloonTeam;
                    for (var i = 0; i < myBalloonTeam.length; i++) {
                        if (myBalloonTeam[i].my_team.team_id == team_id) {
                            myBalloonTeam[i].puzzle_pos = e.detail.index;
                            break;
                        }
                    }
                    this.setData({
                        myBalloonTeam: myBalloonTeam
                    });
                } else {
                    var myBalloonTeam = this.myBalloonTeamHelp;
                    for (var i = 0; i < myBalloonTeam.length; i++) {
                        if (myBalloonTeam[i].my_team.team_id == team_id) {
                            myBalloonTeam[i].puzzle_pos = e.detail.index;
                            break;
                        }
                    }
                    this.setData({
                        myBalloonTeamHelp: myBalloonTeam
                    });
                }
                console.log(dataset);
                this.getJigsaw(team_id, e.detail.index, team_type);
            }
        },

        getBalloonTeamInfo(team_id) {
            //获取某小队信息
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php';
            console.log(url);
            const params = {
                action: 'get_team_info',
                team_id: team_id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data && !resp.data.error) {
                    oThis.showVotePanel({
                        currentTarget: {
                            dataset: {
                                type: 2,
                                team_id: team_id,
                                item: resp.data
                            }
                        }
                    });
                }
            });
        },

        getLeftDamuList() {
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php';
            console.log(url);
            const params = {
                action: 'get_bullet_list'
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data && !resp.data.error) {
                    oThis.setData({
                        leftDamuList: resp.data.list
                    });
                    if (!oThis.intervalTimeoutWordRoll) {
                        oThis.initWordRollInterval();
                    }
                }
            });
        },

        getUserRankingInStar() {
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php';
            console.log(url);
            const params = {
                action: 'get_user_ranking_in_star',
                starid: this.userRankInStarSelectStarid ? this.userRankInStarSelectStarid : this.pageDataObj.user_select_starid
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data && !resp.data.error) {
                    oThis.setData({
                        userRankInStar: resp.data
                    });
                }
            });
        },

        gotoIsland() {
            uni.switchTab({
                url: '/pages/idol_home/idol_home'
            });
        },

        gotoSearchGame() {
            uni.navigateTo({
                url: '/subpage_v4/pages/map_game/search_game'
            });
        },

        gotoTeamBattle(e) {
            var team_id = e.currentTarget.dataset.team_id;
            if (team_id) {
                uni.navigateTo({
                    url: 'activity_team_battle?team_id=' + team_id
                });
            }
        },

        agreeJoin() {
            console.log('占位：函数 agreeJoin 未声明');
        },

        selectTeam() {
            console.log('占位：函数 selectTeam 未声明');
        },

        confirmSelectTeam() {
            console.log('占位：函数 confirmSelectTeam 未声明');
        },

        grabCandy() {
            console.log('占位：函数 grabCandy 未声明');
        }
    }
};
</script>
<style>
@import './activity_formal.css';
@import '@/subpage_v3/pages/202305S/activity_formal.css';
</style>
