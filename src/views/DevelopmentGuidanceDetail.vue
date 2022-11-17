<template>
    <div>
        <div class="button-wrap">
            <el-button type="primary" v-print="printObj" icon="el-icon-printer" style="background-color: #20BE18; border-color: #20BE18">打印</el-button>
            <el-button type="warning" icon="el-icon-arrow-left" @click="$router.go(-1)">返回上一页</el-button>
        </div>
        <div class="gestationalAge-content">
            <div class="gestationalAge-content_con" ref="print" id="print">
                <div class="table-name">
                    <p>{{ form.month }}儿童发育指导</p>
                </div>
                <div class="table_content">
                    <div v-for="(item,index) in form.content">
                        <div class="table_content_title" v-html="item.title"> </div>
                        <div class="val" v-html="item.content"></div>
                    </div>
                </div>
                <div style="height: 50px;"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "DevelopmentGuidanceDetail",
    data() {
        return {
            printObj: {
                id: "print",//要打印的id名 无#号
                popTitle:'&nbsp;',//页眉标题 默认浏览器标题 空字符串时显示undefined 使用html语言
                extraHead:'&nbsp;',//头部文字 默认空
            },
            dialogReport      : false,
            weekInfo_list:'',
            form:{}
        };
    },
    computed: {},
    watch: {},
    mounted() {
        //消息接口
        this.$store.dispatch('GetXiao')
        this.onWeekInfo()
    },
    methods: {
        onWeekInfo(){
            this.$axios.post('api/tool/growth_info', {
                id: this.$route.query.id,
            }).then(res => {
                if (res.data.code === 1) {
                    this.form = res.data.data
                }
            })
        }
    },
}
</script>
<style media="print">
@page{
    size :auto;
    margin: 25px;
}
</style>

<style scoped lang="scss">
.table_content_title {
    padding-left: 56px;
    padding-right: 56px;
    margin-top: 40px;
    padding-bottom: 20px;
    font-size: 16px;
    font-weight: 400;
    color: #333333;
    line-height: 22px;
}
.val {
    padding-left: 56px;
    padding-right: 56px;
    padding-bottom: 40px;
    font-size: 16px;
    font-weight: bold;
    color: #333333;
    line-height: 22px;
    border-bottom: 1px solid #E6E6E6;
}
.title {
    height: 64px;
    font-size: 18px;
    font-weight: bold;
    color: #252525;
    line-height: 64px;
    text-align: center;
    border-bottom: 1px solid #E8E8E8;
}
.value {
    padding: 40px 60px;
    font-size: 16px;
    color: #252525;
    font-weight: 400;
    border-bottom: 1px solid #E8E8E8;
}
.button-wrap {
    display: flex;
    justify-content: flex-end;
}

.gestationalAge-content {
    margin-top: 15px;
    background-color: #fff;
    border-radius: 5px;
    //height: calc(100vh - 205px);
    height: 79vh;
    overflow: scroll;
}

.gestationalAge-content_con {
    width: 977px;
    margin: 0 auto;
    background: #FFFFFF;
}

.table-name {
    position: relative;
    display: flex;
    align-items: flex-end;
    justify-content: center;
    padding: 59px 0 44px 0;
    font-size: 16px;
    color: #333333;
}

.table-name p {
    font-size: 24px;
    font-weight: bold;
}

.edit-btn.active {
    display: block;
}

.edit-btn {
    color: #20BE18;
    position: absolute;
    right: 0;
    cursor: pointer;
    @media print {
        display: none;
    }
}
.edit-btn img{
    margin-right: 5px;
}

.table_content {
    border: 1px solid #E6E6E6;
}
</style>
