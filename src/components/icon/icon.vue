<!--Created in 2017/2/24 by su
eg:
<v-icon :size="16" :type="seller.supports[$index].type"></v-icon>
param:
size:icon height(px)
type:icon type(number)[decrease, discount, guarantee, invoice, special]

-->
<template>
    <span class="icon" :class="iconClass"></span>
</template>
<script type="text/ecmascript-6">
    export default {
        props: {
            type: Number,
            size: Number
        },
        created() {
            this.preMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
            this.sufMap = {
                12: '1',
                16: '2'
            };
        },
        computed: {
            iconClass (){
                return this.preMap[this.type] + this.sufMap[this.size];
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "../../common/stylus/mixin.styl"
    .icon
        display: inline-block
        width: 12px
        height: 12px
        margin-right: 4px
        background-size: 12px 12px
        background-repeat: no-repeat
        vertical-align: top
        &.decrease1
            bg-image('decrease_1')
        &.discount1
            bg-image('discount_1')
        &.guarantee1
            bg-image('guarantee_1')
        &.invoice1
            bg-image('invoice_1')
        &.special1
            bg-image('special_1')
        &.decrease2
            bg-image('decrease_2')
        &.discount2
            bg-image('discount_2')
        &.guarantee2
            bg-image('guarantee_2')
        &.invoice2
            bg-image('invoice_2')
        &.special2
            bg-image('special_2')
</style>
