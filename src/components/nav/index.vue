<template>
    <div class="nav">
        <router-link tag="div" class="nav-item " to="/recommend">
            <span class="nav-link">推荐</span>
        </router-link>
        <router-link tag="div" class="nav-item" to="/singer">
            <span class="nav-link">歌手</span>
        </router-link>
        <router-link tag="div" class="nav-item" to="/rank">
            <span class="nav-link">排行</span>
        </router-link>
        <router-link tag="div" class="nav-item" to="/search">
            <span class="nav-link">搜索</span>
        </router-link>
    </div>
</template>

<script>
export default {
    
}
</script>

<style lang="less" scoped>
@import '~styles/vars.less';
.nav{
    display: flex;
    height:44px;
    line-height: 44px;
    background-color: @head-bg;
    .nav-item{
        flex:1;
        text-align: center;
        .nav-link{
            padding-bottom: 5px;
            color:#fff;

        }
        &.router-link-active{
            .nav-link{
                color: @nice-gold;
                border-bottom: 2px solid @nice-gold
            }
        }
    }
    

}
</style>


