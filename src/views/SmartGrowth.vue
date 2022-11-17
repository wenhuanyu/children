<template>
    <div class="content_body">
        <span>请选择儿童月龄</span>
        <el-select v-model="value" placeholder="请选择" @change="onWeekInfo($event)">
            <el-option
                v-for="item in options"
                :key="item.id"
                :label="item.age"
                :value="item.id"
            >
            </el-option>
        </el-select>
        <span style="margin-left: 30px;">请选择观察领域</span>
        <el-select v-model="value_" placeholder="请选择" @change="onWeekInfo_($event)">
            <el-option
                v-for="item in options_"
                :key="item.id"
                :label="item.name"
                :value="item.id"
            >
            </el-option>
        </el-select>
        <el-button type="primary" round size="small" class="confirm" @click="weekInfo">确认
        </el-button>
    </div>
</template>

<script>
export default {
    name: "SmartGrowth",
    data() {
        return {
            value: '',
            value_: '',
            options: [],
            options_: [
                {id:1,name:"语言能力"},
                {id:2,name:"认知能力"},
                {id:3,name:"观察动作"},
                {id:4,name:"大运动"},
                {id:5,name:"社会交往"},
            ],
            week_id:'',
            week_id_:''
        };
    },
    computed: {},
    watch: {},
    mounted() {
        //消息接口
        this.$store.dispatch('GetXiao')
        this.onWeek()
    },
    methods: {
        onWeek() {
            this.$axios.post('api/tool/brain_age_lst').then(res => {
                if (res.data.code === 1) {
                    this.options = res.data.data
                }
            })
        },
        onWeekInfo(e){
            this.week_id = e
        },
        onWeekInfo_(e) {
            this.week_id_ = e
        },
        weekInfo(){
            if(this.value === ''){
                this.$message({
                    message: '请选择儿童月龄'
                })
                return;
            }
            if(this.value_ === ''){
                this.$message({
                    message: '请选择观察领域'
                })
                return;
            }
            this.$router.push({name:'SmartGrowthDetail', query:{age_id:this.week_id,type:this.week_id_}})
        }
    },
}
</script>

<style scoped lang="scss">
.content_body {
    padding: 100px;

    > span {
        color: #333333;
        font-size: 22px;
        font-weight: 600;
        margin-right: 20px;
    }

}

.confirm {
    margin-left: 20px;
    height: 35px;
    width: 100px;
    line-height: 16px;
    background: #FE9834;
    border-color: #FE9834;
}

</style>
