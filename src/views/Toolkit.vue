<template>
  <div class="content_body">
      <div class="hezi">
          <router-link to="/HeightPrediction" v-if="check('儿童未来身高预测')">
              <div class="img_">
                    <img src="../assets/images/shengaoyuce.png" alt=""  style="height: 359px">
                    <div class="top">儿童未来身高预测</div>
                </div>
          </router-link>
          <router-link to="/ProprietaryChineseMedicine" v-if="check('儿童常用中成药数据库查询')">
              <div class="img_">
                  <img src="../assets/images/zhongxhengyao.png" alt="" style="height: 359px">
                  <div class="top_">儿童常用中成药数据库查询</div>
              </div>
          </router-link>
          <router-link to="/SmartGrowth" v-if="check('儿童智能开发指导')">
              <div class="img_">
                  <img src="../assets/images/zhinengkaifa.png" alt="" style="height: 359px">
                  <div style="color: #2AB2FF" class="bottom">儿童智能开发指导</div>
              </div>
          </router-link>
          <router-link to="/NutritionalGuidance" v-if="check('儿童膳食营养指导')">
              <div class="img_">
                  <img src="../assets/images/shanshiyingyang.png" alt="" style="height: 359px">
                  <div style="color: #FF6B6B" class="bottom">儿童膳食营养指导</div>
              </div>
          </router-link>
          <router-link to="/DevelopmentGuidance" v-if="check('儿童发育指导')">
              <div class="img_">
                  <img src="../assets/images/fayuzhidao.png" alt="" style="height: 359px">
                  <div style="color: #1FD89E" class="bottom_">儿童发育指导</div>
              </div>
          </router-link>
      </div>
  </div>
</template>

<script>
export default {
  name: "Toolkit",
  data() {
    return {};
  },
  computed: {},
  watch: {},
    mounted() {
        //获取权限列表
        this.$store.dispatch('GetAuth')
        //消息接口
        this.$store.dispatch('GetXiao')
    },
  methods: {
      //检测按钮权限
      check(_name) {
          let show = false
          this.$store.state.auth.map(item => {
              if (item.name === this.$store.state.cmenu) {
                  let child = item.child
                  child.map(item2 => {
                      if (_name === item2.name) {
                          show = true
                      }
                  })
              }
          })
          return show
      },
  },
}
</script>

<style scoped lang="scss">
.content_body {
  padding-left: 200px;
  padding-top: 30px;
  font-size: 28px;
  font-weight: 400;
  line-height: 60px;
    .hezi{
         width: 1000px;
        display: flex;
        //justify-content: space-between;
        flex-wrap: wrap;

    }

  .img_ {
      position: relative;
  }
  .top {
      z-index: 100;
      position: absolute;
      top: 70%;
      left: 24%;
      color: #F28829;
  }
  .top_ {
      z-index: 100;
      position: absolute;
      top: 70%;
      left: 18%;
      color: #72BB1B;
  }
    .bottom {
        z-index: 100;
        position: absolute;
        top: 70%;
        left: 14%;
    }
    .bottom_ {
        z-index: 100;
        position: absolute;
        top: 70%;
        left: 22%;
    }
}
</style>
