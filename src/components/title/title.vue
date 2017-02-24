<!--Created in 2017/2/23 by su
eg:
<v-title msg="商家公告"></v-title>
param:
mag:title
-->
<template>
    <div id="title" track-by="$index">
        <div class="line"></div>
        <div class="text">{{msg}}</div>
        <div class="line"></div>
    </div>
</template>

<script type="text/ecmascript-6">
    export default {
        props: ['msg']
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    #title
        display: flex
        width: 80%
        margin: 28px auto 24px auto
        .line
            flex: 1
            position: relative
            top: -6px
            border-bottom: 1px solid rgba(255, 255, 255, 0.2)
        .text
            padding: 0 12px
            font-size: 14px
            font-weight: 700
</style>
