<template>
  <div id="login">
    <h2>智能云联综合管理系统</h2>
    <div class="container">
      <form class="form1" action="#">
        <h2>用户登录</h2>
        <div style="overflow: hidden;">
          <input type="text"  placeholder="UserName" class="input" v-model="UserName" />
          <input type="password" placeholder="PassWord" class="input" v-model="PassWord" />
          <input
            type="text"
            placeholder="验证码"
            class="input"
            v-model="dtoUserCode"
            style="width: 61%;float: left;"
          />
          <div class="s-canvas">
            <canvas
              style="height: 40px;"
              @click="btn2()"
              id="s-canvas"
              :width="contentWidth"
              :height="contentHeight"
            ></canvas>
          </div>
        </div>
        <!-- <span>欢迎来到后台管理系统</span> -->
        <button v-loading.fullscreen.lock="fullscreenLoading" class="btn" @click="btn()">登录</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name:"login",/***** */
  props: {
    identifyCode: {
      type: String,
      default: ""
    },
    fontSizeMin: {
      type: Number,
      default: 25
    },
    fontSizeMax: {
      type: Number,
      default: 30
    },
    backgroundColorMin: {
      type: Number,
      default: 255
    },
    backgroundColorMax: {
      type: Number,
      default: 255
    },
    colorMin: {
      type: Number,
      default: 0
    },
    colorMax: {
      type: Number,
      default: 160
    },
    lineColorMin: {
      type: Number,
      default: 100
    },
    lineColorMax: {
      type: Number,
      default: 255
    },
    dotColorMin: {
      type: Number,
      default: 0
    },
    dotColorMax: {
      type: Number,
      default: 255
    },
    contentWidth: {
      type: Number,
      default: 80
    },
    contentHeight: {
      type: Number,
      default: 31
    }
  },
  created: function() {
    this.btn2();
  },
  data() {
    return {
      UserName: "",
      PassWord: "",
      dtoUserCode: "",
      fullscreenLoading: false
    };
  },
  methods: {
    btn2() {
      var identifyCode = "";
      //设置长度，这里看需求，我这里设置了4
      var codeLength = 4;

      //设置随机字符
      var random = new Array(0, 1, 2, 3, 4, 5, 6, 7, 8, 9);

      //循环codeLength 我设置的4就是循环4次
      for (var i = 0; i < codeLength; i++) {
        //设置随机数范围,这设置为0 ~ 36
        var index = Math.floor(Math.random() * 9);

        //字符串拼接 将每次随机的字符 进行拼接
        identifyCode += random[index];
      }
      //将拼接好的字符串赋值给展示的code
      this.identifyCode = identifyCode;
    },
    // 生成一个随机数
    randomNum(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    },
    // 生成一个随机的颜色
    randomColor(min, max) {
      let r = this.randomNum(min, max);
      let g = this.randomNum(min, max);
      let b = this.randomNum(min, max);
      return "rgb(" + r + "," + g + "," + b + ")";
    },
    drawPic() {
      let canvas = document.getElementById("s-canvas");
      let ctx = canvas.getContext("2d");
      ctx.textBaseline = "bottom";
      // 绘制背景
      ctx.fillStyle = this.randomColor(
        this.backgroundColorMin,
        this.backgroundColorMax
      );
      ctx.fillRect(0, 0, this.contentWidth, this.contentHeight);
      // 绘制文字
      for (let i = 0; i < this.identifyCode.length; i++) {
        this.drawText(ctx, this.identifyCode[i], i);
      }
      this.drawLine(ctx);
      this.drawDot(ctx);
    },
    drawText(ctx, txt, i) {
      ctx.fillStyle = this.randomColor(this.colorMin, this.colorMax);
      ctx.font =
        this.randomNum(this.fontSizeMin, this.fontSizeMax) + "px SimHei";
      let x = (i + 1) * (this.contentWidth / (this.identifyCode.length + 1));
      let y = this.randomNum(this.fontSizeMax, this.contentHeight - 5);
      var deg = this.randomNum(-45, 45);
      // 修改坐标原点和旋转角度
      ctx.translate(x, y);
      ctx.rotate((deg * Math.PI) / 180);
      ctx.fillText(txt, 0, 0);
      // 恢复坐标原点和旋转角度
      ctx.rotate((-deg * Math.PI) / 180);
      ctx.translate(-x, -y);
    },
    drawLine(ctx) {
      // 绘制干扰线
      for (let i = 0; i < 5; i++) {
        ctx.strokeStyle = this.randomColor(
          this.lineColorMin,
          this.lineColorMax
        );
        ctx.beginPath();
        ctx.moveTo(
          this.randomNum(0, this.contentWidth),
          this.randomNum(0, this.contentHeight)
        );
        ctx.lineTo(
          this.randomNum(0, this.contentWidth),
          this.randomNum(0, this.contentHeight)
        );
        ctx.stroke();
      }
    },
    drawDot(ctx) {
      // 绘制干扰点
      for (let i = 0; i < 80; i++) {
        ctx.fillStyle = this.randomColor(0, 255);
        ctx.beginPath();
        ctx.arc(
          this.randomNum(0, this.contentWidth),
          this.randomNum(0, this.contentHeight),
          1,
          0,
          2 * Math.PI
        );
        ctx.fill();
      }
    },
    btn() {
      var that = this;
      // let param = {
      //   dtoUserName: this.UserName,
      //   password: this.PassWord
      // };
      if (this.UserName == "") {
        this.$message.error("请输入正确的账号");
        return false;
      } else if (this.PassWord == "") {
        this.$message.error("请输入正确的密码");
        return false;
      } else if (this.dtoUserCode == "") {
        this.$message.error("请输入验证码");
      } else if (this.dtoUserCode !== this.identifyCode) {
        this.$message.error("验证码错误");
      } else if (this.dtoUserCode == this.identifyCode) {
        this.$message({
          message: "登陆成功",
          type: "success"
        });
        this.fullscreenLoading = true;
        
        axios
          .get(window.g.baseURL + "workOrder/login", {
            params: {
              userName: that.UserName, // E00000
              passWord: that.PassWord // 123456
            }
          })
          .then(res => {
            setTimeout(() => {
              this.fullscreenLoading = false;
            }, 3000);
            this.$router.push({ path: "/index" });
            console.log(res);
          })
          .catch();
      }
    }
  },
  watch: {
    identifyCode() {
      this.drawPic();
    }
  },
  mounted() {
    this.drawPic();
  }
};
</script>

<style scoped>
#login {
  align-items: center;
  background: url("../assets/bg.png");
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  display: grid;
  height: 100vh;
  place-items: center;
}
.container {
  max-width: 400px;
  background: #e9e9e9;
  background: rgba(99, 99, 100, 0.4);
  border-radius: 15px;
  box-shadow: 0 0.9rem 1.7rem rgba(0, 0, 0, 0.25),
    0 0.7rem 0.7rem rgba(0, 0, 0, 0.22);
  margin: 0 18px;
}
.form1 {
  background: url("../assets/pic.png");
  padding: 2.75rem;
  text-align: center;
}
h2 {
  font-weight: 400;
  margin-bottom: 1.25rem;
  text-align: center;
  color: #fff;
}

.input {
  background-color: #fff;
  border: none;
  padding: 0.9rem 0.9rem;
  margin: 0.5rem 0;
  width: 100%;
  /* border-radius: 6px; */
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
span {
  color: #8b8b8b;
  text-align: center;
  display: block;
  margin-top: 25px;
}
.btn {
  background-image: linear-gradient(90deg, #0367a6 0%, #008997 74%);
  /* border-radius: 20px; */
  border: 1px solid var(--blue);
  cursor: pointer;
  font-size: 0.8rem;
  font-weight: bold;
  letter-spacing: 0.1rem;
  padding: 0.9rem 8.5rem;
  text-transform: uppercase;
  color: white;
  transition: transform 80ms ease-in;
  margin-top: 20px;
  transition: 0.8s !important;
}
.btn:hover {
  border-radius: 50px;
}
.s-canvas {
  padding: 4px 0;
  height: 38px;
  margin: 0.5rem 0;
  padding: 3px 0;
  float: right;
}
#s-canvas {
  /* border-radius: 10px; */
  margin-top: 0;
  cursor: pointer;
}
.s-canvas canvas {
  margin-top: 1px;
  margin-left: 8px;
}
</style>