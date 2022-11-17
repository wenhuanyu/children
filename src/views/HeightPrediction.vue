<template>
    <div class="content_body">
        <div class="title">儿童未来身高预测</div>
        <el-form label-width="160px">
            <div style="width: 40%; margin-left: 20%;  margin-top: 50px;">
                <el-form-item label="父亲身高" prop="father_height">
                    <div class="flex">
                        <el-input v-model="father_height"></el-input>
                        <div style="margin-left: 5px;">cm</div>
                    </div>
                </el-form-item>
                <el-form-item label="母亲身高" prop="mother_height">
                    <div class="flex">
                        <el-input v-model="mother_height"></el-input>
                        <div style="margin-left: 5px;">cm</div>
                    </div>
                </el-form-item>
                <el-form-item label="儿童性别" prop="gender">
                    <el-radio-group v-model="gender">
                        <el-radio :label="0" name="0">男</el-radio>
                        <el-radio :label="1" name="1">女</el-radio>
                    </el-radio-group>
                </el-form-item>
            </div>
        </el-form>
        <div class="flex" style="margin-left: 30%; margin-top: 80px;">
            <el-button type="primary" round size="small" class="confirm" style="background-color: #20BE18; border-color: #20BE18; margin-right: 40px" @click="weekInfo">
                预测
            </el-button>
            <el-button type="primary" round size="small" class="confirm"  @click="onWeekInfo">
                清空
            </el-button>
        </div>
        <div style="margin-left: 30%; margin-top: 40px; font-size: 15px; color: #333333; font-weight: 400" v-if="xianshi">
            预测身高
            <span style="font-size: 43px; color: #20BE18; font-weight: bold; margin-left: 10px;">{{height}}</span>
        </div>
    </div>
</template>

<script>
export default {
    name: "HeightPrediction",
    data() {
        return {
            value: '',
            options: [],
            week_id:'',
            father_height:'',
            mother_height:'',
            height:'',
            gender:'',
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
            this.$axios.post('api/common/week_lst').then(res => {
                if (res.data.code === 1) {
                    this.options = res.data.data
                }
            })
        },
        onWeekInfo(){
            this.mother_height = ''
            this.father_height = ''
            this.gender = ''
            this.height = ''
            this.xianshi = false
        },
        weekInfo(){
            if(this.father_height === ''){
                this.$message({
                    message: '请输入父亲身高'
                })
                return;
            }
            if(this.mother_height === ''){
                this.$message({
                    message: '请输入母亲身高'
                })
                return;
            }
            if(this.gender === ''){
                this.$message({
                    message: '请输入儿童性别'
                })
                return;
            }
            if (this.gender == 0) {
                this.height = Math.round((Number(this.father_height) + Number(this.mother_height)) * 1.08 / 2) + 'cm'
                this.xianshi = true
            } else {
                this.height = Math.round(((Number(this.father_height)*0.923)+Number(this.mother_height))/2) + 'cm'
                this.xianshi = true
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
        margin-left: 28%;
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
