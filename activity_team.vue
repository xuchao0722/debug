<template>
    <view style="height: 100%">
        <!-- <view class="flex_row flex_center" style='width: 750rpx;margin-top:30rpx'>
      <view class="btn-group" style="border-radius: 25rpx;">
          <view class="{{tabIndex == 0?'type-btn-on':'type-btn'}}" style="{{isCaptain!=1?'width:230rpx;':''}}" bindtap="changeTab" data-tab="0">
              队伍排行
          </view>
          <view class="{{tabIndex == 1?'type-btn-on':'type-btn'}}" style="{{isCaptain!=1?'width:230rpx;':''}}" bindtap="changeTab" data-tab="1">
              队伍列表
          </view>
          <view wx:if="{{isCaptain==1}}" class="{{tabIndex == 2?'type-btn-on':'type-btn'}}" style="{{isCaptain!=1?'width:230rpx;':''}}" bindtap="changeTab" data-tab="2">
              申请列表
          </view>
          <view class="{{tabIndex == 3?'type-btn-on':'type-btn'}}" style="{{isCaptain!=1?'width:230rpx;':''}}" bindtap="changeTab" data-tab="3">
              擂台奖励
          </view>
      </view>
  </view> -->

        <block v-if="tabIndex == 1">
            <view class="top_bar" v-if="myBalloonTeamCurrentId">
                <view class="top_bar_title" style="width: 85%">
                    当前队伍：
                    <text>{{ myBalloonTeamCurrent.team_name }}</text>
                </view>
                <view class="top_bar_title" style="font-size: 10px; width: 15%" @tap="showSelectTeam">切换></view>
            </view>
            <view class="top_bar" style="display: flex; align-items: center" v-else>
                <!-- <image style="width: 32rpx;height: 32rpx;" src="https://star-img.xingxiaoculture.com/2022/09/14/6525111df63d627bae08f0d5d7ae4776.png"></image> -->
                <text class="top_bar_title" style="color: #ffffff; margin-left: 10rpx">◔{{ actArenaCountdownTimeStr }}</text>
            </view>
        </block>
        <block v-else-if="tabIndex == 0">
            <!-- <view class="top_bar">
      <text class="top_bar_title">果树森林会展示等级排名前10名的果树</text>
    
    </view> -->
        </block>
        <block v-else-if="tabIndex == 2">
            <!-- <view class="act_rank_list_date2" bindtap="showArenaDateSelect">{{balloonTermDate}} <text wx:if="{{!bShowBalloonDateSelect}}">▼</text><text wx:else>▲</text></view>
      <view class="act_rank_list_date_select2" wx:if="{{bShowBalloonDateSelect}}">
          <view class="act_rank_list_date_item2" wx:for="{{arenaList}}" data-item="{{item}}" wx:key="index" bindtap="changeArenaRank">{{item.datetime}}</view>
      </view> -->
        </block>

        <view class="contentBox" v-if="tabIndex == 2">
            <!-- <view class="btn-group" wx:if="{{location==-1}}">
        <view class="{{levelTab == 0?'type-btn-on':'type-btn'}}" bindtap="changeTab" data-tab="0">
          队伍积分排名
        </view>
        <view class="{{levelTab == 1?'type-btn-on':'type-btn'}}"  bindtap="changeTab" data-tab="1">
          反超登顶列表
        </view>
    </view> -->
            <scroll-view :scroll-y="true" @scrolltolower="lower" class="scrollBox">
                <view class="scrollBoxA" v-if="noData == 0">
                    <image style="width: 270rpx; height: 270rpx" src="https://star-img.xingxiaoculture.com/2023/04/14/870cbdf0f7b06d66e98c9fd1338352e2.png"></image>
                    <text style="margin-top: 30rpx">暂无数据</text>
                </view>
                <block v-else>
                    <!-- <view class="act-title-bar">
          <view class="act-title {{tabIndexSe==0?'act-title-active':''}}" bindtap="changeTabSe" data-tab="0">按{{tool_b_name}}数量排名</view>
          <view class="act-title {{tabIndexSe==1?'act-title-active':''}}" bindtap="changeTabSe" data-tab="1">队伍魔法攻击力排名</view>
          <view class="act-title {{tabIndexSe==2?'act-title-active':''}}" bindtap="changeTabSe" data-tab="2">队伍魔法防御力排名</view>
        </view> -->
                    <view class="top_area_line2" v-if="item.title">
                        <view class="top_area_tex2">{{ item.title }}</view>
                    </view>
                    <view v-if="levelTab == 1" class="flex_column flex_center" style="width: 100%; margin-top: 20rpx">
                        <view class="flex_column flex_center" style="width: 680rpx; height: 250rpx; border-radius: 20rpx; border: 2rpx solid #ffeaf6; background: #ffeaf6">
                            <text style="font-size: 13px; color: #333333; width: 643rpx; text-align: center">反超登顶规则</text>
                            <text style="font-size: 12px; color: #333333; width: 643rpx; margin-top: 10rpx">
                                1.在各个魅力类型排行榜上，每超越了排名第一的用户即可视为反超登顶一次
                            </text>
                            <text style="font-size: 12px; color: #333333; width: 643rpx; margin-top: 10rpx">
                                2.每个小时前
                                <text style="color: #ff881a">30次</text>
                                反超登顶，即可以获得反超登顶奖励
                            </text>
                            <text style="font-size: 12px; color: #333333; width: 643rpx; margin-top: 10rpx">3.反超登顶奖励：30万魅力值+30万爱心</text>
                        </view>
                        <view style="color: #ff881a; width: 450rpx; line-height: 52rpx; background-color: #ffe3f5; margin-top: 20rpx; text-align: center; font-size: 13px">
                            {{ '暂无排名信息，快去为TA送' + tool_b_name + '吧！' }}
                        </view>
                    </view>
                    <view class="tipBox" v-if="levelTab == 0">
                        <text>{{ '暂无排名信息，快去为TA送' + tool_b_name + '吧！' }}</text>
                    </view>
                    <view class="myTitle2" v-if="levelTab == 0 && myBalloonTeamByTerm.length > 0">
                        <view class="line"></view>
                        <view class="lineText">{{ levelTab == 0 ? '我的队伍' : '我的排名' }}</view>
                        <view class="line"></view>
                    </view>
                    <block v-if="levelTab == 0">
                        <block v-if="myBalloonTeamByTerm.length > 0" v-for="(item, index) in myBalloonTeamByTerm" :key="index">
                            <view class="flex_row flex_center" v-if="tabIndexSe == 0">
                                <view class="extra_text">
                                    {{ item.my_team.extra_text }}
                                </view>
                            </view>

                            <view class="xingqiuItem" style="padding: 10rpx 0 20rpx 0">
                                <view class="xingqiuItem1" style="background: none">
                                    <!-- <view class="xiangqing" bindtap="details1" data-info="{{item}}">详情</view> -->
                                    <view
                                        :class="
                                            'scrollOrder ' +
                                            (item.my_team.rank == 1 ? 'scrollOne' : item.my_team.rank == 2 ? 'scrollTwo' : item.my_team.rank == 3 ? 'scrollThree' : '')
                                        "
                                        :style="item.my_team.rank == '未上榜' ? 'line-height:25rpx' : ''"
                                    >
                                        {{ item.my_team.rank }}
                                    </view>
                                    <image class="tree_img" :src="item.my_team.star_tree"></image>
                                    <view class="m11" style="width: 280rpx; margin-left: 10rpx">
                                        <view class="group-num-info4">{{ item.my_team.team_name }}</view>

                                        <view style="margin-top: 10rpx">
                                            <image
                                                :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                                :src="
                                                    member.userinfo.image.thumbnail_pic
                                                        ? member.userinfo.image.thumbnail_pic
                                                        : 'https://star-img.xingxiaoculture.com/2023/05/17/12d82de4080c6e8c56cad0c211d588fb.png'
                                                "
                                                @tap="parseEventDynamicCode($event, member.userinfo.image.thumbnail_pic ? 'gotoDressRoom' : '')"
                                                :data-userid="member.userinfo._id"
                                                :data-obj="member"
                                                v-for="(member, index1) in item.my_team.members_pos"
                                                :key="index1"
                                            ></image>
                                        </view>
                                    </view>
                                    <!-- <view class="group-data-info">
                      <view class="group-num-info1">LV.{{item.level}}</view>
                      <view class="group-title1">{{ item.team_name }}</view>
                    </view> -->
                                    <view class="group-data-info">
                                        <!-- <view class="group-num-info3" style="text-align: right;" wx:if="{{location!=-1}}">已收集魅力值</view>
                      <view class="group-num-info3" style="text-align: right;" wx:else>共收集<text style="color: #FF881A;">{{item.my_team.team_tools}}</text>魅力值</view> -->
                                        <view class="group-num-info3" style="text-align: right">
                                            <text style="color: #ff881a">{{ item.my_team.race_info.speed }}</text>
                                            <text style="color: #333333">米/分钟</text>
                                        </view>
                                        <view class="group-num-info3" style="text-align: right">
                                            <text style="color: #ff881a">{{ item.my_team.team_fruit }}</text>
                                            <text style="color: #333333">{{ location == -1 ? '米' : '点' }}</text>
                                        </view>
                                    </view>
                                    <!-- <view class="group-data-btn">
                      <view class="detail-btn" style="{{item.is_digg?'background-image: url(https://star-img.xingxiaoculture.com/2022/08/09/bfcd1967886f50c6beb5133fcac19029.png);':'background-image: url(https://star-img.xingxiaoculture.com/2022/08/08/9b53a233b9b1fc2cb84238bf8e403295.png);'}}" bindtap="dingTeam" data-obj="{{item}}"></view>
                      <view class="group-num-info2" bindtap="dingTeam" data-obj="{{item}}">{{item.digg_num}}</view>
                    </view> -->
                                </view>
                                <view class="location_top1" v-if="item.my_team.team_pos_top1user.nickname">
                                    <text style="color: #333333; margin-left: 60rpx">{{ item.my_team.team_pos_top1user.text }}</text>
                                    <image
                                        style="width: 42rpx; height: 42rpx; border-radius: 50%; margin-left: 20rpx"
                                        :src="item.my_team.team_pos_top1user.image.thumbnail_pic"
                                    ></image>
                                    <text style="color: #333333; margin-left: 10rpx; width: 230rpx">{{ item.my_team.team_pos_top1user.nickname }}</text>
                                </view>
                            </view>
                        </block>
                    </block>
                    <!-- <view class="xingqiuItem" style="padding: 10rpx 0 20rpx 0;"  wx:elif="{{userMine}}">
            <view class="xingqiuItem1" style="background: none;">
              <view
                class="scrollOrder {{userMine.rank==1 ?'scrollOne':userMine.rank==2?'scrollTwo':userMine.rank==3?'scrollThree':'' }}" style="{{userMine.rank=='-1'?'line-height:25rpx':''}}">
                {{userMine.rank==-1?'未上榜':''}}
              </view>
              <image  class="tree_img"  style="border-radius: 50%;" src="{{userMine.userinfo.image.middle_pic}}"></image>
              <view class="m11"  style="width:280rpx;margin-left: 10rpx;" wx:key="index">
                {{userMine.userinfo.nickname}}
              </view>
                <view class="group-data-info">
                  <view class="group-num-info3" style="text-align: right;" wx:if="{{levelTab==0}}"><text style="color: #FF881A;">{{levelTab==0?userMine.my_team.team_fruit:userMine.exceed_times}}</text><text style="color: #FF881A;">{{location==-1?(levelTab==0?'积分':'反超次数'):(tool_b_unit)}}</text></view>
                  <view class="group-num-info3" style="text-align: right;" wx:else><text style="color: #FF881A;">反超次数\n</text><text style="color: #FF881A;">{{userMine.exceed_times}}</text>次</view>
                </view>
            </view>
        </view> -->

                    <!-- <block wx:for="{{myBalloonTeamByTerm}}" wx:if="{{myBalloonTeamByTerm.length>0}}" wx:key="index">
          <view class="flex_row flex_center" wx:if="{{tabIndexSe==0}}">
            <view class="extra_text">
              <image class="extra_text_img" mode="aspectFit" src="https://star-img.xingxiaoculture.com/2022/12/09/32f103335811747533a1d5cf951c4474.png"></image>
              {{item.my_team.extra_text}}
            </view>
          </view>
          <view class="xingqiuItem"  >
            <view class="xingqiuItem1" >
              <view wx:if="{{tabIndexSe==0}}"
                class="scrollOrder {{item.my_team.rank==-1?'scrollNo':item.my_team.rank==1 ?'scrollOne':item.my_team.rank==2?'scrollTwo':item.my_team.rank==3?'scrollThree':'' }}" style="{{item.my_team.rank=='未上榜'?'line-height:25rpx':''}}">
                {{item.my_team.rank==-1?"未上榜":item.my_team.rank}}
              </view>
              <view wx:else
                class="scrollOrder {{item.my_team.team_level_rank==-1?'scrollNo':item.my_team.team_level_rank==1 ?'scrollOne':item.my_team.team_level_rank==2?'scrollTwo':item.my_team.team_level_rank==3?'scrollThree':'' }}" style="{{item.my_team.team_level_rank=='未上榜'?'line-height:25rpx':''}}">
                {{item.my_team.team_level_rank==-1?"未上榜":item.my_team.team_level_rank}}
              </view>
              <view class="group-data-info">
                <view class="group-num-info3">{{item.my_team.team_name}}</view>
              </view>
              <block wx:if="{{tabIndexSe==0}}">
                <view class="group-data-info">
                  <view class="group-num-info3">投入{{tool_s_name}}{{item.my_team.sugar_num}}</view>
                </view>
                <view class="group-data-info">
                  <view class="group-num-info3" style="color:#FF8686">{{tool_b_name}}数量{{item.my_team.team_fruit+item.my_team.free_candy_num}}</view>
                </view>
              </block>
              <block wx:else>
                <image src="https://star-img.xingxiaoculture.com/2022/10/14/5276845184a71ac8158658a594d55abc.png" style="width:38rpx;height: 41rpx;background-color: #6454AE;border-radius: 50%;;margin-left: 20rpx;"></image>
                <text style="font-size: 11px;color: #ffffff;margin-left: 10rpx;width:136rpx;color:#8DE3FF;line-height: 26rpx;">魔法攻击力\n{{item.my_team.attack_num}}</text>
                <image src="https://star-img.xingxiaoculture.com/2022/10/14/3aac371e19830660fc052a5faeefd635.png" style="width:38rpx;height: 41rpx;background-color: #6454AE;border-radius: 50%;;margin-left: 20rpx;"></image>
                <text style="font-size: 11px;color: #ffffff;margin-left: 10rpx;;width:136rpx;color: #FFD672;line-height: 26rpx;">魔法防御力\n{{item.my_team.defend_num}}</text>
              </block>
            </view>
            <view class="xingqiuItem1A" wx:if="{{item.my_team.text}}" style="	border-radius: 0rpx;">
                <text class="text">{{item.my_team.text}}</text>
            </view>
            <view class="flex_column flex_center" style="width: 100%;background-color: #1A3F62;border-radius: 0 0 14rpx 14rpx;padding-bottom: 10rpx;">
              <view class="group_user_box" wx:for="{{item.my_team.members}}" wx:key="index" wx:for-item="items">
                <view class="group_user_item">
                  <image class="group_user_item_image" src="{{items.userinfo.image.middle_pic}}"></image>
                  <view class="group_user_item_title" style="color: #ffffff;">{{items.userinfo.nickname}}</view>
                  <view class="group_user_item_level" wx:if="{{tabIndexSe==0}}">投入{{tool_s_name}}{{items.sugar_num}}</view>
                  <block wx:else>
                    <image src="https://star-img.xingxiaoculture.com/2022/10/14/5276845184a71ac8158658a594d55abc.png" style="width:38rpx;height: 41rpx;background-color: #6454AE;border-radius: 50%;;margin-left: 10rpx;"></image>
          <text style="font-size: 11px;color: #ffffff;margin-left: 5rpx;width:136rpx;color:#8DE3FF;line-height: 26rpx;">魔法攻击力\n{{items.attack_num}}</text>
          <image src="https://star-img.xingxiaoculture.com/2022/10/14/3aac371e19830660fc052a5faeefd635.png" style="width:38rpx;height: 41rpx;background-color: #6454AE;border-radius: 50%;;margin-left: 10rpx;"></image>
          <text style="font-size: 11px;color: #ffffff;margin-left: 5rpx;;width:136rpx;color: #FFD672;line-height: 26rpx;">魔法防御力\n{{items.defend_num}}</text>
                  </block>
                </view>
                <view class="my_tree_team_rewardtext" wx:if="{{items.expect_text || items.text}}">
                    <text> {{items.text}}{{items.text?'\n':''}}{{items.expect_text}}</text>
                  </view>
              </view>
            </view>
          </view>
        </block> -->
                    <view class="myTitle2">
                        <view class="line"></view>
                        <view class="lineText">{{ levelTab == 0 ? '队伍排名' : '反超动态' }}</view>
                        <view class="line"></view>
                    </view>
                    <block v-if="levelTab == 0">
                        <block v-if="listDisplay == 1">
                            <view class="xingqiuItem" style="padding: 10rpx 0 20rpx 0" v-for="(item, index) in allBalloonteam" :key="index">
                                <view class="xingqiuItem1" style="background: none">
                                    <!-- <view class="xiangqing" bindtap="details1" data-info="{{item}}">详情</view> -->
                                    <view
                                        :class="'scrollOrder ' + (item.rank == 1 ? 'scrollOne' : item.rank == 2 ? 'scrollTwo' : item.rank == 3 ? 'scrollThree' : '')"
                                        :style="item.rank == '未上榜' ? 'line-height:25rpx' : ''"
                                    >
                                        {{ item.rank }}
                                    </view>
                                    <image v-if="levelTab == 0" class="tree_img" :src="item.star_tree"></image>
                                    <image v-else class="tree_img" style="border-radius: 50%" :src="item.userinfo.image.middle_pic"></image>
                                    <view v-if="levelTab == 0" class="m11" style="width: 280rpx; margin-left: 10rpx">
                                        <view class="group-num-info4">{{ item.team_name }}</view>

                                        <view style="margin-top: 10rpx">
                                            <image
                                                :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                                :src="
                                                    member.userinfo.image.thumbnail_pic
                                                        ? member.userinfo.image.thumbnail_pic
                                                        : 'https://star-img.xingxiaoculture.com/2023/05/17/12d82de4080c6e8c56cad0c211d588fb.png'
                                                "
                                                @tap="parseEventDynamicCode($event, member.userinfo.image.thumbnail_pic ? 'gotoDressRoom' : '')"
                                                :data-userid="member.userinfo._id"
                                                :data-obj="member"
                                                v-for="(member, index1) in item.members_pos"
                                                :key="index1"
                                            ></image>
                                        </view>
                                    </view>
                                    <view v-else class="m11" style="width: 150rpx; margin-left: 10rpx; font-size: 13px">{{ myRankObject.userinfo.nickname }}</view>
                                    <!-- <view class="group-data-info">
                    <view class="group-num-info1">LV.{{item.level}}</view>
                    <view class="group-title1">{{ item.team_name }}</view>
                  </view> -->
                                    <view class="group-data-info">
                                        <!-- <view class="group-num-info3" style="text-align: right;" wx:if="{{location!=-1}}">已水晶海之战</view>
                    <view class="group-num-info3" style="text-align: right;" wx:else>共收集<text style="color: #FF881A;">{{item.team_tools}}</text>魅力值</view> -->
                                        <view class="group-num-info3" style="text-align: right">
                                            <text style="color: #ff881a">{{ item.race_info.speed }}</text>
                                            <text style="color: #333333">米/分钟</text>
                                        </view>
                                        <view class="group-num-info3" style="text-align: right" v-if="levelTab == 0">
                                            <text style="color: #ff881a">{{ levelTab == 0 ? item.team_fruit : item.exceed_times }}</text>
                                            <text style="color: #333333">{{ location == -1 ? (levelTab == 0 ? '米' : '反超次数') : '点' }}</text>
                                        </view>
                                        <view class="group-num-info3" style="text-align: right" v-else>
                                            <text style="color: #ff881a">反超次数\n</text>
                                            <text style="color: #ff881a">{{ item.exceed_times }}</text>
                                            次
                                        </view>
                                        <view class="button_zhuli" v-if="levelTab == 0 && item.in_team != 1" @tap="gotoTeamBattle" :data-team_id="item.team_id" :data-item="item">
                                            助力
                                        </view>
                                    </view>
                                    <!-- <view class="group-data-btn">
                    <view class="detail-btn" style="{{item.is_digg?'background-image: url(https://star-img.xingxiaoculture.com/2022/08/09/bfcd1967886f50c6beb5133fcac19029.png);':'background-image: url(https://star-img.xingxiaoculture.com/2022/08/08/9b53a233b9b1fc2cb84238bf8e403295.png);'}}" bindtap="dingTeam" data-obj="{{item}}"></view>
                    <view class="group-num-info2" bindtap="dingTeam" data-obj="{{item}}">{{item.digg_num}}</view>
                  </view> -->
                                </view>

                                <view class="location_top1" v-if="item.team_pos_top1user.nickname">
                                    <text style="color: #333333; margin-left: 60rpx">{{ item.team_pos_top1user.text }}</text>
                                    <image style="width: 42rpx; height: 42rpx; border-radius: 50%; margin-left: 20rpx" :src="item.team_pos_top1user.image.thumbnail_pic"></image>
                                    <text style="color: #333333; margin-left: 10rpx; width: 230rpx">{{ item.team_pos_top1user.nickname }}</text>
                                </view>

                                <block v-if="tabIndexSe == 0">
                                    <view class="my_tree_team_rewardtext" v-if="pageDataObj.single_user_rank_info_all_time.text">
                                        <text class="text">{{ pageDataObj.single_user_rank_info_all_time.text }}</text>
                                    </view>
                                </block>

                                <block v-else>
                                    <view class="xingqiuItem1A" v-if="pageDataObj.single_user_rank_info_all_time.text" style="justify-content: left">
                                        <text class="magic_attack_text">魔法攻击力 {{ item.attack_num }}</text>
                                        <text class="magic_defend_text">魔法防御力 {{ item.defend_num }}</text>
                                    </view>
                                </block>
                            </view>
                            <view class="jiazai" v-if="levelTab == 0">仅展示排名前10</view>
                        </block>
                        <view v-else-if="" class="list_lock">
                            <text style="margin-top: 82rpx">为保护票数隐私,当前场次排名详细未公布\n每晚22:03公布排名情况</text>
                            <image
                                src="https://star-img.xingxiaoculture.com/2022/11/09/1e21f905d03cbdde04f1d61cd27f1e7d.png"
                                style="width: 134rpx; height: 163rpx; margin-top: 40rpx"
                            ></image>
                        </view>
                    </block>
                    <block v-else-if="levelTab == 1">
                        <block v-if="listDisplay == 1">
                            <view class="xingqiuItem" style="padding: 10rpx 0 20rpx 0" v-for="(item, index) in allBalloonteam" :key="index">
                                <view class="xingqiuItem1" style="background: none">
                                    <image class="tree_img" style="border-radius: 50%; width: 62rpx; height: 62rpx" :src="item.user_image"></image>
                                    <view class="content_roll_text">
                                        <text
                                            :style="citem.highlight == 1 ? 'color:#FF881A;' : citem.highlight == 2 ? 'color:#333333;' : ''"
                                            v-for="(citem, index1) in item.content"
                                            :key="index1"
                                        >
                                            {{ citem.text + ' ' }}
                                        </text>
                                    </view>
                                    <view class="content_roll_time">{{ item.time }}</view>
                                </view>
                            </view>
                        </block>
                    </block>
                    <view style="height: 200rpx"></view>
                </block>
            </scroll-view>
        </view>

        <view v-if="tabIndex == 0">
            <image class="tree_info_back" src="https://star-img.xingxiaoculture.com/2022/10/16/fa797c8ccfdc8bfcb2331685c2e43e41.png" mode="aspectFill"></image>
            <view class="act_rank_list_date" @tap="showArenaDateSelect">
                {{ balloonTermDate }}
                <text v-if="!bShowBalloonDateSelect">▼</text>
                <text v-else>▲</text>
            </view>
            <view class="act_rank_list_date_select" v-if="bShowBalloonDateSelect">
                <view class="act_rank_list_date_item" :data-item="item" @tap="changeArenaRank" v-for="(item, index) in arenaList" :key="index">{{ item.datetime }}</view>
            </view>
            <view :class="'tree_info tree_location_' + index" @tap="showTreeInfo" :data-item="item" v-for="(item, index) in allBalloonteamTree" :key="index">
                <image class="tree_info_image" :src="item.star_tree"></image>

                <view class="arena_box_msg">
                    <view class="tree_info_message" :style="'margin-left: ' + (item.slogan_len ? item.slogan_len : 11) * 10 * transX + 'px;'">{{ item.slogan }}</view>
                </view>

                <view class="arena_box_msg2">
                    <view class="tree_info_message">{{ item.team_name }}</view>
                </view>
            </view>
        </view>

        <view v-if="tabIndex == 1">
            <!-- <view class="flex_column flex_center" wx:if="{{(arenaInfo.status=='is_end' && arenaInfo.next_term_id && myBalloonTeamNext.length==0) || (arenaInfo.status!='is_end' && myBalloonTeam.length==0)}}"> -->
            <view class="flex_column flex_center" v-if="!myBalloonTeamCurrentId">
                <view class="tree_item">
                    <view class="tree_team" style="text-align: center; height: 82rpx">
                        <text class="tree_team_title" style="width: 100%" v-if="arenaInfo.status == 'is_end'">当前场次已结束</text>
                        <text class="tree_team_title" style="width: 100%" v-else-if="arenaInfo.status == 'not_begin'">当前场次未开始</text>
                        <text class="tree_team_title" style="width: 100%" v-else>当前场次进行中</text>
                    </view>
                    <view class="tree_info_column" style="width: 100%">
                        <text class="tree_info_text4" v-if="arenaInfo.status == 'is_end'">
                            您可以提前选择创建或者加入一个
                            <text style="color: #ff8146">明日</text>
                            队伍\n组成队伍后系统会赠送
                            <text style="color: #ff8146">{{ tool_s_name }}*200{{ tool_s_unit }}</text>
                            用于制作{{ tool_b_name }}
                        </text>
                        <text class="tree_info_text4" v-else-if="arenaInfo.status == 'not_begin'">
                            您可以提前选择创建或者加入一个队伍\n组成队伍后系统会赠送
                            <text style="color: #ff8146">{{ tool_s_name }}*200{{ tool_s_unit }}</text>
                            用于制作{{ tool_b_name }}
                        </text>
                        <text class="tree_info_text4" v-else>
                            您可以选择创建或者加入一个队伍\n组成队伍后系统会赠送
                            <text style="color: #ff8146">{{ tool_s_name }}*200{{ tool_s_unit }}</text>
                            用于制作{{ tool_b_name }}
                        </text>
                        <view class="tree_info_button_box">
                            <view class="tree_info_button2" @tap="showBalloonCreateTeamPanel">创建队伍</view>
                            <view class="tree_info_button2" @tap="handleTeamJoinlist">加入其他队伍</view>
                        </view>
                    </view>
                </view>
            </view>

            <scroll-view :scroll-y="true" @scrolltolower="lower" class="scrollBox" v-if="myBalloonTeamCurrentId">
                <view class="my_tree_info">
                    <view class="flex_column flex_center">
                        <view class="my_tree_button" style="background: #413781">
                            <view class="candy_img2">
                                <image src="https://star-img.xingxiaoculture.com/2022/10/13/bb13aaa577edc16f08ba197eefa72061.png" style="width: 64rpx; height: 56rpx"></image>
                            </view>
                            <text class="my_tree_team_user_text" style="width: 70%; font-size: 11px; text-align: start">制作数量 {{ myBalloonTeamCurrent.team_fruit }}</text>
                        </view>
                        <text style="margin-top: 5rpx; font-size: 10px; color: #b1aad9">按成员制作占比分配</text>
                    </view>

                    <!-- <view class="my_tree_produce" style="line-height: 76rpx;" wx:if="{{arenaInfo.status=='not_begin' || arenaInfo.term_id < myBalloonTeamCurrent.term_id}}">
          <text>当前场次未开始</text>
        </view>
        <view class="my_tree_produce" style="line-height: 76rpx;" wx:elif="{{arenaInfo.term_id>myBalloonTeamCurrent.term_id || arenaInfo.status=='is_end'}}">
          <text>当前场次已结束</text>
        </view>
        <view class="my_tree_produce" wx:else>
          <text>当前时段投入\n每1级=<text style="color:#E1621B;">{{arenaInfo.current_hour_gen_fruit_num}}{{tool_b_name}}数量</text></text>
        </view> -->
                    <view class="flex_column flex_center">
                        <view class="my_tree_button" style="background: #413781" @tap="gotoFreeCandyDetail" :data-team_id="myBalloonTeamCurrentId">
                            <view class="candy_img2">
                                <image src="https://star-img.xingxiaoculture.com/2022/10/13/bb13aaa577edc16f08ba197eefa72061.png" style="width: 64rpx; height: 56rpx"></image>
                            </view>
                            <text class="my_tree_team_user_text" style="width: 70%; font-size: 11px; text-align: start">
                                免费数量 {{ myBalloonTeamCurrent.free_candy_num }}
                                <text style="text-decoration: underline">查明细</text>
                            </text>
                        </view>
                        <text style="margin-top: 5rpx; font-size: 10px; color: #b1aad9">包含赠送/抢夺/被抢夺，队伍内成员平分</text>
                    </view>
                </view>

                <!-- <view class="my_tree_boss">
        <view class="tree_boss"  wx:if="{{arenaInfo.term_id < myBalloonTeamCurrent.term_id}}">
              <view class="tree_boss_text">
                当前场次未开始
              </view> 
        </view>
        <view class="tree_boss"  wx:elif="{{arenaInfo.term_id>myBalloonTeamCurrent.term_id || treeBossCountdownTimeStr=='当前场次已无BOSS'}}">
              <view class="tree_boss_text">
                当前场次已无BOSS
              </view> 
        </view>
        <view class="tree_boss" wx:elif="{{bossInfo}}">
              <view class="tree_boss_text">
                提升果树等级击败BOSS
              </view> 
              <view class="tree_boss_process">
                  <view class="tree_boss_process_ing" style="width:{{(bossInfo.team_level_in_hour>bossInfo.beat_boss_need_level?bossInfo.beat_boss_need_level:bossInfo.team_level_in_hour)/bossInfo.beat_boss_need_level*186}}rpx"></view>
                  <view class="tree_boss_text4"><text>{{bossInfo.team_level_in_hour < bossInfo.beat_boss_need_level?bossInfo.team_level_in_hour:bossInfo.beat_boss_need_level}}</text>/<text>{{bossInfo.beat_boss_need_level}}</text></view>
              </view> 
        </view>
        <view class="tree_boss" wx:else>
              <view class="tree_boss_text">
                BOSS来袭还剩
              </view> 
              <view class="tree_boss_text2">
                {{treeBossCountdownTimeStr}}
              </view> 
        </view> 
      </view> -->

                <view class="my_tree_img">
                    <view class="my_tree_rule" @tap="gotoTreeRule">玩法规则</view>
                    <image style="width: 750rpx; height: 750rpx" :src="myBalloonTeamCurrent.star_tree_inner"></image>
                    <view class="flex_row">
                        <view class="my_tree_button" @tap="showVotePanel">
                            <view class="candy_img">
                                <image src="https://star-img.xingxiaoculture.com/2022/10/13/bb13aaa577edc16f08ba197eefa72061.png" style="width: 54rpx; height: 48rpx"></image>
                            </view>
                            <view class="flex_column flex_start" style="width: 60%; margin-left: 40rpx">
                                <text class="my_tree_team_user_text" style="width: 100%; font-size: 14px; text-align: start">制作{{ tool_b_name }}</text>
                                <text class="my_tree_team_user_text" style="width: 100%; font-size: 10px; text-align: start">
                                    {{
                                        arenaInfo.term_id < myBalloonTeamCurrent.term_id
                                            ? '当前场次未开始'
                                            : arenaInfo.term_id > myBalloonTeamCurrent.term_id
                                            ? '当前场次已结束'
                                            : actArenaCountdownTimeStr
                                    }}
                                </text>
                            </view>
                        </view>
                        <view class="my_tree_button" @tap="gotoGradCandyPage" :data-team_id="myBalloonTeamCurrent.team_id">
                            <view class="candy_img">
                                <image src="https://star-img.xingxiaoculture.com/2022/10/14/2d6d29aa4b53a7448afb2bc108bc1a77.png" style="width: 54rpx; height: 45rpx"></image>
                            </view>
                            <view class="flex_column flex_start" style="width: 50%; margin-left: 40rpx; text-align: start">
                                <text class="my_tree_team_user_text" style="width: 100%; font-size: 14px; text-align: start">抢夺{{ tool_b_name }}</text>
                            </view>
                        </view>
                    </view>
                    <view class="my_tree_blank_box" v-if="bShowTreeBlank1">
                        <!-- <view wx:if="{{arenaInfo.term_id==myBalloonTeamCurrent.term_id && bossInfo && !bossInfo.beat_boss}}" class="my_tree_blank_normal" style="margin-top: 50rpx;">
            <text style="color: #E36200;">{{treeBossCountdownTimeStr}}后离开</text>
          </view> -->
                        <view class="my_tree_blank_normal">
                            <text @tap="gotoTreeRule" :data-type="1">
                                小豆会赠送10%的免费 {{ tool_b_name }}，只有免费{{ tool_b_name }} 才可被抢夺，
                                <text style="text-decoration: underline; color: #e36200">查看规则</text>
                            </text>
                            <view class="my_tree_blank_dialog_close" @tap="closeTreeBank" data-type="1">X</view>
                        </view>
                        <view class="my_tree_blank_triangle"></view>
                    </view>
                    <view class="my_tree_blank_box" style="top: 650rpx; left: 80rpx" v-if="bShowTreeBlank2">
                        <!-- <view wx:if="{{arenaInfo.term_id==myBalloonTeamCurrent.term_id && bossInfo && !bossInfo.beat_boss}}" class="my_tree_blank_normal" style="margin-top: 50rpx;">
             <text style="color: #E36200;">{{treeBossCountdownTimeStr}}后离开</text>
           </view> -->
                        <view class="my_tree_blank_normal" style="width: 339rpx">
                            <text @tap="gotoTreeRule" :data-type="0">
                                同样数量的{{ tool_s_name }}使用越早， 制作的{{ tool_b_name }}数量越多，
                                <text style="text-decoration: underline; color: #e36200">查看规则</text>
                            </text>
                            <view class="my_tree_blank_dialog_close" @tap="closeTreeBank" data-type="2">X</view>
                        </view>
                        <view class="my_tree_blank_triangle"></view>
                    </view>
                    <!-- <image class="my_tree_blank_boss_s" bindtap="gotoTreeRule" data-type="{{1}}" src="https://star-img.xingxiaoculture.com/2022/09/13/cfcbc464c724cf17710d2cfe9c587e18.png">
        </image> -->
                    <image
                        v-if="arenaInfo.term_id == myBalloonTeamCurrent.term_id && bossInfo && !bossInfo.beat_boss"
                        class="my_tree_blank_boss_l"
                        src="https://star-img.xingxiaoculture.com/2022/09/13/962ae6da940f51cefd8ba6367dbc0c64.gif"
                    ></image>
                    <!-- <view class="my_tree_time_box">
          {{arenaInfo.term_id < myBalloonTeamCurrent.term_id?"当前场次未开始":(arenaInfo.term_id>myBalloonTeamCurrent.term_id?"当前场次已结束":actArenaCountdownTimeStr)}}
        </view> -->
                </view>
                <view class="my_tree_team_box">
                    <view class="my_tree_team">
                        <view class="my_tree_team_title_box">
                            <view style="top: 20rpx; left: 20rpx; line-height: 76rpx; color: #4e419a" :class="'tree_info_icon tree_info_icon' + myBalloonTeamCurrent.rank">
                                {{ myBalloonTeamCurrent.rank > 3 ? myBalloonTeamCurrent.rank : '' }}
                            </view>
                            <view class="my_tree_team_title">
                                {{ myBalloonTeamCurrent.team_name }}
                                <image
                                    v-if="myBalloonTeamCurrent.is_captain"
                                    style="width: 36rpx; height: 34rpx; margin-left: 10rpx"
                                    src="https://star-img.xingxiaoculture.com/2022/10/13/a584acc0c836b59b1336439651f5fad6.png"
                                    @tap="showShortnameSet"
                                    data-type="1"
                                    :data-item="myBalloonTeamCurrent"
                                ></image>
                            </view>
                            <view class="my_tree_team_apply" v-if="myBalloonTeamCurrent.is_captain" @tap="handleTeamApply" data-type="1" :data-item="myBalloonTeamCurrent">
                                申请列表
                                <view class="content_set_cloud_log_point" v-if="myBalloonTeamCurrent.is_captain && myBalloonTeamCurrent.not_agree_num > 0">
                                    {{ myBalloonTeamCurrent.not_agree_num }}
                                </view>
                            </view>
                            <block v-if="arenaInfo.status == 'not_begin' || arenaInfo.term_id < myBalloonTeamCurrent.term_id">
                                <view class="dialog_main_confirm_btn" v-if="myBalloonTeamCurrent.is_captain" :data-team_id="myBalloonTeamCurrentId" @tap="showDismissTeam">
                                    解散
                                </view>
                                <view class="dialog_main_confirm_btn" v-else-if="!myBalloonTeamCurrent.is_captain" :data-team_id="myBalloonTeamCurrentId" @tap="showQuitTeam">
                                    退出
                                </view>
                            </block>
                        </view>

                        <view class="line" style="background-color: #32296d; width: 95%"></view>

                        <view class="my_tree_team_rewardtext2">
                            <text class="magic_attack_text">队伍魔法攻击力 {{ myBalloonTeamCurrent.attack_num }}</text>
                            <text class="magic_defend_text">队伍魔法防御力 {{ myBalloonTeamCurrent.defend_num }}</text>
                        </view>

                        <view class="my_tree_team_rewardtext" style="margin-bottom: 20rpx" v-if="myBalloonTeamCurrent.expect_text">
                            {{ myBalloonTeamCurrent.expect_text }}
                        </view>

                        <view class="my_tree_team_item_outer_box">
                            <view class="my_tree_team_item_tabbar">
                                <view @tap="changeTabSeTem" data-tab="0" class="my_tree_team_item_tabbar_item" :style="tabIndexSeTeam != 0 ? 'color:#9187D1' : ''">
                                    抢夺记录
                                    <view v-if="tabIndexSeTeam == 0" style="width: 54rpx; background-color: #d08bdc; height: 6rpx; margin-top: 5rpx"></view>
                                </view>
                                <view @tap="changeTabSeTem" data-tab="1" class="my_tree_team_item_tabbar_item" :style="tabIndexSeTeam != 1 ? 'color:#9187D1' : ''">
                                    成员信息
                                    <view v-if="tabIndexSeTeam == 1" style="width: 54rpx; background-color: #d08bdc; height: 6rpx; margin-top: 5rpx"></view>
                                </view>
                            </view>
                            <block v-if="tabIndexSeTeam == 0">
                                <block v-if="grabCandyList.length > 0">
                                    <view class="my_tree_team_item" v-for="(item, index) in grabCandyList" :key="index">
                                        <view class="my_tree_team_rewardtext2" style="background-color: #5c4daf; border-radius: 10rpx 10rpx 0rpx 0rpx">
                                            <text class="magic_time_text">● {{ item.time }}</text>
                                        </view>

                                        <view class="my_tree_team_head_box">
                                            <image class="my_tree_team_user_img" :src="item.userinfo.image.middle_pic"></image>
                                            <view class="flex_column flex_center" style="width: 60%; margin-left: 10rpx">
                                                <view class="my_tree_team_user_text" style="width: 100%">{{ item.userinfo.nickname }} 所属队伍【{{ item.team_name }}】</view>
                                                <view class="my_tree_team_user_text" style="width: 100%">{{ pageDataObj.single_user_rank_info_all_time.text }}</view>
                                            </view>
                                            <view
                                                class="my_tree_team_user_button"
                                                :style="item.status ? '	opacity: 0.4;' : ''"
                                                @tap="grabCandy"
                                                :data-team_id="item.status ? '' : item.team_id"
                                            >
                                                {{ item.status ? item.status : '回抢' }}
                                            </view>
                                        </view>

                                        <view class="my_tree_team_rewardtext2" style="background-color: #5c4daf">
                                            <text class="magic_attack_text">队伍魔法攻击力 {{ item.team_attack_num }}</text>
                                            <text class="magic_defend_text">队伍魔法防御力 {{ item.team_defend_num }}</text>
                                        </view>
                                    </view>
                                </block>
                                <block v-else>
                                    <view class="my_tree_team_nodata">
                                        <image
                                            src="https://star-img.xingxiaoculture.com/2022/10/17/7f8e3762dd655fce30a6ab88e5f79eaa.png"
                                            style="width: 221rpx; height: 283rpx"
                                        ></image>
                                    </view>
                                </block>
                            </block>
                            <block v-else>
                                <view class="my_tree_team_item" v-for="(member, index) in myBalloonTeamCurrent.members" :key="index">
                                    <view class="my_tree_team_head_box">
                                        <view class="dialog_tree_info_captain_icon" style="right: 14rpx" v-if="member.is_captain">队长</view>
                                        <image class="my_tree_team_user_img" :src="member.userinfo.image.middle_pic"></image>
                                        <view class="my_tree_team_user_text">{{ member.userinfo.nickname }}</view>
                                        <view class="my_tree_team_user_text">
                                            <text>投入{{ tool_s_name }}\n{{ member.sugar_num }}</text>
                                        </view>

                                        <block v-if="arenaInfo.status == 'not_begin' || arenaInfo.term_id < myBalloonTeamCurrent.term_id">
                                            <view
                                                class="dialog_main_confirm_btn"
                                                style="background-color: #c7c7c7"
                                                v-if="myBalloonTeamCurrent.is_captain && !member.is_captain"
                                                :data-item="member"
                                                :data-team_id="myBalloonTeamCurrent.team_id"
                                                @tap="showGetoutTeam"
                                            >
                                                移除
                                            </view>
                                            <!-- <view class="dialog_main_confirm_btn" style="background-color: #C7C7C7;" wx:elif="{{!myBalloonTeamCurrent.is_captain && user_info._id==member.userinfo._id}}" data-team_id="{{myBalloonTeamCurrentId}}" bindtap="showQuitTeam">退出</view> -->
                                        </block>
                                        <block v-else>
                                            <view class="my_tree_team_user_text" style="color: #fbc85d; width: 28%" v-if="member.level_percent">
                                                制作占比{{ member.level_percent }}
                                            </view>
                                        </block>
                                    </view>

                                    <view class="my_tree_team_rewardtext2" style="background-color: #5c4daf">
                                        <text class="magic_attack_text">魔法攻击力 {{ member.attack_num }}</text>
                                        <view
                                            class="my_tree_team_user_add"
                                            v-if="user_info._id == member.userinfo._id"
                                            style="transform: scale(0.5)"
                                            data-open_type="magic"
                                            :data-team_id="myBalloonTeamCurrentId"
                                            @tap="gotoGradCandyPage"
                                        >
                                            +
                                        </view>
                                        <text class="magic_defend_text">魔法防御力 {{ member.defend_num }}</text>
                                        <view
                                            class="my_tree_team_user_add"
                                            v-if="user_info._id == member.userinfo._id"
                                            style="transform: scale(0.5)"
                                            data-open_type="magic"
                                            :data-team_id="myBalloonTeamCurrentId"
                                            @tap="gotoGradCandyPage"
                                        >
                                            +
                                        </view>
                                    </view>

                                    <view class="my_tree_team_rewardtext" v-if="member.expect_text || member.text">
                                        <text>{{ member.text }}{{ member.text ? '\n' : '' }}{{ member.expect_text }}</text>
                                    </view>
                                </view>
                                <view class="my_tree_team_item" v-if="myBalloonTeamCurrent.members.length < 5">
                                    <button class="my_tree_team_user_text2" open-type="share" :data-team_id="myBalloonTeamCurrentId">
                                        <view class="my_tree_team_user_add">+</view>
                                        邀请他人加入队伍
                                    </button>
                                </view>
                            </block>
                        </view>
                    </view>
                </view>
                <view style="height: 150rpx"></view>
            </scroll-view>
            <!-- <view style="height: 1rpx;"></view>
    <view class="scrollBoxA" wx:if="{{noData == 0}}">暂无数据哦～</view>
    <scroll-view scroll-y="{{true}}" bindscrolltolower="lower" class="scrollBox" wx:else>
      <view class="tipBox" >通过申请后，在当前场次的抢占擂台中无法踢出队员，请谨慎通过</view>
      <view class="xingqiuItem" wx:for="{{applyUserList}}" wx:key="index">
          <view class="group_user_item">
            <image class="group_user_item_image" src="{{item.userinfo.image.middle_pic}}"></image>
            <view class="group_user_item_title" style="width: 180rpx;">{{item.userinfo.nickname}}</view>
            <view class="group_user_item_level">已送出{{item.user_votetimes_num}}{{tool_s_unit}}</view>
            <view class="group_user_item_button_agree" wx:if="{{item.status==0}}" bindtap="agreeJoinBoxShow" data-reqid="{{item.req_id}}" data-obj="{{item}}">同意</view>
            <view class="group_user_item_button_done" wx:elif="{{item.status==2}}">无法加入</view>
            <view class="group_user_item_button_done" wx:else>已通过</view>
          </view>
      </view>
      <view class="jiazai">{{wenanType == 0?'':wenanType == 1?'已展示全部数据':'已展示全部数据'}}</view>
      <view style="height: 200rpx;"></view>
    </scroll-view> -->
        </view>

        <view class="contentBox" v-if="tabIndex == 3">
            <view style="height: 1rpx"></view>
            <scroll-view :scroll-y="true" @scrolltolower="lower" class="scrollBox">
                <image :class="'topbg topbg_' + index" mode="widthFix" :src="item" v-for="(item, index) in imgList" :key="index"></image>
                <view style="height: 200rpx"></view>
            </scroll-view>
        </view>

        <!-- 个人状态栏2 -->
        <!-- <view class="user_msg_view">
      <view class="user_msg_view_box_upper">
        <view class="{{tabIndex == 0?'content_set_cloud_button_on':'content_set_cloud_button'}}" bindtap="changeTab" data-tab="0">万圣小镇</view>
        <view class="{{tabIndex == 1?'content_set_cloud_button_on':'content_set_cloud_button'}}" bindtap="changeTab" data-tab="1">糖果屋</view>
        <view class="{{tabIndex == 2?'content_set_cloud_button_on':'content_set_cloud_button'}}" bindtap="changeTab" data-tab="2">队伍排名</view>
      </view>
    </view> -->

        <view class="dialog_view" v-if="bTipsWindowShow">
            <view class="dialog_main_view">
                <view class="dialog_close">
                    <image class="dialog_close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png" @tap="dialogCloseClick"></image>
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
                                    :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                    :src="
                                        joinTeamInfo.members[0].userinfo.image.thumbnail_pic
                                            ? joinTeamInfo.members[0].userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                    "
                                    @tap="gotoDressRoom"
                                    :data-userid="joinTeamInfo.members[0].userinfo._id"
                                ></image>
                                <image
                                    :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                    :src="
                                        joinTeamInfo.members[1].userinfo.image.thumbnail_pic
                                            ? joinTeamInfo.members[1].userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                    "
                                    @tap="gotoDressRoom"
                                    :data-userid="joinTeamInfo.members[1].userinfo._id"
                                ></image>
                                <image
                                    :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                    :src="
                                        joinTeamInfo.members[2].userinfo.image.thumbnail_pic
                                            ? joinTeamInfo.members[2].userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                    "
                                    @tap="gotoDressRoom"
                                    :data-userid="joinTeamInfo.members[2].userinfo._id"
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
                        <view class="dialog_main_cancel_btn" @tap="dialogCloseClick">取消</view>
                        <view class="dialog_main_confirm_btn" @tap="joinTeam">确认申请</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 1">
                    <view class="dialog_text01">同意当前用户加入队伍</view>
                    <text class="dialog_main_stop_data">制作{{ tool_b_name }}开始后则不可踢出或退出，请谨慎通过</text>
                    <view class="dialog_teaminfo" v-if="applyUserinfo">
                        <view class="group_user_item">
                            <image class="group_user_item_image" :src="applyUserinfo.userinfo.image.middle_pic"></image>
                            <view class="group_user_item_title" style="width: 180rpx">{{ applyUserinfo.userinfo.nickname }}</view>
                            <view class="group_user_item_level" style="font-size: 12px">已送出{{ applyUserinfo.user_votetimes_num }}{{ tool_b_unit }}</view>
                        </view>
                    </view>
                    <view class="dialog_button_box">
                        <view class="dialog_main_cancel_btn" @tap="dialogCloseClick">取消</view>
                        <view class="dialog_main_confirm_btn" @tap="agreeJoin">同意</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 2">
                    <view class="dialog_text01">加入队伍成功</view>
                    <text class="dialog_main_stop_data" style="text-align: center">成功加入{{ myBalloonTeamCurrent.team_name }}，快去制作{{ tool_b_name }}获得福利吧~</text>
                    <text class="dialog_main_stop_data" style="text-align: center; margin-top: 0rpx">
                        <text>获得组队奖励：\n < text style="color: #8465FF;">{{ tool_s_name }}*200（将在制作{{ tool_b_name }}开始以后自动帮您制作）</text>
                    </text>
                    <view class="dialog_teaminfo" v-if="myBalloonTeamCurrentId">
                        <!-- <view
              class="scrollOrder {{myBalloonTeam.rank==-1?'scrollNo':myBalloonTeam.rank==1 ?'scrollOne':myBalloonTeam.rank==2?'scrollTwo':myBalloonTeam.rank==3?'scrollThree':'' }}">
              {{myBalloonTeam.rank==-1?"未上榜":myBalloonTeam.rank}}
            </view> -->
                        <image class="tree_img" :src="myBalloonTeamCurrent.star_tree"></image>
                        <view class="m11" style="width: 280rpx; margin-left: 20rpx">
                            <view>
                                <image
                                    :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                    :src="
                                        myBalloonTeamCurrent.members[0].userinfo.image.thumbnail_pic
                                            ? myBalloonTeamCurrent.members[0].userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                    "
                                    @tap="gotoDressRoom"
                                    :data-userid="myBalloonTeamCurrent.members[0].userinfo._id"
                                ></image>
                                <image
                                    :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                    :src="
                                        myBalloonTeamCurrent.members[1].userinfo.image.thumbnail_pic
                                            ? myBalloonTeamCurrent.members[1].userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                    "
                                    @tap="gotoDressRoom"
                                    :data-userid="myBalloonTeamCurrent.members[1].userinfo._id"
                                ></image>
                                <image
                                    :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                    :src="
                                        myBalloonTeamCurrent.members[2].userinfo.image.thumbnail_pic
                                            ? myBalloonTeamCurrent.members[2].userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                    "
                                    @tap="gotoDressRoom"
                                    :data-userid="myBalloonTeamCurrent.members[2].userinfo._id"
                                ></image>
                                <image
                                    :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                    :src="
                                        myBalloonTeamCurrent.members[3].userinfo.image.thumbnail_pic
                                            ? myBalloonTeamCurrent.members[3].userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                    "
                                    @tap="gotoDressRoom"
                                    :data-userid="myBalloonTeamCurrent.members[3].userinfo._id"
                                ></image>
                                <image
                                    :class="'m11Pic ' + (index == 0 ? 'm11Pic1' : index == 1 ? 'm11Pic2' : 'm11Pic3')"
                                    :src="
                                        myBalloonTeamCurrent.members[4].userinfo.image.thumbnail_pic
                                            ? myBalloonTeamCurrent.members[4].userinfo.image.thumbnail_pic
                                            : 'https://star-img.xingxiaoculture.com/2022/08/10/2ae36e2e559cd8dc0f1b5254a54ffd26.png'
                                    "
                                    @tap="gotoDressRoom"
                                    :data-userid="myBalloonTeamCurrent.members[4].userinfo._id"
                                ></image>
                            </view>
                        </view>
                        <view class="group-data-info">
                            <view class="group-title1">{{ myBalloonTeamCurrent.team_name }}</view>
                            <!-- <view class="group-num-info1">{{myBalloonTeamCurrent.need_votetimes>0?("需要"+myBalloonTeamCurrent.need_votetimes+"果肥"):"无门槛"}}</view> -->
                        </view>
                    </view>
                    <view class="dialog_button_box">
                        <view class="dialog_main_confirm_btn2" @tap="dialogCloseClick">知道了</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 3">
                    <view class="dialog_text01">创建队伍成功</view>
                    <text class="dialog_main_stop_data2">
                        <text v-if="bTipsWindowShowType3ShowPrice">
                            获得组队奖励：\n < text style="color: #8465FF;">{{ tool_s_name }}*200（将在制作{{ tool_b_name }}开始以后自动帮您制作）
                        </text>
                        \n
                    </text>
                    <text v-else>当日获得创建奖励已达上限\n无法再获得奖励\n</text>
                    <text class="dialog_main_stop_data2" style="margin-top: 20rpx">快去邀请好友一起制作{{ tool_b_name }}吧~</text>
                    <view class="dialog_button_box">
                        <view class="dialog_main_confirm_btn2" @tap="dialogCloseClick">知道了</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 4">
                    <view class="dialog_text01">切换队伍</view>
                    <text class="dialog_main_stop_data2">只展示未开始和进行中的队伍</text>
                    <view class="team_select_item" @tap="selectTeam" :data-team_id="item.my_team.team_id" v-for="(item, index) in myBalloonTeam" :key="index">
                        <view class="team_select_item_icon_selected'" v-if="item.my_team.team_id == myBalloonTeamCurrentId">√</view>

                        <view class="team_select_item_icon'" v-else></view>

                        <view class="team_select_item_name">{{ item.my_team.team_name }}</view>

                        <view class="dialog_tree_info_captain_icon">{{ item.my_team.term_id }}</view>
                    </view>
                    <view class="team_select_item" @tap="selectTeam" :data-team_id="item.my_team.team_id" v-for="(item, index) in myBalloonTeamNext" :key="index">
                        <view class="team_select_item_icon_selected'" v-if="item.my_team.team_id == myBalloonTeamCurrentId">√</view>

                        <view class="team_select_item_icon'" v-else></view>

                        <view class="team_select_item_name">{{ item.my_team.team_name }}</view>

                        <view class="dialog_tree_info_captain_icon">{{ item.my_team.term_id }}</view>
                    </view>
                    <view
                        class="team_select_item_create_team"
                        v-if="
                            (arenaInfo.status != 'is_end' && myBalloonTeam.length < 3) || (arenaInfo.status == 'is_end' && arenaInfo.next_term_id && myBalloonTeamNext.length < 3)
                        "
                        @tap="showBalloonCreateTeamPanel"
                    >
                        +创建队伍
                    </view>
                    <view class="dialog_button_box">
                        <view class="dialog_main_confirm_btn2" @tap="confirmSelectTeam">确定</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 5">
                    <view class="dialog_text01">是否解散当前队伍</view>
                    <text class="dialog_main_stop_data2">解散后队伍内成员将退出当前队伍\n请问是否解散队伍？</text>
                    <view class="dialog_button_box">
                        <view class="dialog_main_cancel_btn" @tap="dialogCloseClick">取消</view>
                        <view class="dialog_main_confirm_btn3" @tap="dismissTeam">同意</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 6">
                    <view class="dialog_text01">是否退出当前队伍</view>
                    <text class="dialog_main_stop_data2">退出队伍后将无法撤回该操作\n请问是否退出当前队伍？</text>
                    <view class="dialog_button_box">
                        <view class="dialog_main_cancel_btn" @tap="dialogCloseClick">取消</view>
                        <view class="dialog_main_confirm_btn3" @tap="quitTeam">同意</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 7">
                    <view class="dialog_text01">是否踢出当前成员</view>
                    <view class="dialog_teaminfo" style="flex-direction: column; margin-top: 20rpx" v-if="getoutMember">
                        <view class="group_user_item">
                            <image class="group_user_item_image" :src="getoutMember.userinfo.image.middle_pic"></image>
                            <view class="group_user_item_title" style="width: 180rpx">{{ getoutMember.userinfo.nickname }}</view>
                            <view class="group_user_item_level" style="font-size: 12px">已送出{{ getoutMember.user_votetimes_num }}{{ tool_b_unit }}</view>
                        </view>
                        <view class="group_user_item2" style="color: #5c4daf; border-radius: 0 0 14rpx 14rpx">
                            <text class="magic_attack_text">魔法攻击力 {{ getoutMember.attack_num }}</text>
                            <text class="magic_defend_text">魔法防御力 {{ getoutMember.defend_num }}</text>
                        </view>
                    </view>
                    <text class="dialog_main_stop_data2">踢出该成员后将无法撤回该操作\n请问是否踢出当前成员</text>
                    <view class="dialog_button_box">
                        <view class="dialog_main_cancel_btn" @tap="dialogCloseClick">取消</view>
                        <view class="dialog_main_confirm_btn3" @tap="getoutTeam">确定</view>
                    </view>
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
                        <view class="dialog_main_confirm_btn2" @tap="dialogCloseClick">制作{{ tool_b_name }}</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 9">
                    <image style="margin-top: 20rpx; width: 188rpx; height: 240rpx" src="https://star-img.idol001.com/2022/09/15/c9056ba3145f1d6f5de0a7f37263bb94.png"></image>
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
                        <view class="dialog_main_confirm_btn2" @tap="dialogCloseClick">我知道了</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 10">
                    <image style="margin-top: 20rpx; width: 235rpx; height: 240rpx" :src="showTreeInfoObj.star_tree"></image>
                    <text class="dialog_tree_info_teamname">{{ showTreeInfoObj.team_name }}</text>
                    <view class="dialog_tree_info_slogan">
                        <image
                            style="width: 80rpx; height: 80rpx; position: absolute; left: 0; top: -30rpx"
                            src="https://star-img.xingxiaoculture.com/2022/10/17/5412b2150f88c098ba891879f217096d.png"
                        ></image>
                        <image
                            v-if="showTreeInfoObj.in_team"
                            @tap="showShortnameSet"
                            :data-item="showTreeInfoObj"
                            data-type="2"
                            style="width: 31rpx; height: 30rpx; position: absolute; right: 5rpx; top: 8rpx"
                            src="https://star-img.xingxiaoculture.com/2022/10/17/ea41c097998ef380a4aeb40601cf3c4e.png"
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
                    <view class="dialog_main_confirm_btn2" @tap="grabCandy" v-if="showTreeInfoObj.team_id != myBalloonTeamCurrentId" :data-team_id="showTreeInfoObj.team_id">
                        抢夺{{ tool_b_name }}
                    </view>
                    <view style="margin-bottom: 20rpx"></view>
                </block>
                <block v-else-if="bTipsWindowShowType == 11">
                    <image style="width: 670rpx; height: 212rpx" src="https://star-img.xingxiaoculture.com/2022/10/10/7e43db05a8adb7cab393bef454939f12.png"></image>
                    <scroll-view :scroll-y="true">
                        <view class="get_item_box">
                            <view class="dialog_text01">去偷星</view>
                            <view class="dialog_text02">偷星成功后，可以随机掉{{ tool_s_name }}，更有偷星机器人加入，更多玩法等你来~</view>
                            <view class="dialog_text_btn" :data-type="0" @tap="dialogGetBookClick">立即偷星</view>
                            <view class="dialog_text01">去送星</view>
                            <view class="dialog_text02">单次送星达到300颗，即可获得1{{ tool_s_unit }}{{ tool_s_name }}，可以累加，快去为星球送守护星获得吧～</view>
                            <view class="dialog_text_btn" :data-type="1" @tap="dialogGetBookClick">立即送星</view>
                            <view class="dialog_text01">公会币购买</view>
                            <view class="dialog_text02">使用公会币直接购买{{ tool_s_name }}</view>
                            <view class="dialog_text_btn" data-open_type="sugar" :data-team_id="myBalloonTeamCurrentId" @tap="gotoGradCandyPage">去购买</view>
                        </view>
                    </scroll-view>
                    <view style="margin-bottom: 20rpx"></view>
                </block>
                <block v-else-if="bTipsWindowShowType == 12">
                    <view class="dialog_text01">抢夺成功</view>
                    <text class="dialog_main_stop_data2">
                        本次成功抢夺{{ grabResult.team_name }}免费{{ tool_b_name }}的
                        <text style="color: #8465ff">{{ grabResult.grab_candy_percent }}\n共计{{ tool_b_name }}{{ grabResult.grab_candy_num }}{{ tool_b_unit }}</text>
                    </text>
                    <text class="dialog_main_stop_data2" style="margin-top: 20rpx">
                        该{{ tool_b_name }}将进入我方队伍的免费{{ tool_b_name }}中，也可以被其他队伍抢夺，本场制作{{ tool_b_name }}结束后到账
                    </text>
                    <view class="dialog_button_box">
                        <view class="dialog_main_confirm_btn2" @tap="dialogCloseClick">知道了</view>
                    </view>
                </block>
                <block v-else-if="bTipsWindowShowType == 13">
                    <image style="width: 670rpx; height: 212rpx" src="https://star-img.xingxiaoculture.com/2022/10/10/7e43db05a8adb7cab393bef454939f12.png"></image>
                    <view class="dialog_text01">{{ grabResult.title }}</view>
                    <text class="dialog_main_stop_data2" style="margin-top: 20rpx">{{ grabResult.content }}</text>

                    <view class="dialog_main_confirm_btn2" @tap="gotoGradCandyPage" data-open_type="grab_times" :data-team_id="myBalloonTeamCurrentId" v-if="grabResult.code == 2">
                        获取抢夺次数
                    </view>
                    <view class="dialog_main_confirm_btn2" @tap="gotoGradCandyPage" data-open_type="magic" :data-team_id="myBalloonTeamCurrentId" v-if="grabResult.code == 3">
                        提升魔法攻击力
                    </view>
                    <view class="dialog_main_cancel_btn2" @tap="dialogCloseClick">知道了</view>

                    <view style="margin-bottom: 20rpx"></view>
                </block>
            </view>
        </view>
        <!-- 设置公会简称弹窗 -->
        <view class="dialog_view flex-c_center" v-if="isShowDialogShortName">
            <view class="img-bg-tips-area3">
                <view class="content_shortname_set_title">
                    {{ bShowDialogShortNameType == 1 ? '修改队伍名称' : '修改队伍口号' }}
                </view>
                <input
                    class="content_shortname_set_input"
                    type="text"
                    maxlength="30"
                    :placeholder="bShowDialogShortNameType == 1 ? '请输入队伍名称，6个字符以内' : '请输入队伍口号，20个字符以内'"
                    :value="bShowDialogShortNameType == 1 ? showTreeInfoObj.team_name : showTreeInfoObj.slogan"
                    @input="inputShortName"
                />
                <view class="content_shortname_set_button_box">
                    <view class="content_shortname_set_button_cancel" @tap="closeAreaInfo">取消</view>
                    <view class="content_shortname_set_button_confirm" @tap="updateShortnameSet">保存</view>
                </view>
            </view>
            <view class="dialog_close">
                <image class="dialog_close" src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png" @tap="closeAreaInfo"></image>
            </view>
        </view>

        <!-- 放气球弹窗 -->
        <vote_panel
            v-if="bShowVotePanel"
            :votePanelViewType="votePanelViewType"
            :votePanelPlaceholder="votePanelPlaceholder"
            :votePanelStar="votePanelStar"
            :myBalloonTeam="myBalloonTeamCurrent"
            :arenaInfo="arenaInfo"
            @voteCallback="voteCallback"
            @closeViewEvent="closeVoteView"
            @notEnoughEvent="showGetGuofei"
        ></vote_panel>
        <vote_panel_balloon_record v-if="bShowBalloonRecordPanel" @closeViewEvent="hideBalloonRecordPanel"></vote_panel_balloon_record>
        <vote_panel_balloon_rule v-if="bShowBalloonRulePanel" @closeViewEvent="hideBalloonRulePanel"></vote_panel_balloon_rule>
        <vote_panel_balloon_create_team
            v-if="bShowBalloonCreateTeamPanel"
            @closeViewEvent="hideBalloonCreateTeamPanel"
            @createTeamResultEvent="createTeamResult"
            :arenaInfo="arenaInfo"
        ></vote_panel_balloon_create_team>
    </view>
</template>

<script>
import dialog_end from '../../component/7s/7s_dialog/dialog_end';
import vote_panel_balloon_record from '../../component/6s/vote_panel_balloon_record';
import vote_panel_balloon_rule from '../../component/6s/vote_panel_balloon_rule';
import vote_panel_balloon_create_team from '../../component/6s/vote_panel_balloon_create_team';
import vote_panel from '../../component/6s/vote_panel';
var net = require('../../../commonscript/common/netLoad.js');
var IDLoginManager = require('../../../commonscript/common/IDLoginManager.js');
var globalData = require('../../../commonscript/common/globalData.js');
var utils = require('../../../utils/util.js');
var app = getApp();
var urlX = 'https://data.idol001.com/';
// 1上半场 2下半场
var type = 1;
//测试带
export default {
    components: {
        dialog_end,
        vote_panel_balloon_record,
        vote_panel_balloon_rule,
        vote_panel_balloon_create_team,
        vote_panel
    },
    data() {
        return {
            main_activity_id: '202305S',
            main_activity_id_api: '202305s',
            tool_s_name: '贝币',
            tool_s_unit: '个',
            tool_b_name: '海洋之心',
            tool_b_unit: '个',
            user_info: globalData.idolUserInfo,
            rankTip: '',
            noData: 0,

            // 0没有数据  1有数据
            wenanType: 0,

            //0没有 1已加载全部数据 2只显示前100
            time: 0,

            //0未开始，1上半场，2下半场，3已结束
            is_main: 0,

            //0不是主群 1是主群
            text: '',

            //顶部描述
            title: '',

            //顶部标题
            listDisplay: 0,

            //列表是否显示
            tabIndex: 2,

            //选中态
            tabIndexSe: 0,

            //列表二级选中态
            tabIndexSeTeam: 0,

            //我的队伍二级选中态
            myBalloonTeamByTerm: [],

            //我的气球小队根据termid变化
            myBalloonTeamByTermDivide: {
                1: 0,
                2: 0,
                3: 0
            },

            //我的气球小队分场次数量
            myBalloonTeam: [],

            //我的气球小队
            myBalloonTeamNext: [],

            //我的气球小队下一场
            myBalloonTeamCurrentId: null,

            //我当前的小队id
            myBalloonTeamCurrent: {
                team_name: '',
                team_fruit: '',
                star_tree_inner: '',
                term_id: 0,
                team_id: '',
                rank: '',
                is_captain: '',
                not_agree_num: 0,
                attack_num: '',
                defend_num: '',
                expect_text: '',
                members: [],
                star_tree: ''
            },

            //当前的队伍信息
            bossInfo: null,

            //当前BOSS信息
            isCaptain: 0,

            //是否隊長
            allBalloonteam: [],

            //气球小队列表
            userMine: null,

            //我的反超数据
            allBalloonteamTree: [],

            //果园
            canJoinBalloonteam: [],

            //可加入的气球小队列表
            applyUserList: [],

            //小队申请列表
            applyReqid: null,

            //申请id
            applyUserinfo: null,

            //申请用户信息
            bTipsWindowShow: false,

            //提示窗显示
            bTipsWindowShowType: 13,

            //提示窗类型
            bTipsWindowShowType3ShowPrice: false,

            //创建队伍成功是否有奖励
            bShowBalloonCreateTeamPanel: false,

            bShowTreeBlank1: false,

            //展示提示1
            bShowTreeBlank2: false,

            //展示提示2
            exceedList: [],

            //放气球/点亮星球
            bShowVotePanel: false,

            bShowBalloonRecordPanel: false,
            votePanelViewType: 1,
            votePanelPlaceholder: {},
            votePanelStar: {},
            bShowBalloonRecordPanel: false,
            bShowBalloonRulePanel: false,
            transX: 0.3,

            //祝福语初始位置
            openCloud: false,

            //是不是进来就要打开编织云彩
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
            joinTeamid: null,

            //申请加入队伍id
            joinTeamInfo: null,

            //申请加入队伍信息
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

                user_votetimes_num: '',
                attack_num: '',
                defend_num: ''
            },

            //移除成员信息
            showTreeInfoObj: null,

            //果林树展示信息
            grabCandyList: [],

            //糖果抢夺记录
            grabResult: {
                grab_candy_percent: '',
                grab_candy_num: '',
                title: '',
                content: '',
                code: 0
            },

            //抢夺结果
            levelTab: 0,

            //1初级场；2中级场；3高级场
            location: -1,

            //-1全部；0-4分位置
            exceedUserRank: [],

            //反超用户列表

            isShowDialogShortName: false,

            //简称设置
            bShowDialogShortNameType: 1,

            //1是队伍名称；2是队伍口号

            imgList: [
                'https://star-img.xingxiaoculture.com/2022/08/13/0bc5a51f86cd417a832e44bbc4393c02.png',
                'https://star-img.xingxiaoculture.com/2022/08/13/72f5f1bc6a2bda9524b1e10f25c8f486.png',
                'https://star-img.xingxiaoculture.com/2022/08/16/c0813f217d9e9f4e6234ae5054ee7bc4.png'
            ],

            list: [
                {
                    rank: 1,
                    votes: 1,
                    content: '',
                    //
                    gid: '',
                    starid: '',
                    name: '',
                    group_medal_img: '',
                    group_emblem_img: '',
                    img_arr: [
                        {
                            thumbnail_pic: ''
                        },
                        {
                            thumbnail_pic: ''
                        },
                        {
                            thumbnail_pic: ''
                        }
                    ]
                }
            ],

            list1: [
                {
                    rank: 1,
                    votes: 1,
                    content: '',
                    //
                    gid: '',
                    starid: '',
                    name: '',
                    group_medal_img: '',
                    group_emblem_img: '',
                    img_arr: [
                        {
                            thumbnail_pic: ''
                        },
                        {
                            thumbnail_pic: ''
                        },
                        {
                            thumbnail_pic: ''
                        }
                    ]
                }
            ],

            optionObj: {},

            //擂台时间
            timeoutLimitArena: 0,

            intervalTimeoutActArena: null,

            //树BOSS
            timeoutLimitTreeBoss: 0,

            intervalTimeoutTreeBoss: null,

            //祝福语轮播
            intervalTimeoutWordRoll: null,

            actArenaCountdownTimeStr: '',
            treeBossCountdownTimeStr: '',
            treeBoss: '',
            isShowDialog: false,
            bShowLoginPanel: false,

            member: {
                userinfo: {
                    image: {
                        thumbnail_pic: false,
                        middle_pic: ''
                    },

                    _id: '',
                    nickname: ''
                },

                is_captain: '',
                sugar_num: '',
                level_percent: '',
                attack_num: '',
                defend_num: '',
                expect_text: '',
                text: ''
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

            citem: {
                highlight: 0,
                text: ''
            },

            userinfo: {
                image: {
                    thumbnail_pic: false,
                    middle_pic: ''
                },

                _id: '',
                nickname: ''
            },

            image: {
                middle_pic: ''
            },

            nickname: ''
        };
    },
    onLoad: function (options) {
        this.setData({
            optionObj: options
        });
        console.log('type', options.type);
        if (options.type) {
            type = options.type;
            this.setData({
                tabIndex: type ? type : 0
            });
        }
        console.log('location', options.location);
        if (typeof options.location != 'undefined') {
            location = options.location;
            this.setData({
                location: location
            });
        }
        if (options.action == 'create_team') {
            this.showBalloonCreateTeamPanel();
        }
        //传过来的队伍id
        if (options.team_id) {
            this.setData({
                myBalloonTeamCurrentId: options.team_id
            });
        }
        //传过来的场次id
        if (options.term_id) {
            this.setData({
                balloonTermId: options.term_id
            });
        }
        if (options.level_tab) {
            this.setData({
                levelTab: options.level_tab
            });
        }
        if (this.levelTab) {
            uni.setNavigationBarTitle({
                title: '反超登顶列表'
            });
        }

        // if(!wx.getStorageSync(this.data.main_activity_id+"_blanktree1")){
        //   this.setData({
        //     bShowTreeBlank1:true
        //   })
        // }
        // if(!wx.getStorageSync(this.data.main_activity_id+"_blanktree2")){
        //   this.setData({
        //     bShowTreeBlank2:true
        //   })
        // }
        this.initPage();
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
    onPullDownRefresh: function () {},
    /**
     * 页面上拉触底事件的处理函数
     */
    onReachBottom: function () {},
    //用户点击右上角分享
    onShareAppMessage: function (res) {
        if (res.target) {
            let team_id = res.target.dataset.team_id;
        } else {
            let team_id = '';
        }
        if (res.from === 'button') {
            return {
                title: '加入我的队伍，一起水晶海之战得超值福利~',
                path: '/subpage_v3/pages/202305S/activity_team_joinlist?team_id=' + team_id,
                imageUrl: 'https://star-img.xingxiaoculture.com/2023/05/18/770aa32fac270aa8ce277801d9dd4e18.png'
            };
        } else {
            return {
                title: '【海岛音乐节，夏日畅想曲】活动进行中~',
                path: '/subpage_v3/pages/202305S/activity_formal',
                imageUrl: 'https://star-img.xingxiaoculture.com/2023/05/18/770aa32fac270aa8ce277801d9dd4e18.png'
            };
        }
    },
    methods: {
        /**
         * 生命周期函数--监听页面加载
         */
        lower: function (e) {
            console.log('下滑');
        },

        details1(e) {
            console.log('粉丝群解锁详情');
            let query = e.currentTarget.dataset['info'];
            uni.navigateTo({
                url: `./7s_fansdetails?type=${type}&gid=${query.gid}&starid=${query.starid}&is_main=${query.is_main}&name=${query.name}`
            });
        },

        callbackRefresh() {
            // 更新信息
            this.get_group_rank();
            this.get_time();
        },

        get_group_rank() {
            //获取用户主群（设置星球）
            var oThis = this;
            var url = urlX + 'api_2021_01s.php?action=group_rank&is_inner=1';
            var params = {
                is_inner: 1,
                type: type
            };
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        let data = resp.data;
                        this.setData({
                            list: data.user_group,
                            list1: data.list,
                            text: data.text
                        });
                        if (this.list.length > 0) {
                            this.setData({
                                is_main: 1
                            });
                        }
                        if (this.list1.length > 0) {
                            this.setData({
                                noData: 1
                            });
                        }
                        if (this.list1.length <= 50) {
                            this.setData({
                                wenanType: 1
                            });
                        } else {
                            this.setData({
                                wenanType: 2
                            });
                        }
                    } else {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
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

        get_time() {
            this.setData({
                rankTip: '公会排名总榜'
            });
            return;
            //活动时间
            var oThis = this;
            var url = urlX + 'api_activity_2022_01s.php?action=get_time';
            var params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    if (resp && resp.data) {
                        let data = resp.data;
                        let { type } = this.optionObj;
                        this.setData({
                            rankTip: data.title_star,
                            time: data.type
                        });
                        if (type == 1) {
                            //上半场
                            this.setData({
                                rankTip: data.title_group_first
                            });
                        } else if (type == 2 || type == 3) {
                            //下半场 已结束
                            this.setData({
                                rankTip: data.title_group_second
                            });
                        }
                    } else {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
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

        initActArenaInterval: function () {
            var that = this;
            var t = that.timeoutLimitArena - new Date().getTime() / 1000;
            var et = that.timeChange(t);
            if (that.arenaInfo.status == 'not_begin') {
                that.setData({
                    actArenaCountdownTimeStr: '还剩' + et + '开始'
                });
            } else {
                that.setData({
                    actArenaCountdownTimeStr: '还剩' + et + '结束'
                });
            }
            that.intervalTimeoutActArena = setInterval(function () {
                var t = that.timeoutLimitArena - new Date().getTime() / 1000;
                if (t >= 0) {
                    //if(!oThis.data.balloonTermId || oThis.data.balloonTermId==oThis.data.arenaInfo.term_id){
                    var et = that.timeChange(t);
                    if (that.arenaInfo.status == 'not_begin') {
                        that.setData({
                            actArenaCountdownTimeStr: '还剩' + et + '开始'
                        });
                    } else {
                        that.setData({
                            actArenaCountdownTimeStr: '还剩' + et + '结束'
                        });
                    }
                    // }else{
                    //   oThis.setData({ actArenaCountdownTimeStr: '当前场次已结束'  });
                    // }
                }

                if (that.timeoutLimitArena != 0 && new Date().getTime() / 1000 >= that.timeoutLimitArena) {
                    //oThis.setData({isShowDialog: true, showDialogType: 5})
                    that.getArenaInfo();
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
                    treeBossCountdownTimeStr: et
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
                            treeBossCountdownTimeStr: et
                        });
                    }
                }
                if (that.timeoutLimitTreeBoss != 0 && new Date().getTime() / 1000 >= that.timeoutLimitTreeBoss) {
                    that.getArenaInfo();
                }
            }, 998);
        },

        initWordRollInterval: function () {
            var that = this;
            var t = that.transX;
            that.intervalTimeoutWordRoll = setInterval(function () {
                var t = that.transX - 0.02;
                if (t < -1) {
                    var t = 0.3;
                }
                that.setData({
                    transX: t
                });
            }, 100);
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
                        //balloonTermId:resp.data.term_list[0].term_id,
                        //balloonTermDate:resp.data.term_list[0].datetime,
                        arenaInfo: resp.data
                    });
                    if (oThis.balloonTermId == '') {
                        oThis.setData({
                            balloonTermId: resp.data.term_list[0].term_id,
                            balloonTermDate: resp.data.term_list[0].datetime
                        });
                    }
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
                    //       oThis.getBossInfo();
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
                    } else if (resp.data.status == 'during_challenge') {
                        oThis.timeoutLimitArena = resp.data.end_time - resp.data.current_time + new Date().getTime() / 1000;
                        oThis.initActArenaInterval();
                    } else {
                        oThis.setData({
                            actArenaCountdownTimeStr: '本场已结束'
                        });
                    }
                    //oThis.getArenaRank();
                    this.getMyBalloonTeam();
                }
            });
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
            } else {
                return '--';
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
            if (d != 0) {
                var last_time = d + '天';
            } else {
                var last_time = '';
            }
            last_time += h + '时' + m + '分' + s + '秒';
            return last_time;
        },

        initPage() {
            this.get_time();
            this.getArenaInfo();
        },

        switchTab() {
            if (this.tabIndex == 0) {
                this.getAllBalloonTeamTree();
            } else if (this.tabIndex == 1) {
                if (this.myBalloonTeamCurrentId) {
                    this.getMyBalloonTeamCurrent();
                    this.getGrabCandyList();
                }
            } else if (this.tabIndex == 2) {
                this.getMyBalloonTeamByTerm();
                this.getAllBalloonTeam();
                //this.getApplyList()
            }
        },

        getBossInfo() {
            //BOSS
            return;
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php';
            const params = {
                action: 'get_boss_info',
                team_id: this.myBalloonTeamCurrentId
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        bossInfo: resp.data
                    });
                    if (oThis.bossInfo.beat_boss == 0 && oThis.bossInfo.boss_id && !uni.getStorageSync(oThis.myBalloonTeamCurrentId + '_meetboss_' + oThis.bossInfo.boss_id)) {
                        oThis.setData({
                            bTipsWindowShow: true,
                            bTipsWindowShowType: 8
                        });
                        uni.setStorageSync(oThis.myBalloonTeamCurrentId + '_meetboss_' + oThis.bossInfo.boss_id, 1);
                    } else if (
                        oThis.bossInfo.beat_boss == 1 &&
                        oThis.bossInfo.boss_id &&
                        !uni.getStorageSync(oThis.myBalloonTeamCurrentId + '_beatboss_' + oThis.bossInfo.boss_id)
                    ) {
                        oThis.setData({
                            bTipsWindowShow: true,
                            bTipsWindowShowType: 9
                        });
                        uni.setStorageSync(oThis.myBalloonTeamCurrentId + '_beatboss_' + oThis.bossInfo.boss_id, 1);
                    }
                }
            });
        },

        getMyBalloonTeamCurrent() {
            //我当前的小队
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php';
            const params = {
                action: 'get_team_info',
                team_id: this.myBalloonTeamCurrentId
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    //调整我排在最前
                    if (resp.data.members) {
                        var members = [];
                        for (var m in resp.data.members) {
                            if (resp.data.members[m]['userinfo']['_id'] == this.user_info._id) {
                                //console.log(resp.data.members[m]['userinfo']['_id']);
                                members.push(resp.data.members[m]);
                            }
                        }
                        for (var m in resp.data.members) {
                            if (resp.data.members[m]['userinfo']['_id'] != this.user_info._id) {
                                //console.log(resp.data.members[m]['userinfo']['_id']);
                                members.push(resp.data.members[m]);
                            }
                        }
                        resp.data.members = members;
                    }
                    oThis.setData({
                        myBalloonTeamCurrent: resp.data,
                        isCaptain: resp.data.is_captain ? resp.data.is_captain : 0
                    });
                    // if(oThis.data.myBalloonTeamCurrent && !oThis.data.isCaptain && !wx.getStorageSync(oThis.data.myBalloonTeamCurrent.team_id+"_jointeam")){
                    //   oThis.setData({
                    //     bTipsWindowShow:true,
                    //     bTipsWindowShowType:2,
                    //    });
                    //   wx.setStorageSync(oThis.data.myBalloonTeamCurrent.team_id+"_jointeam",1);
                    // }
                }
            });
        },

        getMyBalloonTeam() {
            //我的小队
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_my_team';
            const params = {
                //term_id: this.data.optionObj.term_id,
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        myBalloonTeam: resp.data.list
                    });
                    if (!oThis.myBalloonTeamCurrentId && oThis.myBalloonTeam.length > 0) {
                        oThis.setData({
                            myBalloonTeamCurrentId: oThis.myBalloonTeam[0].my_team.team_id
                        });
                        if (oThis.bossInfo) {
                            oThis.getBossInfo();
                        }
                    }
                    if (oThis.arenaInfo.status == 'is_end' && oThis.arenaInfo.next_term_id) {
                        oThis.getMyBalloonTeamNext();
                    } else {
                        oThis.switchTab();
                    }
                }
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
                    oThis.setData({
                        myBalloonTeamNext: resp.data.list
                    });
                    if (!oThis.myBalloonTeamCurrentId && oThis.myBalloonTeamNext.length > 0) {
                        oThis.setData({
                            myBalloonTeamCurrentId: oThis.myBalloonTeamNext[0].my_team.team_id
                        });
                    }
                    oThis.switchTab();
                }
            });
        },

        getMyBalloonTeamByTerm() {
            //我的小队
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_my_team';
            const params = {
                term_id: this.balloonTermId
            };
            if (this.location > -1) {
                params['user_pos'] = this.location;
            }
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
                        myBalloonTeamByTerm: resp.data.list,
                        myBalloonTeamByTermDivide: myBalloonTeamByTermDivide
                    });
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

        getJoinBalloonTeam() {
            //我的小队
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_team_list';
            const params = {
                term_id: this.balloonTermId,
                can_join: 1
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        canJoinBalloonteam: resp.data.list
                    });
                    if (resp.data.list.length > 0) {
                        oThis.setData({
                            noData: 1
                        });
                    }
                }
            });
        },

        getAllBalloonTeam(order_by) {
            //队伍排名
            var oThis = this;
            order_by = order_by ? order_by : this.tabIndexSe == 0 ? 'team_time' : this.tabIndexSe == 1 ? 'attack_ranking' : 'defend_ranking';
            var url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_team_fruit_ranking';
            if (order_by == 'attack_ranking') {
                url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_team_attack_ranking';
            } else if (order_by == 'defend_ranking') {
                url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_team_defend_ranking';
            }
            let params = {
                term_id: this.balloonTermId
                //type:this.data.levelTab?this.data.levelTab:3
                //order_by:order_by?order_by:(this.data.tabIndexSe==0?"team_time":"team_level")
            };

            if (this.location > -1) {
                params['user_pos'] = this.location;
            } else {
                if (this.levelTab == 1) {
                    //url = 'https://data.xingxiaoculture.com/api_activity_'+this.data.main_activity_id_api+'.php?action=get_user_exceed_ranking';
                    url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_exceed_msg_list';
                }
            }
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.list) {
                        resp.data.list.reverse();
                    }
                    oThis.setData({
                        allBalloonteam: resp.data.team_list ? resp.data.team_list : resp.data.list,
                        text: resp.data.text,
                        title: resp.data.title,
                        listDisplay: 1,
                        userMine: resp.data.user_mine ? resp.data.user_mine : null
                    });
                    if ((resp.data.team_list && resp.data.team_list.length > 0) || (resp.data.list && resp.data.list.length > 0)) {
                        oThis.setData({
                            noData: 1
                        });
                    }
                }
            });
        },

        getExceedMsgList() {
            var oThis = this;
            var url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_exceed_msg_list';
            let params = {
                term_id: this.balloonTermId
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        exceedList: resp.data.list,
                        listDisplay: 1
                    });
                    if (resp.data.list && resp.data.list.length > 0) {
                        oThis.setData({
                            noData: 1
                        });
                    }
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
                limit: 10
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    if (resp.data.team_list) {
                        var arr = resp.data.team_list;
                        var length = arr.length;
                        var randomIndex;
                        var temp;
                        while (length) {
                            randomIndex = Math.floor(Math.random() * length--);
                            temp = arr[randomIndex];
                            arr[randomIndex] = arr[length];
                            arr[length] = temp;
                        }
                        oThis.setData({
                            allBalloonteamTree: arr
                        });
                        if (!oThis.intervalTimeoutWordRoll) {
                            oThis.initWordRollInterval();
                        }
                    } else {
                        oThis.setData({
                            allBalloonteamTree: []
                        });
                    }
                }
            });
        },

        getApplyList() {
            //小队加入申请
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_apply_join_team_list';
            const params = {
                term_id: this.optionObj.term_id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        applyUserList: resp.data.list
                    });
                    if (resp.data.list.length > 0) {
                        oThis.setData({
                            noData: 1
                        });
                    }
                }
            });
        },

        dingTeam(e) {
            //点赞
            if (this.optionObj.term_id) {
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
                        this.getMyBalloonTeam();
                        this.getAllBalloonTeam();
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

        joinTeamBoxShow(e) {
            if (this.optionObj.term_id) {
                uni.showToast({
                    title: '不能加入往期队伍',
                    icon: 'none'
                });
                return;
            }
            this.setData({
                joinTeamid: e.currentTarget.dataset.obj.team_id,
                joinTeamInfo: e.currentTarget.dataset.obj,
                bTipsWindowShow: true,
                bTipsWindowShowType: 0
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

        agreeJoinBoxShow(e) {
            if (this.optionObj.term_id) {
                uni.showToast({
                    title: '不能操作往期申请',
                    icon: 'none'
                });
                return;
            }
            this.setData({
                applyReqid: e.currentTarget.dataset.reqid,
                applyUserinfo: e.currentTarget.dataset.obj,
                bTipsWindowShow: true,
                bTipsWindowShowType: 1
            });
        },

        agreeJoin() {
            //加入队伍
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=agree_join_team';
            const params = {
                req_id: this.applyReqid
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
                    oThis.getApplyList();
                }
            });
        },

        // changeTab(e) {
        //   let tab = e.currentTarget.dataset
        //   console.log("changeTab tab==" + JSON.stringify(tab))
        //   this.setData({
        //       tabIndex: tab.tab
        //   })
        //   this.switchTab()
        // },
        changeTab(e) {
            let tab = e.currentTarget.dataset;
            console.log('changeTab tab==' + JSON.stringify(tab));
            this.setData({
                levelTab: tab.tab
            });
            //this.switchTab()
            this.getAllBalloonTeam();
        },

        changeTabSe(e) {
            let tab = e.currentTarget.dataset;
            this.setData({
                tabIndexSe: tab.tab
            });
            this.getAllBalloonTeam();
        },

        changeTabSeTem(e) {
            let tab = e.currentTarget.dataset;
            this.setData({
                tabIndexSeTeam: tab.tab
            });
            if (tab.tab == 0) {
                this.getGrabCandyList();
            }
        },

        dialogCloseClick() {
            this.setData({
                bTipsWindowShow: false
            });
        },

        // showVotePanel(){
        //   if(this.data.optionObj.term_id){
        //     wx.showToast({ title: "不能抢占往期擂台", icon: 'none' });
        //     return
        //   }
        //   wx.navigateTo({
        //     url: 'activity_formal?cloud=1',
        //   })
        // },
        // 显示气球记录弹窗
        showBalloonRecordPanel: function () {
            console.log('showBalloonRecordPanel 显示气球记录弹窗');
            var that = this;
            that.setData({
                bShowBalloonRecordPanel: true
            });
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
                bTipsWindowShowType3ShowPrice: item.get_prize ? true : false,
                myBalloonTeamCurrentId: item.team_info.team_id
            });
            this.initPage();
        },

        // 放气球操作回调
        voteCallback: function (e) {
            var that = this;
            if (e.detail.success) {
                that.hideVotePanel();
            }
        },

        // 关闭放气球弹窗操作回调
        closeVoteView: function () {
            var that = this;
            that.hideVotePanel();
        },

        // 显示放气球弹窗
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
            that.initPage();
        },

        selectTeam: function (e) {
            var team_id = e.currentTarget.dataset.team_id;
            this.setData({
                myBalloonTeamCurrentId: team_id
            });
        },

        confirmSelectTeam: function (e) {
            this.initPage();
            this.dialogCloseClick();
        },

        showSelectTeam: function (e) {
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 4
            });
        },

        /* 点击加入其他队伍 */
        handleTeamJoinlist: function (e) {
            uni.navigateTo({
                url: `activity_team_joinlist`
            });
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
            this.switchTab();
        },

        /* 点击申请列表 */
        handleTeamApply: function (e) {
            let team_id = e.currentTarget.dataset.item.team_id;
            let team_name = e.currentTarget.dataset.item.team_name;
            uni.navigateTo({
                url: `activity_team_apply?team_id=` + team_id + '&team_name=' + team_name
            });
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
                    oThis.setData({
                        myBalloonTeamCurrentId: null
                    });
                    oThis.initPage();
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
                    oThis.setData({
                        myBalloonTeamCurrentId: null
                    });
                    oThis.initPage();
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
                    oThis.initPage();
                }
            });
        },

        showTreeInfo(e) {
            //果林树信息
            let item = e.currentTarget.dataset.item;
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 10,
                showTreeInfoObj: item
            });
        },

        gotoTreeRule: function (e) {
            let type = e.currentTarget.dataset.type ? e.currentTarget.dataset.type : 0;
            uni.navigateTo({
                url: 'activity_boss_rule?type=' + type
            });
        },

        showGetGuofei: function (e) {
            this.setData({
                bTipsWindowShow: true,
                bTipsWindowShowType: 11
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
                this.openExchangeXinyi();
            }
        },

        returnDes: function () {
            uni.showToast({
                title: '培育获得的果实将在该场次结束15分钟左右到账~',
                icon: 'none'
            });
        },

        getGrabCandyList() {
            //抢夺记录
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=get_grab_candy_list';
            const params = {
                team_id: this.myBalloonTeamCurrentId
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        grabCandyList: resp.data.list
                    });
                }
            });
        },

        grabCandy(e) {
            //抢夺糖果
            var team_id = e.currentTarget.dataset.team_id;
            if (!team_id) {
                return;
            }
            var oThis = this;
            const url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=grab_candy';
            const params = {
                team_id: this.myBalloonTeamCurrentId,
                target_team_id: team_id
            };
            net.fetchApi(url, params, 'GET').then((resp) => {
                if (resp.data) {
                    oThis.setData({
                        grabResult: resp.data
                    });
                    if (resp.data.code == 0) {
                        oThis.setData({
                            bTipsWindowShow: true,
                            bTipsWindowShowType: 12
                        });
                        oThis.setGrabListStatus(team_id, '已抢夺');
                    } else if (resp.data.code == 2 || resp.data.code == 3) {
                        oThis.setData({
                            bTipsWindowShow: true,
                            bTipsWindowShowType: 13
                        });
                    } else if (resp.data.code == 4) {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
                        });
                        oThis.setGrabListStatus(team_id, resp.data.left_time ? resp.data.left_time : '抢夺失败');
                    } else {
                        uni.showToast({
                            title: resp.data.msg,
                            icon: 'none'
                        });
                        oThis.setGrabListStatus(team_id, '抢夺失败');
                    }
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

        gotoFreeCandyDetail(e) {
            var team_id = e.currentTarget.dataset.team_id;
            uni.navigateTo({
                url: 'activity_free_candy_detail?' + (team_id ? 'team_id=' + team_id : '')
            });
        },

        closeAreaInfo() {
            this.setData({
                isShowDialogShortName: false
            });
        },

        showShortnameSet(e) {
            //设置公会简称弹窗
            var oThis = this;
            oThis.setData({
                isShowDialogShortName: true,
                bShowDialogShortNameType: e.currentTarget.dataset.type ? e.currentTarget.dataset.type : 1,
                showTreeInfoObj: e.currentTarget.dataset.item
            });
        },

        updateShortnameSet(e) {
            //设置信息
            var oThis = this;
            var myBalloonTeamCurrent = this.showTreeInfoObj;
            if (this.bShowDialogShortNameType == 1) {
                var url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=set_team_name';
                var params = {
                    team_name: myBalloonTeamCurrent.team_name,
                    team_id: myBalloonTeamCurrent.team_id
                };
            } else {
                var url = 'https://data.xingxiaoculture.com/api_activity_' + this.main_activity_id_api + '.php?action=set_team_slogan';
                var params = {
                    slogan: myBalloonTeamCurrent.slogan,
                    team_id: myBalloonTeamCurrent.team_id
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
                        oThis.setData({
                            transX: 0.3
                        });
                        oThis.closeAreaInfo();
                        oThis.switchTab();
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
            let check_len = this.bShowDialogShortNameType == 1 ? 6 : 20;
            if (len > check_len) {
                text = text.slice(0, check_len);
            }
            if (text != '') {
                var myBalloonTeamCurrent = this.showTreeInfoObj;
                if (this.bShowDialogShortNameType == 1) {
                    myBalloonTeamCurrent['team_name'] = text;
                } else {
                    myBalloonTeamCurrent['slogan'] = text;
                }
                this.setData({
                    showTreeInfoObj: myBalloonTeamCurrent
                });
            }
        },

        closeTreeBank(e) {
            var type = e.currentTarget.dataset.type;
            if (type == 1) {
                this.setData({
                    bShowTreeBlank1: false
                });
                uni.setStorageSync(this.main_activity_id + '_blanktree1', 1);
            } else {
                this.setData({
                    bShowTreeBlank2: false
                });
                uni.setStorageSync(this.main_activity_id + '_blanktree2', 1);
            }
        },

        setGrabListStatus(teamid, status) {
            var grabCandyList = this.grabCandyList;
            for (var o in grabCandyList) {
                if (grabCandyList[o].team_id == teamid) {
                    grabCandyList[o].status = status;
                }
            }
            this.setData({
                grabCandyList: grabCandyList
            });
        },

        // onShareAppMessage: function () {
        // }
        gotoTeamBattle(e) {
            var team_id = e.currentTarget.dataset.team_id;
            if (team_id) {
                uni.navigateTo({
                    url: 'activity_team_battle?team_id=' + team_id
                });
            }
        }

        /**
         * 用户点击右上角分享
         */
    }
};
</script>
<style>
@import './activity_team.css';
</style>
