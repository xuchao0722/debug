<template>
    <view style="height: 100%">
        <!-- component/supportAct_windows/supportAct_windows.wxml -->

        <!-- wxs模块里进行逻辑运算 -->

        <view class="bgView" @touchmove.stop.prevent="grayBgTouch">
            <!-- 限时活动中奖弹窗 -->
            <block v-if="windowsTypeClone == 0">
                <view class="mainView" style="background: #ffffff">
                    <image class="top_image" src="http://img.xingxiaoculture.com/origin/2020/03/25/af5251f2a83c2859379de54439a713b91585114281.png"></image>
                    <text class="topTitle">贡献{{ supportActText }}贡献点</text>
                    <view class="center_contentView">
                        <text class="topTitle" style="font-size: 28rpx; color: #232323; margin-top: 10rpx">
                            恭喜，在“
                            <text style="color: #ff5e65">勇夺元旦第一屏</text>
                            ”活动中掉落
                        </text>

                        <view class="pro_list_content">
                            <block v-for="(item, index) in supportGiftinfo.gifts" :key="index">
                                <view class="pro_list_content_item">
                                    <image class="pro_list_content_item_img" :src="item.icon"></image>
                                    <text class="pro_list_content_item_text">{{ item.name }}x{{ item.gift_count }}</text>
                                </view>
                            </block>
                        </view>

                        <text class="center_detail_title" style="margin-bottom: 20rpx; font-size: 24rpx; color: #666666; margin-bottom: 10rpx">
                            <block v-if="festivalActStatus > 0">
                                <text class="topView_timeText">星愿卡可在活动页兑换稀有物品~\n距离活动结束剩</text>

                                <block v-for="(item, index) in ['天' + '&nbsp;', '时' + '&nbsp;', '分' + '&nbsp;', '秒']" :key="index">
                                    <text class="topView_timeText" style="color: #ff5e65">{{ value.time(festivalActStatus)[index] }}</text>

                                    <text class="topView_timeText" :decode="true">{{ item }}</text>
                                </block>
                            </block>
                        </text>
                        <text class="center_detail_title" style="color: #ff5e65; margin-top: 10rpx; margin-bottom: 20rpx" @tap="rareSureClick">点此查看详情></text>
                    </view>

                    <view class="support_btn_frame" style="margin-top: 30rpx; margin-bottom: 20rpx">
                        <button class="support_btn" style="margin-left: 6%; border: 1px solid rgba(255, 94, 101, 1); background: #ffffff; color: #ff5e65" @tap="cancelReward">
                            放弃翻倍
                        </button>
                        <button class="all_support_btn" style="margin-left: 53%" @tap="doubleRewardClick">翻倍星星奖励</button>
                    </view>

                    <text class="bottomDesc">花公会币可翻倍星星奖励</text>
                </view>
            </block>

            <!-- 限时活动送星星提示弹窗 -->
            <block v-else-if="windowsTypeClone == 1">
                <view class="mainView" style="background: #ffffff">
                    <image class="top_image" src="https://star-img.xingxiaoculture.com/2019/07/10/953a14546f74318213631e0d134fcee9.png"></image>
                    <text class="top_title">
                        助力
                        <text class="top_title_red">{{ name }}</text>
                    </text>

                    <view class="tab flex_h">
                        <!-- <view class="{{isSelectedCoinTab?'tab-item':'tab-item tab_selected'}}"
				      style="border-radius: 36rpx;border-bottom-left-radius: 36rpx;width:100%" bindtap="switchTab"
				      data-type="0">送星星</view> -->
                        <!-- 8.9 -->
                        <view
                            :class="isSelectedCoinTab ? 'tab-item' : 'tab-item tab_selected'"
                            style="border-top-left-radius: 36rpx; border-bottom-left-radius: 36rpx"
                            @tap="switchTab"
                            data-type="0"
                        >
                            送星星
                        </view>
                        <view
                            :class="isSelectedCoinTab ? 'tab-item tab_selected' : 'tab-item'"
                            style="border-top-right-radius: 36rpx; border-bottom-right-radius: 36rpx"
                            @tap="switchTab"
                            data-type="1"
                        >
                            花公会币
                        </view>
                    </view>

                    <view class="need_coin_content">
                        <image
                            class="need_coin_img"
                            :src="
                                isSelectedCoinTab
                                    ? 'http://img.xingxiaoculture.com/origin/2020/03/25/ed767794214cf37125df53f8f2df9a4f1585114237.png'
                                    : 'http://img.xingxiaoculture.com/origin/2020/03/25/47d16c3f8d9317046d9367b65b6a0af81585114263.png'
                            "
                        ></image>
                        <view class="need_coin">
                            当前剩余
                            <text style="color: #f7941c">{{ isSelectedCoinTab ? coinCount : hasSupportStar }}</text>
                            {{ isSelectedCoinTab ? '公会币' : '星星' }}
                        </view>
                    </view>

                    <!-- 火力值 -->
                    <block v-if="supportActStatus > 0 && birthActData.activity_name">
                        <view class="div_birth_act">
                            <text class="txt_title">
                                “
                                <text style="color: #ff5e65">{{ birthActData.activity_name }}</text>
                                ”进行中
                            </text>
                            <text class="txt_subtitle" @tap="gotoactivityDetail">查看活动详情></text>
                            <text class="txt_tips">
                                送星可获得
                                <text style="color: #ff5e65">{{ birthActData.power_name }}</text>
                                为ta < text style="color:#FF5E65;">{{ birthActData.activity_gift_name }}
                            </text>
                            <view class="div_timer">
                                <block v-for="(item, index) in ['天' + '&nbsp;', '时' + '&nbsp;', '分' + '&nbsp;', '秒']" :key="index">
                                    <text class="txt_sub_tips" style="color: #ff5e65">{{ value.time(supportActStatus)[index] }}</text>

                                    <text class="txt_sub_tips" :decode="true">{{ item }}</text>
                                </block>
                            </view>
                        </view>
                    </block>

                    <block v-else>
                        <view class="center_contentView" style="background: #ffeff5" v-if="festivalActStatus">
                            <text class="topTitle" style="font-size: 30rpx; color: #232323; margin-top: 10rpx">
                                "
                                <text style="color: #ff5e65">大型活动</text>
                                " 进行中
                            </text>
                            <text class="center_detail_title" style="color: #ff5e65; margin-top: 10rpx; margin-bottom: 20rpx" @tap="rareSureClick">点此查看详情></text>

                            <text class="center_detail_title" style="margin-bottom: 20rpx; font-size: 24rpx; color: #666666; margin-bottom: 10rpx">
                                <block v-if="festivalActStatus > 0">
                                    <text class="topView_timeText">距离活动结束剩</text>

                                    <block v-for="(item, index) in ['天' + '&nbsp;', '时' + '&nbsp;', '分' + '&nbsp;', '秒']" :key="index">
                                        <text class="topView_timeText" style="color: #ff5e65">{{ value.time(festivalActStatus)[index] }}</text>

                                        <text class="topView_timeText" :decode="true">{{ item }}</text>
                                    </block>
                                </block>
                            </text>
                        </view>
                    </block>

                    <view class="action">
                        {{ isSelectedCoinTab ? '花' : '送出' }}
                        <input
                            class="action_input"
                            id="input_text"
                            placeholder-class="input_placeholder"
                            confirm-type="done"
                            placeholder="请输入"
                            type="number"
                            @input="bindKeyInput"
                            :value="inputValue"
                        />
                        {{ isSelectedCoinTab ? '公会币送星星' : '颗星星' }}
                    </view>

                    <text class="center_detail_title" v-if="isSelectedCoinTab" style="color: #232323; margin-top: 20rpx; margin-bottom: 20rpx">
                        可兑换并送出
                        <text style="color: #f7941c">{{ coinExchangeStarClone }}</text>
                        星星
                    </text>

                    <block v-if="isSelectedCoinTab">
                        <view class="support_btn_frame" style="margin-top: 0px">
                            <form @submit="payCoin" :report-submit="true">
                                <button class="support_btn" style="width: 65%" formType="submit">立即送出</button>
                            </form>
                        </view>
                    </block>

                    <block v-else>
                        <view class="support_btn_frame">
                            <form @submit="payCoin" :report-submit="true">
                                <button class="support_btn" style="margin-left: 53%" formType="submit">立即送出</button>
                            </form>
                            <form @submit="payAllCoin" :report-submit="true">
                                <button class="all_support_btn" formType="submit">一键送全部</button>
                            </form>
                        </view>
                    </block>

                    <image src="/static/images/common/big_close.png" class="rightTopCloseImage" @tap="closeRareView"></image>
                </view>
            </block>

            <!-- 限时活动提示弹窗 -->
            <block v-if="windowsTypeClone == 2">
                <view class="mainView" style="width: 609rpx; height: 660rpx">
                    <image class="bg_image" src="https://star-img.xingxiaoculture.com/2022/05/15/5b672d6088af8f62e09185748ddbcfb0.png"></image>
                    <image
                        src="https://star-img.xingxiaoculture.com/search/topics/images/2020/12/8/ytwKcTpXpN1607398549173.png"
                        class="topCloseImage"
                        style="top: 170rpx"
                        @tap="closeRareView"
                    ></image>

                    <view class="center_detail_title" style="margin-top: 300rpx; font-size: 24rpx; color: #ffffff; position: absolute; width: 100%">
                        <block v-if="festivalActStatus > 0">
                            <text class="act_topView_timeText" style="color: #9c3170">大型活动进行中\n < text style="color:#9c3170;">超值福利等你来领</text>
                        </block>
                    </view>

                    <text class="topTitle" style="color: #7b1520; font-size: 38rpx; margin-top: 440rpx; position: absolute; width: 100%">{{ name }}</text>
                    <text class="topTitle" style="color: #ff8226; font-size: 30rpx; position: absolute; margin-top: 490rpx; width: 100%">+{{ supportActText }}贡献点</text>
                    <button class="bottom_button" @tap="rareSureClick" style="margin-top: 575rpx; background: #f3697c; color: #ffffff">前往活动</button>
                </view>
            </block>

            <!-- 限时活动 翻倍星星奖励 弹窗 -->
            <block v-if="windowsTypeClone == 3">
                <view class="mainView" style="background: #ffffff">
                    <text class="topTitle" style="color: #000000; margin-top: 50rpx">贡献{{ supportActText }}贡献点</text>

                    <view class="pro_list_content">
                        <view class="pro_list_content_item">
                            <image class="pro_list_content_item_img" :src="/static/component/supportAct_windows/supportGiftinfo.gifts[0].icon"></image>
                            <text class="pro_list_content_item_text">{{ supportGiftinfo.gifts[0].name }}x{{ supportGiftinfo.gifts[0].gift_count }}</text>
                        </view>
                    </view>

                    <view class="double_reward_content">
                        <view class="double_reward_title">选择倍数：</view>
                        <view class="double_reward_list">
                            <block v-for="(item, index) in supportGiftinfo.rank_star_multi_in_times" :key="index">
                                <view
                                    class="double_reward_item"
                                    :style="
                                        selectIndexClone == index
                                            ? 'background:rgba(255,89,129,1);box-shadow:0px 4px 12px 0px rgba(255,35,0,0.26);color:#FFFFFF;'
                                            : 'background:rgba(255,247,247,1);color:rgba(255,94,101,1);'
                                    "
                                    :data-index="index"
                                    :data-needcoin="item.need_coin"
                                    :data-multinum="item.multi_in_number"
                                    @tap="selectClick"
                                >
                                    {{ item.multi_in_number }}倍
                                </view>
                            </block>
                        </view>

                        <view class="double_reward_coin_content">
                            <view class="double_reward_coin_title">需要消耗公会币：</view>
                            <image class="double_reward_coin_img" src="http://img.xingxiaoculture.com/origin/2020/03/25/ed767794214cf37125df53f8f2df9a4f1585114237.png"></image>
                            <view class="double_reward_coin_num">{{ needCoinClone }}</view>
                        </view>
                    </view>

                    <text class="double_select_text" v-if="selectIndexClone >= 0" style="">
                        翻倍后可获得
                        <text style="color: #f7941c">{{ supportGiftinfo.gifts[0].gift_count * supportGiftinfo.rank_star_multi_in_times[selectIndex].multi_in_number }}</text>
                        星星
                    </text>

                    <button
                        class="support_btn"
                        :style="
                            'width:65%;margin-bottom:40rpx;background:' +
                            (selectIndexClone >= 0
                                ? 'linear-gradient(90deg,rgba(255,127,98,1) 0%,rgba(255,72,141,1) 100%);'
                                : 'linear-gradient(90deg,rgba(255,184,168,1) 0%,rgba(255,170,202,1) 100%);')
                        "
                        @tap="confirmDoubleClick"
                    >
                        确认翻倍
                    </button>

                    <image src="/static/images/common/big_close.png" class="rightTopCloseImage" @tap="backTypeReward"></image>
                </view>
            </block>

            <!-- 限时活动 公会币不足 弹窗 -->
            <block v-if="windowsTypeClone == 4">
                <view class="mainView" style="background: #ffffff; height: 450rpx">
                    <image class="coinImage" src="/static/pages/subpages_v2/images/listSupport/supportCoin_two.png"></image>
                    <text class="coinTitle">公会币不足</text>
                    <text class="coinDetail">当前剩余{{ ownSupportCoinCount }}公会币</text>

                    <button class="support_btn" style="width: 65%; margin-bottom: 40rpx; font-size: 32rpx" @tap="actGotoCharge">获取公会币</button>

                    <image src="/static/images/common/big_close.png" class="rightTopCloseImage" @tap="closeRareView"></image>
                </view>
            </block>

            <!-- 限时活动 对话记录 弹窗 -->
            <block v-if="windowsTypeClone == 5">
                <view class="mainView" style="background: #ffffff">
                    <view class="user_info_content">
                        <idc_image class="top_starInfo_icon" radius="50%" :src="userIcon"></idc_image>
                        <view class="top_starName_content">
                            <view class="top_starInfo_left">
                                <view class="top_starInfo_name">{{ userName }}</view>
                                <view class="top_starInfo_card_num">我的星愿卡余额：{{ convertList.star_wish_card_count }} 张</view>
                                <view class="top_starInfo_tips">囤星愿卡，活动期间可兑换，活动结束将失效。</view>
                            </view>
                        </view>
                    </view>

                    <view class="activity_record_content">
                        <view class="activity_record_text">活动期间获得记录</view>
                        <text class="total_card_num">累计获得星愿卡：{{ convertList.has_get_star_wish_card_count }}张</text>
                        <text class="support_star_num">
                            星星：
                            <text style="color: #f7941c">{{ convertList.has_get_rank_star_count }}颗</text>
                        </text>
                    </view>

                    <view class="rechange_reward_content">
                        <view class="rechange_reward_title">兑换获得</view>

                        <block v-for="(item, index) in convertList.list" :key="index">
                            <view class="rechange_reward_item" @tap="proptoastclick" :data-theindex="index">
                                <image class="rechange_reward_item_img" :src="item.iteminfo.icon"></image>
                                <view class="rechange_reward_item_title">
                                    {{ item.self_exchange_count == 0 ? '未获得' : item.self_exchange_count + item.iteminfo.measure_word }}
                                </view>
                                <view class="gray_bg" v-if="item.self_exchange_count == 0"></view>
                            </view>
                        </block>
                    </view>

                    <button class="support_btn" style="width: 65%; margin-bottom: 30rpx; font-size: 32rpx" @tap="rareSureClick">前往活动兑换</button>
                    <image src="/static/images/common/big_close.png" class="rightTopCloseImage" @tap="closeRareView"></image>
                </view>
            </block>

            <!-- 生日活动弹窗 -->
            <block v-if="windowsTypeClone == 6">
                <view class="mainView" style="height: 619rpx">
                    <image class="bg_image_act" :src="birthActData.star_activity_bg"></image>
                    <image src="http://img.xingxiaoculture.com/origin/2020/03/24/16927d01638872dc6b666159f9ddd2621585017490.png" class="img_close_act" @tap="closeRareView"></image>

                    <view
                        class="center_detail_title"
                        style="margin-top: 238rpx; font-size: 24rpx; color: #ffffff; position: absolute; margin-left: 50%; transform: translateX(-50%); width: 100%"
                    >
                        <block v-if="supportActStatus > 0">
                            <text class="act_topView_timeText">
                                送星可获得
                                <text style="color: #fff700">{{ birthActData.power_name }}</text>
                                为ta < text style="color:#FFF700;">{{ birthActData.activity_gift_name }}
                            </text>
                            \n距离活动结束剩

                            <block v-for="(item, index) in ['天' + '&nbsp;', '时' + '&nbsp;', '分' + '&nbsp;', '秒']" :key="index">
                                <text class="act_topView_timeText" style="color: #fff700">{{ value.time(supportActStatus)[index] }}</text>

                                <text class="act_topView_timeText" :decode="true">{{ item }}</text>
                            </block>
                        </block>
                    </view>

                    <text
                        class="topTitle"
                        style="color: #ffffff; font-size: 38rpx; margin-top: 340rpx; position: absolute; margin-left: 50%; transform: translateX(-50%); width: 100%"
                    >
                        {{ name }}
                    </text>
                    <text
                        class="topTitle"
                        style="color: #fff700; font-size: 30rpx; position: absolute; margin-top: 390rpx; margin-left: 50%; transform: translateX(-50%); width: 100%"
                    >
                        +{{ supportActText }}贡献点
                    </text>
                    <text class="btn_go_act" @tap="gotoactivityDetail">活动详情</text>
                </view>
            </block>

            <!-- 生日火力值掉落 -->
            <block v-if="windowsTypeClone == 7">
                <view class="mainView" style="background: #ffffff">
                    <image class="img_top" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/9/18/hDYjXw8i8x1600427221004.png"></image>
                    <text class="topTitle">贡献{{ supportActText }}贡献点</text>
                    <view class="center_contentView_birth">
                        <text class="txt_reward_title">
                            恭喜，在“
                            <text style="color: #ff5e65">{{ birthActData.activity_name }}</text>
                            ”活动中获得
                        </text>
                        <view class="div_img">
                            <image class="img_muilt" v-if="actAwardData.fire_power_multi > 0" :src="actAwardData.fire_power_multi_img"></image>
                            <image class="img_icon" :src="actAwardData.icon"></image>
                        </view>
                        <text class="txt_reward">
                            <text v-if="actAwardData.fire_power_multi > 0" style="color: #ffa334">{{ actAwardData.fire_power_multi }}倍暴击 共</text>
                            <text :style="actAwardData.fire_power_multi > 0 ? 'color: #FFA334;' : 'color: #ff5e65;'">
                                +{{ actAwardData.fire_power }}点{{ birthActData.power_name }}
                            </text>
                        </text>
                        <text class="txt_rewrad_desc">
                            送星可获得
                            <text style="color: #ff5e65">{{ birthActData.power_name }}</text>
                            为ta < text style="color:#FF5E65;">{{ birthActData.activity_gift_name }}
                        </text>
                    </view>
                    <text class="txt_count">
                        我的累计贡献{{ birthActData.power_name }}：
                        <text style="color: #ff5e65">{{ actAwardData.all_fire_power_to_star }}</text>
                    </text>
                    <text class="btn_act_detail" @tap="gotoactivityDetail">活动详情</text>
                    <image src="https://star-img.xingxiaoculture.com/2019/08/09/e90f4ca4c75435c121d44ce3ebfb3d80.png" class="close_img" @tap="closeRareView"></image>
                </view>
            </block>

            <!-- 优惠券掉落 -->
            <block v-if="windowsTypeClone == 8">
                <view class="mainView" style="background: #ffffff">
                    <image class="img_top" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/9/18/hDYjXw8i8x1600427221004.png"></image>
                    <image src="/static/images/common/big_close.png" class="close_imgs" @tap="closeClick"></image>
                    <text class="topTitle" :style="'margin-top: ' + (dropData.drop_coupons.list ? '6rpx' : '40rpx') + ';'">贡献{{ supportActText }}贡献点</text>
                    <text v-if="couponActText > 0 || !dropData.drop_coupons.list" class="coupon_use_count">含加成券加成{{ couponActText }}贡献点</text>
                    <scroll-view v-if="dropData.drop_coupons.list" class="scroll_coupon" :scroll-y="true">
                        <view class="coupon_top_text">
                            恭喜,掉落{{ dropData.good_will_num ? dropData.good_will_num + ',' : '' }}{{ dropData.drop_coupon_num != 0 ? dropData.drop_coupon_num + '张券' : '' }}
                        </view>
                        <view
                            class="inner_view_item"
                            :style="'background: url(' + item.bg_img + '); background-size: cover;' + (item.type == 'buff' ? 'margin-bottom: 20rpx;' : '')"
                            v-for="(item, index) in dropData.drop_coupons.list"
                            :key="index"
                        >
                            <view v-if="item.type == 'vip'" class="inner_left_submit">减</view>

                            <view v-if="item.type == 'props' || item.type == 'vip'" class="inner_left_view" :style="item.type == 'props' ? 'width: 210rpx;' : 'width: 160rpx;'">
                                <view v-if="item.type == 'props'" class="inner_left_unit" style="margin: 74rpx 0rpx 0rpx 24rpx">减</view>
                                <view class="inner_left_value" :style="item.type == 'props' ? 'font-size:50rpx;margin-left:0rpx;margin-top:32rpx;' : ''">
                                    {{ item.deduction_fee_str }}
                                </view>
                                <view v-if="item.type == 'vip'" class="inner_left_unit">元</view>
                            </view>

                            <view v-if="item.type == 'props'" class="inner_left_coin">公会币</view>

                            <view v-if="item.type == 'props' || item.type == 'vip'" class="inner_right_view" :style="item.type == 'props' ? 'margin-left:10rpx;' : ''">
                                <view class="inner_right_text1">{{ item.title }}</view>
                                <view class="inner_right_text2" :style="item.type == 'props' ? 'background: #fffa86;padding: 0rpx 5rpx 0rpx 5rpx;' : ''">
                                    {{ item.desc }}
                                </view>
                                <view class="inner_right_text3">{{ item.time_limit_str }}</view>
                            </view>

                            <view v-if="item.type == 'buff'" class="inner_left_view">
                                <view class="inner_left_unit2" style="color: #f0ff6c">加</view>
                                <view class="inner_left_value2" style="color: #f0ff6c">{{ item.deduction_fee_str }}</view>
                            </view>

                            <view v-if="item.type == 'buff'" class="inner_left_coin" style="color: #f0ff6c">贡献点</view>

                            <view v-if="item.type == 'buff'" class="inner_right_view" style="margin-left: 8rpx">
                                <view class="inner_right2_text1" style="color: #f0ff6c">{{ item.title }}</view>
                                <view class="inner_right2_text2" style="background: #ffc275; color: #cf7508; font-size: 20rpx">
                                    {{ item.desc }}
                                </view>
                                <view class="inner_right2_text3" style="color: #ffca86">{{ item.time_limit_str }}</view>
                            </view>
                        </view>
                        <text v-if="dropData.drop_coupon_num > 10" class="bottom_tip_text">当前只显示10张优惠券\n更多请到优惠券列表查看</text>
                    </scroll-view>
                    <view v-if="dropData.link_url" class="coupon_ac_more" @tap="goDropActivity">查看活动详情></view>
                    <view class="bottom_btn_view" :style="!dropData.drop_coupons.list ? 'margin-top: 70rpx;' : ''">
                        <view
                            v-if="dropData.drop_coupons.list && suppordropDatatGiftinfo.drop_coupons.list.length == 1"
                            class="bottom_btn_use"
                            :style="
                                dropData.drop_coupons.list[0].can_use_now
                                    ? 'background: linear-gradient(90deg,#ff7f62 5%, #ff488d);'
                                    : 'background: linear-gradient(90deg,#ffcdc2 5%, #ffabcb);'
                            "
                            @tap="userCouponClick"
                        >
                            {{ dropData.drop_coupons.list[0].can_use_now ? '立即使用' : '未到使用时间' }}
                        </view>
                        <view class="bottom_btn_look" @tap="lookMoreCoupon">{{ dropData.drop_coupons.list ? '查看优惠券' : '我知道了' }}</view>
                    </view>
                </view>
            </block>

            <!-- 投票券掉落 -->
            <block v-if="windowsTypeClone == 9">
                <view class="mainView" style="background: #ffffff">
                    <image class="top_image" src="http://img.xingxiaoculture.com/origin/2020/03/25/af5251f2a83c2859379de54439a713b91585114281.png"></image>
                    <image src="/static/images/common/big_close.png" class="rightTopCloseImage" @tap="closeClick"></image>
                    <text class="topTitle">贡献{{ supportActText }}贡献点</text>
                    <view class="center_contentView">
                        <text class="topTitle" style="font-size: 28rpx; color: #232323; margin-top: 10rpx">
                            恭喜，在“
                            <text style="color: #ff5e65">不忘童心，永葆纯真</text>
                            ”活动中掉落
                        </text>

                        <view v-if="false" class="inner_content">
                            <view class="inner_content_item" v-for="(item, index) in supportGiftinfo.gifts" :key="index">
                                <image class="inner_image" mode="aspectFit" :src="item.icon"></image>

                                <view class="inner_unit">{{ item.name }}*{{ item.gift_count }}</view>
                            </view>
                        </view>

                        <scroll-view v-else class="handler_coupon_scrollview" :scroll-y="true">
                            <view class="handler_coupon_view" v-for="(item, index) in supportGiftinfo.gifts" :key="index">
                                <image class="handler_coupon_image" :src="item.icon"></image>

                                <view class="handler_coupon_desc">
                                    <view class="handler_coupon_title">{{ item.name }}*{{ item.gift_count }}</view>
                                    <view class="handler_coupon_text">主活动开启后可用</view>
                                </view>
                            </view>
                        </scroll-view>

                        <view class="inner_text" style="color: #999999">单次送星5000颗可掉落投票券1张，向上累计且直接到账，投票券正式活动时可送给</view>

                        <text class="center_detail_title" style="color: #ff5e65; margin-top: 10rpx; margin-bottom: 20rpx" :data-index="5" @tap="lookMoreDetail">查看活动详情></text>
                    </view>

                    <text
                        class="btn_act_detail"
                        :style="
                            supportGiftinfo.gifts[0].can_use_now == 1
                                ? 'background: linear-gradient(90deg,#ff7f62 5%, #ff488d);'
                                : 'background: linear-gradient(90deg,#ffd5cb 5%, #ffb9d4);'
                        "
                        @tap="loveLetterSend"
                    >
                        {{ supportGiftinfo.gifts[0].can_use_now == 1 ? '赠送投票券' : '未到赠送时间' }}
                    </text>
                </view>
            </block>

            <block v-if="windowsTypeClone == 10">
                <view class="mainView" style="background: #ffffff">
                    <image class="img_top" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/9/18/hDYjXw8i8x1600427221004.png"></image>
                    <image src="/static/images/common/big_close.png" class="close_imgs" @tap="closeClick"></image>
                    <text class="txt_send_num">赠送{{ dropData.votetimes }}颗星星</text>
                    <!-- 掉落区 -->
                    <view class="div_drop_info">
                        <text class="txt_title_tips">恭喜，在爱豆八周年活动中掉落</text>
                        <view class="div_num_info">
                            <text class="txt_aimu_num">{{ dropData.drop_loves_num }}\n爱慕值</text>
                            <text class="txt_aimu_num">{{ dropData.drop_coupon_num }}张\n优惠券</text>
                        </view>
                        <!-- 优惠券列表 -->
                        <scroll-view class="div_root_list" :scroll-y="true">
                            <!-- dropData.drop_coupons -->
                            <block
                                wx:item="item"
                                v-if="dropData.drop_coupons && dropData.drop_coupons.list.length > 0"
                                v-for="(item, index) in dropData.drop_coupons.list"
                                :key="index"
                            >
                                <view class="div_coupons">
                                    <text class="txt_deduction" style="margin-left: 18rpx; padding-bottom: 20rpx">减</text>
                                    <text class="txt_deduction" style="font-size: 66rpx">{{ item.deduction_fee_str }}\n < text style="font-size: 21rpx;">公会币</text>
                                    <view class="div_coupons_info">
                                        <text class="txt_coupons_title">{{ item.title }}</text>
                                        <text class="txt_coupons_desc">{{ item.desc }}</text>
                                        <text class="txt_coupons_limit">{{ item.time_limit_str }}</text>
                                    </view>
                                </view>
                            </block>
                            <text class="txt_num_hide">{{ value.numHideTxt(dropData.drop_coupon_num) }}</text>
                        </scroll-view>
                    </view>
                    <!-- 爱慕值和操作按钮 -->
                    <text class="div_my_aimu_num">
                        我当前拥有：
                        <text style="color: #fe395b">{{ dropData.user_own_loves_num }}爱慕值</text>
                    </text>
                    <view class="div_btn_coupons">
                        <text class="txt_go_my_coupons" @tap="go2MyCoupon">查看我的优惠券</text>
                        <text class="txt_go_act_url" @tap="goActUrl" :data-url="dropData.activity_url">去活动页</text>
                    </view>
                    <text class="txt_go_coupons_rule" @tap="showCouponsRule">查看掉落规则</text>
                </view>
            </block>

            <block v-if="windowsTypeClone == 11">
                <view class="mainView" style="background: #ffffff">
                    <image class="img_top" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/9/18/hDYjXw8i8x1600427221004.png"></image>
                    <image src="/static/images/common/big_close.png" class="close_imgs" @tap="closeClick"></image>
                    <text class="txt_send_num">赠送{{ dropData.votetimes }}颗星星</text>
                    <!-- 掉落区 -->
                    <view class="div_drop_info">
                        <text class="txt_title_tips">恭喜，在圣诞活动中掉落</text>
                        <view class="div_num_info">
                            <text class="txt_aimu_num">{{ dropData.drop_snow_num }}\n雪花</text>
                        </view>
                    </view>
                    <!-- 爱慕值和操作按钮 -->
                    <text class="div_my_aimu_num">
                        我当前拥有：
                        <text style="color: #fe395b">{{ dropData.user_own_snow_num }}点雪花</text>
                    </text>
                    <view class="div_btn_coupons">
                        <text class="txt_go_my_coupons" @tap="goActUrlSnow">去活动页</text>
                    </view>
                    <text class="txt_go_coupons_rule" @tap="showCouponsRule">查看掉落规则</text>
                </view>
            </block>
            <!-- 202202s掉落 -->
            <block v-if="windowsTypeClone == 12">
                <view class="mainView" style="background: #ffffff">
                    <image class="img_top" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/9/18/hDYjXw8i8x1600427221004.png"></image>
                    <image src="/static/images/common/big_close.png" class="close_imgs" @tap="closeClick"></image>
                    <text class="txt_send_num">赠送{{ dropData.votetimes }}颗星星</text>
                    <view class="suit_content">
                        <view class="inner_title" style="font-weight: 600">{{ dropData.title }}</view>
                        <view class="inner_content">
                            <view class="inner_content_item1" style="min-width: 170rpx">
                                <image class="inner_image1" style="width: 160rpx; height: 160rpx; margin-top: 16rpx" :src="dropData.icon"></image>
                                <view class="inner_unit" style="margin-top: 12rpx">{{ dropData.good_will_num }}</view>
                            </view>
                        </view>
                        <view class="inner_text1" style="color: #666666">{{ dropData.desc }}</view>
                        <!-- <view class="act_look_more_detail" data-url="{{dropData.link_url}}" bindtap="goNewActivityDetail">查看活动详情></view> -->
                    </view>
                    <view v-if="dropData.link_url" class="div_btn1" :style="dropData.can_use_now ? '' : 'opacity: 0.6;'" @tap="goDropActivity">{{ dropData.btn_word }}</view>
                    <view v-else class="div_btn1" @tap="closeClick">知道了</view>
                </view>
            </block>
            <block v-if="windowsTypeClone == 13">
                <view class="mainView" style="background: #ffffff">
                    <image class="img_top" src="https://star-img.xingxiaoculture.com/search/topics/images/2020/9/18/hDYjXw8i8x1600427221004.png"></image>
                    <image src="/static/images/common/big_close.png" class="close_imgs" @tap="closeClick"></image>
                    <text class="txt_send_num">赠送{{ dropData.votetimes }}颗星星</text>
                    <view class="suit_content">
                        <view class="inner_title" style="font-weight: 600">{{ dropData.title }}</view>
                        <view class="inner_content">
                            <view class="inner_content_item1" style="min-width: 170rpx">
                                <image class="inner_image1" style="width: 180rpx; height: 161rpx; margin-top: 16rpx" :src="dropData.icon"></image>
                                <view class="inner_unit" style="margin-top: 12rpx">{{ dropData.good_will_num }}</view>
                            </view>
                        </view>
                        <view class="inner_text1" style="color: #666666">{{ dropData.desc }}</view>
                        <!-- <view class="act_look_more_detail" data-url="{{dropData.link_url}}" bindtap="goNewActivityDetail">查看活动详情></view> -->
                    </view>
                    <view class="div_btn1" :style="dropData.can_use_now ? '' : 'opacity: 0.6;'" @tap="goDropActivity">{{ dropData.btn_word }}</view>
                </view>
            </block>
        </view>

        <!-- 赠送全部二次确认弹窗 -->
        <view class="dialog_view" v-if="bneedConfirm">
            <view class="dialog_root">
                <text class="txt_title_dg">提醒您</text>
                <text class="txt_desc">请确认是否送出全部星星</text>
                <view class="div_btn">
                    <text class="btn_no" @tap="confirmDialogNo">我再想想</text>
                    <text class="btn_yes" @tap="confirmDialogYes">非常确定</text>
                </view>
            </view>
        </view>

        <!-- 掉落规则弹窗 -->
        <view class="dialog_view" v-if="bShowRuleDg">
            <view class="dialog_root" style="max-height: 1000rpx">
                <text class="txt_title_dg">送星掉落规则</text>
                <text class="txt_desc" style="padding: 25rpx; margin-top: 0rpx">
                    1.单次送星1000颗，必掉落5点爱慕值；而且必掉落优惠券1张；爱慕值和优惠券可同时掉落；\n\n2.掉落爱慕值和优惠券可向上累加，掉落机会的规则为送星数量/1000（向下取整）\n\n3.爱慕值掉落无上限，送星满足1000的多少倍，就会掉落5*对应倍数的爱慕值；\n\n4.优惠券掉落分为道具优惠券和VIP优惠券，两种优惠券单次合计掉落上限为500张；所以单次送星可送50w颗，可避免多送的星不掉落优惠券；\n\n5.爱慕值和优惠券仅在活动期间有效，请及时使用哦，过期就不能再用啦~
                </text>
                <view class="div_btn" style="margin-top: 20rpx; margin-bottom: 30rpx">
                    <text class="btn_no" @tap="confirmDialogNo">我知道了</text>
                </view>
            </view>
        </view>

        <!-- 掉落规则弹窗 雪花 -->
        <view class="dialog_view" v-if="bShowRuleDgSnow">
            <view class="dialog_root" style="max-height: 1000rpx">
                <text class="txt_title_dg">送星掉落规则</text>
                <text class="txt_desc" style="padding: 25rpx; margin-top: 0rpx">
                    1.单次送星1000颗，必掉落10点雪花； \n\n2.掉落雪花的机会可向上累加，掉落机会的规则为送星数量/1000（向下取整）
                    \n\n3.雪花的掉落无上限，送星满足1000的多少倍，就会掉落N*对应倍数的雪花； \n\n4.雪花仅在活动期间有效，请及时使用哦，过期就不能再用啦~
                </text>
                <view class="div_btn" style="margin-top: 20rpx; margin-bottom: 30rpx">
                    <text class="btn_no" @tap="confirmDialogNo">我知道了</text>
                </view>
            </view>
        </view>
    </view>
</template>
<script module="value" lang="wxs">
var time = function (leftTime) {
    var day = parseInt(leftTime / (24 * 3600));
    day = day >= 10 ? day : '0' + day;
    var hourTime = leftTime - day * 24 * 3600;
    var hour = parseInt(hourTime / 3600);
    hour = hour >= 10 ? hour : '0' + hour;
    var minuteTime = hourTime - hour * 3600;
    var minute = parseInt(minuteTime / 60);
    minute = minute >= 10 ? minute : '0' + minute;
    var second = minuteTime - minute * 60;
    second = second >= 10 ? second : '0' + second;
    return [day, hour, minute, second];
}
//自动隐藏优惠券提示
var numHideTxt = function (num) {
    if (num <= 50) {
        return '已加载全部';
    } else if (num > 50) {
        var hideNum = num - 50;
        return "只加载50张，自动隐藏" + hideNum + "张";
    } else {
        return '';
    }
}
module.exports = {
    time: time,
    numHideTxt: numHideTxt
}
</script>
<script>
import idc_image from '../../component/idc_image/idc_image';
// component/supportAct_windows/supportAct_windows.js
var net = require('../../commonscript/common/netLoad.js');
var globalData = require('../../commonscript/common/globalData.js');
export default {
    components: {
        idc_image
    },
    data() {
        return {
            coinCount: 0,
            needPayCoin: 0,
            inputValue: '',
            desc: net.getThingIsin(),
            force: String.fromCharCode(20805) + String.fromCharCode(20540),
            hasSupportStar: 0,
            bneedConfirm: false,
            bShowRuleDg: false,
            bShowRuleDgSnow: false,
            ownSupportCoinCount: '',

            suppordropDatatGiftinfo: {
                drop_coupons: {
                    list: []
                }
            },

            coinExchangeStarClone: '',
            selectIndexClone: '',
            needCoinClone: '',
            multinumClone: '',
            windowsTypeClone: 0
        };
    },
    /**
     * 组件的属性列表
     */
    props: {
        windowsType: {
            type: String,
            default: ''
        },
        perGardener: {
            type: Object,
            default: () => ({})
        },
        isSelectedCoinTab: {
            type: Boolean,
            default: false
        },
        coinCount: {
            type: Number,
            default: 0
        },
        hasSupportStar: {
            type: Number,
            default: 0
        },
        name: {
            type: String,
            default: ''
        },
        supportActStatus: {
            type: Number,
            default: 0
        },
        festivalActStatus: {
            type: Number,
            default: 0
        },
        supportActText: {
            type: Number,
            default: 0
        },
        couponActText: {
            type: Number,
            default: 0
        },
        supportGiftinfo: {
            type: Object,
            default: () => ({})
        },
        coinExchangeStar: {
            type: Number,
            default: 0
        },
        selectIndex: {
            type: Number,
            default: -1
        },
        needCoin: {
            type: Number,
            default: 0
        },
        multinum: {
            type: Number,
            default: 0
        },
        convertList: {
            type: Object,
            default: () => ({})
        },
        userIcon: {
            type: String,
            default: ''
        },
        userName: {
            type: String,
            default: ''
        },
        birthActData: {
            type: Object,
            default: () => ({})
        },
        birthTimeEn: {
            type: Number,
            default: 0
        },
        actAwardData: {
            type: Object,
            default: () => ({})
        },
        dropData: {
            type: Object,
            default: () => ({})
        }
    },
    inputString: '',
    timer: null,
    setInvalidTimer: null,
    /**
     * 组件的方法列表
     */
    methods: {
        grayBgTouch: function (e) {},
        rareSureClick: function (e) {
            this.$emit('rareSureClick');
        },
        closeRareView: function () {
            this.$emit('closeRareView');
        },
        switchTab: function (e) {
            this.setData({
                inputValue: ''
            });
            this.$emit('switchTab', {
                detail: {
                    type: e.target.dataset.type
                }
            });
        },
        calCoin: function (stars) {
            stars = parseInt(stars);
            var va = 0;
            if (stars <= 15) {
                va = stars * 6.6667;
            } else if (stars > 15 && stars <= 160) {
                va = stars * 6.25;
            } else if (stars > 160 && stars <= 520) {
                va = stars * 5.7692;
            } else if (stars > 520 && stars <= 1314) {
                va = stars * 5.7078;
            } else if (stars > 1314) {
                va = stars * 5.5506;
            }
            return Math.round(va);
        },
        closeRareView: function () {
            this.$emit('closeRareView');
        },
        gotoCharge: function () {
            this.$emit('gotoCharge');
        },
        bindKeyInput: function (e) {
            var that = this;
            that.inputString = e.detail.value;
            var realStars = parseInt(this.inputString);
            if (isNaN(realStars)) {
                realStars = 0;
                that.setData({
                    inputValue: ''
                });
            } else {
                that.setData({
                    inputValue: realStars + ''
                });
            }
            if (that.isSelectedCoinTab) {
                that.setData({
                    coinExchangeStarClone: realStars * 20
                });
            }
        },
        getNeedPayCoin: function () {
            var that = this;
            if (that.timer) {
                clearTimeout(that.timer);
                that.timer = null;
                that.setData({
                    needPayCoin: '...'
                });
            }
            that.timer = setTimeout(function () {
                that.timer = null;
                var url = 'https://data.idol001.com/api_mobile_star_rank.php?action=exchange_coin_to_guard_star';
                var params = {
                    num: that.inputValue
                };
                var a = net.fetchApi(url, params, 'GET').then(
                    (resp) => {
                        clearTimeout(that.setInvalidTimer);
                        if (resp && resp.data && resp.data.coin >= 0) {
                            that.setData({
                                needPayCoin: resp.data.coin
                            });
                        } else {
                            that.setData({
                                needPayCoin: '...'
                            });
                        }
                    },
                    function () {}
                );
            }, 200);
            that.setInvalidTimer = setTimeout(function () {
                that.setData({
                    needPayCoin: '...'
                });
            }, 300);
        },
        payCoin: function (e) {
            var that = this;
            var needPayCoin = that.needPayCoin;
            if (parseInt(this.inputValue) == 0 || !this.inputValue) {
                if (that.isSelectedCoinTab) {
                    uni.showToast({
                        title: '请输入公会币个数',
                        icon: 'none'
                    });
                } else {
                    uni.showToast({
                        title: '请输入星星个数',
                        icon: 'none'
                    });
                }
                return;
            }
            this.$emit('payCoin', {
                detail: {
                    formId: e.detail.formId,
                    supportCoin: needPayCoin,
                    addSupportValue: parseInt(this.inputValue)
                }
            });
        },
        payAllCoin: function (e) {
            if (parseInt(this.hasSupportStar) == 0 || !this.hasSupportStar) {
                uni.showToast({
                    title: '你当前无剩余星星',
                    icon: 'none'
                });
                return;
            }
            this.setData({
                bneedConfirm: true
            });
            //this.triggerEvent('payAllCoin');
        },

        confirmDialogNo() {
            this.setData({
                bneedConfirm: false,
                bShowRuleDg: false,
                bShowRuleDgSnow: false
            });
        },
        confirmDialogYes() {
            this.setData({
                bneedConfirm: false
            });
            this.$emit('payAllCoin');
        },
        selectClick: function (e) {
            this.setData({
                selectIndexClone: e.target.dataset.index,
                needCoinClone: e.target.dataset.needcoin,
                multinumClone: e.target.dataset.multinum
            });
        },
        //放弃翻倍
        cancelReward: function () {
            this.$emit('cancelReward');
        },
        //翻倍星星奖励
        doubleRewardClick: function () {
            this.setData({
                windowsTypeClone: 3
            });
            this.$emit('doubleRewardClick');
        },
        backTypeReward: function () {
            this.$emit('backTypeReward');
        },
        //确认翻倍
        confirmDoubleClick: function () {
            if (this.selectIndex >= 0) {
                this.$emit('confirmDoubleClick', {
                    detail: {
                        multinum: this.multinum
                    }
                });
            }
        },
        actGotoCharge: function () {
            this.$emit('actGotoCharge');
        },
        proptoastclick: function (options) {
            this.$emit('propToastClick', {
                detail: {
                    index: options.currentTarget.dataset.theindex
                }
            });
        },
        gotoactivityDetail() {
            this.$emit('gotoactivityDetail');
        },
        closeClick() {
            this.$emit('closeCustomPayCoinView');
        },
        lookMoreDetail(e) {
            this.closeClick();
            if (e.currentTarget.dataset.index) {
                var index = e.currentTarget.dataset.index;
            } else {
                var index = 1;
            }
            uni.navigateTo({
                url: '/pages/web_page/web_page?url=' + encodeURIComponent('https://m.idol001.com/2021_' + index + 'month_s/index.php')
            });
        },
        userCouponClick() {
            var oThis = this;
            var url = 'https://data.idol001.com/api_coupon_act.php?action=get_202101s_act_time';
            var params = {};
            net.fetchApi(url, params, 'GET').then(
                (resp) => {
                    var data = resp.data;
                    var actIng = data.now_time >= data.pre_start_time && data.now_time < data.end_time;
                    if (actIng) {
                        if (!oThis.supportGiftinfo.gifts[0].can_use_now) {
                            return;
                        }
                        if (oThis.supportGiftinfo.gifts[0].gift_id == 'vip') {
                            // wx.navigateTo({
                            //     url: '/pages/subpages_v2/pages/vip_center/vip_center'
                            // })
                            oThis.$emit('vipBuyClick');
                        } else if (oThis.supportGiftinfo.gifts[0].gift_id == 'props') {
                            uni.navigateTo({
                                url: '/pages/subpages/pages/game_prop/game_prop?type=' + oThis.supportGiftinfo.gifts[0].target_id + '&level=' + oThis.supportGiftinfo.gifts[0].level
                            });
                        } else if (oThis.supportGiftinfo.gifts[0].type == 'buff') {
                            uni.switchTab({
                                url: '/pages/rank_list/general_rank_list'
                            });
                        }
                    } else {
                        oThis.$emit('couponUseClick');
                    }
                    oThis.closeClick();
                },
                function (resp) {}
            );
        },
        lookMoreCoupon() {
            this.closeClick();
            uni.navigateTo({
                url: '/pages/web_page/web_page?url=' + encodeURIComponent('https://m.xingxiaoculture.com/2020_11s/quan.php?') + '&title=我的优惠券&type=my_coupon'
            });
        },
        loveLetterSend: function () {
            if (this.supportGiftinfo.gifts[0].can_use_now == 0) {
                return;
            }
            uni.navigateTo({
                url: '/subpage_v3/pages/act_vote_search/act_vote_search'
            });
            this.closeClick();
        },
        goActUrl(e) {
            this.closeClick();
            var url = e.target.dataset.url;
            uni.navigateTo({
                url: '/pages/web_page/web_page?url=' + encodeURIComponent(url)
            });
        },
        //我的优惠券列表
        go2MyCoupon() {
            uni.navigateTo({
                url: '/pages/web_page/web_page?url=' + encodeURIComponent('https://m.xingxiaoculture.com/2020_11s/quan.php?') + '&title=我的优惠券&type=my_coupon'
            });
        },
        //掉落规则
        showCouponsRule() {
            this.setData({
                bShowRuleDg: true
            });
        },
        //掉落规则-雪花
        showCouponsRule() {
            this.setData({
                bShowRuleDgSnow: true
            });
        },
        goActUrlSnow(e) {
            //打开12s活动页
            if (globalData.cookie) {
                let sessionCookie = globalData.cookie.split(/^PHPSESSID=([^\s;]+)\;/g)[1];
            } else {
                let sessionCookie = '';
            }
            sessionCookie = sessionCookie || '';
            let web_url = `https://m.idol001.com/2021_12s/#/?from=minip&sessionid=${sessionCookie}`;
            uni.navigateTo({
                url: '/pages/web_page/web_page?url=' + encodeURIComponent(web_url)
            });
            this.closeClick();
        },
        goNewActivityDetail: function (e) {
            this.closeClick();
            var url = e.currentTarget.dataset.url;
            if (url.indexOf('http') != -1) {
                url = '/pages/web_page/web_page?url=' + encodeURIComponent(url);
            } else {
                url = decodeURIComponent(url);
            }
            uni.navigateTo({
                url: url
            });
        },
        goDropActivity() {
            if (!this.dropData.can_use_now) {
                return;
            }
            var url = this.dropData.link_url;
            if (url.indexOf('http') != -1) {
                url = '/pages/web_page/web_page?url=' + encodeURIComponent(url);
            } else {
                url = decodeURIComponent(url);
            }
            uni.navigateTo({
                url: url
            });
            this.closeClick();
        }
    },
    created: function () {},
    watch: {
        coinExchangeStar: {
            handler: function (newVal, oldVal) {
                this.coinExchangeStarClone = newVal;
            },

            immediate: true
        },

        selectIndex: {
            handler: function (newVal, oldVal) {
                this.selectIndexClone = newVal;
            },

            immediate: true
        },

        needCoin: {
            handler: function (newVal, oldVal) {
                this.needCoinClone = newVal;
            },

            immediate: true
        },

        multinum: {
            handler: function (newVal, oldVal) {
                this.multinumClone = newVal;
            },

            immediate: true
        },

        windowsType: {
            handler: function (newVal, oldVal) {
                this.windowsTypeClone = newVal;
            },

            immediate: true
        }
    }
};
</script>
<style>
@import './supportAct_windows.css';
</style>
