<template>
    <div class="ratings" ref="ratings">
        <ul>
            <li>
                <div class="overview">
                    <div class="overview-left">
                        <h1 class="score">{{info.score}}</h1>
                        <div class="title">综合评分</div>
                        <div class="rank">高于周边商家{{info.rankRate}}%</div>
                    </div>
                    <div class="overview-right">
                        <div class="score-wrapper">
                            <span class="title">服务态度</span>
                            <div><Star :score="info.serviceScore" :size="36"/></div>
                            <span class="score">{{info.serviceScore}}</span>
                        </div>
                        <div class="score-wrapper">
                            <span class="title">商品评分</span>
                            <div><Star :score="info.serviceScore" :size="36"/></div>
                            <span class="score">{{info.foodScore}}</span></div>
                        <div class="delivery-wrapper">
                            <span class="title">送达时间</span>
                            <span class="delivery">{{info.deliveryTime}}分钟</span>
                        </div>
                    </div>
                </div>

            </li>
            <li>
                <div class="ratingselect">
                    <div class="rating-type border-1px" >
                      <span class="block" :class="{active:chooesType===2}" @click="chooesType=2">
                        全部<span class="count">{{retings.length}}</span>
                      </span>
                        <span class="block" :class="{active:chooesType===0}" @click="chooesType=0">
                        推荐<span class="count">{{recommendCount.length}}</span>
                      </span>
                        <span class="block" :class="{active:chooesType===1}" @click="chooesType=1">
                        吐槽<span class="count">{{retings.length-recommendCount.length}}</span>
                      </span>
                    </div>
                    <div class="switch" :class="{ on:showText}" @click="showText=!showText">
                        <span class="iconfont icon-gou"></span>
                        <span class="text">只看有内容的评价</span>
                    </div>
                </div>
            </li>

            <li>
                <div class="rating-wrapper" >
                    <ul>
                        <li class="rating-item" v-for="(reting,i) in retingsFillterArr" :key="i">
                            <div class="avatar">
                                <img width="28" height="28" :src="reting.avatar">
                            </div>
                            <div class="content">
                                <h1 class="name">{{reting.username}}</h1>
                                <div class="star-wrapper">
                                    <div><Star :score="reting.score" :size="24"/></div>
                                    <span class="delivery">{{reting.deliveryTime}}</span>
                                </div>
                                <p class="text">{{reting.text}}</p>
                                <div class="recommend">
                                    <span class="iconfont icon-zan" style="color: #ff4220" v-if="reting.score>=4"></span>
                                    <div>
                                        <span class="iconfont icon-thumb_up" v-for="(re,i) in reting.recommend" :key="i">{{re}}</span>
                                    </div>
                                </div>
                                <div class="time">{{reting.rateTime | date-fromat}}</div>
                            </div>
                        </li>
                    </ul>
                </div>
            </li>

        </ul>
    </div>
</template>

<script>
    import Star from "../../../components/Star/Star";
    import {mapState} from 'vuex'
    export default {
        data(){
            return{
                showText:false,
                chooesType:0
            }
        },
        mounted(){
            this.$store.dispatch('getRetings',()=>{
                this.$nextTick(()=>{

                })
            })
        },
        components:{Star},
        computed:{
            ...mapState(['retings','info']),
            retingsFillterArr(){
                const {retings,showText,chooesType} = this;
                return retings.filter(re=>{
                    return (chooesType===2||re.rateType===chooesType)&&(!showText||re.text!=='')
                })
            },
            recommendCount(){
                const {retings} = this;
                return retings.filter(re=>{
                    return re.rateType===0
                })

            }
        }
    }
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "../../../common/stylus/mixins.styl"

    .ratings
        width: 100%
        overflow hidden
        .overview
            display: flex
            padding: 18px 0
            .overview-left
                flex: 0 0 137px
                padding: 6px 0
                width: 137px
                border-right: 1px solid rgba(7, 17, 27, 0.1)
                text-align: center
                @media only screen and (max-width: 320px)
                    flex: 0 0 120px
                    width: 120px
                .score
                    margin-bottom: 6px
                    line-height: 28px
                    font-size: 24px
                    color: rgb(255, 153, 0)
                .title
                    margin-bottom: 8px
                    line-height: 12px
                    font-size: 12px
                    color: rgb(7, 17, 27)
                .rank
                    line-height: 10px
                    font-size: 10px
                    color: rgb(147, 153, 159)
            .overview-right
                flex: 1
                padding: 6px 0 6px 24px
                @media only screen and (max-width: 320px)
                    padding-left: 6px
                .score-wrapper
                    display flex
                    margin-bottom: 8px
                    font-size: 0
                    .title
                        display: inline-block
                        line-height: 18px
                        vertical-align: top
                        font-size: 12px
                        color: rgb(7, 17, 27)
                    .star
                        display: inline-block
                        margin: 0 12px
                        vertical-align: top
                    .score
                        display: inline-block
                        line-height: 18px
                        vertical-align: top
                        font-size: 12px
                        color: rgb(255, 153, 0)
                .delivery-wrapper
                    display flex
                    font-size: 0
                    .title
                        line-height: 18px
                        font-size: 12px
                        color: rgb(7, 17, 27)
                    .delivery
                        margin-left: 12px
                        font-size: 12px
                        color: rgb(147, 153, 159)
        .ratingselect
            .rating-type
                padding: 18px 0
                margin: 0 18px
                border-1px(rgba(7, 17, 27, 0.1))
                font-size: 0
                .block
                    display: inline-block
                    padding: 8px 12px
                    margin-right: 8px
                    line-height: 16px
                    border-radius: 1px
                    font-size: 12px
                    color: rgb(77, 85, 93)
                    background: rgba(77, 85, 93, 0.2)
                    &.active
                        background: $green
                        color: #fff
                    .count
                        margin-left: 2px
                        font-size: 8px
            .switch
                display flex
                padding: 12px 18px
                line-height: 24px
                border-bottom: 1px solid rgba(7, 17, 27, 0.1)
                color: rgb(147, 153, 159)
                font-size: 0
                &.on
                    .icon-gou
                        color: $green
                .icon-gou
                    display: inline-block
                    vertical-align: top
                    margin-right: 4px
                    font-size: 24px
                .text
                    display: inline-block
                    vertical-align: top
                    font-size: 12px

        .rating-wrapper
            padding: 0 18px

            .rating-item
                display: flex
                padding: 18px 0
                bottom-border-1px(rgba(7, 17, 27, 0.1))
                .avatar
                    flex: 0 0 28px
                    width: 28px
                    margin-right: 12px
                    img
                        border-radius: 50%
                .content
                    position: relative
                    flex: 1
                    .name
                        margin-bottom: 4px
                        line-height: 12px
                        font-size: 10px
                        color: rgb(7, 17, 27)
                    .star-wrapper
                        margin-bottom: 6px
                        font-size: 0
                        .star
                            display: inline-block
                            margin-right: 6px
                            vertical-align: top
                        .delivery
                            display: inline-block
                            vertical-align: top
                            line-height: 12px
                            font-size: 10px
                            color: rgb(147, 153, 159)
                    .text
                        margin-bottom: 8px
                        line-height: 18px
                        color: rgb(7, 17, 27)
                        font-size: 12px
                    .recommend
                        display flex
                        line-height: 16px
                        font-size: 0
                        .icon-thumb_up, .icon-thumb_down, .item
                            display: inline-block
                            margin: 0 8px 4px 0
                            font-size: 9px
                        .icon-thumb_up
                            color: $yellow
                        .icon-thumb_down
                            color: rgb(147, 153, 159)

                        .item
                            padding: 0 6px
                            border: 1px solid rgba(7, 17, 27, 0.1)
                            border-radius: 1px
                            color: rgb(147, 153, 159)
                            background: #fff
                    .time
                        position: absolute
                        top: 0
                        right: 0
                        line-height: 12px
                        font-size: 10px
                        color: rgb(147, 153, 159)
</style>
