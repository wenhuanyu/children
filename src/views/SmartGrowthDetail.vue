<template>
    <div>
        <div class="button-wrap">
            <el-button type="primary" v-print="printObj" icon="el-icon-printer" style="background-color: #20BE18; border-color: #20BE18">打印</el-button>
            <el-button type="warning" icon="el-icon-arrow-left" @click="$router.go(-1)">返回上一页</el-button>
        </div>
        <div class="gestationalAge-content">
            <div class="gestationalAge-content_con" ref="print" id="print">
                <div class="table-name">
                    <p>智能开发指导</p>
                </div>
                <div class="table_content">
                    <div class="table_content_header flex flex-justify-content-around">
                        <div style="width:15%; text-align: center; line-height: 54px; border-right: 1px solid #E6E6E6;">
                            {{ form.num }}</div>
                        <div style="width:15%; text-align: center; line-height: 54px; border-right: 1px solid #E6E6E6; color: #252525; font-weight: bold;">观察领域</div>
                        <div style="width:15%; text-align: center; line-height: 54px; border-right: 1px solid #E6E6E6;"> {{form.watch_area}} </div>
                        <div style="width:15%; text-align: center; line-height: 54px; border-right: 1px solid #E6E6E6; color: #252525; font-weight: bold;">观察内容</div>
                        <div style="width:40%; text-align: center;">{{form.watch_content}}</div>
                    </div>
                    <div class="table_content_header">
                        <div style="width:15%; text-align: center; line-height: 54px; border-right: 1px solid #E6E6E6; color: #252525; font-weight: bold;">
                            发展目标</div>
                        <div style="text-align: center;width: 85%">{{form.develop_goals}}</div>
                    </div>
                    <div v-if="form.counseling_program.length>0">
                        <div class="title">辅导方案</div>
                        <div class="value" v-html="form.counseling_program"></div>
                    </div>
                    <div  v-if="form.notice.length>0">
                        <div class="title">注意事项</div>
                        <div class="value" v-html="form.notice"></div>
                    </div>
                    <div  v-if="form.parents_read.length>0">
                        <div class="title">父母必读</div>
                        <div class="value" v-html="form.parents_read"></div>
                    </div>
                    <div v-if="form.metro_mirro.length>0">
                        <div class="title">同步方案</div>
                        <div class="value" v-html="form.metro_mirro"></div>
                    </div>
                </div>
                <div style="height: 50px;"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "SmartGrowthDetail",
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
            this.$axios.post('api/tool/brain_info', {
                age_id: this.$route.query.age_id,
                type: this.$route.query.type
            }).then(res => {
                if (res.data.code === 1) {
                    this.form = res.data.data
                    console.log('this.form',this.form)
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

.table_content_header {
    height: 54px;
    border-bottom: 1px solid #E6E6E6;
    display: flex;
    align-items: center;
}

.table_content_header p {
    margin-left: 30px;
    font-size: 16px;
    color: #333333;
}

.table_content_text {
    padding: 25px;
    //border-bottom: 1px solid #E6E6E6;
    //display: flex;
    //flex-direction: column;
    //align-items: center;
}
.table_content_text-name{
    text-align: center;
}
.table_content_text-name p{
    font-size: 20px;
    font-weight: bold;
    color: #333333;
}
.table_content_text-con{
    position: relative;
    font-size: 16px;
    font-weight: 400;
    color: #333333;
    padding-left: 20px;
    margin-top: 20px;
}
.table_content_text-con span{
    position: absolute;
    top: 7px;
    left: 0;
    display: block;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: #F5605A;
}
.table_content_footer{
    padding: 25px;
}
</style>
