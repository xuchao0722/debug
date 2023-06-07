<template>
    <view style="height: 100%">
        <!-- pages/idol_home/idol_home.wxml -->

        <template name="loading_holder">
            <view class="loading_holder_cell">
                <view class="loading_holder_cell_img"></view>
                <view class="loading_holder_cell_desc"></view>
            </view>
        </template>

        <!-- template对应的原始代码，为保证正常显示，已对其进行隐藏。 -->
        <block name="support_list_cell_group" v-if="false">
            <group_rank_weekList_view
                :admin_name="admin_name"
                :title_color="title_color"
                :group_widget_img="group_widget_img"
                @groupDetailAttackCLick="groupDetailAttackCLick"
                :index="index"
                :score="score"
                :name="name"
                :member="member"
                :people_num="people_num"
                :app_platform="app_platform"
                :time="time"
                :isFansGroup="1"
                :biggest_gather_num="biggest_gather_num"
                :group_emblem_img="group_emblem_img"
                :group_medal_img="group_medal_img"
            ></group_rank_weekList_view>
        </block>
        <!-- <view style="position: relative;" wx:if="{{rewardId!=''}}">
	<image class="reward-img" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/12/6/zhfn5wzAFr1638778903383.png" bindtap="gotoReward"></image>
</view> -->
        <view v-if="isGroupViewHistoryLoading || groupViewHistory.is_office_group == 1" class="main">
            <block v-if="bShowIdolHome">
                <view class="header">
                    <view class="userCoin-box flex_h">
                        <view class="group_concentrate">
                            <view class="group_concentrate_btn" @tap="beginConcentrate">
                                <image class="group_concentrate_btn_icon" src="https://star-img.xingxiaoculture.com/2019/08/12/cee0f770360119e8f52914652674b03b.png"></image>
                                发起集结
                            </view>
                            <view class="group_concentrate_desc">公会集结动态</view>

                            <block v-if="concentrateFeeds.length > 0">
                                <view class="group_concentrate_swiper" :style="'position:relative;overflow:hidden;height:' + concentrateZoneHeight + 'px;'">
                                    <view :style="'position:absolute;left:0px;top:' + concentrateTop + 'px;' + (concentrateTop < 0 ? 'transition: top 1s;' : '')">
                                        <button class="group_concentrate_swiper_cell" @tap="beginConcentrate" v-for="(item, index) in concentrateFeeds" :key="index">
                                            <view v-if="item.type == 2" class="group_concentrate_cell">
                                                <image
                                                    class="group_concentrate_cell_horn_icon"
                                                    src="https://star-img.xingxiaoculture.com/2019/08/09/014c810052731a4f863275e93a33e273.png"
                                                ></image>
                                                <text class="group_concentrate_cell_txt_red">{{ item.name }}</text>
                                                <text class="group_concentrate_cell_txt_gray">完成</text>
                                                <text class="group_concentrate_cell_txt_yellow">{{ item.user_num }}</text>
                                                <text class="group_concentrate_cell_txt_gray">人集结，为Ta</text>
                                                <text class="group_concentrate_cell_txt_yellow">+{{ item.vote_num }}</text>
                                                <text class="group_concentrate_cell_txt_gray">贡献点</text>
                                            </view>

                                            <view v-else class="group_concentrate_cell">
                                                <image
                                                    class="group_concentrate_cell_horn_icon"
                                                    src="https://star-img.xingxiaoculture.com/2019/08/09/889aaa9988fbfc2b2de8d0c9bd7aaee6.png"
                                                ></image>
                                                <text class="group_concentrate_cell_txt_red">{{ item.name }}</text>
                                                <text class="group_concentrate_cell_txt_gray">刚刚发起了集结</text>
                                            </view>
                                        </button>
                                    </view>
                                </view>
                            </block>

                            <block v-else>
                                <image class="concentrateFeeds_none_img" src="https://star-img.xingxiaoculture.com/2019/08/12/8363122d7a44eeeb164333dc47da9d55.png"></image>
                                <view class="concentrateFeeds_none_text">还没有人发起集结哦～</view>
                            </block>
                        </view>
                    </view>
                </view>

                <view v-if="false" style="background: rgba(255, 94, 101, 0.1)">
                    <view class="group_rank_btn" @tap="go2NoneGroupAttack" style="margin-top: 12px; width: 80px">查看详情</view>
                    <view class="group_rank_desc" style="padding-top: 12px; margin-top: 90rpx">
                        <view class="red_line"></view>
                        公会技能玩法
                    </view>
                    <view class="group_rank__detail" style="padding-bottom: 12px">想让你的公会更强大？快去了解公会技能玩法吧！</view>
                </view>

                <view class="group_rank_btn" @tap="checkRankClick">查看公会内排行</view>
                <view class="group_rank_desc">
                    <view class="red_line"></view>
                    公会贡献周排行
                </view>
                <view class="group_rank__detail">本周贡献公会贡献前10名 (每五分钟更新)</view>

                <!-- 请求出错的处理 -->
                <view class="error_view_frame" :style="'display:' + (errorDic && errorDic.needShowErrorView ? 'block' : 'none') + ';'">
                    <error_view :error-dic="errorDic" @clickRetryEvent="retryGetData"></error_view>
                </view>
                <block v-for="(item, index) in fansSupportList" :key="index">
                    <!-- parse <template is="support_list_cell_group" :data="...item, index:index"/> -->
                    <block name="support_list_cell_group">
                        <group_rank_weekList_view
                            :admin_name="admin_name"
                            :title_color="title_color"
                            :group_widget_img="group_widget_img"
                            @groupDetailAttackCLick="groupDetailAttackCLick"
                            :index="index"
                            :score="score"
                            :name="name"
                            :member="member"
                            :people_num="people_num"
                            :app_platform="app_platform"
                            :time="time"
                            :isFansGroup="1"
                            :biggest_gather_num="biggest_gather_num"
                            :group_emblem_img="group_emblem_img"
                            :group_medal_img="group_medal_img"
                        ></group_rank_weekList_view>
                    </block>
                </block>
                <view class="bottom_refresh" :style="'display:' + (showBottomRefresh ? 'block' : 'none') + ';'">
                    <refresh_view :is-animation="bottomAnimation"></refresh_view>
                </view>
                <view class="bottom_desc" :style="'display:' + (!showBottomRefresh && fansSupportList.length > 0 && fansSupportList.length < 10 ? 'block' : 'none') + ';'">
                    暂无更多数据
                </view>
            </block>

            <block v-else>
                <!-- 头部 -->
                <view class="banner">
                    <view class="shadow">
                        <image v-if="groupInfo.backgroup" class="banner-img" mode="widthFix" :src="groupInfo.backgroup ? groupInfo.backgroup : ''" />
                        <view v-else class="banner-img"></view>
                        <view class="switch_group" @tap="bindSwitchGroup">切换</view>
                        <view class="shadow_conWrap">
                            <view class="banner-left">
                                <view class="group-name-bg" v-if="groupInfo.group_union">
                                    <view class="group-alliance-name">{{ groupInfo.group_union.name }}</view>
                                    <image
                                        class="group-alliance-leader"
                                        v-if="groupInfo.group_union.is_leader"
                                        src="https://star-img.xingxiaoculture.com/2022/08/24/f55016271c962379d1f869d8ec4997a5.png"
                                    ></image>
                                </view>
								<view class="group-name-bg">
								  <!-- 公会挂件 -->
								  <image v-if="groupInfo.group_widget_img !== undefined" mode="widthFix" class="group-pendant-image" :src="groupInfo.group_widget_img"></image>
								  <view class="group-famous-brand" v-if="groupInfo.group_emblem_img">
									<image class="group-famous-brand-bg" :src="groupInfo.group_emblem_img"></image>
								  </view>
								  <view class="group-name">
									<text :style="'color: ' + (groupInfo.title_color ? groupInfo.title_color : 'white') + ';'">{{ groupInfo.name }}</text>
									<text class="theMain_group" v-if="groupInfo.gid === mainGroupInfo.gid">主公会</text>
								  </view>
								</view>
                                <view class="group-rank" @tap="go2GroupRankWeekPage">
                                    <image class="group-famous-brand-icon" :src="groupInfo.group_medal_img" v-if="groupInfo.group_medal_img" />
                                    <!-- <text class="group-rankDetail" style="margin-left:10rpx">公会贡献周排行 NO.{{groupInfo.rank}}</text> -->
                                    <text class="group-rankDetail" style="margin-left: 10rpx">公会贡献周排行</text>
                                    <!-- <text class="group-rankDetail">公会贡献周排行 {{groupInfo.rank <= 10 ? 'NO.' + groupInfo.rank : '未上排行'}}</text> -->
                                    <image class="rightArrow" mode="aspectFit" src="https://star-img.xingxiaoculture.com/2019/08/09/7b75cc0bdbdd9a44a3f0fe45f3f78967.png" />
                                </view>
                            </view>
                            <!-- 未签到 -->
                            <view class="checkin_btn" v-if="!is_checkIn" @tap="checkIn">
                                <text v-if="!is_click_checkIn">立即签到</text>
                                <image v-else src="https://star-img.xingxiaoculture.com/2019/08/09/3915ae3f99b6c437ac6684bbbab61412.png" class="loading" mode="aspectFill" />
                            </view>
                            <!-- 9.2去除签到 -->
                            <!-- 已签到 -->
                            <view class="checkin_btn checkin_btn_long" v-else @tap="nav2GroupCheckIn">
                                <image src="https://star-img.xingxiaoculture.com/2019/08/09/c8fc52cac34823efb6bba865cdda0be9.png" mode="aspectFill" class="feather_small" />
                                <text style="font-size: 24rpx">共{{ checkIn_num }}人签到</text>
                            </view>
                        </view>

                        <view class="leader_conWrap" v-if="isManagerAuthority">
                            <view class="leaderGroup_btn" @tap="switchGroupAdminDialog">
                                <text>公会管理</text>
                            </view>
                        </view>
                    </view>
                </view>

                <!-- 公会介绍 -->
                <view class="group-intro">{{ groupInfo.intro ? groupInfo.intro : '' }}</view>

                <!-- 公会相关按钮 -->
                <view class="group-entrances">
                    <view class="group-entrance" @tap="go2GroupRankHotPage">
                        <image src="https://img.xingxiaoculture.com/origin/2020/03/10/367f951f401bdf9946ee005001701e0c1583825711.png" mode="aspectFill" />
                        <view>公会集结</view>
                    </view>
                    <view class="group-entrance" @tap="go2GroupRankPage">
                        <image src="https://img.xingxiaoculture.com/origin/2020/03/10/f991e4eea845ce85ca5e20727d30cdf11583825754.png" mode="aspectFill" />
                        <view>公会排名</view>
                    </view>
                    <view class="group-entrance" @tap="go2YidouExchange">
                        <image src="https://star-img.xingxiaoculture.com/search/topics/images/2021/10/22/GD8XbwZN8B1634891274588.png" mode="aspectFill" />
                        <view>益豆兑换</view>
                    </view>
                    <!-- 9.1 -->
                    <view class="group-entrance" @tap="go2GroupPKPage" v-if="bShowGroupPkBtn">
                        <image src="https://img.xingxiaoculture.com/origin/2020/03/10/877c0cbbd7614e7ec923994074ed99d21583825771.png" mode="aspectFill" />
                        <view>公会战</view>
                    </view>
                    <view class="group-entrance" @tap="go2GroupAttack" v-if="isIdolAdmin">
                        <image src="https://img.xingxiaoculture.com/origin/2020/03/10/c5500edc60d33e5ab643bc1055598f171583825786.png" mode="aspectFill" />
                        <view>公会技能</view>
                    </view>
                    <!-- 9.2 -->
                    <!-- <view class="group-entrance" bind:tap="go2IdolCard">
				<image src="https://img.xingxiaoculture.com/origin/2020/03/10/51850a53db7b4240a02ebb515a5a3c721583825800.png" mode="aspectFill" />
				<view>打卡</view>
			</view> -->
                </view>

                <prepareGuild :groupInfo="groupInfo" :prepareGuild="prepareGuild" v-if="groupInfo.is_prepared == 1"></prepareGuild>

                <!-- 轮播图 -->
                <swiper
                    class="operation_lunbo"
                    :autoplay="swiperAutoPlay"
                    :indicator-dots="operationLunbotu && operationLunbotu.length > 1"
                    indicator-active-color="#FF5686"
                    :circular="true"
                    v-if="operationLunbotu && operationLunbotu.length > 0"
                >
                    <swiper-item
                        @tap="gotoLunbo"
                        :data-ind="index2"
                        :data-url="item.url"
                        :data-activity="item.is_clock_in_activity"
                        :data-item="item"
                        v-for="(item, index2) in operationLunbotu"
                        :key="index2"
                    >
                        <idc_image
                            class="operation_lunbo_item"
                            :src="item.img"
                            :mode="'scaleToFill'"
                            place-holder-img="https://star-img.xingxiaoculture.com/2019/08/09/fc4af5e6ca867294d7f831fa3aa725ef.png"
                            radius="10px"
                        ></idc_image>
                    </swiper-item>
                </swiper>

                <!-- 加公会福利入口 -->
                <!-- 9.1 -->
                <!-- <view class="fuli_btn" bindtap="go2CoderUrl" wx:if="{{bShowFuliBtn}}">
			<image mode="aspectFit" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/8/30/kESjAPQd2i1630307130518.png" class="fuli_img" />
		</view> -->

                <!-- 星球联赛 -->
                <view class="starLeague">
                    <view class="starLeague-title">
                        <view class="starLeague-text" style="display: flex">
                            星球联赛
                            <image
                                @tap="gotoRulePageLeague"
                                class="starLeague-text-icon"
                                src="https://star-img.xingxiaoculture.com/2022/05/26/c9a1f53f79cbe01ff3cdd87ab1780228.png"
                            ></image>
                        </view>
                        <view class="starLeague-text" :style="'text-align: center;' + (starBallLeagueInfo.in_game == 0 ? 'visibility:hidden' : '')">
                            {{
                                starBallLeagueInfo.in_counting
                                    ? '数据结算中'
                                    : starBallLeagueInfo.game_status == 'is_closed'
                                    ? '本月星球联赛已结束'
                                    : starBallLeagueInfo.game_tomorrow_info.begin_time
                                    ? actCountdownTimeStr + '后开启'
                                    : actCountdownTimeStr + '后结束'
                            }}
                        </view>
                        <view class="starLeague-text" style="text-align: right" @tap="lookMoreLeague">查看更多></view>
                    </view>

                    <!-- 结算状态 -->
                    <view class="starLeague-detail" v-if="starBallLeagueInfo.game_yesterday_info.begin_time && starBallLeagueInfo.in_counting">
                        <view class="starLeague-detail-blackview">
                            <text style="text-align: center">{{ '今日PK已结束，数据结算中，3分钟后更新结果\n届时请手动刷新' }}</text>
                        </view>
                        <view class="starLeague-starball">
                            <view
                                class="starLeague-starball-leader"
                                v-if="starBallLeagueInfo.game_yesterday_info.host_votetimes > starBallLeagueInfo.game_today_info.visitor_votetimes"
                            >
                                领先
                            </view>
                            <image
                                class="starLeague-starball-icon"
                                :src="starBallLeagueInfo.game_yesterday_info.host_star_ball.logo_url"
                                @tap="gotoStarPage"
                                :data-starid="starBallLeagueInfo.game_yesterday_info.host_star_ball.starid"
                            ></image>
                            <view class="starLeague-starball-text" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_yesterday_info.host_star_ball.starid">
                                {{ starBallLeagueInfo.game_yesterday_info.host_star_ball.name }}
                            </view>
                            <view class="starLeague-starball-score">贡献值：{{ starBallLeagueInfo.game_yesterday_info.host_votetimes }}</view>
                            <view class="starLeague-starball-head" v-if="starBallLeagueInfo.game_yesterday_info.host_user_list_day.length > 0">
                                <block v-for="(item, index) in starBallLeagueInfo.game_yesterday_info.host_user_list_day" :key="index">
                                    <image class="starLeague-starball-headicon" :src="item.userinfo.image.middle_pic" @tap="gotoDressRoom" :data-userid="item.userinfo._id"></image>
                                </block>
                            </view>
                        </view>
                        <view class="starLeague-vs">
                            <view class="starLeague-vs-text">VS</view>
                            <view class="starLeague-vs-date">{{ starBallLeagueInfo.game_yesterday_info.date_text }}</view>
                        </view>
                        <view class="starLeague-starball">
                            <view
                                class="starLeague-starball-leader"
                                v-if="starBallLeagueInfo.game_yesterday_info.host_votetimes < starBallLeagueInfo.game_today_info.visitor_votetimes"
                            >
                                领先
                            </view>
                            <image
                                class="starLeague-starball-icon"
                                :src="starBallLeagueInfo.game_yesterday_info.visitor_star_ball.logo_url"
                                @tap="gotoStarPage"
                                :data-starid="starBallLeagueInfo.game_yesterday_info.visitor_star_ball.starid"
                            ></image>
                            <view class="starLeague-starball-text" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_yesterday_info.visitor_star_ball.starid">
                                {{ starBallLeagueInfo.game_yesterday_info.visitor_star_ball.name }}
                            </view>
                            <view class="starLeague-starball-score">贡献值：{{ starBallLeagueInfo.game_yesterday_info.visitor_votetimes }}</view>
                            <view class="starLeague-starball-head" v-if="starBallLeagueInfo.game_yesterday_info.visitor_user_list_day.length > 0">
                                <block v-for="(item, index) in starBallLeagueInfo.game_yesterday_info.visitor_user_list_day" :key="index">
                                    <image class="starLeague-starball-headicon" :src="item.userinfo.image.middle_pic" @tap="gotoDressRoom" :data-userid="item.userinfo._id"></image>
                                </block>
                            </view>
                        </view>
                    </view>
                    <!-- 进行中状态 -->
                    <view class="starLeague-detail" v-else-if="starBallLeagueInfo.game_today_info.begin_time">
                        <view class="starLeague-starball">
                            <view
                                class="starLeague-starball-leader"
                                v-if="starBallLeagueInfo.game_today_info.host_votetimes > starBallLeagueInfo.game_today_info.visitor_votetimes"
                            >
                                领先
                            </view>
                            <image
                                class="starLeague-starball-icon"
                                :src="starBallLeagueInfo.game_today_info.host_star_ball.logo_url"
                                @tap="gotoStarPage"
                                :data-starid="starBallLeagueInfo.game_today_info.host_star_ball.starid"
                            ></image>
                            <view class="starLeague-starball-text" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_today_info.host_star_ball.starid">
                                {{ starBallLeagueInfo.game_today_info.host_star_ball.name }}
                            </view>
                            <view class="starLeague-starball-score">贡献值：{{ starBallLeagueInfo.game_today_info.host_votetimes }}</view>
                            <view class="starLeague-starball-head" v-if="starBallLeagueInfo.game_today_info.host_user_list_day.length > 0">
                                <block v-for="(item, index) in starBallLeagueInfo.game_today_info.host_user_list_day" :key="index">
                                    <image class="starLeague-starball-headicon" :src="item.userinfo.image.middle_pic" @tap="gotoDressRoom" :data-userid="item.userinfo._id"></image>
                                </block>
                            </view>
                        </view>
                        <view class="starLeague-vs">
                            <view class="starLeague-vs-text">VS</view>
                            <view class="starLeague-vs-date">{{ starBallLeagueInfo.game_today_info.date_text }}</view>
                        </view>
                        <view class="starLeague-starball">
                            <view
                                class="starLeague-starball-leader"
                                v-if="starBallLeagueInfo.game_today_info.host_votetimes < starBallLeagueInfo.game_today_info.visitor_votetimes"
                            >
                                领先
                            </view>
                            <image
                                class="starLeague-starball-icon"
                                :src="starBallLeagueInfo.game_today_info.visitor_star_ball.logo_url"
                                @tap="gotoStarPage"
                                :data-starid="starBallLeagueInfo.game_today_info.visitor_star_ball.starid"
                            ></image>
                            <view class="starLeague-starball-text" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_today_info.visitor_star_ball.starid">
                                {{ starBallLeagueInfo.game_today_info.visitor_star_ball.name }}
                            </view>
                            <view class="starLeague-starball-score">贡献值：{{ starBallLeagueInfo.game_today_info.visitor_votetimes }}</view>
                            <view class="starLeague-starball-head" v-if="starBallLeagueInfo.game_today_info.visitor_user_list_day.length > 0">
                                <block v-for="(item, index) in starBallLeagueInfo.game_today_info.visitor_user_list_day" :key="index">
                                    <image class="starLeague-starball-headicon" :src="item.userinfo.image.middle_pic" @tap="gotoDressRoom" :data-userid="item.userinfo._id"></image>
                                </block>
                            </view>
                        </view>
                    </view>
                    <!-- 预告状态 -->
                    <view class="starLeague-detail" v-else-if="starBallLeagueInfo.game_tomorrow_info.begin_time">
                        <view class="starLeague-starball">
                            <!-- <view class="starLeague-starball-leader">领先</view> -->
                            <image
                                class="starLeague-starball-icon"
                                :src="starBallLeagueInfo.game_tomorrow_info.host_star_ball.logo_url"
                                @tap="gotoStarPage"
                                :data-starid="starBallLeagueInfo.game_tomorrow_info.host_star_ball.starid"
                            ></image>
                            <view class="starLeague-starball-text" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_tomorrow_info.host_star_ball.starid">
                                {{ starBallLeagueInfo.game_tomorrow_info.host_star_ball.name }}
                            </view>
                        </view>
                        <view class="starLeague-vs">
                            <view class="starLeague-vs-text">VS</view>
                            <view class="starLeague-vs-date">{{ starBallLeagueInfo.game_tomorrow_info.date_text }}</view>
                        </view>
                        <view class="starLeague-starball">
                            <image
                                class="starLeague-starball-icon"
                                :src="starBallLeagueInfo.game_tomorrow_info.visitor_star_ball.logo_url"
                                @tap="gotoStarPage"
                                :data-starid="starBallLeagueInfo.game_tomorrow_info.visitor_star_ball.starid"
                            ></image>
                            <view class="starLeague-starball-text" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_tomorrow_info.visitor_star_ball.starid">
                                {{ starBallLeagueInfo.game_tomorrow_info.visitor_star_ball.name }}
                            </view>
                        </view>
                    </view>
                    <!-- 赛季结束状态 -->
                    <view class="starLeague-end" v-else-if="starBallLeagueInfo.in_game == 1">
                        <image
                            class="starLeague-end-icon"
                            @tap="gotoStarPage"
                            :data-starid="starBallLeagueInfo.star_ball.starid"
                            :src="starBallLeagueInfo.star_ball.logo_url"
                        ></image>
                        <view class="starLeague-end-textbox">
                            <view class="starLeague-end-name" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.star_ball.starid">{{ starBallLeagueInfo.star_ball.name }}</view>
                            <view class="starLeague-end-score">{{ starBallLeagueInfo.star_ball_score }}积分</view>
                        </view>
                        <view class="starLeague-starball-head2" v-if="starBallLeagueInfo.user_list_month.length > 0">
                            <block v-for="(item, index) in starBallLeagueInfo.user_list_month" :key="index">
                                <image class="starLeague-starball-headicon" :src="item.userinfo.image.middle_pic"></image>
                            </block>
                        </view>
                    </view>
                    <!-- 不参与状态 -->
                    <view class="starLeague-nogame" v-else-if="starBallLeagueInfo.in_game == 0">
                        <view class="starLeague-nogame-text">当前星球未参与星球联赛</view>
                        <view class="starLeague-nogame-desc">
                            点击查看
                            <text class="starLeague-nogame-link" @tap="gotoRulePageLeague">星球联赛参赛规则</text>
                        </view>
                    </view>
                </view>
                <!-- 我在本公会排名 -->
                <view class="my_support">
                    <image class="honor_bgImg" @tap="myHonorClick" :src="user_info_rank.userinfo.honor.background"></image>
                    <view class="myRank-title">我的守护</view>

                    <view class="my_support_self">
                        <topImage :pendant="user_info_rank.userinfo.head_frame" width="96" height="96" padding="14">
                            <image slot="topImage" class="my_support_image" :src="user_info_rank.userinfo.image.middle_pic"></image>
                        </topImage>
                        <image
                            class="userIconImg_honor"
                            :src="user_info_rank.userinfo.honor.icon"
                            @tap="parseEventDynamicCode($event, is_login ? 'gotoHomePage' : 'getUserIcon')"
                            :data-userid="userId"
                        ></image>
                        <view class="my_support_self_layout">
                            <view class="my_support_name_txt_layout">
                                <text class="my_support_name_txt" :style="'color:#' + (user_info_rank.userinfo.is_vip == 1 ? 'ff872d' : '666666')">
                                    {{ user_info_rank.userinfo.nickname }}
                                </text>
                                <image
                                    v-if="user_info_rank.userinfo.is_vip == 1"
                                    class="my_support_name_txt_vip_icon"
                                    src="https://star-img.xingxiaoculture.com/search/topics/images/2020/8/4/ecKCCpRzeR1596530582740.png"
                                ></image>
                                <image v-if="user_info_rank.userinfo.medal" class="my_support_name_icon" :src="user_info_rank.userinfo.medal.img"></image>
                            </view>
                            <text class="my_support_self_txt my_support_white" space="nbsp">
                                累计守护
                                <text class="font_yellow">{{ user_info_rank.vote.all_days }}</text>
                                天 连续守护
                                <text class="font_yellow">{{ user_info_rank.vote.continual_days }}</text>
                                天
                            </text>
                        </view>
                        <view class="my_support_guard" @tap="go2starExpPage">
                            <image class="my_support_guard_image" src="/static/images/listSupport/idol_guard_main_support.png"></image>
                            <text class="my_support_guard_txt">公会守护历程</text>
                        </view>
                    </view>

                    <view class="my_support_name" @tap="parseEventDynamicCode($event, is_login ? '' : 'getUserIcon')">
                        <!-- 
				<block wx:elif="{{!is_login}}">
					<text class='my_support_login'>立即登录</text>
				</block> -->
                        <!-- 8.9 -->
                        <!-- 	<button wx:if="{{is_login}}" class='my_support_image_change' open-type="contact" bindtap='go2BuyCoinPage' session-from="coin_pay">获取公会币</button>
				<button wx:if="{{is_login}}" class='my_support_image_change' open-type="contact" bindtap='go2WebPage' session-from="guard_pay">获取补守护次数</button> -->
                    </view>
                    <!-- 9.1去掉vip -->
                    <!-- <view class="vip_card" style="margin-top: 60rpx;">
				<vip_guide_card refresh="{{refreshVipcard}}" uiType="2" isGuard="true" isSet="{{isSet}}" starid="{{starIDStr}}" starName="{{starName}}" bind:changeSwitch='onChangeSwitch'  bind:buyVip="buyVip"></vip_guide_card>
			</view> -->
                </view>

                <!-- 这里需要加入我新写的部分 -->
                <view class="myTopRankView" v-if="topRankData.length > 0">
                    <view class="topRankTitleView">
                        <text class="topRankTitleText">{{ topRankTitle }}</text>
                        <text class="topRankDetailText">{{ topRankDetail }}</text>
                    </view>
                    <swiper class="topRankSwiper" :indicator-dots="false" :autoplay="false" indicator-active-color="#F7941C" @change="onSlideChangeEnd">
                        <block v-for="(item, index) in topRankData" :key="index">
                            <swiper-item class="topRankItem">
                                <block v-for="(item, idx1) in topRankData[index]" :key="idx1">
                                    <view class="topRankCell" @tap="topRankItemCellClick" :data-idx="idx1" :data-itemindex="index">
                                        <view class="div_header">
                                            <topImage :pendant="item.head_frame" width="76" height="76" padding="14">
                                                <view slot="topImage" class="boardLi-imgWrap">
                                                    <image
                                                        :src="
                                                            item.image.thumbnail_pic
                                                                ? item.image.thumbnail_pic
                                                                : 'https://star-img.xingxiaoculture.com/2022/04/24/79a49fcc69ca20f710cd86673cf537db.png'
                                                        "
                                                        mode="aspectFill"
                                                        class="boardLi-img"
                                                        :style="'border: 2rpx solid ' + (idx1 == 0 ? '#FFD931' : idx1 == 1 ? '#CECDC8' : '#F3BB71') + ';'"
                                                    />
                                                </view>
                                            </topImage>
                                            <image :src="'../../images/common/num' + (idx1 + 1) + '/static/pages/idol_home/.png'" mode="aspectFill" class="boardLi-num" />
                                        </view>

                                        <view class="boardLi-rightBgView">
                                            <view class="boardLi-uName">{{ item.nickname }}</view>
                                            <view class="boardLi-scoreWrap">
                                                <view class="boardLi-score">{{ item.continual_days >= 0 ? item.continual_days + '天' : item.rq_score }}</view>
                                                <image
                                                    src="https://star-img.xingxiaoculture.com/2019/08/09/dea5a2a612f05334e8cf483bf7cadb8b.png"
                                                    mode="aspectFill"
                                                    v-if="item.continual_days == nil"
                                                    class="i-fire"
                                                />
                                            </view>
                                        </view>
                                    </view>
                                </block>
                            </swiper-item>
                        </block>
                    </swiper>
                    <view class="topRankIndicatorView">
                        <block v-for="(item, index) in topRankData" :key="index">
                            <view class="itemIndicatorView" :style="'background:' + (index == currentIndex ? '#F7941C' : '#D8D8D8') + ';'"></view>
                        </block>
                    </view>
                </view>

                <!-- 动态 -->
                <view class="contentHeaderView" v-if="false">
                    <image class="headerImage" src="https://star-img.xingxiaoculture.com/2019/08/09/ffb2b911ef7082f964e840b9974b9f82.png"></image>
                    <text class="headerText">{{ headerText }}</text>
                </view>

                <view class="mainDynamicView">
                    <block v-for="(item, index) in dynamicItemLists" :key="index">
                        <!-- parse <template is="dynamicCellList" :data="item:dynamicItemLists[index],windowWidth:windowWidth,dynamicData:dynamicItemLists[index].data_quanzi,isDisplayTotalCount:true,cellIndex:index,angelLevel:dynamicItemLists[index].data_quanzi.angleinfo.level,showFrom:1"></template> -->
                        <block name="dynamicCellList">
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
                        <text class="noDataText">{{ noDataText }}</text>
                    </view>
                    <view
                        class="bottom_desc"
                        :style="'margin-bottom:10px;margin-top: 10px;display:' + (!showBottomRefresh && dynamicItemLists.length > 0 ? 'block' : 'none') + ';'"
                    >
                        没有更多动态了~
                    </view>
                    <view class="bottom_refresh" :style="'display:' + (showBottomRefresh ? 'block' : 'none') + ';'">
                        <refresh_view :is-animation="bottomAnimation"></refresh_view>
                    </view>
                </view>

                <image
                    class="bottom_rose_tree_tips"
                    v-if="isShowRoseTips"
                    src="https://img.xingxiaoculture.com/origin/2020/08/06/eca02b371359a28c8edf25978f2685691596700603.png"
                    @tap="roseTipsClick"
                ></image>

                <view class="div_menu">
                    <!-- 发布动态按钮 -->
                    <image
                        class="bottom_publish_button"
                        src="http://img.xingxiaoculture.com/origin/2020/03/02/666ee8a7184fa94a76f49f3ce29641611583135357.png"
                        @tap="gotoPublishPage"
                    ></image>
                    <!-- 守护树 -->
                    <block v-if="groupViewHistory && groupViewHistory.is_office_group == 1">
                        <!-- <block wx:if="{{isFestival}}">
					<image class='bottom_support_tree_button' src="{{treeInfo.pass==1?treeInfo.img_water:treeInfo.img_normal}}" bindtap='gotoSupportTreePage'></image>
				</block>
				<block wx:else>
					<image class='bottom_support_tree_button' src="{{treeInfo.pass==1?'http://img.xingxiaoculture.com/origin/2020/03/02/a85e54251f9f2fd652a6288ca61bf73b1583135171.png':'http://img.xingxiaoculture.com/origin/2020/03/02/bdbdae90a55002cd2ffc08f6aa3ca1c61583135192.png'}}" bindtap='gotoSupportTreePage'></image>
				</block> -->
                        <image
                            class="bottom_support_tree_button"
                            style="width: 99rpx; height: 99rpx"
                            src="https://star-img.xingxiaoculture.com/2023/05/19/1a2844f74a98def66eea2a47fccc5c0d.png"
                            @tap="gotoIslandPage"
                        ></image>
                    </block>
                </view>

                <image v-if="spreadImg.animate_img_src" class="share_group_icon daily_welfare_animate" @tap="groupJoinActImage" :src="spreadImg.animate_img_src"></image>

                <!-- 底部buttom -->
                <view style="width: 100vw; height: 130rpx"></view>

                <!-- 送星模块 -->
                <send_star @initPageData="initPageData($event, { tagId: 'sendStar' })" :sid="starIDStr" :user_info="user_info" id="sendStar"></send_star>
            </block>
        </view>
        <view v-else class="no_group_block">
            <view class="no_group_top">随机展示公会</view>
            <view class="no_group_item" v-for="(item, index) in nogroupList" :key="index">
                <image class="no_group_item_img" :src="item.icon"></image>

                <view class="no_group_item_title">{{ item.name }}</view>
            </view>
            <view class="no_group_bottom">
                您当前还没有公会哦～
                <view
                    :class="'no_group_bottom_botton ' + (nogroupApply ? 'no_group_bottom_botton_forbid' : '')"
                    @tap="parseEventDynamicCode($event, is_login ? 'applyGroup' : 'getUserIcon')"
                >
                    {{ nogroupApply ? '已申请' : '一键申请' }}
                </view>
            </view>
        </view>
        <!-- <web-view wx:else src="https://m.xingxiaoculture.com/clock_in_activity/base_address.html?from=minip"></web-view> -->
        <!-- <image wx:else style="width: 100%;position: relative;" mode="widthFix" src="https://star-img.xingxiaoculture.com/search/topics/images/2021/8/31/ySBfSrQsEH1630416449781.jpg"></image> -->

        <login_panel v-if="bShowLoginPanel" @loginCallback="loginCallback" @closeViewEvent="closeLoginView"></login_panel>

        <share_panel panel_title="数据加载失败了～" panel_button_title="再试一次" isShare="0" v-if="bShowNotGetGroupGid" @requsetGidCallback="requsetGidCallback"></share_panel>

        <view class="bottomThreeDisplayView" v-if="!isHiddenBottomBtnView">
            <view class="bottomThreeDisplayViewInner">
                <view class="rankListView" @tap="gotoSupportPage">
                    <idc_image class="rankListView_icon" :src="groupInfo.starinfo.logo_img_v2" radius="50%"></idc_image>
                    <text class="bottomTitle">送星星</text>
                </view>
                <view class="publishView" @tap="gotoPublishPage">
                    <image class="bottomIconImage" src="https://star-img.xingxiaoculture.com/2019/08/09/3df269598d405ff588c5b9b0c33ab62c.png"></image>
                    <text class="bottomMiddleTitle">发布</text>
                </view>
                <view class="messageView" @tap="gotoMessagePage">
                    <image class="rankListView_icon" mode="aspectFit" src="https://star-img.xingxiaoculture.com/2019/08/12/acd7a25210f3973838e86a3c1a6bf74e.png">
                        <view class="messageCountView" v-if="messageCount > 0">{{ messageCount }}</view>
                    </image>
                    <text class="bottomTitle">消息</text>
                </view>
            </view>
            <view style="display: flex; flex-direction: row; justify-content: center; align-items: center; height: 8px"></view>
        </view>

        <!-- 9.1去掉发布 -->
        <group_spaceDynamicTip
            @tap.native="closeTipDisplay"
            v-if="isFirstComePage && false"
            tipImg="https://star-img.xingxiaoculture.com/2019/08/10/83af193a774f933e05cf46ad206be6b2.png"
            btnImg="https://star-img.xingxiaoculture.com/2019/08/10/d8483b4a40fddda5d5568461e18b724f.png"
            :tipImgFrame="tipFrameDic"
            :btnImgFrame="btnTipFraameDic"
        ></group_spaceDynamicTip>

        <idol_toastTip_view
            v-if="isDisplayToast"
            :tipType="tipType"
            :toastIcon="tipToastIcon"
            :toastTitle="tipToastTitle"
            alertTitle="确定删除这条动态吗？"
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
            alertTitle="该条动态已被删除！"
            alertLeftBtnTitle="知道了"
            alertLeftBtnTitleColor="#262626"
            @oneBtnClick="myAlertLeftIKnowBtnClick"
        ></idol_toastTip_view>

        <view class="modal flex-h_center" v-if="bShowGotoGroupGather && !isShowSetingMainGroupWindows">
            <view class="modal-con flex-c_center">
                <image src="https://star-img.xingxiaoculture.com/2019/08/10/b1dcab7df054af79972d344b97f8a524.png" class="i-close" @tap="closeModalGroupGather" />
                <image src="https://star-img.xingxiaoculture.com/2019/08/10/eb0eec3336eadb33a9cc81625e74538b.png" class="i-horn" />
                <view class="modal-txt">小伙伴喊你来集结啦！</view>
                <view class="modal-txt">助力{{ starName }}更多贡献点！</view>
                <button class="modal-btn" @tap="modalGGBtnConfirm">立即集结</button>
            </view>
        </view>

        <view class="modal flex-h_center" v-if="bShowGotoActivityDialogue">
            <view class="modal-con flex-c_center" v-if="isApp">
                <image src="https://star-img.xingxiaoculture.com/2019/08/10/b1dcab7df054af79972d344b97f8a524.png" class="i-close" @tap="closeModalActivityDialogue" />
                <view class="modal-txt" style="margin-top: 28rpx">点击下方按钮，把链接发送到微信</view>
                <view class="modal-txt">再从微信内打开即可</view>
                <button class="modal-btn" open-type="share">点击发送链接</button>
            </view>
            <view class="modal-con flex-c_center" v-else-if="">
                <image src="https://star-img.xingxiaoculture.com/2019/08/10/b1dcab7df054af79972d344b97f8a524.png" class="i-close" @tap="closeModalActivityDialogue" />
                <view class="modal-txt" style="margin-top: 28rpx">点击按钮进入客服消息后</view>
                <view class="modal-txt">回复3即可</view>
                <button class="modal-btn" open-type="contact" session-from="clock_in_activity_banner">点击后回复"3"</button>
            </view>
        </view>

        <idolConcentrateAlert v-if="bShowConcentrateAlert" @closeView="closeConcentrateAlert"></idolConcentrateAlert>

        <fansGroupView
            class="fansGroupView"
            v-if="bShowWindowsStarSupportView"
            :groupGid="WeChatGroupGid"
            :bHasShareTicket="bHasShareTicket"
            :windowsType="fansGroupWindowsType"
            :selectIndex="selectIndex"
            :starArr="supportStarList"
            @sureClick="sureClick"
            @supportPageClick="supportPageClick"
            @closeClick="closeClick"
            @selectClick="selectClick"
        ></fansGroupView>

        <attack_selectGroup_windows
            v-if="bShowAttackWindows"
            :groupInfo="selectGroupInfo"
            :groupname="selectGroupInfo.name"
            :selectGroupAttackInfo="selectGroupAttackInfo"
            @clickCloseModal="clickCloseModal"
            @frezzeClick="frezzeClick"
            :windowsType="showAttackType"
        ></attack_selectGroup_windows>

        <group_freeze_windows v-if="bFrezzeWindows" @backRewardClick="backRewardClick" :windowsType="bFrezzeWindowsType"></group_freeze_windows>

        <honor_windows
            v-if="bHonorWindows"
            @backRewardClick="backRewardClick"
            @moreGroupInfoClick="moreGroupInfoClick"
            @userSelectClick="userSelectClick"
            @pushUserHome="pushUserHome"
            :windowsType="honorWindowsType"
            :userArr="userArr"
            :honorinfo="honorWindowsModel"
        ></honor_windows>

        <mainGroup_settingWindows
            v-if="isShowSetingMainGroupWindows"
            :windowsType="mainGroupSetingWindowsType"
            :starName="starName"
            :groupInfo="groupInfo"
            :mainGroupInfo="mainGroupInfo"
            @closeMainGroupWindowsClick="closeMainGroupWindowsClick"
            @singleSetingClick="singleSetingClick"
            @leftSetingOthersClick="leftSetingOthersClick"
            @rightSetingThisClick="rightSetingThisClick"
            :setGroupMsg="setGroupMsg"
        ></mainGroup_settingWindows>

        <!-- <vip_auto_dialog show="{{showVipAuto}}" starid="{{starIDStr}}" defaultStar="{{defaultStar}}" chooseStar="{{starName}}" bind:sureClick="setAutoGuard"></vip_auto_dialog> -->

        <vip_buy_tip v-if="buyVipDialog" :registerTime="userInfo.register_time" @sureClick="buyVipSureClick" @closeClick="buyVipCloseClick"></vip_buy_tip>

        <real_name_authentication v-if="isItRealName" @closeViewEvent="closeViewEvent" :official_group="official_group"></real_name_authentication>

        <!-- 切换公会弹窗 -->
        <dialogFinal :type="dialogFinalType" @dialogCallback="dialogCallback" :groupList="getGroupMainList.list" :group="thisGroupMain"></dialogFinal>
        <rewardWelfareDialog id="rewardWelfareDialog"></rewardWelfareDialog>

        <!-- 铁粉浇水任务提示 -->
        <view class="modal flex-h_center" v-if="bShowTips">
            <view class="img-bg-tips flex-c_center">
                <image src="https://star-img.xingxiaoculture.com/search/topics/images/2022/3/7/EsTk3zZDDx1646655365046.png" class="i-close1" @tap="closeTips" />
                <view class="modal-txt">去公会树完成浇水任务</view>
            </view>
        </view>

        <block v-if="bShowsStarLeagueTips">
            <!-- 星球联赛结果弹窗 -->
            <view class="modal flex-c_center" v-if="bShowsStarLeagueTipsType == 1">
                <view class="img-bg-tips-league">
                    <view class="img-bg-tips-league-title">星球联赛</view>
                    <view class="img-bg-tips-league-item" v-if="starBallLeagueInfo.game_yesterday_info.begin_time">
                        <view class="img-bg-tips-league-item-textbox">
                            <view class="img-bg-tips-league-item-text1">昨日PK对手</view>
                            <view class="img-bg-tips-league-item-text2" @tap="lookMoreLeague">查看更多></view>
                        </view>
                        <view class="img-bg-tips-league-item-textbox">
                            <view class="img-bg-tips-league-item-column-out" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_yesterday_info.host_star_ball.starid">
                                <view class="img-bg-tips-league-item-column-in" style="margin-right: 50rpx">
                                    <view class="img-bg-tips-league-item-winscore" v-if="starBallLeagueInfo.game_yesterday_info.host_win == 1">积分+3</view>
                                    <view class="img-bg-tips-league-item-winscore" v-if="starBallLeagueInfo.game_yesterday_info.host_win == 0">积分+1</view>
                                    <image class="img-bg-tips-league-item-icon" :src="starBallLeagueInfo.game_yesterday_info.host_star_ball.logo_url"></image>
                                    <view class="img-bg-tips-league-item-text3">{{ starBallLeagueInfo.game_yesterday_info.host_star_ball.name }}</view>
                                    <view class="img-bg-tips-league-item-score">贡献值：{{ starBallLeagueInfo.game_yesterday_info.host_votetimes }}</view>
                                </view>
                                <image
                                    class="img-bg-tips-league-item-hosticon"
                                    v-if="starBallLeagueInfo.game_yesterday_info.host_win == 1"
                                    src="https://star-img.xingxiaoculture.com/2022/05/27/bab034f7f4f172faeca1abf1865111db.png"
                                ></image>
                                <image
                                    class="img-bg-tips-league-item-hosticon"
                                    v-if="starBallLeagueInfo.game_yesterday_info.host_win == -1"
                                    src="https://star-img.xingxiaoculture.com/2022/05/27/bc5586320a2b411a80e3bbb26bc2299d.png"
                                ></image>
                                <image
                                    class="img-bg-tips-league-item-hosticon"
                                    v-if="starBallLeagueInfo.game_yesterday_info.host_win == 0"
                                    src="https://star-img.xingxiaoculture.com/2022/05/31/540fb458ee893d75d96dd27b8c17e766.png"
                                ></image>
                            </view>
                            <view class="img-bg-tips-league-item-vs">VS</view>
                            <view class="img-bg-tips-league-item-column-out" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_yesterday_info.visitor_star_ball.starid">
                                <view class="img-bg-tips-league-item-column-in" style="margin-left: 50rpx">
                                    <view class="img-bg-tips-league-item-winscore" v-if="starBallLeagueInfo.game_yesterday_info.visitor_win == 1">积分+3</view>
                                    <view class="img-bg-tips-league-item-winscore" v-if="starBallLeagueInfo.game_yesterday_info.visitor_win == 0">积分+1</view>
                                    <image class="img-bg-tips-league-item-icon" :src="starBallLeagueInfo.game_yesterday_info.visitor_star_ball.logo_url"></image>
                                    <view class="img-bg-tips-league-item-text3">{{ starBallLeagueInfo.game_yesterday_info.visitor_star_ball.name }}</view>
                                    <view class="img-bg-tips-league-item-score">贡献值：{{ starBallLeagueInfo.game_yesterday_info.visitor_votetimes }}</view>
                                </view>
                                <image
                                    class="img-bg-tips-league-item-visiticon"
                                    v-if="starBallLeagueInfo.game_yesterday_info.visitor_win == 1"
                                    src="https://star-img.xingxiaoculture.com/2022/05/27/bab034f7f4f172faeca1abf1865111db.png"
                                ></image>
                                <image
                                    class="img-bg-tips-league-item-visiticon"
                                    v-if="starBallLeagueInfo.game_yesterday_info.visitor_win == -1"
                                    src="https://star-img.xingxiaoculture.com/2022/05/27/bc5586320a2b411a80e3bbb26bc2299d.png"
                                ></image>
                                <image
                                    class="img-bg-tips-league-item-visiticon"
                                    v-if="starBallLeagueInfo.game_yesterday_info.visitor_win == 0"
                                    src="https://star-img.xingxiaoculture.com/2022/05/31/540fb458ee893d75d96dd27b8c17e766.png"
                                ></image>
                            </view>
                        </view>
                    </view>
                    <view class="img-bg-tips-league-line" v-if="starBallLeagueInfo.game_yesterday_info.begin_time && starBallLeagueInfo.game_today_info.begin_time"></view>
                    <view class="img-bg-tips-league-item" v-if="starBallLeagueInfo.game_today_info.begin_time">
                        <view class="img-bg-tips-league-item-textbox">
                            <view class="img-bg-tips-league-item-text1">今日PK对手</view>
                        </view>
                        <view class="img-bg-tips-league-item-textbox">
                            <view class="img-bg-tips-league-item-column-out" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_today_info.host_star_ball.starid">
                                <view class="img-bg-tips-league-item-column-in" style="margin-right: 50rpx">
                                    <image class="img-bg-tips-league-item-icon" :src="starBallLeagueInfo.game_today_info.host_star_ball.logo_url"></image>
                                    <view class="img-bg-tips-league-item-text3">{{ starBallLeagueInfo.game_today_info.host_star_ball.name }}</view>
                                </view>
                            </view>
                            <view class="img-bg-tips-league-item-vs">VS</view>
                            <view class="img-bg-tips-league-item-column-out" @tap="gotoStarPage" :data-starid="starBallLeagueInfo.game_today_info.visitor_star_ball.starid">
                                <view class="img-bg-tips-league-item-column-in" style="margin-left: 50rpx">
                                    <image class="img-bg-tips-league-item-icon" :src="starBallLeagueInfo.game_today_info.visitor_star_ball.logo_url"></image>
                                    <view class="img-bg-tips-league-item-text3">{{ starBallLeagueInfo.game_today_info.visitor_star_ball.name }}</view>
                                </view>
                            </view>
                        </view>
                    </view>
                    <view class="img-bg-tips-league-msg">{{ starBallLeagueInfo.rules }}</view>
                </view>
                <image src="https://star-img.xingxiaoculture.com/2022/05/27/aba62d554f894fdae48c45deacdbe33f.png" class="img-bg-tips-close-under" @tap="closeLeagueTips" />
            </view>

            <!-- 星球联赛赛程弹窗 -->
            <view class="modal flex-c_center" v-if="bShowsStarLeagueTipsType == 2">
                <scroll-view scroll-y class="img-bg-tips-league" style="height: 900rpx">
                    <view class="img-bg-tips-league-title">本月赛程表</view>
                    <view class="img-bg-tips-league-item2" v-for="(item, index) in starLeagueScheduleList" :key="index">
                        <view class="img-bg-tips-league-item2-date">{{ item.date_text }}</view>

                        <view class="img-bg-tips-league-item2-vs">vs</view>

                        <view class="img-bg-tips-league-item2-name" @tap="gotoStarPage" :data-starid="item.visitor_star_ball.starid">{{ item.visitor_star_ball.name }}</view>

                        <image
                            class="img-bg-tips-league-item2-icon"
                            :src="item.visitor_star_ball.logo_url"
                            @tap="gotoStarPage"
                            :data-starid="item.visitor_star_ball.starid"
                        ></image>
                    </view>
                </scroll-view>
                <image src="https://star-img.xingxiaoculture.com/2022/05/27/aba62d554f894fdae48c45deacdbe33f.png" class="img-bg-tips-close-under" @tap="closeLeagueTips" />
            </view>
        </block>

        <view class="modal flex-h_center" v-if="bShowGroupAdminDialog">
            <view class="modal-con flex-c_center">
                <image src="https://star-img.xingxiaoculture.com/2019/08/10/b1dcab7df054af79972d344b97f8a524.png" class="i-close" @tap="switchGroupAdminDialog" />
                <view class="modal-txt" style="margin-top: 28rpx; margin-bottom: 30rpx; font-weight: 600">公会管理</view>
                <view class="flex_row">
                    <view class="modal-btn" @tap="checkGroupMemberClick" style="width: 240rpx; text-align: center; margin: 10rpx">查看公会成员</view>
                    <view class="modal-btn" @tap="gotoGroupFrame" style="width: 240rpx; text-align: center; margin: 10rpx">更换公会框</view>
                </view>
            </view>
        </view>
        <water_mark :bottom="110" @refreshEvent="refresh"></water_mark>
    </view>
</template>

<script>
import login_panel from '../../component/login_panel/login_panel';
import share_panel from '../../component/share_panel/share_panel';
import refresh_view from '../../component/refresh_view/refresh_view';
import idc_image from '../../component/idc_image/idc_image';
import group_spaceDynamicTip from '../../component/group_spaceDynamicTip/group_spaceDynamicTip';
import idol_toastTip_view from '../../component/idol_toastTip_view/idol_toastTip_view';
import group_rank_weekList_view from '../../component/group_rank_weekList_view/group_rank_weekList_view';
import idolConcentrateAlert from '../../component/idol_support_list/idolConcentrateAlert';
import fansGroupView from '../../component/fansGroupView/fansGroupView';
import error_view from '../../component/error_view/error_view';
import attack_selectGroup_windows from '../../component/attack_selectGroup_windows/attack_selectGroup_windows';
import group_freeze_windows from '../../component/group_freeze_windows/group_freeze_windows';
import honor_windows from '../../component/honor_windows/honor_windows';
import mainGroup_settingWindows from '../../component/mainGroup_settingWindows/mainGroup_settingWindows';
import vip_guide_card from '../../component/vip_guide_card/vip_guide_card';
import vip_buy_tip from '../../component/vip_center_dialog/vip_buy_tip';
import real_name_authentication from '../../component/real_name_authentication/real_name_authentication';
import send_star from '../../component/send_star/send_star';
import dialogFinal from '../../component/dialog_final/dialog_final';
import topImage from '../../component/top_image/top_image';
import prepareGuild from '../../component/prepareGuild/prepareGuild';
import rewardWelfareDialog from '../../component/rewardWelfareDialog/rewardWelfareDialog';
import water_mark from '../../component/water_mark/water_mark';
// pages/idol_home/idol_home.js
const app = getApp();
const net = require('../../commonscript/common/netLoad.js');
const IDLoginManager = require('../../commonscript/common/IDLoginManager.js');
const globalData = require('../../commonscript/common/globalData.js');
var Config = require('../../Config.js').miniConfig();
var wxNotificationCenter = require('../../commonscript/common/WxNotificationCenter.js');
var time = require('../../utils/util.js');
var CryptoJS = require('../../commonscript/common/CryptoJS.js');
const { store, storeObj } = require('../../utils/localStorage');
export default {
    components: {
        login_panel,
        share_panel,
        refresh_view,
        idc_image,
        group_spaceDynamicTip,
        idol_toastTip_view,
        group_rank_weekList_view,
        idolConcentrateAlert,
        fansGroupView,
        error_view,
        attack_selectGroup_windows,
        group_freeze_windows,
        honor_windows,
        mainGroup_settingWindows,
        vip_guide_card,
        vip_buy_tip,
        real_name_authentication,
        send_star,
        dialogFinal,
        topImage,
        prepareGuild,
        rewardWelfareDialog,
        water_mark
    },
    //倒计时定时器
    data() {
        return {
            isApp: Config.IS_APP ? true : false,
            isItRealName: false,

            // 是否实名认证
            official_group: 0,

            // 是否青少年模式官方公会 1或0
            bShowLoginPanel: false,

            bShowNotGetGroupGid: false,

            //是否展示重试获取gid弹窗
            bShowGroupAdminDialog: false,

            //公会管理弹窗
            retryGetgroupgid: 0,

            WeChatGroupCode: '',
            WeChatGroupiv: '',
            WeChatGroupEncrypted: '',

            groupInfo: {
                backgroup: '',

                group_union: {
                    name: '',
                    is_leader: ''
                },

                group_widget_img: '',
                group_emblem_img: '',
                title_color: false,
                name: '',
                gid: '',
                group_medal_img: '',
                intro: false,
                is_prepared: 0,

                starinfo: {
                    logo_img_v2: ''
                }
            },

            is_login: false,
            user_info: globalData.idolUserInfo,
            is_checkIn: false,
            checkIn_num: '',
            is_click_checkIn: false,
            dynamicItemLists_userinfo: [],
            dynamicItemLists_thumbpic: [],
            dynamicItemLists: [],
            windowWidth: 0,
            messageCount: 0,
            tipFrameDic: {},
            btnTipFraameDic: {},
            isFirstComePage: false,
            topRankTitles: [],
            topRankDetails: [],
            topRankTitle: '',
            topRankDetail: '',
            topRankData: [],
            currentIndex: 0,
            headerText: '动态',
            noDataText: '还没有发布过最新动态哦~',
            showBottomRefresh: false,
            bottomAnimation: true,

            errorDic: {
                needShowErrorView: false,
                needShowRetryBtn: false,
                isNothing: false,
                isNoSupportData: false,
                isSomethingError: false,
                errorText: '没有数据哦，请点击重试'
            },

            isDisplayToast: false,

            //toast alert 提示
            tipType: '',

            //提示类型
            tipToastIcon: 'https://star-img.xingxiaoculture.com/2019/08/12/b368234f27e2635c0ca4a8247077c86c.png',

            // toast图片
            tipToastTitle: '删除成功',

            //toast 文案
            myCurrentDeleteDynamicCellIndex: 0,

            //当前点击的删除cell的下标
            isDisplayOnlyIKnowTip: false,

            //我知道了 那种alert提示的控制
            bShowGroupPkBtn: false,

            groupPkActId: '',

            //公会战活动id
            official_group: '',

            isHiddenBottomBtnView: true,
            swiperAutoPlay: true,
            operationLunbotu: [],

            //品牌活动、运营广告位
            isSuperTopic: 0,

            bShowGotoGroupGather: false,
            bShowFuliBtn: false,

            // 加公会福利按钮
            userAngelModel: '',

            bShowGotoActivityDialogue: false,
            concentrateFeeds: [],

            //集结动态
            concentrateTop: 0,

            concentrateZoneHeight: 0,
            fansSupportList: [],
            bShowConcentrateAlert: false,
            bHasShareTicket: false,
            bShowWindowsStarSupportView: false,
            supportStarList: [],
            selectIndex: -1,
            bShowIdolHome: false,
            isGroupRank: false,
            starIDStr: '',

            // id
            getofficial: 1,

            WeChatGroupGid: '',

            // 公会id
            starName: '',

            isShareType: '',
            pageFrom: '',
            firstRequest: false,
            currentUrl: '',
            backFromDetail: true,
            bOpenGaterbutton: false,
            bShowAttackWindows: false,
            showAttackType: 0,
            userSkillTimes: 0,

            selectGroupInfo: {
                name: ''
            },

            selectGroupAttackInfo: '',
            bFrezzeWindows: false,
            bFrezzeWindowsType: 0,
            bHonorWindows: false,
            honorWindowsType: 0,
            honorWindowsModel: '',
            isShowSetingMainGroupWindows: false,
            mainGroupSetingWindowsType: 'setRemind',
            setGroupMsg: '',

            mainGroupInfo: {
                gid: ''
            },

            hasShowMainGroupWindows: false,
            isManagerAuthority: false,
            mainGroupUserid: '',
            treeInfo: '',
            showVipAuto: false,

            //自动集结开启确认弹窗
            isSet: false,

            //自动贡献设置开关
            defaultStar: '',

            //已设置的
            chooseStar: '',

            //当前
            buyVipDialog: false,

            //兑换vip弹窗

            isFestival: false,

            isShowRoseTips: false,
            refreshVipcard: false,
            hasHide: false,
            isIdolAdmin: false,

            //是否是管理员

            spreadImg: {
                animate_img_src: ''
            },

            //裂变推广资源位
            isFirstLoad: true,

            dialogFinalType: 0,

            // 0为关闭公会选择弹窗，1.1未打开弹窗
            getGroupMainList: {
                list: ''
            },

            // 公会列表
            thisGroupMain: {},

            // 缓存中选中的公会
            groupViewHistory: {
                is_office_group: 0
            },

            //官方群
            isGroupViewHistoryLoading: true,

            prepareGuild: {},

            //预备公会信息
            rewardId: '',

            //抽奖活动id
            errorObj: {},

            bShowTips: false,

            //浇水提示

            bShowsStarLeagueResultTips: false,

            //星球联赛弹窗
            bShowsStarLeagueTipsType: 1,

            //1结果弹窗；2赛程弹窗
            starBallLeagueInfo: {
                in_game: 0,
                in_counting: false,
                game_status: '',

                game_tomorrow_info: {
                    begin_time: false,

                    host_star_ball: {
                        logo_url: '',
                        starid: '',
                        name: ''
                    },

                    date_text: '',

                    visitor_star_ball: {
                        logo_url: '',
                        starid: '',
                        name: ''
                    }
                },

                game_yesterday_info: {
                    begin_time: '',
                    host_votetimes: 0,

                    host_star_ball: {
                        logo_url: '',
                        starid: '',
                        name: ''
                    },

                    host_user_list_day: [],
                    date_text: '',

                    visitor_star_ball: {
                        logo_url: '',
                        starid: '',
                        name: ''
                    },

                    visitor_votetimes: '',
                    visitor_user_list_day: [],
                    host_win: 0,
                    visitor_win: 0
                },

                game_today_info: {
                    visitor_votetimes: 0,
                    begin_time: '',
                    host_votetimes: 0,

                    host_star_ball: {
                        logo_url: '',
                        starid: '',
                        name: ''
                    },

                    host_user_list_day: [],
                    date_text: '',

                    visitor_star_ball: {
                        logo_url: '',
                        starid: '',
                        name: ''
                    },

                    visitor_user_list_day: []
                },

                star_ball: {
                    starid: '',
                    logo_url: '',
                    name: ''
                },

                star_ball_score: '',
                user_list_month: [],
                rules: ''
            },

            //星球模块数据
            starLeagueScheduleList: [],

            //星球联赛赛程
            nowTime: 0,

            //当前时间
            timeoutLimit: 0,

            //阶段时间
            actCountdownTimeStr: '',

            //倒计时时间字符串
            continueCountTime: 0,

            //模拟器清除定时器不起作用，不得不用标志位来做
            notOnShowRefresh: false,

            //规避previewImage返回触发onshow后的刷新

            nogroupApply: false,

            nogroupList: [
                {
                    name: '十二星光岛',
                    icon: 'https:\/\/star-img.idol001.com\/activity\/2021_10_s\/35_ty_1.png'
                },
                {
                    name: '鬼琳精怪',
                    icon: 'https:\/\/star-img.idol001.com\/2021\/10\/11\/594768962954be7a6fba2f18b3c72347.png'
                },
                {
                    name: '丞心相伴',
                    icon: 'https:\/\/star-img.idol001.com\/2021\/10\/12\/674627a0b213a547f8e2226b7fd442c1.png'
                },
                {
                    name: '富贵让我们相遇',
                    icon: 'https:\/\/star-img.idol001.com\/2021\/10\/12\/c69014d32fb7f4b9404ffafbb0338964.png'
                },
                {
                    name: '甜心奶农',
                    icon: 'https:\/\/star-img.idol001.com\/2021\/10\/12\/5e5d9a9b3530ad43483571f21f226768.png'
                },
                {
                    name: '银海之梦',
                    icon: 'https:\/\/star-img.idol001.com\/2021\/10\/11\/61ac76be064c1414f68c08553691df82.png'
                },
                {
                    name: 'THE STORY BEGINS',
                    icon: 'https:\/\/star-img.idol001.com\/activity\/2021_10_s\/35_ty_1.png'
                },
                {
                    name: '粉墨花坊',
                    icon: 'https:\/\/star-img.idol001.com\/2021\/10\/12\/cc0e00cf6698bca2d7a1d01919105fcf.png'
                },
                {
                    name: 'nana girl',
                    icon: 'https:\/\/star-img.idol001.com\/2021\/10\/12\/c69014d32fb7f4b9404ffafbb0338964.png'
                },
                {
                    name: '0608号飞船',
                    icon: 'https:\/\/star-img.idol001.com\/activity\/2021_10_s\/35_ty_1.png'
                }
            ],

            thisLunboObj: null,
            myPage: 1,

            //分页
            isloading: false,

            intervalTimeoutAct: null,
            concentrateFeedsHeights: [],

            //集结动态高度数组
            concentrateFeedsTimeout: null,

            concentrateFeedsNowIndex: 0,
            retryGetUserInfo: false,
            supportRankTotalList: [],
            fansGroupWindowsType: 0,

            user_info_rank: {
                userinfo: {
                    honor: {
                        background: '',
                        icon: ''
                    },

                    head_frame: '',

                    image: {
                        middle_pic: ''
                    },

                    is_vip: 0,
                    nickname: '',

                    medal: {
                        img: ''
                    }
                },

                vote: {
                    all_days: '',
                    continual_days: ''
                }
            },

            userArr: '',
            excCoderUrl: '',
            bShowsStarLeagueTips: false,
            admin_name: '',
            title_color: '',
            group_widget_img: '',
            score: '',
            name: '',
            member: '',
            people_num: 0,
            app_platform: '',
            time: '',
            biggest_gather_num: 0,
            group_emblem_img: '',
            group_medal_img: '',
            userId: '',
            idx1: 0,
            nil: '',

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

            isHiddenBottomBtn: '',

            userInfo: {
                register_time: ''
            }
        };
    },
    /**
     * 生命周期函数--监听页面加载
     */
    onLoad: function (options) {
        console.log(`onLoad`);
        /* 验证是否官方主群 */
        this.requestGroupViewHistory();
        // this.initPageData();
        this.shuffleGroup();
    },
    /**
     * 生命周期函数--监听页面显示
     */
    onShow: function () {
        if (!this.notOnShowRefresh) {
            /* 验证是否官方主群 */
            this.requestGroupViewHistory();
            this.initPageData();
        }
        this.setData({
            notOnShowRefresh: false
        });
    },
    onReady() {
        // this.openDialog(6)
    },
    /**
     * 页面上拉触底事件的处理函数
     */
    onReachBottom: function () {
        console.log('onReachBottom', this.isloading, this.showBottomRefresh);
        var that = this;
        if (!this.isloading && this.showBottomRefresh) {
            console.log('onReachBottom');
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
     * 页面相关事件处理函数--监听用户下拉动作
     */
    onPullDownRefresh: function () {
        this.refreshDynamicList();
        if (this.bShowIdolHome) {
            //获取集结动态
            this.requestConcentrateFeed();
            this.getFansSupportList(1);
        } else {
            if (this.WeChatGroupGid) {
                this.getGroupInfo();
                // this.getRankInfo();
                // this.getCheckInStatus();
                // this.getGroupPkInfo();
                // //请求数据
                this.requestDynamicDataList(this.myPage);
                // if (globalData.idolUserInfo !== '') {
                //   this.getUserInfo();
                // }
            }
        }
    },
    /**
     * 生命周期函数--监听页面显示
     */
    onHide: function () {
        var that = this;
        this.hasHide = true;
        /**
         * 封面
         */
        // var _groupInfo = oThis.data.groupInfo;
        // var groupInfoName = 'groupInfo'
        // _groupInfo.backgroup = ''
        /**
         * 轮播图
         */
        // var lunbotuList = oThis.data.operationLunbotu;
        // var lunbotuListName = 'operationLunbotu'
        // if (lunbotuList && lunbotuList.length > 0) {
        //     for (let index = 0; index < lunbotuList.length; index++) {
        //         var lunbotu = lunbotuList[index];
        //         lunbotu.img = ""
        //     }
        // }
        /**
         * 动态
         */
        // var dynamicItemLists_thumbpic = [];
        // var dynamicItemLists_userinfo = [];
        // var dynamicList = oThis.data.dynamicItemLists;
        // var dynamicListName = 'dynamicItemLists'
        // if (dynamicList && dynamicList.length > 0) {
        //     for (let index = 0; index < dynamicList.length; index++) {
        //         var dynamic = dynamicList[index];

        //         var userinfo = dynamic.data_quanzi.userinfo;
        //         var userinfo_ = {};
        //         if (userinfo) {
        //             var image = {};
        //             if (userinfo.image) {
        //                 var origin_pic = userinfo.image.origin_pic;
        //                 var middle_pic = userinfo.image.middle_pic;
        //                 var thumbnail_pic = userinfo.image.thumbnail_pic;
        //                 image.origin_pic = origin_pic;
        //                 image.middle_pic = middle_pic;
        //                 image.thumbnail_pic = thumbnail_pic;
        //                 userinfo.image.thumbnail_pic = "";
        //             }
        //             userinfo_.image = image;
        //             userinfo_.abnormal = userinfo.abnormal;
        //             userinfo_.bi_follow = userinfo.bi_follow;
        //             userinfo_.care_num = userinfo.care_num;
        //             userinfo_.fans_num = userinfo.fans_num;
        //             userinfo_.idol_num = userinfo.idol_num;
        //             userinfo_.last_login_time = userinfo.last_login_time;
        //             userinfo_.level = userinfo.level;
        //             userinfo_.level_img = userinfo.level_img;
        //             userinfo_.level_img = userinfo.level_img;
        //             userinfo_.level_img_v6 = userinfo.level_img_v6;
        //             userinfo_.nickname = userinfo.nickname;
        //             userinfo_.publish_num = userinfo.publish_num;
        //             userinfo_.qq_unionid = userinfo.qq_unionid;
        //             userinfo_.register_time = userinfo.register_time;
        //             userinfo_.score = userinfo.score;
        //             userinfo_.score_left = userinfo.score_left;
        //             userinfo_.user_type = userinfo.user_type;
        //             userinfo_._id = userinfo._id;
        //         }
        //         dynamicItemLists_userinfo.push(userinfo_);

        //         var images = dynamic.data_quanzi.images;
        //         var images_ = [];
        //         if (images && images.length > 0) {
        //             for (let _index = 0; _index < images.length; _index++) {
        //                 var _dynamic = images[_index];
        //                 var images_i = {};
        //                 var img_url = {};
        //                 var origin_pic = _dynamic.img_url.origin_pic;
        //                 var middle_pic = _dynamic.img_url.middle_pic;
        //                 var thumbnail_pic = _dynamic.img_url.thumbnail_pic;
        //                 img_url.origin_pic = origin_pic;
        //                 img_url.middle_pic = middle_pic;
        //                 img_url.thumbnail_pic = thumbnail_pic;
        //                 var size = _dynamic.size;
        //                 var _id = _dynamic._id
        //                 images_i.img_url = img_url;
        //                 images_i.size = size;
        //                 images_i._id = _id;
        //                 images_.push(images_i);
        //                 _dynamic.img_url.thumbnail_pic = ""
        //             }
        //         }
        //         dynamicItemLists_thumbpic.push(images_);
        //     }
        // }
        // oThis.setData({
        //     [groupInfoName]: _groupInfo,
        //     [lunbotuListName]: lunbotuList,
        //     [dynamicListName]: dynamicList,
        //     dynamicItemLists_userinfo: dynamicItemLists_userinfo,
        //     dynamicItemLists_thumbpic: dynamicItemLists_thumbpic
        // });
    },
    /**
     * 用户点击右上角分享
     */
    onShareAppMessage: function (e) {
        var that = this;
        if (!e.target) {
            e.target = {
                dataset: {}
            };
        }
        setTimeout(function () {
            if (e.from == 'button' && e.target.dataset.concentrate == '1') {
                uni.showToast({
                    title: '公会内点击链接 即可集结哦~',
                    icon: 'none'
                });
            } else if (e.from == 'button' && e.target.dataset.noadd == '1') {
                uni.showToast({
                    title: '公会内点击链接 即可查看公会排名哦~',
                    icon: 'none',
                    duration: 3000
                });
            } else if (e.from == 'button' && e.target.dataset.attack == '1') {
                that.setData({
                    bFrezzeWindows: true,
                    bFrezzeWindowsType: 2,
                    bShowWindowsStarSupportView: false
                });
            } else if (e.from == 'button' && e.target.dataset.maingroup == '1') {
                that.setData({
                    mainGroupSetingWindowsType: 'continueAgainSet'
                });
            } else if (e.from == 'button' && e.target.dataset.title) {
                uni.showToast({
                    title: e.target.dataset.title,
                    icon: 'none',
                    duration: 3000
                });
            }
            that.closeConcentrateAlert();
        }, 500);
        var path = `/pages/index/index?action=gopage&page=idol_home&sid=${this.starIDStr}`;
        var title = '快来爱豆公会一起贡献ta';
        var imageUrl = 'https://star-img.xingxiaoculture.com/2023/02/21/0efb4d0b1604dfd4a7054df2e43f61d4.png';
        if (e.from == 'button' && e.target.dataset.concentrate == '1' && this.selectIndex >= 0) {
            var starModel = this.supportStarList[this.selectIndex];
            path = '/pages/index/index?action=gopage&page=group_fall_in_detail&sid=' + starModel.starinfo.sid + '&starName=' + starModel.starinfo.name;
            title = '吹响公会集结号角，助力' + starModel.starinfo.name + '更多贡献点！';
            imageUrl = 'https://star-img.xingxiaoculture.com/2019/08/12/db4ea24a3a3894435247aedb2a44d444.png';
        } else if (e.from == 'button' && e.target.dataset.noadd == '1') {
            var starModel = this.supportStarList[this.selectIndex];
            path = '/pages/index/index?action=gopage&page=group_rank_list_detail&sid=' + starModel.starinfo.sid + '&isShowButton=1&starName=' + starModel.starinfo.name;
            title = starModel.starinfo.name + '公会内成员排名';
            imageUrl = 'https://star-img.xingxiaoculture.com/2019/08/12/006bab2661503ea9a16926f25ac1b63c.png';
        } else if (e.from == 'button' && e.target.dataset.attack == '1') {
            var starModel = this.supportStarList[this.selectIndex];
            path =
                '/pages/subpages_v2/pages/group_freezing/group_freezing?from_sid=' +
                starModel.starinfo.sid +
                '&to_sid=' +
                this.selectGroupInfo.starid +
                '&to_gid=' +
                this.selectGroupInfo.gid +
                '&is_share=1';
            title = '集结冰冻，攻击对象' + this.selectGroupInfo.name + '，家人们战斗啦！';
            imageUrl = 'https://img.xingxiaoculture.com/origin/2019/12/05/a374f2e0fa7ecd295e9a00efce9125d01575533753.png';
        } else if (e.from == 'button' && e.target.dataset.continuemaingroup == '1') {
            var path = `/pages/index/index?action=gopage&page=mainGroup_setting&sid=${this.starIDStr}` + '&starName=' + this.groupInfo.star_name;
            var title = '我正在将此公会设为' + this.groupInfo.star_name + '公会主公会，请大家多多关照~';
            var imageUrl = 'https://img.xingxiaoculture.com/origin/2020/01/20/d430a2e424ee3d76523fb6ceaab02b5c1579488498.png';
        } else if (e.from == 'button' && e.target.dataset.maingroup == '1') {
            var path = `/pages/index/index?action=gopage&page=mainGroup_setting&sid=${this.starIDStr}` + '&starName=' + this.groupInfo.star_name;
            var title = '我正在将此公会设为' + this.groupInfo.star_name + '公会主公会，请大家多多关照~';
            var imageUrl = 'https://img.xingxiaoculture.com/origin/2020/01/20/d430a2e424ee3d76523fb6ceaab02b5c1579488498.png';
        } else if (e.from == 'button' && e.target.dataset.title) {
            //点击的按钮 options.gid !== '' && options.gid !== undefined
            path = 'pages/idol_home/idol_home';
            title = e.target.dataset.title;
            imageUrl = 'https://star-img.xingxiaoculture.com/search/topics/images/2021/12/7/nt4QB26QtF1638886106536.png';
        } else if (e.from == 'button') {
            if (this.thisLunboObj && this.isApp) {
                path = this.thisLunboObj.url;
                title = this.thisLunboObj.name ? this.thisLunboObj.name : '参与活动';
                imageUrl = this.thisLunboObj.img;
            } else {
                //点击的按钮 options.gid !== '' && options.gid !== undefined
                let obj = that.dynamicItemLists[e.target.dataset.cellindex].data_quanzi;
                path =
                    '/pages/index/index?action=gopage&page=user_feed_detail&qz_id=' +
                    obj.qzid +
                    '&message_id=' +
                    obj._id +
                    '&is_share=1' +
                    '&sid=' +
                    that.starIDStr +
                    '&getofficial=1';
                title = that.is_login ? that.user_info.nickname + '分享了新动态，快去看看吧~' : obj.text;
                imageUrl =
                    obj.images && obj.images.length > 0
                        ? obj.images[0].img_url.thumbnail_pic
                        : 'https://star-img.xingxiaoculture.com/2023/02/21/0efb4d0b1604dfd4a7054df2e43f61d4.png';
            }
        }

        // #if MP
        return {
            title,
            path,
            imageUrl
        };
        // #else
        uni.miniapp.invokeMiniappNativeExtension({
            method: 'shareCustomAppMesssage',
            param: {
                title,
                path,
                imageUrl
            },
            success: (res) => {
                console.log('shareCustomAppMesssage success', res);
            }
        });
        // #endif
    },
    methods: {
        initPageData(e, _dataset) {
            /* ---处理dataset begin--- */
            this.handleDataset(e, _dataset);
            /* ---处理dataset end--- */
            uni.hideShareMenu({});
            IDLoginManager.stepLoginState();
            this.loginLogicStar();
            this.setData({
                windowWidth: uni.getSystemInfoSync().windowWidth,
                user_info: globalData.idolUserInfo
            });
            if (globalData.idolUserInfo !== '') {
                this.setData({
                    is_login: true
                });
                this.getUserSkillTimes();
            } else {
                this.setData({
                    bShowIdolHome: true
                });
                uni.setNavigationBarTitle({
                    title: '公会'
                });
                //获取集结动态
                this.requestConcentrateFeed();
                this.getFansSupportList(1);
            }

            // //获取选中公会缓存
            // this.setData({
            // 	thisGroupMain: storeObj('groupSelected')
            // })

            // 有缓存选中的公会就显示选中的公会
            if (storeObj('groupSelected_' + this.user_info._id)) {
                app.globalData.sid = storeObj('groupSelected_' + this.user_info._id).starid;
                app.globalData.gid = storeObj('groupSelected_' + this.user_info._id).gid;
            }
            if (app.globalData.isShareType) {
                this.setData({
                    starIDStr: app.globalData.sid,
                    isShareType: 1,
                    mainGroupUserid: app.globalData.userid
                });
                if (app.globalData.pageFrom == 'group_fall_in') {
                    this.setData({
                        bShowGotoGroupGather: true,
                        starName: app.globalData.starname,
                        pageFrom: 'group_fall_in'
                    });
                }

                // 请求公会分享gid
                const oThis = this;
                if (app.globalData.gid !== '' && app.globalData.gid !== undefined) {
                    console.log('idol_home == 有gid');
                    console.log('idol_home == 已登录');
                    this.setData({
                        WeChatGroupGid: app.globalData.gid
                    });
                    this.getGroupInfo();
                } else {
                    if (app.globalData.shareTicket) {
                        if (app.globalData.shareTicket && !oThis.bHasShareTicket) {
                            oThis.setData({
                                bHasShareTicket: true
                            });
                        }
                        uni.getShareInfo({
                            shareTicket: app.globalData.shareTicket,
                            success: function (res) {
                                console.log(res.encryptedData);
                                oThis.setData({
                                    WeChatGroupEncrypted: res.encryptedData,
                                    WeChatGroupiv: res.iv
                                });
                                uni.login({
                                    success: function (obj) {
                                        console.log(obj.code);
                                        oThis.setData({
                                            WeChatGroupCode: obj.code
                                        });
                                        oThis.getwechatGid();
                                    }
                                });
                            }
                        });
                    } else {
                        this.normarlType();
                    }
                }
            } else {
                console.log('app.globalData.sid ==' + app.globalData.sid);
                if (app.globalData.sid !== '' && app.globalData.sid !== undefined) {
                    this.setData({
                        starIDStr: app.globalData.sid
                    });
                    //请求公会分享gid
                    const oThis = this;
                    if (app.globalData.gid !== '' && app.globalData.gid !== undefined) {
                        this.setData({
                            WeChatGroupGid: app.globalData.gid
                        });
                        this.getGroupInfo();
                    }
                } else {
                    this.getOfficialGroup();
                }
            }
            this.resetOfficialGroup();
            // 获取公会群
            this.getGroups();
            if (this.zpSelectComponent('#sendStar')) {
                this.zpSelectComponent('#sendStar').initPageData(); // 调用送星子组件初始化方法!!!
            }

            this.requestDynamicDataList(this.myPage);
            this.getStarLeagueInfo();
        },

        closeViewEvent: function () {
            this.setData({
                isItRealName: false
            });
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
            const url = `https://data.idol001.com/api_moblie_idol.php?action=get_has_real_name_authentication`;
            const params = {
                set_young_limit: 1
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('实名认证======', resp.data);
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

        normarlType: function () {
            this.setData({
                bShowIdolHome: true
            });
            uni.setNavigationBarTitle({
                title: '公会'
            });
            //获取集结动态
            this.requestConcentrateFeed();
            this.getFansSupportList(1);
        },

        resetOfficialGroup: function () {
            app.globalData.sid = '';
            app.globalData.pageFrom = '';
            app.globalData.gid = '';
            app.globalData.starname = '';
            app.globalData.getofficial = '';
            app.globalData.isShareType = '';
            app.globalData.userid = '';
        },

        /**
         * 获取是否官方公会
         */
        getOfficialGroup() {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_view_history';
            const oThis = this;
            net.fetchApi(url, {}, 'GET').then((resp) => {
                if (resp && resp.data) {
                    console.log('getOfficialGroup resp' + JSON.stringify(resp.data));
                }
                if (resp && resp.data && resp.data.official_group == 1 && oThis.is_login) {
                    this.setData({
                        firstRequest: true
                    });
                    oThis.setData({
                        bShowIdolHome: false,
                        starIDStr: app.globalData.sid != '' ? app.globalData.sid : resp.data.starid,
                        official_group: 1
                    });
                    oThis.showTipDisplay();
                    this.setData({
                        WeChatGroupGid: resp.data.gid
                    });
                    this.getGroupInfo();
                } else {
                    this.setData({
                        firstRequest: true
                    });
                    oThis.setData({
                        bShowIdolHome: true
                    });
                    uni.setNavigationBarTitle({
                        title: '公会'
                    });

                    //获取集结动态
                    oThis.requestConcentrateFeed();
                    oThis.getFansSupportList(1);
                }
            });
        },

        getGroups() {
            const url = 'https://data.idol001.com/api_activity_2021_08s.php?action=get_group_main_list';
            const oThis = this;
            net.fetchApi(url, {}, 'GET').then((resp) => {
                if (resp && resp.data) {
                    let rows = resp.data.list;
                    rows.map((row) => {
                        // 循环主公会列表,选中当前展示的群
                        if (!!this.WeChatGroupGid && this.WeChatGroupGid == row.gid) {
                            this.setData({
                                thisGroupMain: row
                            });
                            storeObj('groupSelected_' + this.user_info._id, row);
                            this.getPrepareGuildInfo();
                        }
                    });
                    this.setData({
                        getGroupMainList: resp.data
                    });
                }
            });
        },

        // 获取公会是否为预备公会
        getPrepareGuildInfo() {
            // https://data.idol001.com/api_mobile_star_rank.php?action=get_prepared_group_task_status&gid=GUvVY5FYjdoaxRMIUeKqx0kotsqU&starid=880&date=2021-12-04
            const url = `https://data.idol001.com/api_mobile_star_rank.php?action=get_prepared_group_task_status&gid=${this.thisGroupMain.gid}&starid=${this.thisGroupMain.starid}`;
            net.fetchApi(url, {}, 'GET').then((res) => {
                if (res && res.data) {
                    console.log('预备公会数据', res);
                    this.setData({
                        prepareGuild: res.data
                    });
                }
            });
        },

        closeConcentrateAlert: function () {
            this.setData({
                bShowConcentrateAlert: false
            });
        },

        //获取集结动态
        requestConcentrateFeed: function () {
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_group_gather_action';
            var that = this;
            net.fetchApi(url, {}, 'GET').then(
                (resp) => {
                    if (resp && resp.data && Array.isArray(resp.data)) {
                        uni.stopPullDownRefresh(); //停止下拉刷新
                        clearTimeout(that.concentrateFeedsTimeout);
                        that.concentrateFeedsNowIndex = 0;
                        that.setData(
                            {
                                concentrateFeeds: resp.data.length > 4 ? resp.data.concat(resp.data.slice(0, 4)) : resp.data,
                                concentrateTop: 0,
                                concentrateZoneHeight: resp.data.length == 0 ? 0 : that.concentrateZoneHeight
                            },
                            function () {
                                var heights = [];
                                var h = 0;
                                uni.createSelectorQuery()
                                    .in(uni)
                                    .selectAll('.group_concentrate_swiper_cell')
                                    .boundingClientRect(function (rects) {
                                        rects.forEach(function (rect) {
                                            heights.push(h);
                                            h += rect.height;
                                        });
                                        that.concentrateFeedsHeights = heights;
                                        if (resp.data.length > 4) {
                                            that.setData(
                                                {
                                                    concentrateZoneHeight: 95
                                                },
                                                function () {
                                                    that.concentrateSwip();
                                                }
                                            );
                                        } else {
                                            that.setData({
                                                concentrateZoneHeight: h
                                            });
                                        }
                                    })
                                    .exec();
                            }
                        );
                    }
                },
                function () {
                    uni.stopPullDownRefresh(); //停止下拉刷新
                }
            );
        },

        //动态的轮播
        concentrateSwip: function () {
            var that = this;
            clearTimeout(that.concentrateFeedsTimeout);
            that.concentrateFeedsTimeout = setTimeout(function () {
                that.concentrateFeedsNowIndex++;
                if (that.concentrateFeedsNowIndex >= that.concentrateFeedsHeights.length) {
                    that.concentrateFeedsNowIndex = 0;
                }
                if (that.concentrateFeedsNowIndex >= that.concentrateFeedsHeights.length - 3) {
                    that.concentrateFeedsNowIndex = 0;
                    that.setData(
                        {
                            concentrateTop: -that.concentrateFeedsHeights[that.concentrateFeedsNowIndex]
                        },
                        function () {
                            that.concentrateFeedsNowIndex++;
                            if (that.concentrateFeedsNowIndex >= that.concentrateFeedsHeights.length) {
                                that.concentrateFeedsNowIndex = 0;
                            }
                            that.setData({
                                concentrateTop: -that.concentrateFeedsHeights[that.concentrateFeedsNowIndex]
                            });
                        }
                    );
                } else {
                    that.setData({
                        concentrateTop: -that.concentrateFeedsHeights[that.concentrateFeedsNowIndex]
                    });
                }
                that.concentrateSwip();
            }, 5000);
        },

        /**
         * 通知刷新的方法
         */
        refreshDynamicList: function () {
            //请求数据
            this.requestDynamicDataList(1);
            this.initPageData(); // 刷新列表数据
        },

        loginLogicStar: function () {
            var that = this;
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                IDLoginManager.checkIsLogin(function (v) {
                    if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                    } else {
                    }
                });
            }
        },

        /**
         * 请求列表数据
         */
        requestDynamicDataList: function (page) {
            var url = 'https://data.idol001.com/api_moblie_idol_userdt.php?action=get_star_group_dongtai';
            var that = this;
            that.isloading = true;
            var params = {
                group_id: that.WeChatGroupGid && that.WeChatGroupGid.length > 0 ? that.WeChatGroupGid : '',
                starid: that.starIDStr,
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
                        console.log('requestDynamicDataList = ', list);
                        var bShowBottomRefresh = false;
                        if (resp.data.list.length > 0 && resp.data.list.length >= 10) {
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
                                    showBottomRefresh: false,
                                    errorDic: {
                                        needShowErrorView: true,
                                        needShowRetryBtn: true,
                                        isSomethingError: true,
                                        errorText: resp.data && resp.data.error_description ? resp.data.error_description : '网络似乎不太顺畅哦'
                                    }
                                },
                                function () {
                                    that.endRefresh();
                                }
                            );
                        } else {
                            uni.showToast({
                                title: resp.data && resp.data.error_description ? resp.data.error_description : '网络似乎不太顺畅哦',
                                icon: 'none',
                                duration: 2000
                            });
                            that.setData(
                                {
                                    showBottomRefresh: false
                                },
                                function () {
                                    that.endRefresh();
                                }
                            );
                        }
                    }
                },
                function () {
                    if (page == 1) {
                        uni.stopPullDownRefresh(); //停止下拉刷新
                        that.setData(
                            {
                                dynamicItemLists: [],
                                showBottomRefresh: false,
                                errorDic: {
                                    needShowErrorView: true,
                                    needShowRetryBtn: true,
                                    isSomethingError: true,
                                    errorText: '网络似乎不太顺畅哦'
                                }
                            },
                            function () {
                                that.endRefresh();
                            }
                        );
                    } else {
                        uni.showToast({
                            title: '网络似乎不太顺畅哦',
                            icon: 'none',
                            duration: 2000
                        });
                        that.setData(
                            {
                                showBottomRefresh: false
                            },
                            function () {
                                that.endRefresh();
                            }
                        );
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
         * 送星星按钮点击
         */
        gotoSupportPage: function () {
            //获取obj
            var that = this;
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/support_list/support_list_new?sid=' + that.groupInfo.starinfo.sid
            });
        },

        /**
         * 发布按钮点击
         */
        gotoPublishPage: function () {
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            this.setData({
                backFromDetail: true
            });
            uni.navigateTo({
                url: '../subpages/pages/group_spaceDynamic/publish_dynamicMessage?gid=' + (that.WeChatGroupGid && that.WeChatGroupGid.length > 0 ? that.WeChatGroupGid : '')
            });
        },

        /**
         * 公会树点击
         */
        gotoSupportTreePage: function () {
            if (this.isShowRoseTips) {
                this.setData({
                    isShowRoseTips: false
                });
                if (globalData.roseTreeTips == 0) {
                    globalData.roseTreeTips = 1;
                }
            }
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            console.log('gid===== ', that.WeChatGroupGid);
            uni.navigateTo({
                url: '../subpages_v2/pages/support_tree/support_tree?gid=' + (that.WeChatGroupGid && that.WeChatGroupGid.length > 0 ? that.WeChatGroupGid : '')
            });
        },

        /**
         * 消息按钮点击
         */
        gotoMessagePage: function () {
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            uni.navigateTo({
                url: '../subpages/pages/group_space/news_notification'
            });
        },

        /**
         * 左上角去主页
         */
        gotoHomePage: function () {
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var userInfo = this.user_info;
            var newTempDic = {};
            newTempDic['image'] = userInfo['image'];
            newTempDic['nickname'] = userInfo['nickname'];
            newTempDic['care_num'] = userInfo['care_num'];
            newTempDic['_id'] = userInfo['_id'];
            newTempDic['fans_num'] = userInfo['fans_num'];
            console.log('left  = ', JSON.stringify(newTempDic));
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(newTempDic))
            });
        },

        /**
         * 点他人头像去主页
         */
        topRankItemCellClick: function (e) {
            var that = this;
            var cellObj = this.topRankData[e.currentTarget.dataset.itemindex][e.currentTarget.dataset.idx];
            if (cellObj) {
                var userInfo = cellObj;
                console.log('other = ', JSON.stringify(userInfo));
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(userInfo))
                });
            }
        },

        showTipDisplay: function () {
            var that = this;

            //首次弹窗
            var showTip = globalData.getDataForKey('show_first_guide_tip');
            if (!showTip) {
                globalData.saveDataForKey('show_first_guide_tip', '1');
                that.setData({
                    tipFrameDic: {
                        right: 13,
                        bottom: 85,
                        width: 376,
                        height: 120
                    },
                    btnTipFraameDic: {
                        right: 0,
                        bottom: 14,
                        width: 128,
                        height: 128
                    },
                    isFirstComePage: true
                });
            }
        },

        /**
         * 关闭tip
         */
        closeTipDisplay: function () {
            console.log('++closeTipDisplay++');
            this.setData({
                isFirstComePage: false
            });
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
         * alert 左边按钮点击 取消
         */
        myAlertLeftBtnClick: function () {
            var that = this;
            that.setData({
                isDisplayToast: false
            });
        },

        /**
         * alert 右边按钮点击 确定
         */
        myAlertRightBtnClick: function () {
            var that = this;
            that.setData(
                {
                    isDisplayToast: false
                },
                function () {
                    var cellObj = that.dynamicItemLists[that.myCurrentDeleteDynamicCellIndex];
                    if (cellObj) {
                        that.requestDeleteDynamic(cellObj.data_quanzi._id, that.myCurrentDeleteDynamicCellIndex);
                    }
                }
            );
        },

        /**
         * 删除动态
         */
        deleteDynamicCellClick: function (e) {
            //弹窗提示
            var that = this;
            that.setData({
                myCurrentDeleteDynamicCellIndex: e.currentTarget.dataset.cellindex,
                tipType: 'alert',
                isDisplayToast: true
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
                    } else if (resp.data && resp.data && resp.data.error_code == 10120) {
                        that.setData({
                            isDisplayOnlyIKnowTip: true
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
         * 点击图片的方法
         */
        showTripImages: function (e) {
            var urls = [];
            var nowUrl = '';
            var i = 0;
            var cellObj = this.dynamicItemLists[e.currentTarget.dataset.cellindex];
            if (cellObj.data_quanzi.images.length > 0) {
                this.setData({
                    notOnShowRefresh: true
                });
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

        gotoHomePageAction: function (e) {
            console.log('++gotoHomePageAction++');
            var that = this;
            var cellObj = that.dynamicItemLists[e.currentTarget.dataset.cellindex].data_quanzi;
            var userInfo = cellObj.userinfo;
            if (userInfo) {
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/dress_room/dress_room?user=' + encodeURIComponent(JSON.stringify(userInfo)) + '&showBackHomeButton=' + '1'
                });
            }
        },

        /**
         * 动态cell点击
         */
        dynamicCellItemClick: function (e) {
            var that = this;
            // oThis.setData({
            //     backFromDetail: true
            // })
            var cellObj = that.dynamicItemLists[e.currentTarget.dataset.cellindex].data_quanzi;
            if (cellObj) {
                uni.navigateTo({
                    url: '../subpages/pages/user_feed_detail/user_feed_detail?qz_id=' + cellObj.qzid + '&message_id=' + cellObj._id + '&sid=' + that.starIDStr
                });
            }
        },

        /**
         * swiper滑动获取当前索引
         */
        onSlideChangeEnd: function (e) {
            var that = this;
            var myTopTitles = that.topRankTitles;
            var myTopDetails = that.topRankDetails;
            that.setData({
                currentIndex: e.detail.current,
                topRankTitle: myTopTitles.length > e.detail.current ? myTopTitles[e.detail.current] : '',
                topRankDetail: myTopDetails.length > e.detail.current ? myTopDetails[e.detail.current] : ''
            });
        },

        /**
         * 获取未读消息数
         */
        requestNoReadMessageNum: function () {
            var url = 'https://data.idol001.com/api_moblie_idol_notify.php?action=get_user_all_notification_news_count_v3';
            var that = this;
            var params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data && resp.data && resp.data.newscount_1) {
                        var newCount =
                            parseInt(resp.data.newscount_1) +
                            parseInt(resp.data.newscount_2) +
                            parseInt(resp.data.newscount_3) +
                            parseInt(resp.data.newscount_4) +
                            parseInt(resp.data.newscount_11) +
                            parseInt(resp.data.newscount_12);
                        if (newCount > 0) {
                            that.setData({
                                messageCount: newCount > 99 ? 99 : newCount
                            });
                        } else {
                            that.setData({
                                messageCount: 0
                            });
                        }
                    }
                },
                function () {}
            );
        },

        // 获取公会贡献周排行
        getFansSupportList: function (page) {
            var that = this;
            var url = '';
            url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_all_group_rank_top10';
            that.isloading = true;
            net.fetchApi(url, {}, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.list) {
                        that.page = page;
                        var list = that.fansSupportList;
                        var originList = resp.data.list;
                        for (var iii in originList) {
                            var item = originList[iii];
                            item.time = time.formatTimeTwo(item.add_time, 'Y.M.D h:m');
                        }
                        if (page == 1) {
                            list = originList;
                        } else {
                            list = list.concat(originList);
                        }
                        var bShowBottomRefresh = false;
                        if (originList.length > 0 && originList.length == 20) {
                            bShowBottomRefresh = true;
                        }
                        if (page == 1) {
                            if (list.length > 0) {
                                that.setData(
                                    {
                                        fansSupportList: list,
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
                                        fansSupportList: [],
                                        showBottomRefresh: false,
                                        errorDic: {
                                            needShowErrorView: true,
                                            isNoSupportData: true,
                                            needShowRetryBtn: false,
                                            errorText: '当前暂无公会数据'
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
                                    fansSupportList: list,
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
                                    fansSupportList: [],
                                    showBottomRefresh: false,
                                    errorDic: {
                                        needShowErrorView: true,
                                        isNoSupportData: true,
                                        needShowRetryBtn: false,
                                        errorText: '当前暂无公会数据'
                                    }
                                },
                                function () {
                                    that.endRefresh();
                                }
                            );
                        } else {
                            uni.showToast({
                                title: resp.data && resp.data.error_description ? resp.data.error_description : '网络似乎不太顺畅哦',
                                icon: 'none',
                                duration: 2000
                            });
                            that.endRefresh();
                        }
                    }
                },
                function () {
                    if (page == 1) {
                        that.setData(
                            {
                                fansSupportList: [],
                                showBottomRefresh: false,
                                errorDic: {
                                    needShowErrorView: true,
                                    isNoSupportData: true,
                                    needShowRetryBtn: false,
                                    errorText: '当前暂无公会数据'
                                }
                            },
                            function () {
                                that.endRefresh();
                            }
                        );
                    } else {
                        uni.showToast({
                            title: '网络似乎不太顺畅哦',
                            icon: 'none',
                            duration: 2000
                        });
                        that.endRefresh();
                    }
                }
            );
        },

        loginCallback: function (e) {
            var that = this;
            if (e.detail.success) {
                that.setData({
                    bShowLoginPanel: false,
                    is_login: true,
                    user_info: globalData.idolUserInfo
                });
                if (that.retryGetUserInfo) {
                    that.retryGetUserInfo = false;
                    uni.showToast({
                        title: '更新成功',
                        icon: 'none'
                    });
                }
                that.getUserSkillTimes();
                if (that.WeChatGroupGid !== '' && that.WeChatGroupGid !== undefined) {
                    this.getGroupInfo();
                } else {
                    if (this.isShareType) {
                        this.normarlType();
                    } else {
                        this.getOfficialGroup();
                    }
                }
            } else {
                this.normarlType();
            }
        },

        closeLoginView: function () {
            this.retryGetUserInfo = false;
            this.setData({
                bShowLoginPanel: false
            });
        },

        showLoginView: function () {
            this.setData({
                bShowLoginPanel: true
            });
        },

        // 获取微信gid
        getwechatGid: function () {
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=return_opengid';
            console.log('getwechatGid  this.data.WeChatGroupCode==', this.WeChatGroupCode);
            console.log('getwechatGid  this.data.WeChatGroupEncrypted==', this.WeChatGroupEncrypted);
            console.log('getwechatGid this.data.WeChatGroupiv==', this.WeChatGroupiv);
            var params = {
                code: this.WeChatGroupCode,
                encrypted_data: this.WeChatGroupEncrypted,
                iv: this.WeChatGroupiv
            };
            var that = this;
            net.fetchApi(url, params, 'POST').then(
                (resp) => {
                    console.log('getwechatGid  resp.data==', resp.data);
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
                                            that.getwechatGid();
                                        }
                                    });
                                }
                            });
                        }
                        if (that.retryGetgroupgid == 1) {
                            that.setData({
                                bShowNotGetGroupGid: true
                            });
                        }
                        that.setData({
                            retryGetgroupgid: 1
                        });
                        return;
                    }
                    that.setData({
                        bShowNotGetGroupGid: false
                    });
                    app.globalData.groupGid = resp.data.openGId;
                    that.setData({
                        WeChatGroupGid: resp.data.openGId
                    });

                    // oThis.getJoingroup(resp.data.openGId);
                    console.log('idol_home == 重新请求的gid');
                    that.getGroupInfo();
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

        refresh: function () {
            var that = this;
            app.globalData.currentTabPage = 2;
            // 用户信息
            if (globalData.idolUserInfo !== '') {
                that.setData({
                    is_login: true
                });
            }
            this.getFansSupportList(1);
            if (that.firstRequest) {
                if (that.is_login) {
                    if (app.globalData.isShareType) {
                        this.normarlType();
                    } else {
                        this.getOfficialGroup();
                    }
                } else {
                    this.normarlType();
                }
            }

            //主要是解决分包里发布动态后，主包公会通知无法响应的问题
            if (app.globalData.lastFromPage) {
                this.refreshDynamicList();
                app.globalData.lastFromPage = false;
                uni.pageScrollTo({
                    scrollTop: 0
                });
            }

            //更新登陆态
            if (app.globalData.isShouldCheckInLoginStatus) {
                IDLoginManager.stepLoginState();
                this.loginLogicStar();
                this.getUserInfo();
                this.setData({
                    user_info: globalData.idolUserInfo,
                    is_login: true
                });
                app.globalData.isShouldCheckInLoginStatus = false;
            }
            if (globalData.idolUserInfo !== '' && this.WeChatGroupGid !== '') {
                this.getFuliBtnStatus();
                this.requestSomeStarInfo();
            }
            if (that.bShowGotoActivityDialogue) {
                this.setData({
                    bShowGotoActivityDialogue: false
                });
            }
        },

        copyArr: function (arr) {
            let res = [];
            for (let i = 0; i < arr.length; i++) {
                res.push(arr[i]);
            }
            return res;
        },

        /**
         * 获取消息数
         */
        getUnreadMessageNum: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_moblie_idol_notify.php?action=get_user_all_notification_news_count_v3';
            var params = {};
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('=====getUnreadMessageNum resp==' + resp);
                if (resp && resp.data) {
                    let newscount =
                        parseInt(resp.data.newscount_1) +
                        parseInt(resp.data.newscount_2) +
                        parseInt(resp.data.newscount_3) +
                        parseInt(resp.data.newscount_4) +
                        parseInt(resp.data.newscount_11) +
                        parseInt(resp.data.newscount_12);
                    console.log('=====getUnreadMessageNum newscount==' + newscount);
                    if (newscount > 99) {
                        newscount = 99;
                    }
                    if (newscount > 0) {
                        uni.setTabBarBadge({
                            index: 4,
                            text: newscount + ''
                        });
                    } else {
                        uni.removeTabBarBadge({
                            index: 3
                        });
                    }
                }
            });
        },

        /**
         * 列表
         */
        requestUserSupportStar: function () {
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=user_vote_star_list';
            var that = this;
            net.fetchApi(url, {}, 'GET').then(
                (resp) => {
                    if (resp.data && resp.data.list) {
                        that.setData({
                            supportStarList: resp.data.list
                        });
                    }
                    this.setData({
                        bShowWindowsStarSupportView: true
                    });
                },
                function () {}
            );
        },

        /**
         * 公会集结按钮状态
         */
        requestUserOpenGaterbutton: function () {
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=open_gater_button';
            var that = this;
            net.fetchApi(url, {}, 'GET').then(
                (resp) => {
                    console.log('open_gater_button-公会集结按钮状态 resp==' + JSON.stringify(resp));
                    if (resp.data && resp.data.ok == '1') {
                        that.setData({
                            bOpenGaterbutton: true
                        });
                    } else {
                        that.setData({
                            bOpenGaterbutton: false
                        });
                    }
                },
                function () {}
            );
        },

        /**
         * 分享按钮点击
         */
        shareBtnClick: function () {},

        supportPageClick: function () {
            console.log('supportPageClick');
            this.setData({
                bShowWindowsStarSupportView: false
            });
            uni.switchTab({
                url: '/pages/rank_list/general_rank_list'
            });
        },

        beginConcentrate: function () {
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            this.setData({
                fansGroupWindowsType: 0
            });
            this.requestUserSupportStar();
        },

        checkRankClick: function () {
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            this.setData({
                fansGroupWindowsType: 1
            });
            this.requestUserSupportStar();
        },

        sureClick: function () {
            if (this.bHasShareTicket && this.fansGroupWindowsType == 0) {
                var starModel = this.supportStarList[this.selectIndex];
                this.setData({
                    bShowWindowsStarSupportView: false,
                    selectIndex: -1
                });
                uni.navigateTo({
                    url: '/pages/group_fall_in_detail/group_fall_in_detail?sid=' + starModel.starinfo.sid
                });
            } else if (this.WeChatGroupGid && this.bHasShareTicket && this.fansGroupWindowsType == 1) {
                var starModel = this.supportStarList[this.selectIndex];
                this.setData({
                    bShowWindowsStarSupportView: false,
                    selectIndex: -1
                });
                uni.navigateTo({
                    url: '/pages/subpages/pages/group_rank_list_detail/group_rank_list_detail?sid=' + starModel.starinfo.sid
                });
            } else if (this.fansGroupWindowsType == 3) {
            } else if (this.selectIndex >= 0) {
            } else {
                uni.showToast({
                    title: '请先选择',
                    icon: 'none'
                });
            }
        },

        selectClick: function (e) {
            this.setData({
                selectIndex: e.detail.selectIndex
            });
        },

        closeClick: function () {
            console.log('closeClick');
            this.setData({
                bShowWindowsStarSupportView: false,
                selectIndex: -1
            });
        },

        backRewardClick: function () {
            this.setData({
                bFrezzeWindows: false
            });
        },

        getUserIcon: function () {
            this.retryGetUserInfo = true;
            this.showLoginView();
        },

        //加入公会排行请求
        getJoingroup: function (re) {
            if (re && re != '' && re != 'undefined') {
                var url = 'https://update.idol001.com/api_mobile_star_rank.php?action=join_group';
                var params = {
                    gid: re,
                    starid: this.starIDStr
                };
                var oThis = this;
                net.fetchApi(url, params, 'GET').then(
                    (resp) => {},
                    function () {
                        uni.showToast({
                            title: '网络似乎不太顺畅哦',
                            icon: 'none',
                            duration: 2000
                        });
                        oThis.endRefresh();
                    }
                );
            }
        },

        getGroupInfo() {
            console.log('getGroupInfo WeChatGroupGid=' + this.WeChatGroupGid);
            console.log('getGroupInfo starIDStr=' + this.starIDStr);
            console.log('getGroupInfo getofficial=' + this.getofficial);
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_info';
            const params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr,
                getofficial: this.getofficial
            };
            console.log('获取请求参数');
            console.log(params);
            const oThis = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.starinfo) {
                        this.setData({
                            firstRequest: true
                        });
                        const data = resp.data;
                        // 是否官方公会
                        let is_official_group = parseFloat(data.official_group) || 0;
                        // 是否开启官方公会验证
                        oThis.setData({
                            groupInfo: data,
                            starName: data.star_name
                        });
                        console.log('中奖');
                        oThis.getRewardInfo();
                        // 是否超话
                        if (data.starinfo.ch_url != undefined) {
                            this.setData({
                                isSuperTopic: 1
                            });
                        }
                        //  是官方公会
                        if (is_official_group == 1) {
                            oThis.showTipDisplay();
                            oThis.setData({
                                bShowIdolHome: false,
                                starIDStr: app.globalData.sid != '' ? app.globalData.sid : resp.data.starid,
                                official_group: 1
                            });
                            this.setData({
                                WeChatGroupGid: resp.data.gid
                            });
                            this.getRankInfo();
                            this.getCheckInStatus();
                            if (oThis.backFromDetail) {
                                //请求数据
                                oThis.requestDynamicDataList(1);
                            }
                            this.getGroupPkInfo();
                            this.requestIsAdminStatus();
                            // this.getJoingroup(resp.data.gid)
                            if (globalData.idolUserInfo !== '') {
                                this.getFuliBtnStatus();
                                this.getUserInfo();
                                this.requestSomeStarInfo();
                            }
                            oThis.getLunboList();
                            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                                return;
                            }
                            oThis.setOfficialGroup(this.WeChatGroupGid);
                            oThis.setData({
                                backFromDetail: false,
                                bShowTips: uni.getStorageSync('show_water_tips')
                            });
                            //获取是否加入过主公会
                            oThis.getUserMainGroupInfo();
                            oThis.getUserManagerAuthority();
                            oThis.getTreeInfo();
                            oThis.getStarLeagueInfo();
                        } else {
                            this.setData({
                                firstRequest: true
                            });
                            oThis.setData({
                                bShowIdolHome: true
                            });
                            uni.setNavigationBarTitle({
                                title: '公会'
                            });
                            //获取集结动态
                            oThis.requestConcentrateFeed();
                            oThis.getFansSupportList(1);
                        }
                        oThis.getSpreadImg(); //获取裂变推广资源位
                        // 公会介绍弹窗
                        // wx.getStorage({
                        //     key: 'guild_desc',
                        //     success(res) {
                        //         console.log(res.data)
                        //     },
                        //     fail(err) {
                        //         oThis.openDialog(7, oThis.data.groupInfo)
                        //     }
                        // })
                        if (this.groupInfo.is_prepared == 0) {
                            uni.getStorage({
                                key: 'guild_up',
                                success(res) {
                                    console.log(res.data);
                                },
                                fail(err) {
                                    // if(oThis.data.prepareGuild.task_complete_num >= oThis.data.prepareGuild.task_target_num){
                                    //   oThis.openDialog(6)
                                    // }
                                    //oThis.openDialog(6)
                                }
                            });
                        }
                    } else if (resp && resp.data && resp.data.error_code == 10120) {
                        this.setData({
                            firstRequest: true
                        });
                        oThis.setData({
                            bShowIdolHome: true
                        });
                        uni.setNavigationBarTitle({
                            title: '公会'
                        });
                        //获取集结动态
                        oThis.requestConcentrateFeed();
                        oThis.getFansSupportList(1);
                    }
                    if (oThis.isFirstLoad) {
                        oThis.setData({
                            isFirstLoad: false
                        });
                        // wx.reportAnalytics('fans_tab_views', {
                        //     user_id: app.globalData.idolUserInfo._id ? app.globalData.idolUserInfo._id : '',
                        //     star_id: oThis.data.starIDStr,
                        // });
                    }
                },
                function () {
                    if (oThis.isFirstLoad) {
                        oThis.setData({
                            isFirstLoad: false
                        });
                        // wx.reportAnalytics('fans_tab_views', {
                        //     user_id: app.globalData.idolUserInfo._id ? app.globalData.idolUserInfo._id : '',
                        //     star_id: oThis.data.starIDStr,
                        // });
                    }
                }
            );
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

        getRankInfo() {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_rank_list_main';
            const params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr
            };
            const oThis = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log(resp);
                    if (resp && resp.data) {
                        const data = resp.data;
                        const group_rank_list = data.user_continue_day_rank_list || '';
                        const user_rank_list_week = data.user_score_rank_list_week || '';
                        const user_rank_list_day = data.user_score_rank_list_day || '';
                        var myDatas = [];
                        var myTopTitles = [];
                        var myTopDetails = [];
                        if (user_rank_list_day.list.length > 0) {
                            myDatas.push(user_rank_list_day.list);
                            myTopTitles.push('日排行大咖');
                            myTopDetails.push('今日公会内贡献TOP3');
                        }
                        if (user_rank_list_week.list.length > 0) {
                            myDatas.push(user_rank_list_week.list);
                            myTopTitles.push('铁杆代表');
                            myTopDetails.push('本周公会内贡献TOP3');
                        }
                        if (group_rank_list.list.length > 0) {
                            myDatas.push(group_rank_list.list);
                            myTopTitles.push('真爱贡献');
                            myTopDetails.push('公会内连续贡献总排行TOP3');
                        }
                        oThis.setData({
                            topRankData: myDatas,
                            topRankTitles: myTopTitles,
                            topRankDetails: myTopDetails,
                            topRankTitle: myTopTitles.length > 0 ? myTopTitles[0] : '',
                            topRankDetail: myTopDetails.length > 0 ? myTopDetails[0] : ''
                        });
                    }
                },
                function () {}
            );
        },

        getUserInfo() {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_user_rank';
            const params = {
                gid: this.WeChatGroupGid,
                starid: this.starIDStr
            };
            const oThis = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    uni.stopPullDownRefresh();
                    if (resp.data && resp.data.error && resp.data.error_code == 10115) {
                        IDLoginManager.setLoginState(IDLoginManager.is_not_login);
                        oThis.setData({
                            is_login: false
                        });
                        return;
                    }
                    console.log(resp);
                    if (resp && resp.data) {
                        oThis.setData({
                            user_info_rank: resp.data
                        });
                    }
                },
                function () {
                    uni.stopPullDownRefresh();
                }
            );
        },

        getCheckInStatus() {
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_idolwx_group_signin';
            const params = {
                starid: this.starIDStr,
                gid: this.WeChatGroupGid
            };
            const oThis = this;
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log(resp);
                if (resp && resp.data && resp.data.error_code != 10110) {
                    const data = resp.data;
                    let is_checkIn;
                    if (typeof data.signin_time === 'number') {
                        is_checkIn = true;
                    } else {
                        is_checkIn = false;
                    }
                    oThis.setData({
                        is_checkIn: is_checkIn,
                        checkIn_num: data.signin_people_num
                    });
                }
            });
        },

        checkIn() {
            // this.clickShareApp();
            // return;
            if (!globalData.idolUserInfo) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            this.setData({
                is_click_checkIn: true
            });
            const url = 'https://data.idol001.com/api_mobile_star_rank.php?action=set_idolwx_group_signin';
            const params = {
                starid: this.starIDStr,
                gid: this.WeChatGroupGid
            };
            const oThis = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    oThis.setData({
                        is_click_checkIn: false
                    });
                    if (resp.data && resp.data.error && resp.data.error_code == 10115) {
                        IDLoginManager.setLoginState(IDLoginManager.is_not_login);
                        oThis.setData({
                            bShowLoginPanel: true
                        });
                        return;
                    }
                    console.log('用户签到');
                    console.log(resp);
                    if (resp && resp.data && resp.data.ok == 1) {
                        oThis.getRankInfo();
                        oThis.getCheckInStatus();
                        oThis.nav2GroupCheckIn(1, resp.data.gift_text);
                    } else {
                        uni.showToast({
                            title: '签到失败',
                            icon: 'none',
                            duration: 3000
                        });
                    }
                },
                function (error) {
                    uni.showToast({
                        title: '签到失败',
                        icon: 'none',
                        duration: 3000
                    });
                    oThis.setData({
                        is_click_checkIn: false
                    });
                }
            );
        },

        getGroupPkInfo() {
            const url = 'https://data.idol001.com/api_app_rank.php?action=wxgroup_pk_group_add_act';
            const params = {
                gid: this.WeChatGroupGid
            };
            const oThis = this;
            net.fetchApi(url, params, 'GET').then((resp) => {
                const data = resp.data.msg;
                console.log(resp);
                if (data._id != undefined) {
                    oThis.setData({
                        groupPkActId: data._id,
                        bShowGroupPkBtn: true
                    });
                }
                if (resp.data.allcount >= 1) {
                    oThis.setData({
                        bShowGroupPkBtn: true
                    });
                }
            });
        },

        /**
         * 是否是管理员
         */
        requestIsAdminStatus: function () {
            var url = 'https://data.idol001.com/api_app_chat.php?action=get_idol_admin_status';
            var that = this;
            net.fetchApi(
                url,
                {
                    gid: that.WeChatGroupGid,
                    starid: that.starIDStr
                },
                'GET'
            ).then(
                (resp) => {
                    if (resp.data && resp.data.is_admin_idol) {
                        that.setData({
                            isIdolAdmin: true
                        });
                    } else {
                        that.setData({
                            isIdolAdmin: false
                        });
                    }
                },
                function () {
                    that.setData({
                        isIdolAdmin: false
                    });
                }
            );
        },

        go2YidouExchange() {
            if (globalData.cookie) {
                var sessionCookie = globalData.cookie.split(/^PHPSESSID=([^\s;]+)\;/g)[1];
            } else {
                var sessionCookie = '';
            }
            var url = `https://m.xingxiaoculture.com/idol_web/#/?gid=${this.groupInfo.gid}&starid=${this.groupInfo.starid}&sessionid=${sessionCookie}`;
            uni.navigateTo({
                url: '../web_page/web_page?url=' + encodeURIComponent(url)
            });
        },

        nav2GroupCheckIn(is_show, gift_text) {
            // this.clickShareApp();
            // return;
            let show_modal = 0;
            if (typeof is_show === 'number') {
                show_modal = is_show;
            }
            const url = `/pages/subpages/pages/group_check_in/group_check_in?gid=${this.WeChatGroupGid}&sid=${this.starIDStr}&show_modal=${show_modal}&gift_text=${gift_text}`;
            uni.navigateTo({
                url: url
            });
        },

        go2GroupRankPage() {
            const url = `/pages/subpages/pages/group_rank_list_detail/group_rank_list_detail?sid=${this.starIDStr}&isShowButton=0&ranktype=3&starName=${this.groupInfo.name}&gid=${this.WeChatGroupGid}`;
            uni.navigateTo({
                url: url
            });
        },

        go2GroupRankWeekPage: function () {
            const url = `/pages/subpages/pages/group_rank_list_detail/group_rank_list_detail?sid=${this.starIDStr}&isShowButton=0&ranktype=2&starName=${this.groupInfo.name}&gid=${this.WeChatGroupGid}`;
            uni.navigateTo({
                url: url
            });
        },

        go2GroupGatherPage() {
            const url = `/pages/subpages_v2/pages/group_fall_in_detail/group_fall_in_detail?sid=${this.starIDStr}&starName=${this.groupInfo.starinfo.name}&gid=${this.WeChatGroupGid}`;
            uni.navigateTo({
                url: url
            });
        },

        go2GroupRankHotPage() {
            console.log('this.data.WeChatGroupGid = ', this.WeChatGroupGid);
            const url = `/pages/subpages_v2/pages/group_fall_in_detail/group_fall_in_detail?sid=${this.starIDStr}&starName=${this.groupInfo.name}&gid=${this.WeChatGroupGid}`;
            uni.navigateTo({
                url: url
            });
        },

        go2supportStarPage() {
            const url = `/pages/subpages_v2/pages/support_list/support_list_new?sid=${this.starIDStr}`;
            uni.navigateTo({
                url: url
            });
        },

        go2BuyCoinPage() {
            // const url = `/pages/subpages_v2/pages/support_list/supportCoinPayTransferPage?&type=coin`
            // wx.navigateTo({
            //     url: url
            // })
        },

        go2starExpPage() {
            const url = `/pages/subpages/pages/star_experience/star_experience?sid=${this.starIDStr}&starName=${this.groupInfo.starinfo.name}&isSuperTopic=${this.isSuperTopic}`;
            uni.navigateTo({
                url: url
            });
        },

        go2WebPage() {
            // const url = `/pages/subpages_v2/pages/support_list/supportCoinPayTransferPage?&type=times`
            // wx.navigateTo({
            //     url: url
            // })
        },

        go2GroupPKPage() {
            const actid = this.groupPkActId;
            let url;
            if (actid !== '') {
                url = `/pages/subpages_v2/pages/group_pk/group_pk?actID=${actid}&gid=${this.WeChatGroupGid}`;
            } else {
                url = `/pages/subpages_v2/pages/group_pk/group_pk_record?gid=${this.WeChatGroupGid}`;
            }
            uni.navigateTo({
                url: url
            });
        },

        go2GroupAttack() {
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            uni.navigateTo({
                url: `/pages/subpages_v2/pages/attack_group/attack_group?WeChatGroupGid=${this.WeChatGroupGid}&bHasShareTicket=${this.bHasShareTicket}&starid=${this.starIDStr}`
            });
        },

        go2NoneGroupAttack() {
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            uni.navigateTo({
                url: `/pages/subpages_v2/pages/attack_group/attack_group?WeChatGroupGid=${this.WeChatGroupGid}&bHasShareTicket=${this.bHasShareTicket}&starid=${this.starIDStr}&noGroup=true`
            });
        },

        go2IdolCard() {
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var oThis = this;
            this.setData({
                bShowGotoActivityDialogue: true
            });
            this.modalGPBtnConfirm();
            return;
            var h5Url = 'https://m.xingxiaoculture.com/clock_in_activity/index.html'; //打卡地址
            /**
             * 拼接url地址
             */
            var h5SessionIdUrl = h5Url;
            var addIndex = 0;
            if (globalData.cookie) {
                var sessionCookie = globalData.cookie.split(/^PHPSESSID=([^\s;]+)\;/g)[1];
            } else {
                var sessionCookie = '';
            }
            sessionCookie = sessionCookie || '';
            console.log('gotoIdolCardH5 sessionCookie==' + sessionCookie);
            if (h5SessionIdUrl.indexOf('?') > 0) {
                addIndex = h5SessionIdUrl.indexOf('?');
                if (addIndex + 1 > h5SessionIdUrl.length - 1) {
                    h5SessionIdUrl = h5SessionIdUrl + 'miniprogram_session=' + encodeURIComponent(oThis.encryptId(sessionCookie));
                } else {
                    h5SessionIdUrl =
                        h5SessionIdUrl.slice(0, addIndex + 1) + h5SessionIdUrl.slice(addIndex + 1) + '&miniprogram_session=' + encodeURIComponent(oThis.encryptId(sessionCookie));
                }
            } else if (h5SessionIdUrl.indexOf('#') > 0) {
                addIndex = h5SessionIdUrl.indexOf('#');
                h5SessionIdUrl = h5SessionIdUrl.slice(0, addIndex) + '?miniprogram_session=' + encodeURIComponent(oThis.encryptId(sessionCookie)) + h5SessionIdUrl.slice(addIndex);
            } else {
                h5SessionIdUrl = h5SessionIdUrl + '?miniprogram_session=' + encodeURIComponent(oThis.encryptId(sessionCookie));
            }
            console.log('gotoIdolCardH5 h5SessionIdUrl==' + h5SessionIdUrl);
            var url =
                '/pages/web_page/web_page_shareable?url=' +
                encodeURIComponent(h5SessionIdUrl) +
                '&shareMiniProgramPath=' +
                encodeURIComponent('/pages/subpages_v2/pages/support_card_h5/support_card_h5?is_share=' + oThis.isShare + '&type=' + oThis.type) +
                '&shareTitle=' +
                encodeURIComponent(' 以爱豆之名为爱豆打卡，领取爱豆礼包啦~') +
                '&shareImg=' +
                encodeURIComponent('https://img.xingxiaoculture.com/origin/2020/02/19/eff916fa027de27cd06679cf832edd211582105380.png');
            console.log('gotoIdolCardH5 redirectTo url==' + url);
            uni.navigateTo({
                url: url
            });
        },

        encryptId: function (message) {
            console.log('encryptId message==' + message);
            var encrypt = CryptoJS.AES.encrypt(message, CryptoJS.enc.Utf8.parse('ADB+zg4Ou3fv6rY8kJbuMPeM41ttw20l'), {
                mode: CryptoJS.mode.ECB,
                padding: CryptoJS.pad.Pkcs7
            }).toString();
            console.log('encryptId message encrypt==' + encrypt);
            return encrypt;
        },

        // descryptId: function (encrypt) {
        //     console.log("descryptId encrypt==" + encrypt);
        //     var decrypt = CryptoJS.AES.decrypt(encrypt, CryptoJS.enc.Utf8.parse("ADB+zg4Ou3fv6rY8kJbuMPeM41ttw20l"), {
        //         mode: CryptoJS.mode.ECB,
        //         padding: CryptoJS.pad.Pkcs7
        //     }).toString(CryptoJS.enc.Utf8);
        //     console.log("descryptId encrypt decrypt==" + decrypt);
        //     return decrypt
        // },

        groupDetailAttackCLick: function (e) {
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            var groupInfo = this.fansSupportList[e.detail.nowIndex];
            console.log('groupInfogroupInfo', groupInfo);
            this.setData({
                selectGroupInfo: groupInfo
            });
            this.getBeSkillStatus();
        },

        groupHonorClick: function (e) {
            var groupInfo = this.fansSupportList[e.detail.nowIndex];
            console.log('groupInfogroupInfo', groupInfo);
            if (groupInfo.honor) {
                this.setData({
                    bHonorWindows: true,
                    honorWindowsType: 1,
                    honorWindowsModel: groupInfo
                });
            }
        },

        moreGroupInfoClick: function (e) {
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_group_honor_member_list';
            var params = {
                starid: e.detail.groupInfo.starid,
                gid: e.detail.groupInfo.gid,
                page: 1,
                count: 10
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data && resp.data.list.length > 0) {
                        this.setData({
                            bHonorWindows: true,
                            honorWindowsType: 2,
                            userArr: resp.data.list
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

        getLunboList: function (e) {
            var that = this;
            var url = 'https://data.idol001.com/api_wxapp_baner.php?action=idol_home';
            var params = {
                starid: this.starIDStr
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp.data) {
                        that.setData({
                            operationLunbotu: resp.data
                        });
                    }
                },
                function (resp) {
                    console.log(resp);
                }
            );
        },

        gotoLunbo(e) {
            if (e.currentTarget.dataset.url.length == 0) {
                return;
            }
            this.setData({
                currentUrl: e.currentTarget.dataset.url
            });
            const list = this.operationLunbotu;
            var myUrl = list[e.currentTarget.dataset.ind].url;
            const http_reg = /^http/i;
            var isGotoHttp = http_reg.test(myUrl);
            var lunboObj = list[e.currentTarget.dataset.ind];
            if (lunboObj.is_logoin && !IDLoginManager.isLoginByCheckLocalData()) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            if (e.currentTarget.dataset.activity == 1) {
                this.setData({
                    bShowGotoActivityDialogue: true,
                    thisLunboObj: e.currentTarget.dataset.item
                });
                this.modalGPBtnConfirm();
                return;
            }
            if (isGotoHttp) {
                const gid = this.WeChatGroupGid;
                if (gid == undefined || gid == '') {
                    this.getwechatGid();
                    return;
                }
                var url = lunboObj.url;

                //test data
                // url = "https://www.baidu.com/a.html?e=e#b=b&c=c"

                if (lunboObj.is_userid) {
                    var addIndex = 0;
                    var sessionCookie = globalData.cookie ? globalData.cookie.split(/^PHPSESSID=([^\s;]+)\;/g)[1] : '';
                    sessionCookie = sessionCookie || '';
                    if (url.indexOf('?') > 0) {
                        addIndex = url.indexOf('?');
                        if (addIndex + 1 > url.length - 1) {
                            url = url + 'sessionid=' + sessionCookie + '&gid=' + gid;
                        } else {
                            url = url.slice(0, addIndex + 1) + 'sessionid=' + sessionCookie + '&gid=' + gid + '&' + url.slice(addIndex + 1);
                        }
                    } else if (url.indexOf('#') > 0) {
                        addIndex = url.indexOf('#');
                        url = url.slice(0, addIndex) + '?sessionid=' + sessionCookie + '&gid=' + gid + url.slice(addIndex);
                    } else {
                        url = url + '?sessionid=' + sessionCookie + '&gid=' + gid;
                    }
                } else {
                    if (url.indexOf('?') > 0) {
                        addIndex = url.indexOf('?');
                        if (addIndex + 1 > url.length - 1) {
                            url = url + 'gid=' + gid;
                        } else {
                            url = url.slice(0, addIndex + 1) + 'gid=' + gid + '&' + url.slice(addIndex + 1);
                        }
                    } else if (url.indexOf('#') > 0) {
                        addIndex = url.indexOf('#');
                        url = url.slice(0, addIndex) + '?gid=' + gid + url.slice(addIndex);
                    } else {
                        url = url + '?gid=' + gid;
                    }
                }
                console.log(url);
                uni.navigateTo({
                    url: '../web_page/web_page?url=' + encodeURIComponent(url)
                });
            } else {
                console.log('myUrl = ', myUrl);
                uni.navigateTo({
                    url: myUrl
                });
            }
        },

        closeModalGroupGather() {
            this.setData({
                bShowGotoGroupGather: false
            });
        },

        modalGGBtnConfirm() {
            this.closeModalGroupGather();
            this.go2GroupGatherPage();
        },

        closeModalActivityDialogue() {
            this.setData({
                bShowGotoActivityDialogue: false
            });
        },

        modalGPBtnConfirm() {
            console.log('modalGPBtnConfirm');
            let url = this.currentUrl;
            let wx_open_id = globalData.idolUserInfo['wx_app_openid_' + Config.PLATFORM];
            this.go2BindUserClockInActivity(url, wx_open_id);
        },

        go2BindUserClockInActivity(lunbo_url, wx_open_id) {
            console.log('go2BindUserClockInActivity ');
            console.log('go2BindUserClockInActivity lunbo_url==' + lunbo_url);
            console.log('go2BindUserClockInActivity wx_open_id==' + wx_open_id);
            const url = 'https://data.idol001.com/api_wxapp_list.php?action=set_kefu_url';
            const params = {
                url: lunbo_url,
                wx_openid: wx_open_id,
                word: 3
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('set_kefu_url resp ==' + resp);
            });
        },

        getFuliBtnStatus() {
            const url = 'https://data.idol001.com/api_mobile_idol_redeem_code.php?action=get_redeem_code_status_by_wxopenid';
            const params = {
                wx_openid: this.user_info.wx_unionid
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                console.log('getFuliBtnStatus 获取福利领取状态 resp==' + JSON.stringify(resp.data));
                if (resp.data && resp.data.ok == 1) {
                    let need_show = true;
                    if (resp.data.reedm_code_use_status == 1) {
                        need_show = false;
                    }
                    console.log('getFuliBtnStatus 获取福利领取状态 need_show==' + need_show);
                    this.setData({
                        bShowFuliBtn: need_show,
                        excCoderUrl: `/pages/subpages/pages/exchange_coder/idol_exchange_coder?gid=${this.WeChatGroupGid}`
                    });
                }
            });
        },

        go2CoderUrl: function () {
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                that.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            if (that.excCoderUrl) {
                uni.navigateTo({
                    url: that.excCoderUrl
                });
            }
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
                gid: this.selectGroupInfo.gid,
                starid: this.selectGroupInfo.starid,
                skill: 'group_gather_freeze',
                in: '1',
                userid: globalData.idolUserInfo ? globalData.idolUserInfo._id : ''
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    console.log('getJoingroup==', resp.data);
                    if (resp.data) {
                        that.setData({
                            selectGroupAttackInfo: resp.data
                        });
                        if (resp.data.in_group) {
                            that.setData({
                                bShowAttackWindows: true,
                                showAttackType: 1
                            });
                        } else if (resp.data.status == 'freeze') {
                            that.setData({
                                bShowAttackWindows: true,
                                showAttackType: 2
                            });
                        } else if (resp.data.status == 'attack') {
                            that.setData({
                                bShowAttackWindows: true,
                                showAttackType: 3
                            });
                        } else if (resp.data.group_attack_times == 0) {
                            that.setData({
                                bShowAttackWindows: true,
                                showAttackType: 4
                            });
                        } else {
                            that.setData({
                                bShowAttackWindows: true,
                                showAttackType: 0
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

        clickCloseModal: function () {
            this.setData({
                bShowAttackWindows: false
            });
        },

        frezzeClick: function () {
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            if (this.userSkillTimes == 0) {
                uni.showToast({
                    title: '您今日攻击次数已达上限',
                    icon: 'none',
                    duration: 2000
                });
            } else {
                this.setData({
                    fansGroupWindowsType: 3,
                    bShowAttackWindows: false
                });
                this.requestUserSupportStar();
            }
        },

        //返回部分 送星星数据
        requestSomeStarInfo: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=get_user_personnal_info';
            var params = {
                starid: that.starIDStr,
                type: 'month'
            };
            var that = this;
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        that.setData({
                            user_info_rank: resp.data
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

        userhonorClick: function (e) {
            var that = this;
            var cellObj = that.dynamicItemLists[e.currentTarget.dataset.cellindex].data_quanzi.userinfo;
            if (cellObj.honor) {
                this.setData({
                    bHonorWindows: true,
                    honorWindowsType: 0,
                    honorWindowsModel: cellObj
                });
            }
        },

        backRewardClick: function () {
            this.setData({
                bHonorWindows: false
            });
        },

        myHonorClick: function () {
            if (this.user_info_rank.userinfo.honor) {
                this.setData({
                    bHonorWindows: true,
                    honorWindowsType: 0,
                    honorWindowsModel: this.user_info_rank.userinfo
                });
            }
        },

        userSelectClick: function (e) {
            this.setData({
                bHonorWindows: false
            });
            var groupInfo = this.userArr[e.detail.selectIndex];
            console.log('groupInfogroupInfo', groupInfo);
            if (groupInfo && groupInfo._id) {
                uni.navigateTo({
                    url: '/pages/subpages/pages/persenal_mainPage/persenal_mainPage?user=' + encodeURIComponent(JSON.stringify(groupInfo))
                });
            }
        },

        pushUserHome: function (e) {
            this.setData({
                bHonorWindows: false
            });
            if (e.detail.userinfo && e.detail.userinfo._id) {
                uni.navigateTo({
                    url: '/pages/subpages/pages/persenal_mainPage/persenal_mainPage?user=' + encodeURIComponent(JSON.stringify(e.detail.userinfo))
                });
            }
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
            } else if (this.mainGroupSetingWindowsType == 'continueAgainSet') {
            } else if (this.mainGroupSetingWindowsType == 'comfirmChangeGroup') {
                this.setData({
                    mainGroupSetingWindowsType: 'changeGroup'
                });
            } else if (this.mainGroupSetingWindowsType == 'changeGroup') {
                this.setData({
                    isShowSetingMainGroupWindows: false
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
            } else if (this.mainGroupSetingWindowsType == 'comfirmChangeGroup') {
                this.setData({
                    isShowSetingMainGroupWindows: false
                });
            } else if (this.mainGroupSetingWindowsType == 'changeGroup') {
                this.setMainGroupRequest();
            } else if (this.mainGroupSetingWindowsType == 'treeComfirmChangeGroup') {
                this.setData({
                    mainGroupSetingWindowsType: 'treeChangeGroup'
                });
            } else if (this.mainGroupSetingWindowsType == 'treeChangeGroup') {
                this.setMainGroupRequest();
            }
        },

        //是否加入过主公会
        getUserMainGroupInfo: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_idol_group_main.php?action=get_main_group_info';
            var params = {
                starid: that.starIDStr,
                gid: this.WeChatGroupGid
            };
            console.log('api gid = ', that.WeChatGroupGid);
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        if (resp.data.group_info) {
                            that.setData({
                                mainGroupInfo: resp.data.group_info
                            });
                        } else {
                            that.setData({
                                mainGroupInfo: ''
                            });
                        }
                        if (resp.data.ok == 1) {
                            if (app.globalData.isShareType && resp.data.group_info.gid != that.WeChatGroupGid) {
                                //卡片的id和用户主群ID不一致
                                that.setData({
                                    isShowSetingMainGroupWindows: true,
                                    mainGroupSetingWindowsType: 'treeComfirmChangeGroup'
                                });
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

        setMainGroupRequest: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_idol_group_main.php?action=set_main_group';
            var params = {
                starid: that.starIDStr,
                gid: that.WeChatGroupGid
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
                        that.getUserMainGroupInfo();
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

        getUserManagerAuthority: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_idol_group_main.php?action=is_group_admin';
            var params = {
                starid: that.starIDStr,
                gid: that.WeChatGroupGid
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data && resp.data.is_group_admin == 1) {
                        that.setData({
                            isManagerAuthority: true
                        });
                    } else {
                        that.setData({
                            isManagerAuthority: false
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

        /**
         * 公会树信息
         */
        getTreeInfo: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_mobile_idol_tree.php?action=get_tree_info';
            var params = {
                gid: that.WeChatGroupGid
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        that.setData({
                            treeInfo: resp.data
                        });
                        that.getFestivalStatus();
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

        //当前是否开放社公会裂变
        getSpreadImg: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_split_task.php?action=get_spread_img';
            var params = {
                starid: that.starIDStr,
                gid: that.WeChatGroupGid
                // 'starid': '4105',
                // 'gid': 'GUvVY5JzxbdCKCmgTwJihH76JYRU'
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

        //公会页-裂变推广资源位点击
        groupJoinActImage: function () {
            var that = this;
            // wx.reportAnalytics('new_users_invite_btn_click', {
            //     user_id: app.globalData.idolUserInfo._id ? app.globalData.idolUserInfo._id : '',
            //     star_id: oThis.data.starIDStr,
            //     activity_id: oThis.data.spreadImg.task_id,
            //     join_from: 2,
            // });
            console.log('打开兴趣公会小程序');
            var path = '';
            if (this.official_group == 1) {
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

        //自动集结开关状态改变
        onChangeSwitch: function (e) {
            console.log('onChangeSwitch = ' + JSON.stringify(e.detail));
            this.setData({
                showVipAuto: e.detail.setAuto,
                defaultStar: e.detail.oldStarName
            });
        },

        //设置自动贡献
        setAutoGuard: function (e) {
            console.log('setAutoGuard');
            this.setData({
                isSet: true
            });
        },

        getFestivalStatus: function () {
            var that = this;
            var url = 'https://data.idol001.com/api_guard_angle.php?action=activity_tree_time';
            var params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        if (resp.data.is_open == 1) {
                            that.setData({
                                isFestival: true
                            });
                        } else {
                            that.setData({
                                isFestival: false
                            });
                        }
                    }
                },
                function () {}
            );
        },

        roseTipsClick: function () {
            this.setData({
                isShowRoseTips: false
            });
            if (globalData.roseTreeTips == 0) {
                globalData.roseTreeTips = 1;
            }
        },

        buyVipSureClick: function () {
            this.setData({
                buyVipDialog: false
            });
        },

        buyVipCloseClick: function () {
            this.setData({
                buyVipDialog: false
            });
        },

        buyVip: function (e) {
            this.setData({
                buyVipDialog: true
            });
        },

        dialogCallback(e) {
            console.log('dialogCallback e==' + JSON.stringify(e));
            console.log('dialogCallback e.detail==' + JSON.stringify(e.detail));
            if (e.detail) {
                const { type, group } = e.detail;
                console.log('dialogCallback typeof type==' + JSON.stringify(typeof type));
                console.log('dialogCallback type==' + JSON.stringify(type));
                console.log('dialogCallback group==' + JSON.stringify(group));
                if (type == 1.1) {
                    storeObj('groupSelected_' + this.user_info._id, group);
                    this.setData({
                        dialogFinalType: 0,
                        thisGroupMain: group,
                        starIDStr: group.starid,
                        getofficial: 1,
                        WeChatGroupGid: group.gid,
                        starName: group.star_name
                    });
                    console.log('dialogCallback WeChatGroupGid=' + this.WeChatGroupGid);
                    console.log('dialogCallback starIDStr=' + this.starIDStr);
                    console.log('dialogCallback getofficial=' + this.getofficial);
                    this.initPageData();
                }
            } else {
                uni.showToast({
                    title: '未选中公会',
                    icon: 'none'
                });
                this.setData({
                    dialogFinalType: 0
                });
            }
        },

        bindSwitchGroup() {
            console.log('bindSwitchGroup');
            this.setData({
                dialogFinalType: 1.1
            });
        },

        /* 获取是否官方主群 */
        requestGroupViewHistory() {
            net.fetchApi('https://data.idol001.com/api_activity_2021_08s.php?action=get_group_main_list', {}, 'GET').then((response) => {
                this.setData({
                    groupViewHistory: response.data,
                    isGroupViewHistoryLoading: false
                });
            });
        },

        openDialog(type, data) {
            this.zpSelectComponent('#rewardWelfareDialog').showView(type, data);
        },

        getRewardInfo() {
            net.fetchApi(`https://data.idol001.com/api_lucky_draw.php?action=get_star_lucky_draw&starid=${this.groupInfo.starid}`, {}, 'GET').then((res) => {
                this.setData({
                    rewardId: res.data.lucky_draw_id
                });
            });
        },

        gotoReward() {
            let _this = this;
            uni.navigateToMiniProgram({
                appId: 'wx997f4bf7ba2f8a49',
                path: '/pages/subpages_v2/pages/welfare/lucky_draw?id=' + _this.rewardId + '&from=admin',
                envVersion: 'trial',
                success(res) {
                    // 打开成功
                }
            });
        },

        closeTips() {
            this.setData({
                bShowTips: false
            });
            uni.setStorageSync('show_water_tips', false);
        },

        //星球联赛
        closeLeagueTips() {
            this.setData({
                bShowsStarLeagueTips: false
            });
            //wx.setStorageSync('show_water_tips', false);
        },

        getStarLeagueInfo() {
            var oThis = this;
            if (!this.starIDStr) {
                return;
            }
            //clearInterval(this.intervalTimeoutAct);
            oThis.setData({
                continueCountTime: 0
            });
            net.fetchApi(`https://data.idol001.com/api_star_ball_game.php?action=get_star_ball_game&starid=${this.starIDStr}`, {}, 'GET').then((resp) => {
                if (resp && resp.data) {
                    // console.log("league");
                    // console.log(resp.data.game_tomorrow_info);
                    oThis.setData({
                        starBallLeagueInfo: resp.data
                    });
                    let timeoutLimit = 0;
                    if (resp.data.game_tomorrow_info.begin_time) {
                        let time = new Date(resp.data.time * 1000);
                        if (/*time.getHours()>=11 && */ storeObj('groupPageLeagueTips') != resp.data.date) {
                            oThis.getStarLeagueSchedule();
                            storeObj('groupPageLeagueTips', resp.data.date);
                        }
                        timeoutLimit = resp.data.game_tomorrow_info.begin_time;
                    } else if (resp.data.game_today_info.begin_time || resp.data.game_yesterday_info.begin_time) {
                        if (!resp.data.in_counting) {
                            if (storeObj('groupPageLeagueTips') != resp.data.date) {
                                oThis.setData({
                                    bShowsStarLeagueTipsType: 1,
                                    bShowsStarLeagueTips: true
                                });
                                storeObj('groupPageLeagueTips', resp.data.date);
                            }
                            timeoutLimit = resp.data.game_today_info.begin_time ? resp.data.game_today_info.begin_time + 86400 : 0;
                        }
                    }
                    if (timeoutLimit != 0) {
                        console.log(timeoutLimit);
                        oThis.setData({
                            timeoutLimit: timeoutLimit - resp.data.time + new Date().getTime() / 1000
                        });
                        oThis.initActInterval();
                    }
                }
            });
        },

        //获取日程表
        getStarLeagueSchedule() {
            var url = 'https://data.xingxiaoculture.com/api_star_ball_game.php?action=star_ball_game_plan';
            var params = {
                starid: this.starIDStr
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp && resp.data && resp.data.list) {
                    this.setData({
                        starLeagueScheduleList: resp.data.list,
                        bShowsStarLeagueTipsType: 2,
                        bShowsStarLeagueTips: true
                    });
                }
            });
        },

        // 点击查看更多前往星球联赛子页面
        lookMoreLeague(e) {
            console.log(e);
            uni.navigateTo({
                url: `/subpage_v4/pages/league_detail/league_detail?type=${e.currentTarget.dataset.type}`
            });
        },

        //打开规则页
        gotoRulePageLeague: function () {
            uni.navigateTo({
                url: '/subpage_v4/pages/league_guide/league_guide'
            });
        },

        //倒计时计时器
        initActInterval: function () {
            var that = this;
            console.log('timeoutLimit:' + that.timeoutLimit);
            if (that.timeoutLimit <= 0) {
                return;
            }
            var t = that.timeoutLimit - new Date().getTime() / 1000;
            console.log('league_time:' + t);
            var et = that.timeChange(t);
            this.setData({
                actCountdownTimeStr: et,
                continueCountTime: 1
            });
            if (!this.intervalTimeoutAct) {
                this.intervalTimeoutAct = setInterval(function () {
                    if (that.continueCountTime == 1) {
                        var t = that.timeoutLimit - new Date().getTime() / 1000;
                        if (t >= 0) {
                            var et = that.timeChange(t);
                            that.setData({
                                actCountdownTimeStr: et
                            });
                        }
                        if (that.timeoutLimit != 0 && new Date().getTime() / 1000 >= that.timeoutLimit) {
                            that.getStarLeagueInfo();
                        }
                    }
                }, 998);
            }
        },

        timeChange: function (timestamp) {
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
            // var last_time = d != 0 ? d + "天" : "";
            var last_time = '';
            last_time += h + ':' + m + ':' + s;
            return last_time;
        },

        gotoStarPage(e) {
            // 前往星球页
            var starid = e.currentTarget.dataset.starid;
            if (starid) {
                uni.navigateTo({
                    url: '/pages/subpages_v2/pages/support_list/support_list_new?sid=' + starid
                });
            }
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

        // 前往公会框编辑页
        gotoGroupFrame() {
            uni.navigateTo({
                url: '/subpage_v4/pages/group_frame_edit/group_frame_edit?starid=' + this.starIDStr + '&gid=' + this.WeChatGroupGid
            });
        },

        // 前往公会成员查看
        checkGroupMemberClick: function () {
            uni.navigateTo({
                url: '/pages/subpages_v2/pages/gp_member_manage/gp_member_manage?sid=' + this.starIDStr + '&gid=' + this.WeChatGroupGid
            });
        },

        switchGroupAdminDialog: function () {
            this.setData({
                bShowGroupAdminDialog: !this.bShowGroupAdminDialog
            });
        },

        applyGroup: function () {
            this.setData({
                nogroupApply: true
            });
        },

        shuffleGroup: function () {
            var arr = this.nogroupList;
            var length = arr.length;
            var randomIndex;
            var temp;
            while (length) {
                randomIndex = Math.floor(Math.random() * length--);
                temp = arr[randomIndex];
                arr[randomIndex] = arr[length];
                arr[length] = temp;
            }
            //return arr;
            this.setData({
                nogroupList: arr
            });
        },

        clickShareApp: function (e) {
            var that = this;
            console.log('clickShareApp.click');
            uni.miniapp.invokeMiniappNativeExtension({
                method: 'shareCustomAppMesssage',
                param: {
                    title: '吹响公会集结号角，助力爱豆更多贡献点！',
                    imageUrl: 'http://img.xingxiaoculture.com/origin/2019/12/14/b28e078ad391038f050b516f27c468c31576296592.png',
                    path: '/pages/index/index?action=gopage&page=group_fall_in_detail&sid='
                },
                success: (res) => {
                    console.log('shareCustomAppMesssage success', res);
                }
            });
        },

        /**
         * 小岛跳转点击
         */
        gotoIslandPage: function () {
            if (this.isShowRoseTips) {
                this.setData({
                    isShowRoseTips: false
                });
                if (globalData.roseTreeTips == 0) {
                    globalData.roseTreeTips = 1;
                }
            }
            var that = this;
            //未登录，弹登录框
            if (IDLoginManager.loginState() != IDLoginManager.is_has_login) {
                this.setData({
                    bShowLoginPanel: true
                });
                return;
            }
            console.log('gid===== ', that.WeChatGroupGid);
            console.log('starid===== ', that.starIDStr);
            uni.navigateTo({
                url: '/subpage_v4/pages/map_game/map_game_2?gid=' + (that.WeChatGroupGid && that.WeChatGroupGid.length > 0 ? that.WeChatGroupGid : '') + '&starid=' + that.starIDStr
            });
        },

        requsetGidCallback() {
            console.log('占位：函数 requsetGidCallback 未声明');
        }
    }
};
</script>
<style>
@import './idol_home.css';
@import '@/pages/idol_home/idol_home.css';
@import '@/templates/dynamicCellList/dynamicCellList.css';
</style>
