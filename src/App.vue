<template>
    <div id="app">
        <router-view/>
    </div>
</template>
<script>
export default {
    data() {
        return {}
    },
    mounted() {
        if(window.location.hash.match('AssessDetailReport')
            || window.location.hash.match('EmotionDetailReport'))
        {
        } else {
            this.$axios.post('/api/setting/auth').then(res => {
                if (res.data && res.data.data.length) {
                    this.$store.commit('setAuth', res.data.data)
                }
            })

        }
        // //获取权限列表
        // this.$store.dispatch('GetAuth')
    }
}
</script>
<style lang="scss">
@import "./assets/css/common";
@import "./assets/css/main";
</style>
