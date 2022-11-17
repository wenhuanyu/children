<template>
    <div class="huaban">
        <div class="content_ ">
            <div class="left">
                <img class="img" :src=img alt="">
            </div>
            <div class="right_box">
                <p class="title">绘人智能分数评定</p>
                <div class="right_bottom">
                    <div v-for="(item,index) in form" class="flex" style="margin-top: 20px; margin-bottom: 20px; border-bottom: 1px solid #666666; ">
                        <div style="width:80px;margin-right: 5px; font-size: 16px; color: #FFFFFF;" v-html="item.title"></div>
                        <div style="width: 420px;">
                            <el-radio-group v-model="radioArray[index]" @change="onChange(index)">
                                <el-radio v-for="opt in item.answers" border @change="handleRadioChanges(item.id,opt.id)" :key="opt.id" :label="opt.id" style="border: none; width: 300px;
                            color: #FFFFFF; padding-left: 0px;">
                                    {{opt.title}}
                                </el-radio>
                            </el-radio-group>
                        </div>
                    </div>
                </div>
                <div class="btns" @click="tijiao">
                    <a class="wancheng">完成测试</a>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: "TerminalDrawingBoard",
    data() {
        return {
            img:'',
            form:'',
            radioArray:[],//这个是必须的，你选中的每一题的数据都会存在这个数组里去
            shoarr:[],//这个是事先命名的一个数组,
            music_path: '',
            music_time: '',
            music_cover: '',
            music_name: '',
            prompt_message:'',
        }
    },
    mounted() {
        this.img = this.$route.query.img
        console.log('this.$route.query',this.$route.query)
        this.getInfo()
    },
    methods:{
        getInfo() {
            this.$axios.post('term/user/dpt_questions', {
                test_paper_id:this.$route.query.test_paper_id
            }).then(res => {
                if (res.data.code == 1) {
                    this.form = res.data.data
                }
            })
        },
        onChange() {},
        handleRadioChanges(index,strnum){//index,选中题目的下标，strnum你选中的那个选项，实际上也是个index
            let data = {name:0,answer:0};
            data.name = index;
            data.answer = parseInt(strnum);
            this.shoarr.push(data);//将我选中的哪一题，哪个选项存到实现准备好的数组shoarr
            let myarr=this.shoarr;
            for(var i=0;i<myarr.length;i++){
                for(var j=i+1;j<myarr.length;j++){
                    if(myarr[i].name==myarr[j].name){
                        myarr.splice(i,1);
                        j--;
                    }
                }
            }//数组去重，以最后一个选中为准
            this.shoarr=myarr;
        },
        tijiao() {
            this.$axios.post('term/user/dpt_answer', {
                test_paper_id:this.$route.query.test_paper_id,
                exam_id:this.$route.query.exam_id,
                answer_ids:this.shoarr
            }).then(res => {
                    if (res.data.code == 1 ) {
                        this.$message.success(res.data.info);
                        this.examInfo = this.$store.state.examObj;
                        this.music_path = this.$store.state.examObj.music_path
                        this.music_time = this.$store.state.examObj.music_time
                        this.music_name = this.$store.state.examObj.music_name
                        this.music_cover = this.$store.state.examObj.music_cover
                        this.test_paper_id = this.$route.query.test_paper_id || this.examInfo.test_paper_id;
                        this.prompt_message = this.examInfo.examInfo.prompt_message
                        this.$router.push(
                            {
                                name: 'TerminalEnd',
                                query: {
                                    exam_id: this.$route.query.exam_id,
                                    type: this.examInfo.practiceType,
                                    music_path: this.music_path,
                                    music_time: this.music_time,
                                    music_cover: this.music_cover,
                                    music_name: this.music_name,
                                    prompt_message: this.prompt_message
                                }
                            }
                        )
                    }
            })
        }
    }
}
</script>

<style lang="scss">
.el-radio__input.is-checked + .el-radio__label {
    color: #20BE18;
}
.el-radio__input.is-checked .el-radio__inner {
    background: #20BE18;
    border-color: #20BE18;
}
.el-radio.is-bordered {
    margin-left: 10px;
}
.huaban{
    width: 100%;
    height: 100vh;
    overflow: hidden;
    background-color: #000;
}
.content_{
    display: flex;
    align-items: flex-start;
    padding: 15px;
}
.left .img{
    display: block;
    margin: 0 auto;
    width: 1375px;
    height: 785px;
}
.left{
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 1375px;
    height: 96vh;
    background: #fff;
    margin-right: 15px;
}
.right_box{
    width: 500px;
    height: 90vh;
}
.right_box .title{
    font-weight: bold;
    font-size: 20px;
    color: #fff;
    margin:0 0 20px 0;
}
::-webkit-scrollbar{
    display: none;
}
.right_bottom{
    height: 83vh;
    max-height: 900px;
    overflow-y: scroll;
    background: #4B4B4B;
    border-radius: 5px;
    padding: 0 20px;
}
.right_box .item{
    padding: 20px 0;
    border-bottom: 1px solid #666666;
    display: flex;
    align-items: flex-start;
    color: #fff;
    font-size: 16px;
}
.right_box .item span{
    width: 50px;
    margin-right: 20px;
}
.right_box .item div div{
    display: flex;
    align-items: center;
    margin-bottom: 20px;
}
.right_box .item div div:last-child{
    margin: 0;
}
.right_box .item div input{
    width: 20px;
    height: 20px;
    margin:0 10px 0 0;
}
.right_box .btns{
    display: flex;
    align-items: center;
    justify-content: flex-end;
    margin-top: 20px;
}
.right_box .btns a{
    text-decoration: none;
    width: 150px;
    height: 45px;
    line-height: 45px;
    text-align: center;
    background: #20BE18;
    border-radius: 5px;
    color: #fff;
    font-size: 20px;
}
</style>
