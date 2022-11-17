<template>
    <div>
        <div class="button-wrap">
            <el-button type="primary" v-print="printObj" icon="el-icon-printer" style="background-color: #20BE18; border-color: #20BE18">打印</el-button>
            <el-button type="warning" icon="el-icon-arrow-left" @click="$router.go(-1)">返回上一页</el-button>
        </div>
        <div class="gestationalAge-content">
            <div class="gestationalAge-content_con" ref="print" id="print">
                <div class="table-name">
                    <p v-if="type == 1">婴儿喂养食谱</p>
                    <p v-if="type == 2">常见食物及营养价值</p>
                </div>
                <div class="table_content">
                    <div class="table_content_header flex flex-justify-content-around" style="color: white">
                        <div v-if="type == 1" style="height: 54px; width: 100%; text-align: center; line-height: 54px; background-color: #22BE19">{{age}} 婴儿喂养带量参考食谱 </div>
                        <div v-if="type == 2" style="height: 54px; width: 100%; text-align: center; line-height: 54px;background-color: #29B3FF">{{age}} 婴儿常见食物品种及营养价值参考 </div>
                    </div>
                    <div>
                        <div v-if="type == 1">
                            <div class="title" style="background-color: #EFFFEE">星期一</div>
                            <div class="flex value">
                                <div class="v1">06:00</div>
                                <div class="v2">{{form.monday6}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">09:00</div>
                                <div class="v2">{{form.monday9}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">12:00</div>
                                <div class="v2">{{form.monday12}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">15:00</div>
                                <div class="v2">{{form.monday15}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">18:00</div>
                                <div class="v2">{{form.monday18}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">21:00</div>
                                <div class="v2">{{form.monday21}}</div>
                            </div>
                            <div class="title" style="background-color: #EEF7FF">星期二</div>
                            <div class="flex value">
                                <div class="v1">06:00</div>
                                <div class="v2">{{form.tuesday6}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">09:00</div>
                                <div class="v2">{{form.tuesday9}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">12:00</div>
                                <div class="v2">{{form.tuesday12}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">15:00</div>
                                <div class="v2">{{form.tuesday15}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">18:00</div>
                                <div class="v2">{{form.tuesday18}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">21:00</div>
                                <div class="v2">{{form.tuesday21}}</div>
                            </div>
                            <div class="title" style="background-color: #FFFAEE">星期三</div>
                            <div class="flex value">
                                <div class="v1">06:00</div>
                                <div class="v2">{{form.wednesday6}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">09:00</div>
                                <div class="v2">{{form.wednesday9}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">12:00</div>
                                <div class="v2">{{form.wednesday12}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">15:00</div>
                                <div class="v2">{{form.wednesday15}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">18:00</div>
                                <div class="v2">{{form.wednesday18}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">21:00</div>
                                <div class="v2">{{form.wednesday21}}</div>
                            </div>
                            <div class="title" style="background-color: #E6E6E6">星期四</div>
                            <div class="flex value">
                                <div class="v1">06:00</div>
                                <div class="v2">{{form.thursday6}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">09:00</div>
                                <div class="v2">{{form.thursday9}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">12:00</div>
                                <div class="v2">{{form.thursday12}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">15:00</div>
                                <div class="v2">{{form.thursday15}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">18:00</div>
                                <div class="v2">{{form.thursday18}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">21:00</div>
                                <div class="v2">{{form.thursday21}}</div>
                            </div>
                            <div class="title" style="background-color: #EEEFFF">星期五</div>
                            <div class="flex value">
                                <div class="v1">06:00</div>
                                <div class="v2">{{form.friday6}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">09:00</div>
                                <div class="v2">{{form.friday9}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">12:00</div>
                                <div class="v2">{{form.friday12}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">15:00</div>
                                <div class="v2">{{form.friday15}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">18:00</div>
                                <div class="v2">{{form.friday18}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">21:00</div>
                                <div class="v2">{{form.friday21}}</div>
                            </div>
                            <div class="title" style="background-color: #FFEEFA">星期六</div>
                            <div class="flex value">
                                <div class="v1">06:00</div>
                                <div class="v2">{{form.saturday6}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">09:00</div>
                                <div class="v2">{{form.saturday9}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">12:00</div>
                                <div class="v2">{{form.saturday12}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">15:00</div>
                                <div class="v2">{{form.saturday15}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">18:00</div>
                                <div class="v2">{{form.saturday18}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">21:00</div>
                                <div class="v2">{{form.saturday21}}</div>
                            </div>
                            <div class="title" style="background-color: #EEF4FF">星期日</div>
                            <div class="flex value">
                                <div class="v1">06:00</div>
                                <div class="v2">{{form.sunday6}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">09:00</div>
                                <div class="v2">{{form.sunday9}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">12:00</div>
                                <div class="v2">{{form.sunday12}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">15:00</div>
                                <div class="v2">{{form.sunday15}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">18:00</div>
                                <div class="v2">{{form.sunday18}}</div>
                            </div>
                            <div class="flex value">
                                <div class="v1">21:00</div>
                                <div class="v2">{{form.sunday21}}</div>
                            </div>
                            <div class="title" style="background-color: #FF9D38; color: white;" >{{form.title1}}</div>
                            <div class="value_" v-html="form.content1"></div>
                            <div class="title" style="background-color: #FF9D38; color: white;" >{{form.title2}}</div>
                            <div class="value_" v-html="form.content2"></div>
                        </div>
                        <div v-if="type == 2">
                            <div class="flex value">
                                <div class="v1" style="width: 20%; background-color: #EEFEFF">食物名称</div>
                                <div class="v2" style="width: 80%; background-color: #EEFEFF">营养素成分</div>
                            </div>
                            <div class="flex value" v-for="(item,index) in form">
                                <div class="v1" style="width: 20%">{{item.name}}</div>
                                <div class="v2" style="width: 80%">{{item.value}}</div>
                            </div>
                        </div>
                    </div>
                </div>
                <div style="height: 50px;"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "NutritionalGuidanceDetail",
    data() {
        return {
            printObj: {
                id: "print",//要打印的id名 无#号
                popTitle:'&nbsp;',//页眉标题 默认浏览器标题 空字符串时显示undefined 使用html语言
                extraHead:'&nbsp;',//头部文字 默认空
            },
            dialogReport      : false,
            weekInfo_list:'',
            form:{},
            type:'',
            age:'',
        };
    },
    computed: {},
    watch: {},
    mounted() {
        //消息接口
        this.$store.dispatch('GetXiao')
        this.onWeekInfo()
        this.type = this.$route.query.type
        this.age = this.$route.query.age
    },
    methods: {
        onWeekInfo(){
            this.$axios.post('api/tool/diet_info', {
                age: this.$route.query.age,
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
    height: 55px;
    line-height: 55px;
    text-align: center;
    color: #333333;
    font-size: 16px;
    font-weight: 500;
}
.value {
    border-bottom: 1px solid #E6E6E6;
    .v1 {
        width:15%;
        text-align: center;
        line-height: 54px;
        border-right: 1px solid #E6E6E6;
        font-weight: bold;
    }
    .v2 {
        width:85%;
        padding-left: 20px;
        line-height: 54px;
        color: #252525;
        font-weight: bold;
    }
}
.value_ {
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
    background: #20BE18;
}
.table_content_footer{
    padding: 25px;
}
</style>
