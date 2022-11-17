<template>
  <div class="huaban">
    <div class="content_">
      <div class="container">
        <div class="row">
          <ul name="" class="type">
            <li :class="{'typeactive':item.type===type,'poly':item.type==='poly'}" @click="clicType(item)"
              v-for="(item,index) in typeList" :key="index">
              <span v-html="item.name"></span>
              <div class="bian" v-if="item.type === 'poly' "><span>边数</span><input type="number" v-model="n" min="3"
                  max="15"></div>
            </li>
          </ul>
          <ul class="style">
            <li class="stroke" :class="{'styleactive':style==='stroke'}" @click="clickStyle('stroke')">
              描&nbsp;&nbsp;&nbsp;边</li>
            <li class="fill" :class="{'styleactive':style==='fill'}" @click="clickStyle('fill')"
              v-if=" type !== 'line' && type !== 'pen'">填&nbsp;&nbsp;&nbsp;充</li>
          </ul>
          <div class="space"></div>
          <div class="box"><span>颜色</span><input type="color" v-model="color"></div>
          <div class="box"><span>粗细</span><input type="number" name="number" v-model="linewidth" max="150" min="1">
          </div>
          <div class="space"></div>
          <input class="xinjian_width" type="hidden" max="700" min="500" value="500">
          <input class="xinjian_height" type="hidden" max="648" min="300" value="300">
          <input style="display: none;" type="button" name="queding" value="确定" id="ding">
          <div class="clear shezhi" @click="clickClear">清&nbsp;&nbsp;&nbsp;空</div>
          <div class="back shezhi" @click="clickRevoke">撤&nbsp;&nbsp;&nbsp;销</div>
        </div>
        <div class="save shezhi" @click="clickGetImg">生成图片</div>
      </div>
      <canvas class="canvas-box" id="canvasBox" @mousedown="mousedown"></canvas>
    </div>
  </div>
</template>
<script>
  import {
    Draw
  } from '@/utils/huaban'
  let x, y, w, h;
  let lx, ly, lw, lh;
  let cutdata;
  export default {
    name: "TerminalHuaBan",
    data() {
      return {
        canvasBox: null,
        canvasObj: null,
        width: 0,
        height: 0,
        type: 'pen', // 画笔类型
        typeList: [ // 画笔类型列表
          {
            name: '铅&nbsp;&nbsp;&nbsp;笔',
            type: 'pen'
          },
          {
            name: '直&nbsp;&nbsp;&nbsp;线',
            type: 'line'
          },
          {
            name: '矩&nbsp;&nbsp;&nbsp;形',
            type: 'rect'
          },
          {
            name: '圆&nbsp;&nbsp;&nbsp;圈',
            type: 'circle'
          },
          {
            name: '多&nbsp;边&nbsp;形',
            type: 'poly'
          },
          {
            name: '橡&nbsp;&nbsp;&nbsp;皮',
            type: 'eraser'
          },
        ],
        n: 3, // 多边形变数
        color: '#000000', // 颜色
        linewidth: 1, // 线宽
        style: 'stroke', // 填充样式
        arr: [], // 绘制的数组
        cutflag: false,
        iscut:true,
      };
    },
    computed: {},
    watch: {},
    mounted() {
      this.initCanvas()
    },
    methods: {
      initCanvas() {
        const canvasBox = document.getElementById('canvasBox')
        this.canvasBox = canvasBox;
        this.canvasObj = canvasBox.getContext("2d");
        const screenWidth = document.documentElement.clientWidth;
        const screenHeight = document.documentElement.clientHeight;
        this.width = screenWidth - 290;
        this.height = screenHeight - 40;
        canvasBox.width=this.width;
        canvasBox.height=this.height;
      },
      // 点击画笔类型
      clicType(item) {
        this.type = item.type
        if (item.type === 'pen' || item.type === 'line') {
          this.style = 'stroke'
        }
      },
      // 点击样式
      clickStyle(style) {
        this.style = style
      },
      // 点击清空
      clickClear() {
        this.arr = [];
        this.canvasObj.clearRect(0, 0, this.width, this.height);
      },
      // 点击撤销
      clickRevoke() {
        this.arr.pop();
        this.canvasObj.clearRect(0, 0, this.width, this.height);
        if (this.arr.length > 0) {
          this.canvasObj.putImageData(this.arr[this.arr.length - 1], 0, 0, 0, 0, this.width, this.height);
        }
      },
      // 开始绘画
      mousedown(e) {
        x = e.offsetX;
        y = e.offsetY;
        if (this.type == "pen") {
          this.canvasObj.beginPath();
          this.canvasObj.moveTo(x, y);
        }
        if (this.type == "eraser") {
          this.canvasObj.clearRect(x - 5, y - 5, 10, 10);
        }
        if (this.cutflag && this.type == "cut") {
          if (this.arr.length != 0) {
            this.arr.splice(-1, 1);
          }
        }
        var draw = new Draw(this.canvasObj, {
          type: this.style,
          color: this.color,
          width: this.linewidth
        }); //实例化构造函数
        this.canvasBox.onmousemove = (e) => {
          w = e.offsetX;
          h = e.offsetY;
          if (this.type != "eraser") {
            this.canvasObj.clearRect(0, 0, this.width, this.height);
            if (this.arr.length != 0) {
              this.canvasObj.putImageData(this.arr[this.arr.length - 1], 0, 0, 0, 0, this.width, this.height);
            }
          }
          if (this.cutflag && this.type == "cut") {
            if (this.iscut) {
              this.canvasObj.clearRect(lx, ly, lw - lx, lh - ly);
            }
            var nx = lx + (w - x);
            var ny = ly + (h - y);
            this.canvasObj.putImageData(cutdata, nx, ny);
          } else if (this.type == "poly") {
            draw[this.type](x, y, w, h, this.n);
          } else {
            draw[this.type](x, y, w, h);
          }
        }
        document.onmouseup = () => {
          this.canvasBox.onmousemove = null;
          document.onmouseup = null;
          if (this.type == "cut") {
            if (!this.cutflag) {
              this.cutflag = true;
              cutdata = this.canvasObj.getImageData(x + 1, y + 1, w - x - 2, h - y - 2);
              lx = x;
              ly = y;
              lw = w;
              lh = h;
            } else {
              this.cutflag = false;
            }
          }
          this.arr.push(this.canvasObj.getImageData(0, 0, this.width, this.height));
        }
      },
      // 获取图片
      clickGetImg(){
        const src = this.canvasBox.toDataURL("image/png");//跳转页面手动保存
          this.$axios.post('term/user/upload_base64_img', {
              img:src,
              exam_id:this.$route.query.id
          }).then(res => {
              if (res.data.code == 1) {
                  this.$router.push({
                      name: 'TerminalDrawingBoard',
                      query: {
                          img:src,
                          exam_id: this.$route.query.id,
                          test_paper_id: this.$route.query.test_paper_id
                      }
                  });
              }
          })
        // this.downLoad(src)
      },
      downLoad(url){
        var oA = document.createElement("a");
        oA.download = '画布.png';// 设置下载的文件名，默认是'下载'
        oA.href = url;
        document.body.appendChild(oA);
        oA.click();
        oA.remove(); // 下载之后把创建的元素删除
      }
    },
  }
</script>

<style lang="scss">
  .huaban {
    width: 100%;
    height: 100vh;
    overflow: hidden;
    background-color: #000;
  }

  li {
    list-style-type: none;
  }

  ul {
    padding-left: 10px
  }

  h1 {
    position: fixed;
    left: 15px;
    top: 5px;
    width: 250px;
    height: 60px;
    color: #ff5e83;
    font-size: 26px;
    line-height: 60px;
    text-align: center;
  }

  .canvas-box {
    position: fixed;
    left: 280px;
    top: 10px;
    display: block;
    cursor: pointer;
    background-color: #FFFFFF;
    border: 1px solid #CCCCCC;
  }

  .container {
    width: 250px;
    height: 60px;
    position: fixed;
    top: 10px;
  }

  .container h2 {
    width: 100%;
    height: 60px;
    font-size: 22px;
    text-align: center;
    color: #ff377e;
    font-weight: normal;
    line-height: 60px;
    border-bottom: 1px solid #000;
  }

  .container h3 {
    width: 100%;
    height: 30px;
    font-size: 18px;
    text-align: center;
    color: #41b1ff;
    font-weight: normal;
    line-height: 30px;
  }

  .type {
    width: 100%;
    height: auto;
    margin-top: 30px;
    padding-bottom: 10px;
    border-bottom: 1px solid #666;
  }

  .type:after {
    content: "";
    display: block;
    clear: both;
  }

  .type li {
    float: left;
    width: 90px;
    height: 45px;
    margin: 0 auto;
    background-color: #20BE18;
    color: #fff;
    font-size: 18px;
    line-height: 45px;
    text-align: center;
    margin-bottom: 15px;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.7s;
  }

  .type li:hover {
    background-color: #fff;
    color: #20BE18;
  }

  .type .typeactive {
    background-color: #fff;
    color: #20BE18;
  }

  .type li:nth-child(odd) {
    margin-left: 25px;
  }

  .type li:nth-child(even) {
    margin-left: 20px;
  }

  .style {
    width: 100%;
    height: 40px;
    margin-top: 10px;
  }

  .style li {
    float: left;
    width: 90px;
    height: 45px;
    background-color: #FFBC00;
    color: #fff;
    font-size: 18px;
    line-height: 45px;
    text-align: center;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.7s;
  }

  .style li:hover {
    color: #ffa800;
    background-color: #fff;
    box-shadow: 0 0 10px rgba(255, 232, 95, 0.77);
  }

  .style .styleactive {
    color: #ffa800;
    background-color: #fff;
    box-shadow: 0 0 10px rgba(255, 232, 95, 0.77);
  }

  .style li:nth-child(1) {
    margin-left: 25px;
  }

  .style li:nth-child(2) {
    margin-left: 20px;
  }

  .poly {
    position: relative;

    &:hover .bian {
      transform: scale(1.2);
      opacity: 1;
    }
  }

  .bian {
    position: absolute;
    left: 100px;
    top: 0;
    width: 90px;
    height: auto;
    transition: transform 0.3s;
    border-radius: 5px;
    background-color: #EC494E;
    overflow: hidden;
    transform: scale(0);
    opacity: 0;
    // display: none;
  }

  .bian span {
    display: block;
    float: left;
    width: 40px;
    height: 45px;
    font-size: 16px;
    color: #fff;
    text-align: center;
    line-height: 40px;
    color: #fff;
    transition: all 0.7s;
  }

  .bian input {
    float: left;
    width: 45px;
    height: 35px;
    margin-top: 5px;
    border: 1px solid #fff;
    box-sizing: border-box;
    text-align: center;
    line-height: 40px;
    font-size: 18px;
    color: #fff;
    background: #EC494E;
    border-radius: 3px;
    box-shadow: 0 0 0 4px #fff inset;
    transition: all 0.7s;
  }

  .cut,
  .create,
  .back {
    float: left;
    width: 90px;
    height: 45px;
    margin-left: 25px;
    margin-bottom: 15px;
    background-color: #5bd219;
    color: #fff;
    font-size: 18px;
    line-height: 45px;
    text-align: center;
    border-radius: 5px;
    cursor: pointer;
  }

  .copy,
  .clear {
    float: left;
    width: 90px;
    height: 45px;
    margin-left: 20px;
    margin-bottom: 15px;
    background-color: #5bd219;
    color: #fff;
    font-size: 18px;
    line-height: 45px;
    text-align: center;
    border-radius: 5px;
    cursor: pointer;
  }

  .save {
    position: relative;
    color: #fff;
    height: 77px;
    padding-top: 20px;
    background: url("../../assets/images/button7.png");
    background-size: 100% 100%;
    background-repeat: no-repeat;
    font-size: 18px;
    font-weight: bold;
    width: 177px;
    padding-left: 50px;
    box-sizing: border-box;
    cursor: pointer;
    margin: 450px 0 0 50px;
  }

  .save img {
    width: 45px;
    position: absolute;
    left: -10px;
    bottom: 10px;
  }

  .shezhi {
    transition: all 0.7s;
  }

  .shezhistyle {
    background: #fff;
    color: #5bd219;
  }

  .create {
    position: relative;
  }

  .xinjian {
    position: absolute;
    left: 0;
    top: 0;
    width: 200px;
    height: 165px;
    border-radius: 5px;
    background-color: #EC494E;
    box-shadow: 0 0 15px rgba(236, 73, 78, 0.76);
    cursor: default;
    display: none;
  }

  .xinjian:hover {
    color: #fff;
  }

  .xinjian_before {
    position: absolute;
    right: 0;
    top: 0;
    width: 20px;
    height: 20px;
    color: #fff;
    line-height: 16px;
    text-indent: -1px;
    font-size: 30px;
    border: 1px solid #fff;
    transform: rotate(45deg);
    border-radius: 50%;
    cursor: pointer;
  }

  .xinjian h6 {
    width: 100%;
    height: 50px;
    color: #fff;
    line-height: 50px;
    text-align: center;
    font-size: 20px;
    font-weight: normal;
  }

  #ding {
    width: 60px;
    height: 30px;
    font-size: 14px;
    border-radius: 3px;
    color: #fff;
    background-color: #0078ff;
    border: 0;
    outline: none;
    cursor: pointer;
    box-shadow: 0 0 10px rgba(0, 120, 255, 0.5);
  }

  .xinjian_width,
  .xinjian_height {
    float: left;
    width: 85px;
    height: 40px;
    margin-left: 10px;
    border-radius: 5px;
    margin-bottom: 20px;
    background-color: #FFBC00;
  }

  .xinjian_width:hover,
  .xinjian_height:hover {
    background-color: #fff;
    color: #000;
  }

  .xinjian_width span,
  .xinjian_height span {
    float: left;
    width: 30px;
    height: 100%;
    font-size: 16px;
    text-align: center;
    line-height: 40px;
  }

  .xinjian_width input,
  .xinjian_height input {
    float: left;
    margin-top: 5px;
    width: 50px;
    height: 30px;
    text-align: center;
    line-height: 30px;
    font-size: 14px;
    border: 1px solid #FFBC00;
    box-sizing: border-box;
  }

  .box {
    float: left;
    width: 90px;
    height: auto;
    background-color: #EC494E;
    overflow: hidden;
    border-radius: 5px;
    transition: all 0.7s;
  }

  .box {
    margin-left: 25px;
  }

  .linewidth {
    margin-left: 20px;
  }

  .box:hover {
    background-color: #fff;
  }

  .box span {
    display: block;
    float: left;
    width: 40px;
    height: 45px;
    font-size: 16px;
    color: #fff;
    text-align: center;
    line-height: 40px;
    color: #fff;
    transition: all 0.3s;
  }

  .box input {
    float: left;
    width: 45px;
    height: 35px;
    margin-top: 5px;
    border: 1px solid #fff;
    box-sizing: border-box;
    text-align: center;
    line-height: 40px;
    font-size: 18px;
    color: #fff;
    background: #EC494E;
    border-radius: 3px;
    box-shadow: 0 0 0 4px #fff inset;
    transition: all 0.3s;
  }

  .box2 span {
    width: 55px;
  }

  .box2 input {
    width: 140px;
  }

  .space {
    float: left;
    width: 100%;
    height: 0;
    border-bottom: 1px solid #666;
    margin: 20px 0;
  }

  .right_box {
    position: fixed;
    right: 10px;
    width: 500px;
    height: 70px;
    top: 10px;
  }

  .right_box .title {
    font-weight: bold;
    font-size: 20px;
    color: #fff;
    margin: 0 0 20px 0;
  }

  ::-webkit-scrollbar {
    display: none;
  }

  .right_bottom {
    height: 915px;
    max-height: 915px;
    overflow-y: scroll;
    background: #4B4B4B;
    border-radius: 5px;
    padding: 0 20px;
  }

  .right_box .item {
    padding: 20px 0;
    border-bottom: 1px solid #666666;
    display: flex;
    align-items: flex-start;
    color: #fff;
    font-size: 16px;
  }

  .right_box .item span {
    width: 35px;
    margin-right: 20px;
  }

  .right_box .item div div {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }

  .right_box .item div div:last-child {
    margin: 0;
  }

  .right_box .item div input {
    width: 20px;
    height: 20px;
    margin: 0 10px 0 0;
  }

  .right_box .btns {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    margin-top: 20px;
  }

  .right_box .btns a {
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
