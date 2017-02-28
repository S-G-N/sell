<template>
    <div class="goods">
        <div class="menu-wrapper" v-el:menu-wrapper>
            <ul>
                <li tabindex="1" v-for="item in goods" class="menu-item" :class="{'current':currentIndex===$index}"
                    @click="selectMenu($index,$event)">
                    <span class="text bordewr-1px">
                            <v-icon v-show="item.type>0" :size="12" :type="item.type"></v-icon>{{item.name}}
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" v-el:foods-wrapper>
            <ul>
                <li v-for="item in goods" class="food-list food-list-hook">
                    <h1 class="title">{{item.name}}</h1>
                    <ul>
                        <li v-for="food in item.foods" class="food-item">
                            <div class="icon">
                                <img width="57" height="57" :src="food.icon" alt="">
                            </div>
                            <div class="content">
                                <h2 class="name">{{food.name}}</h2>
                                <p v-show="food.description" class="desc">{{food.description}}</p>
                                <div class="extra">
                                    <span class="extra-sellCount">月售{{food.sellCount}}</span>
                                    <span class="extra-rating">好评率{{food.rating}}</span>
                                </div>
                                <div class="price">
                                    <span class="price-now">￥<span class="priceCount">{{food.price}}</span></span>
                                    <span class="price-old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                                </div>
                                <div class="cartcontrol-wrapper">
                                    <v-cart-control :food="food"></v-cart-control>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <div class="shop-cart-wrapper">
            <v-shop-cart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></v-shop-cart>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
    import icon from '../../components/icon/icon';
    import shopCart from '../../components/shopCart/shopCart';
    import cartControl from '../../components/cartControl/cartControl';
    import BScroll from 'better-scroll';
    const ERR_OK = 0;
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data(){
            return {
                goods: [],
                listHeight: [],
                scrollY: 0
            };
        },
        computed: {
            currentIndex(){
                console.log('currentIndex');
                for (let i = 0; i < this.listHeight.length; i++) {
                    let height1 = this.listHeight[i];
                    let height2 = this.listHeight[i + 1];
                    if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
                        return i;
                    }
                }
                return 0;
            }
        },
        created(){
            this.$http.get('/api/goods').then((res) => {
                res = res.body;
                if (res.errno === ERR_OK) {
                    this.goods = res.data;
                    this.$nextTick(() => {
                        this._initScroll();
                        this._calculateHeight();
                    });
                }
            });
        },
        methods: {
            selectMenu(index, event){
                if (!event._constructed) {
                    return;
                }
                let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
                let el = foodList[index];
                this.foodsScroll.scrollToElement(el, 300);
            },
            _initScroll() {
                this.menuScroll = new BScroll(this.$els.menuWrapper, {
                    click: true
                });
                this.foodsScroll = new BScroll(this.$els.foodsWrapper, {
                    probeType: 3
                });
                this.foodsScroll.on('scroll', (pos) => {
                    this.scrollY = Math.abs(Math.round(pos.y));
//                    console.log(this.currentIndex);
                });
            },
            _calculateHeight() {
                let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
                console.log(foodList);
                let height = 0;
                this.listHeight.push(height);
                for (let i = 0; i < foodList.length; i++) {
                    let item = foodList[i];
                    height += item.clientHeight;
                    this.listHeight.push(height);
                    console.log(this.listHeight);
                }
            }
        },
        components: {
            'v-icon': icon,
            'v-cart-control': cartControl,
            'v-shop-cart': shopCart
        }
    };

</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "../../common/stylus/mixin.styl"
    .goods
        display: flex
        position: absolute
        top: 174px
        bottom: 46px
        width: 100%
        overflow: hidden
        .menu-wrapper
            flex: 0 0 80px
            width: 80px
            background: #f3f5f7
            .menu-item
                display: table
                width: 56px
                height: 54px;
                padding: 0px 12px
                line-height: 14px
                &.current
                    position: relative;
                    margin-top: -1px
                    background-color: #fff
                    z-index: 10
                    font-weight: 700
                    .text
                        border-1pxOFF()
                .text
                    display: table-cell
                    vertical-align: middle
                    font-size: 12px
                    font-weight: 200
                    line-height: 14px
                    border-1px(rgba(7, 17, 21, 0.1))
        .foods-wrapper
            flex: 1
            .food-list
                .title
                    padding-left: 14px
                    background: #f3f5f7
                    height: 26px
                    font-size: 12px
                    color: rgb(147, 153, 159)
                    border-left: 2px solid #d9dde1
                    line-height: 26px
                .food-item
                    display: flex
                    padding-bottom: 18px
                    margin: 18px
                    border-1px(rgba(7, 17, 21, 0.1))
                    &:last-child
                        border-1pxOFF()
                        margin-bottom: 0
                    .icon
                        flex: 0 0 57px
                        margin-right: 10px
                    .content
                        flex: 1
                        .name
                            font-size: 14px
                            color: rgb(7, 17, 27)
                            line-height: 14px
                            margin-top: 2px
                        .desc
                            font-size: 10px
                            color: rgb(147, 153, 159)
                            line-height: 10px
                            margin 8px 0
                        .extra
                            font-size: 0
                            color: rgb(147, 153, 159)
                            margin 8px 0
                            .extra-sellCount
                                font-size: 10px
                                margin-right: 12px
                                line-height: 10px
                            .extra-rating
                                font-size: 10px
                                line-height: 10px

                        .price
                            font-size: 0
                            vertical-align: top
                            .price-now
                                font-size: 10px
                                color: red
                                .priceCount
                                    font-size: 14px
                                    font-weight: 700
                            .price-old
                                font-size: 10px
                                color: rgb(147, 153, 159)
                                font-weight: 700

                        .cartcontrol-wrapper
                            position absolute
                            bottom: 12px
                            right: 0
</style>
