<template>
    <div id="AddPatient" class="content">
        <div class="box">
            <el-form ref="form" :rules="rules" :model="formData" label-width="160px">
                <div class="form" v-show="pageIndex==1" style="display: flex ">
                    <div style="width: 50%">
                        <el-form-item label="姓名" prop="name">
                            <el-input v-model="formData.name"></el-input>
                        </el-form-item>
                        <el-form-item label="性别" prop="gender">
                            <el-radio-group v-model="formData.gender">
                                <el-radio :label="0" name="0">男</el-radio>
                                <el-radio :label="1" name="1">女</el-radio>
                            </el-radio-group>
                        </el-form-item>
                        <el-form-item label="户口类型" prop="hukou">
                            <el-radio-group v-model="formData.hukou">
                                <el-radio :label="1" name="1">农村户口</el-radio>
                                <el-radio :label="2" name="2">城市户口</el-radio>
                            </el-radio-group>
                        </el-form-item>
                        <el-form-item label="出生日期" prop="birth">
                            <el-date-picker type="date" placeholder="选择日期" style="width:100%" v-model="formData.birth"
                                            @change="birthday"
                                            value-format="yyyy-MM-dd"></el-date-picker>
                        </el-form-item>
                        <el-form-item label="年龄">
                            <el-input v-model="formData.age" readonly></el-input>
                        </el-form-item>
                        <el-form-item label="入院时间" >
                            <el-date-picker type="date" placeholder="选择日期" style="width:100%" value-format="yyyy-MM-dd"
                                            v-model="formData.in_time"></el-date-picker>
                        </el-form-item>
                        <el-form-item label="病历号" prop="medical_num">
                            <el-input v-model="formData.medical_num"></el-input>
                        </el-form-item>
                        <el-form-item label="医保号" prop="medical_insurance_num">
                            <el-input v-model="formData.medical_insurance_num"></el-input>
                        </el-form-item>
                        <el-form-item label="身份证号" prop="idcard">
                            <el-input
                                v-model="formData.idcard"
                                @input="
                                    formData.idcard=formData.idcard.replace(
                                        /[^\X0-9]/g,
                                        ''
                                    )"
                            ></el-input>
                        </el-form-item>
                        <el-form-item label="患者来源" prop="source">
                            <div style="display: flex;width: 100%;">
                                <el-select v-model="department_name" placeholder="请选择" @change="getRole($event)">
                                    <el-option v-for="item in stateArr" :key="item.id" :label="item.name"
                                               :value="item.name"></el-option>
                                </el-select>
                                <el-select :disabled="disabled" v-model="formData.source" placeholder="请选择" style="margin-left: 10px">
                                    <el-option v-for="(item,index) in select2" :key="item.id" :label="item.name"
                                               :value="item.name"></el-option>
                                </el-select>
                            </div>
                        </el-form-item>

                    </div>
                    <div style="width: 50%">
                        <el-form-item label="民族"  prop="nation_id">
                            <el-select v-model="formData.nation_id" placeholder="请选择" style="width: 100%;">
                                <el-option v-for="item in nationalList" :key="item.id" :label="item.name"
                                           :value="item.id"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="生产史">
                            <div style="" class="flex">
                                <div style="width: 75px; margin-right: 3px">胎次</div>
                                <el-input v-model="formData.taici"></el-input>
                                <div style="width: 75px; margin-left: 10px; margin-right: 3px" >产次</div>
                                <el-input v-model="formData.chanci"></el-input>
                            </div>
                        </el-form-item>
                        <el-form-item label="">
                            <el-radio-group v-model="formData.scs_radio">
                                <el-radio :label="1" name="1">足月</el-radio>
                                <el-radio :label="2" name="2">早产</el-radio>
                                <el-radio :label="3" name="3">顺产</el-radio>
                                <el-radio :label="4" name="4">难产</el-radio>
                            </el-radio-group>
                        </el-form-item>
                        <el-form-item label="窒息">
                            <el-checkbox-group v-model="formData.zhixi">
                                <el-checkbox :label="1">青紫</el-checkbox>
                                <el-checkbox :label="2">苍白</el-checkbox>
                                <el-checkbox :label="3">有无产伤（颅内出血）</el-checkbox>
                            </el-checkbox-group>
                        </el-form-item>
                        <el-form-item label="">
                            <div class="flex">
                                <div style="width: 100px">持续时间</div>
                                <el-input v-model="formData.zhixi_time" placeholder="(单位：分钟)"></el-input>
                            </div>
                        </el-form-item>
                        <el-form-item label="出生体重">
                            <el-input v-model="formData.birth_weight" placeholder="(单位：kg)"></el-input>
                        </el-form-item>
                        <el-form-item label="畸形">
                            <el-input v-model="formData.malformation"></el-input>
                        </el-form-item>
                        <el-form-item label="现在">
                            <div style="" class="flex">
                                <div style="width: 116px; margin-right: 3px">身长</div>
                                <el-input v-model="formData.now_height" style="width: 90%" placeholder="cm"></el-input>
                                <div style="width: 116px; margin-left: 10px; margin-right: 3px" >体重</div>
                                <el-input v-model="formData.now_weight" style="width: 90%" placeholder="kg"></el-input>
                                <div style="width: 116px; margin-left: 10px; margin-right: 3px" >头围</div>
                                <el-input v-model="formData.now_head" style="width: 90%" placeholder="cm"></el-input>
                            </div>
                        </el-form-item>
                        <el-form-item label="">
                            <div class="flex">
                                <div style="font-size: 18px; line-height: 18px; margin-right: 15px; color: #606266">有无抽风</div>
                                <el-radio-group v-model="formData.covulsions">
                                    <el-radio :label="1" name="0">有</el-radio>
                                    <el-radio :label="2" name="1">无</el-radio>
                                </el-radio-group>
                            </div>

                        </el-form-item>
                        <el-form-item label="">
                            <div class="flex">
                                <div style="font-size: 18px; line-height: 18px; margin-right: 15px; color: #606266">有无卤门</div>
                                <el-radio-group v-model="formData.fontanelle">
                                    <el-radio :label="1" name="0">有</el-radio>
                                    <el-radio :label="2" name="1">无</el-radio>
                                </el-radio-group>
                            </div>

                        </el-form-item>
                        <el-form-item label="">
                            <div class="flex">
                                <div style="font-size: 18px; line-height: 18px; margin-right: 15px; color: #606266">有无黄疸</div>
                                <el-radio-group v-model="formData.jaundice">
                                    <el-radio :label="1" name="1">病理性</el-radio>
                                    <el-radio :label="2" name="2">生理性</el-radio>
                                    <el-radio :label="3" name="0">无</el-radio>
                                </el-radio-group>
                            </div>

                        </el-form-item>
                    </div>
                </div>
            </el-form>
            <div class="margin-top box-nowrap footer">
                <el-button round class="cancel" @click="$router.go(-1)" style="border: none!important;">取消</el-button>
                <el-button round class="save" @click="submit" style="border: none!important;">{{ $route.query.id ? '保存' : '添加' }}</el-button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "AddPatient",
    data() {
        return {
            readonly:true,
            stateArr: [],
            maritalStatusArr:[],
            educationInformationArr:[],
            careerInformationArr:[],
            pageIndex: 1,
            formData: {
                age: '',
                in_time: '',
                zhixi:[],
                idcard:'',
                source:'',
            },
            department_name:'',
            backup: {},
            zhixiSelect: [],
            rules: {
                name: [
                    {required: true,message: '请输入姓名', trigger: 'blur'}
                ],
                gender: [
                    {required: true, message: '请选择性别'}
                ],
                hukou:[
                    {required:true,message:'请选择户口类型'}
                ],
                medical_num: [
                    {required: true,message: '请输入病历号'}
                ],
                medical_insurance_num: [
                    {required: true,message: '请输入医保号'}
                ],
                idcard: [{
                    // required: true,
                    pattern:/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/,
                    message: '请输入正确的身份证号'
                }],
                birth: [
                    {required: true,message: '请选择出生日期'}
                ],
                source: [{
                    required: true,
                    message: '请输入患者来源'
                }],
            },
            wardList: [],
            page_two:true,
            page_One:false,
            select2:[],
            disabled:true,
            nationalList:[],
        };
    },
    computed: {},
    watch: {},
    async mounted() {
        //消息接口
        this.$store.dispatch('GetXiao')
        await this.getWard();
        if (this.$route.query.id) {
            this.getInfo()
        }
        sessionStorage.setItem("detail", true);
    },
    methods: {
        //患者来源选择
        getRole (prov) {
            this.select2= []
            for (var val of this.stateArr) {
                if (prov===val.name) {
                    this.select2 = val.child
                }
            }
            this.disabled = false;
            this.formData.source = ''
        },
        addItem(name) {
            this.formData[name].push('')
        },
        submit() {
            this.$refs['form'].validate((valid) => {
                if (valid) {
                    let formData = {...this.formData};
                    if (!this.$route.query.id) {
                        //添加
                        console.log('formData',formData)
                        this.$axios.post('api/patient/add', this.$qs.stringify(formData)).then(res => {
                            if (res.data.code === 1) {
                                this.$router.go(-1)
                            }
                        })
                    } else {
                        //编辑
                        let form = {id: formData.id, name: formData.name};
                        console.log('this.backup',this.backup)
                        for (let backupKey in this.backup) {
                            if (typeof this.backup[backupKey] == 'object') {
                                form[backupKey] = this.formData[backupKey];
                            } else if (this.backup[backupKey] !== this.formData[backupKey]) {
                                form[backupKey] = this.formData[backupKey];
                            }
                        }
                        this.$axios.post('api/patient/edit', this.$qs.stringify(form)).then(res => {
                            console.log('form',form)
                            if (res.data.code === 1) {
                                this.$router.go(-1)
                            }
                        })
                    }
                }
            });
        },
        getInfo() {
            this.$axios.post('api/patient/info', {id: this.$route.query.id}).then(res => {
                var data = res.data.data
                this.formData = data
                this.department_name = data.department_name
                for (var val of this.stateArr) {
                    if (this.department_name===val.name) {
                        this.select2 = val.child
                    }
                }
                this.disabled = false
                if (this.formData.zhixi !=='') {
                    this.formData.zhixi = this.formData.zhixi.map(item => Number(item))
                } else {
                    this.formData.zhixi = []
                }
                if(this.formData.taici == 0) {
                    this.formData.taici = ''
                }
                if(this.formData.chanci == 0) {
                    this.formData.chanci = ''
                }
                if(this.formData.birth_weight == 0) {
                    this.formData.birth_weight = ''
                }
                if(this.formData.nation_id == 0) {
                    this.formData.nation_id = ''
                }
                this.backup = {...data};
            })
        },
        async getWard() {
            await this.$axios.post('api/district/nation').then(res => {
                this.nationalList = res.data.data;

            })
            await this.$axios.post('api/district/index', {type: 1}).then(res => {
                this.stateArr = res.data.data;
            })
        },
        birthday(e) {
            this.formData.age = this.getAge(e.split('-'))[0];
            if(this.formData.age < 0){
                this.formData.age = 0
            }
        },
        getAge(birthday) {
            // 新建日期对象
            let date = new Date()
            // 今天日期，数组，同 birthday
            let today = [date.getFullYear(), date.getMonth() + 1, date.getDate()]
            // 分别计算年月日差值
            let age = today.map((value, index) => {
                return value - birthday[index]
            })
            // 当天数为负数时，月减 1，天数加上月总天数
            if (age[2] < 0) {
                // 简单获取上个月总天数的方法，不会错
                let lastMonth = new Date(today[0], today[1], 0)
                age[1]--
                age[2] += lastMonth.getDate()
            }
            // 当月数为负数时，年减 1，月数加上 12
            if (age[1] < 0) {
                age[0]--
                age[1] += 12
            }
            return age
        }
    },
}
</script>

<style scoped lang="scss">


.tip {
    padding-left: 100px;
    line-height: 48px;
    color: #20BE18;
    background: #FFF3F3;
    margin-bottom: 20px;
}

.footer {
    display: flex;
    justify-content: flex-end;
}

.content {
    padding: 0 35px;
    background-color: #FFF;
}

.box {
    margin: 40px;
}

.form {
    /*display: flex;*/
    /*flex-wrap: wrap;*/
    /*justify-content: space-between;*/
    width: 100%;
}

.form-box {
    justify-content: space-between;
    display: flex;

    ::v-deep .el-form {
        width: 40%;
    }

}

::v-deep .el-form-item {
    /*width: 40%;*/
}

.cancel {
    height: 50px;
    width: 150px;
    text-align: center;
    border-radius: 50px;
    color: #FFF;
    font-size: 20px;
    background: #FE9834;
    box-shadow: 4px 7px 10px 0px rgba(245, 96, 90, 20%);
}

.save {
    height: 50px;
    width: 150px;
    text-align: center;
    border-radius: 50px;
    color: #FFF;
    font-size: 20px;
    background: #20BE18;
    box-shadow: 4px 7px 10px 0px rgba(243, 147, 53, 20%);
}
::v-deep .el-select .el-input__inner:focus {
    border-color: #20BE18;
}
::v-deep .el-select .el-input.is-focus .el-input__inner {
    border-color: #20BE18;
}
::v-deep .el-form-item.is-error .el-input__inner, .el-form-item.is-error .el-input__inner:focus, .el-form-item.is-error .el-textarea__inner, .el-form-item.is-error .el-textarea__inner:focus {
    border-color: #20BE18;
}
::v-deep .el-form-item__error {
    color: #20BE18;
}
::v-deep .el-radio__input.is-checked + .el-radio__label {
    color: #20BE18;
}
::v-deep .el-radio__input.is-checked .el-radio__inner {
    background: #20BE18;
    border-color: #20BE18;
}
::v-deep .el-form-item.is-required:not(.is-no-asterisk) > .el-form-item__label:before, .el-form-item.is-required:not(.is-no-asterisk) .el-form-item__label-wrap > .el-form-item__label:before {
    color: #20BE18;
}
::v-deep .el-date-table td.current:not(.disabled) span {
    background: #20BE18;
}
.el-select-dropdown__item.selected {
    color: #20BE18;
}
::v-deep .el-checkbox__input.is-checked + .el-checkbox__label {
    color: #20BE18;
}
::v-deep .el-checkbox__input.is-checked .el-checkbox__inner {
    background: #20BE18;
    border-color: #20BE18;
}
::v-deep .el-form-item__label {
    font-size: 18px;
}

::v-deep .el-radio__label {
    font-size: 18px;
}

.box {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100%;
}

::v-deep .el-form-item__content {
    width: 30%;
    min-width: 400px;
}

::v-deep .el-form--inline {
    display: flex;
    justify-content: space-between;

}

::v-deep .el-form--inline .el-form-item {
    width: 50%;
}

::v-deep .el-form--inline .el-input__inner {
    width: 300px;
}

::v-deep .add-input .el-input__inner {
    width: 230px;
    display: inline-block;
}

.add-input {
    margin-bottom: 10px;
}

::v-deep .el-form-item {
    position: relative;
}

.add-btn {
    width: 40px;
    height: 40px;
    background: #F2F2F2;
    border-radius: 2px;
    border: 1px solid #D2D2D2;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    top: 0;
    right: 0;
}

.drop-down {

}
</style>
