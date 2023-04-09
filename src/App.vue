<template>
  <div id="app">
    <h2></h2>
    
    <div class="captcha-main-container" v-if="captchaCreateFlag===true && captchaSubmitFlagValue===false && intervalExceedFlag === false">
      <div class="np-captcha-container">
        <div class="np-captcha" v-if="captcha && captcha.length">
          <div
            v-for="(c, i) in captcha"
            :key="i"
            :style="{
              fontSize: getFontSize() + 'px',
              fontWeight: 800,
              transform: 'rotate(' + getRotationAngle() + 'deg)',
            }"
            class="np-captcha-character"
          >
            {{ c }}
          </div>
        </div>
      </div>
      <CaptchaForm v-bind:captchaData="captcha" @getCaptchaEvent="getCaptchaFlag"/>
    </div>
    <div v-else-if="captchaSubmitFlagValue===true && intervalExceedFlag === false">
      Wait For the Captcha
    </div>
    <div v-else-if="intervalExceedFlag === true ">
      Please wait , Time has not yet started
    </div>  
  </div>
  
</template>

<script>
import CaptchaForm from './components/CaptchaForm.vue';

export default {
  name: "App",
  components : {
    CaptchaForm
  },
  data() {
    return {
      captchaLength: 5,
      captcha: [],
      currentTime: "",
      captchaCreateFlag : false,
      captchaSubmitFlagValue : false,
      intervalExceedFlag : false
    };
  },
  mounted() {

        this.createCaptcha();
        this.intervalTimeStamp = setInterval(()=> {
          this.createCaptcha();
        },30000)
  },
  methods: {

    getCaptchaFlag(captchaSubmitFlag) {
       console.log("My captcha Parent Flag is : ",captchaSubmitFlag);
       this.captchaSubmitFlagValue = captchaSubmitFlag;
    },  

    createCaptcha() {

      this.captchaCreateFlag = true;
      this.captchaSubmitFlagValue = false;
      console.log(this.captchaCreateFlag);
      this.currentTime = new Date();
      console.log("My hours",this.currentTime.getHours());
      var currentHour = this.currentTime.getHours();

      if(currentHour >= 12 && currentHour <= 19) { 
          let tempCaptcha = "";
          for (let i = 0; i < this.captchaLength; i++) {
            tempCaptcha += this.getRandomCharacter();
          }
          this.captcha = tempCaptcha.split("");
      } else {
          if(this.intervalTimeStamp) {
            console.log("Interval is cleared");
            clearInterval(this.intervalTimeStamp);
            this.intervalExceedFlag = true;
          } else {
            this.intervalExceedFlag = true;
          }
      }
    },
    getRandomCharacter() {
      const symbols = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
      const randomNumber = Math.floor(Math.random() * 36);
      return symbols[randomNumber];
    },
    getFontSize() {
      const fontVariations = [14, 20, 30, 36, 40];
      return fontVariations[Math.floor(Math.random() * 5)];
    },
    getRotationAngle() {
      const rotationVariations = [5, 10, 20, 25, -5, -10, -20, -25];
      return rotationVariations[Math.floor(Math.random() * 8)];
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
}

.captcha-main-container {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    padding-left: 30px;
}


.np-captcha-container {
  background: #eee;
  width: 300px;
  margin: 0 auto;
  margin-bottom: 20px;
}
.np-captcha {
  font-size: 24px;
}

.np-captcha-character {
  display: inline-block;
  letter-spacing: 14px;
}
</style>
