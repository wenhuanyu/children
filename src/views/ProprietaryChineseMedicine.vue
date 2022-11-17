<template>
    <div class="content_body">
        <div class="title">儿童常用中成药数据库查询</div>
        <div style="margin-left: 15%; margin-top: 50px;">
            <el-select v-model="value" placeholder="请选择" @change="onWeekInfo($event)">
                <el-option
                    v-for="item in options"
                    :key="item.id"
                    :label="item.name"
                    :value="item.id"
                >
                </el-option>
            </el-select>
            <el-button type="primary" round size="small" class="confirm" @click="weekInfo">确认
            </el-button>
        </div>
        <div style="width: 500px; margin-left: 15%; margin-top: 50px; " v-if="xianshi">
            <div style="margin-bottom: 20px;">
                <span style="font-size: 15px; font-weight: 400; color: #333333; margin-right: 20px;">主要功能</span>
                <span style="font-size: 18px; font-weight: bold; color:  #333333;">{{form.main_function}}</span>
            </div>
            <div style="margin-bottom: 20px;">
                <span style="font-size: 15px; font-weight: 400; color: #333333; margin-right: 20px;">剂型规格</span>
                <span style="font-size: 18px; font-weight: bold; color:  #333333;">{{form.spec}}</span>
            </div>
            <div style="margin-bottom: 30px;">
                <span style="font-size: 15px; font-weight: 400; color: #333333; margin-right: 20px;">主要成分</span>
                <span style="font-size: 18px; font-weight: bold; color:  #333333;">{{form.component}}</span>
            </div>
            <div style="width: 500px; color: #7E7E7E">----------------------------------------------------------------------------------</div>
            <div style="margin-top: 30px;">
                <span style="font-size: 15px; font-weight: 400; color: #333333; margin-right: 20px;">用法用量</span>
                <span style="font-size: 18px; font-weight: bold; color:  #333333;">{{form.usage}}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "ProprietaryChineseMedicine",
    data() {
        return {
            value: '',
            options: [],
            week_id:'',
            father_height:'',
            mother_height:'',
            height:'',
            gender:'',
            form:{},
            xianshi:false
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
            this.$axios.post('api/tool/drug_lst').then(res => {
                if (res.data.code === 1) {
                    this.options = res.data.data
                }
            })
        },
        onWeekInfo(e){
            this.week_id = e
        },
        weekInfo(){
            if(this.value === ''){
                this.$message({
                    message: '请选择中成药'
                })
            }else {
                this.$axios.post('api/tool/drug_info',
                    {id: this.week_id}
            ).then(res => {
                    if (res.data.code === 1) {
                        this.form = res.data.data
                        if (this.form) {
                            this.xianshi = true
                        }
                        console.log('this.form',this.form)
                    }
                })
            }
        }
    },
}
</script>

<style scoped lang="scss">
::v-deep {
    .el-form-item__label {
        font-size: 15px;
        color: #333333;
        font-weight: 400;
    }
    .el-radio__input.is-checked + .el-radio__label {
        color: #20BE18;
    }
    .el-radio__input.is-checked .el-radio__inner {
        background: #20BE18;
        border-color: #20BE18;
    }
}

.content_body {
    padding: 100px;
    .title {
        color: #333333;
        font-size: 24px;
        font-weight: 600;
        margin-left: 15%;
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
