<template>
  <div class="child">
        <form class="input-form">
          <input type="text"
                  class="input-captcha"
                  placeholder="Enter Captcha"
                  ref="inputField">
          <button @click.prevent="handleButtonClick" class="np-button">Go</button>
        </form>
      </div>
</template>

<script>


import { ElNotification } from 'element-plus'
import 'element-plus/dist/index.css'

export default {
    name : "CaptchaForm",
    props:{
        'captchaData' : String,
         method: { type: Function },
    },
    // components : {
    //     ElNotification
    // },
    data() {

        // var captchaData = this.captchaData;
        return {
            error : "",
            captchaSubmitFlag : false
        }
    },

    methods : {

        getCaptchaFlag(captchaSubmitFlag) {
            console.log("hahaha",captchaSubmitFlag);
        },

        handleButtonClick() {

            let inputFieldCaptcha = this.$refs.inputField.value;
            let originalCaptcha = this.captchaData.join("");

            console.log("This is inout text : ",inputFieldCaptcha)
            console.log("My captcha is :",originalCaptcha);


            if(inputFieldCaptcha === "") {
                this.error = "Invalid Captcha";

                ElNotification({
                    title: 'Error',
                    message: this.error,
                    type: 'error',
                })
            } else if(inputFieldCaptcha == originalCaptcha) {
                this.error = "Captcha Submitted Successfully";

                ElNotification({
                    title: 'Success',
                    message: this.error,
                    type: 'success',
                })

                this.$refs.inputField.value = "";
                console.log("CaptchaSubmit Flag before : ",this.captchaSubmitFlag);
                this.captchaSubmitFlag = true;
                console.log("CaptchaSubmit Flag After : ",this.captchaSubmitFlag);
                this.$emit('getCaptchaEvent',this.captchaSubmitFlag)
            } else {
                this.error = "Invalid Captcha";
                this.$refs.inputField.value = "";
                ElNotification({
                    title: 'Error',
                    message: this.error,
                    type: 'error',
                })
            }
        }
    }
}
</script>

<style>
.input-captcha {
  width: 370px;
    border-radius: 50px;
    padding: 20px 30px;
    font-size: 25px;
    border: none;
    transition: .2s;
    box-shadow: inset 0 0 5px #000;
}

.input-form {
  display: flex;
    width: 370px;
    position: relative;
    align-items: center;
    padding-left: 440px;
}

.np-button {
  position: absolute;
    width: 50px;
    height: 50px;
    margin: 12px;
    border-radius: 50px;
    right: 0;
    border: none;
    font-size: 15px;
    background-color: #2f74c0;
    color: #fff;
    transition: all .2s;
    box-shadow: 0 0 10px #000;
    cursor: pointer;
}

</style>