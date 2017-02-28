<template>
    <div id="header">
        <div class="content-wrapper">
            <div class="avatar">
                <img width="64" height="64" :src="seller.avatar" alt="">
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <div v-if="seller.supports" class="supports">
                    <!--<span class="icon" :class="classMap[seller.supports[0].type]"></span>-->
                    <v-icon :size="12" :type="seller.supports[0].type"></v-icon>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div tabindex="1" v-if="seller.supports" class="support-count">
                <span class="count" @click="showDetail()">{{seller.supports.length}}个</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <div class="bulletin-wrapper" @click="showDetail()">
            <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>
        <div class="background">
            <img :src="seller.avatar" width="100%" height="100%">
        </div>
        <div class="detail" v-show="detailShow" transition="fade">
            <div class="detail-wrapper clearfix">
                <div class="detail-main">
                    <h1 class="name">{{seller.name}}</h1>
                    <div class="star-wrapper">
                        <star :size='48' :score="seller.score"></star>
                    </div>
                    <v-title msg="优惠信息"></v-title>
                    <ul class="supports" v-if="seller.supports">
                        <li class="supports-item" v-for="item in seller.supports">
                            <!--<span class="icon" :class="classMap[seller.supports[$index].type]"></span>-->
                            <v-icon :size="16" :type="seller.supports[$index].type"></v-icon>
                            <span class="text">{{seller.supports[$index].description}}</span>
                        </li>
                    </ul>
                    <v-title msg="商家公告"></v-title>
                    <div class="bulletin">
                        <p class="content">{{seller.bulletin}}</p>
                    </div>
                </div>
            </div>
            <div class="detail-close">
                <i class="icon-close" @click="closeDetail()"></i>
            </div>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
    import star from 'components/star/star';
    import title from 'components/title/title';
    import icon from 'components/icon/icon';
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                detailShow: false
            };
        },
        methods: {
            showDetail() {
                this.detailShow = true;
            },
            closeDetail() {
                this.detailShow = false;
            }
        },
        components: {
            star,
            'v-title': title,
            'v-icon': icon
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "../../common/stylus/mixin.styl"

    #header
        color: #fff
        position: relative
        background: rgba(7, 17, 27, 0.5)
        overflow: hidden
        .content-wrapper
            position: relative
            font-size: 0px
            padding: 24px 12px 18px 24px
            .avatar
                display: inline-block
                vertical-align: top
                img
                    border-radius: 2px
            .content
                display: inline-block
                margin-left: 16px
                .title
                    margin: 2px 0 8px 0
                    .brand
                        display: inline-block
                        vertical-align: top
                        width: 30px
                        height: 18px
                        bg-image('brand')
                        background-size: 30px 18px
                        background-repeat: no-repeat
                    .name
                        margin-left: 6px
                        font-size 16px
                        line-height: 18px;

                .description
                    margin-bottom: 10px
                    line-height: 12px
                    font-size: 12px
                .supports
                    .text
                        line-height: 12px
                        font-size 10px
                        margin-left:4px;

            .support-count
                position: absolute
                right: 12px
                bottom: 14px
                padding: 0 8px
                height: 24px
                line-height: 24px
                border-radius: 14px
                background: rgba(0, 0, 0, 0.2)
                text-align center
            .count
                font-size: 10px
            .icon-keyboard_arrow_right
                vertical-align: top
                font-size: 10px
                line-height: 24px
                margin-left: 2px

        .bulletin-wrapper
            height: 28px
            line-height: 28px
            padding: 0 22px 0 12px;
            white-space: nowrap
            overflow: hidden
            -ms-text-overflow: ellipsis
            text-overflow: ellipsis
            /*font-size:0*/
            position: relative
            background: rgba(7, 17, 27, 0.2)
            .bulletin-title
                display: inline-block
                width: 22px
                height: 12px
                bg-image('bulletin')
                background-size: 22px 12px
                background-repeat: no-repeat
            .bulletin-text
                vertical-align: top
                font-size: 10px
                margin: 0 4px
            .icon-keyboard_arrow_right
                position: absolute
                font-size: 10px
                right: 12px
                top: 8px

        .background
            position: absolute
            top: 0
            left: 0
            height: 100%
            width: 100%
            z-index: -1
            filter: blur(5px)
        .detail
            position: fixed
            top: 0
            left: 0
            z-index: 100
            height: 100%
            width: 100%
            overflow: auto
            transition: all 0.5s
            backdrop-filter: blur(10px)
            &.fade-transition
                opacity: 1
                background: rgba(7, 17, 27, 0.8)
            &.fade-enter, &.fade-leave
                opacity: 0
                background: rgba(7, 17, 27, 0)
            .detail-wrapper
                min-height: 100%
                width: 100%
                .detail-main
                    margin-top: 64px
                    padding-bottom: 64px
                    .name
                        text-align: center
                        font-weight: 700
                        font-size: 16px
                        line-height: 16px
                    .star-wrapper
                        margin-top: 18px
                        padding: 2px 0
                        text-align: center
                    .supports
                        width: 80%
                        margin: 24px auto 28px auto
                        .supports-item
                            padding: 0 12px
                            margin-bottom: 12px
                            font-size: 0
                            &:last-child
                                margin-bottom: 0
                            .icon
                                width: 16px
                                height: 16px
                                margin-right: 6px
                                background-size 16px 16px
                            .text
                                font-size: 12px
                                line-height: 16px
                    .bulletin
                        width: 80%
                        margin: 0 auto
                        .content
                            padding: 0 12px
                            line-height: 24px
                            font-size: 12px
            .detail-close
                position: relative
                margin: -64px auto 0 auto
                font-size: 32px
                width: 32px
                height: 32px
                clear: both


</style>
