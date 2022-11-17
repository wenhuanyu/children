<template>
    <div id="index">
        <img class="logo" src="../assets/images/logo.png" alt="">
        <div class="index-content">
            <router-link v-for="(item,index) in line.slice(0,1)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                    <img class="name1" :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="">
            </router-link>
            <router-link v-for="(item,index) in line.slice(1,2)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <img class="name2" :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="" >
            </router-link>
            <router-link v-for="(item,index) in line.slice(2,3)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <img class="name3" :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="" >
            </router-link>
            <router-link v-for="(item,index) in line.slice(3,4)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <img class="name4" :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="" >
            </router-link>
            <router-link v-for="(item,index) in line.slice(4,5)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <img class="name5" :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt=""  >
            </router-link>
            <router-link v-for="(item,index) in line.slice(5,6)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <img class="name6" :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="" >
            </router-link>
            <router-link v-for="(item,index) in line.slice(6,7)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <img class="name7" :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="" >
            </router-link>
            <router-link v-for="(item,index) in line.slice(7,8)"
                         :key="'line1' + index" :to="item.to" :style="{'pointer-events': check(item.name)}">
                <img class="name8" :class="{disabled:!$store.state.mainMenus.includes(item.name),img:true}" :src="item.img" alt="" >
            </router-link>
        </div>

        <div class="all_right">
            <el-popover
                placement="top"
                width="160"
                v-model="logoutShow">
                <p>确定退出登录？</p>
                <div style="text-align: right; margin: 0">
                    <el-button size="mini" type="text" @click="logoutShow = false">取消</el-button>
                    <el-button type="primary" size="mini" @click="logout">确定</el-button>
                </div>
                <button slot="reference" class="all_right_top">
                    <img src="../assets/images/user_red.png" alt="" style="width: 25px; height: 27px;">
                    <p style="padding-right: 5px">退出登录</p>
                    <img src="../assets/images/cuo.png" alt="" style="width: 25px; height: 25px;">
                </button>
            </el-popover>
        </div>
    </div>
</template>

<script>
export default {
    name: "Index",
    data() {
        return {
            logoutShow: false,
            line: [
                {
                    img: require("@/assets/images/index_icon6.png"),
                    name: '终端监控',
                    introduce:'系统终端不同状态监控',
                    to: '/TermMonitor'
                },
                {
                    img: require('@/assets/images/index_icon1.png'),
                    name: '患者管理',
                    introduce:'患者信息管理',
                    to: 'PatientManagement'
                },
                {
                    img: require("@/assets/images/index_icon2.png"),
                    name: '评估测试',
                    introduce:'多种题型测试',
                    to: '/Assess'
                },
                {
                    img: require("@/assets/images/index_icon4.png"),
                    name: '训练治疗',
                    introduce:'多种不同的训练类型',
                    to: '/Emotion'
                },
                //line 2
                {
                    img: require("@/assets/images/index_icon10.png"),
                    name: '数据分析',
                    introduce:'报告随时查看',
                    to: '/DataAnalysis'
                },
                {
                    img: require("@/assets/images/index_icon5.png"),
                    name: '报告中心',
                    introduce:'报告随时查看',
                    to: '/ReportCenter'
                },
                {
                    img: require("@/assets/images/index_icon8.png"),
                    name: '系统设置',
                    introduce:'管理员系统配置',
                    to: '/SystemSetting'
                },

                {
                    img: require("@/assets/images/index_icon9.png"),
                    name: '工具箱',
                    introduce:'管理员系统配置',
                    to: '/Toolkit'
                },

            ]
        };
    },
    computed: {
        check: function () {
            return (name) => {
                return this.$store.state.mainMenus.includes(name) ? 'all' : 'none'
            }
        }
    },
    watch: {
        /*'$store.state.mainMenus':(newVal)=>{
            console.log('mainMenus change',newVal)
        }*/
    },
    async mounted() {
        console.log('用户类型', sessionStorage.getItem('usertype'))

        if (sessionStorage.getItem('usertype') !== 'center') {
            this.$router.push('/TerminalLogin')
        }
        if (!sessionStorage.getItem('usertype')) {
            this.$router.push('/Login')
        }
        this.getRules().then()
    },
    methods: {
        logout() {
            this.$store.commit('setUserInfo', {data: '', status: false});
            this.$router.push('/Login');
        },
        getRules() {
            let rule = new Promise((resolve, reject) => {
                //获取权限列表
                this.$store.dispatch('GetAuth')
            })
            return rule
        }
    },
}
</script>

<style scoped lang="scss">
button {
    border: none;
}
#index {
    width: 100%;
    height: 100vh;
    background: url('../assets/images/indexBG.png') no-repeat;
    background-size: 100% 100%;
}

.logo {
    display: block;
    //height: 10vh;
    position: absolute;
    top: 2vh;
    left: 6%;
}

.index-content {
    position: relative;
    .name1 {
        width: 190px;
        height: 191px;
        position: absolute;
        top: 35vh;
        left: 10%;
    }
    .name2 {
        width: 225px;
        height: 221px;
        position: absolute;
        top: 24vh;
        left: 24%;
    }
    .name3 {
        width: 220px;
        height: 221px;
        position: absolute;
        top: 24vh;
        left: 42%;
    }
    .name4 {
        width: 190px;
        height: 191px;
        position: absolute;
        top: 51vh;
        left: 32%;
    }
    .name5 {
        width: 190px;
        height: 191px;
        position: absolute;
        top: 60vh;
        left: 45%;
    }
    .name6 {
        width: 220px;
        height: 221px;
        position: absolute;
        top: 60vh;
        left: 16%;
    }
    .name7 {
        width: 190px;
        height: 191px;
        position: absolute;
        top: 60vh;
        left: 65%;
    }
    .name8 {
        width: 170px;
        height: 171px;
        position: absolute;
        top: 45vh;
        left: 58%;
    }
}


.all_right_top p::after {
    border-top: 8px solid #97ce42;
}
::v-deep .el-button--primary {
    background-color: #20BE18;
}
::v-deep .el-button--text {
    color: #20BE18;
}
.disabled {
    filter: grayscale(100%);
}
</style>
