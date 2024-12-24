<template>
  <div class="wedding-invitation" :class="{ 'invitation-bounce':canOpen }">
    <div class="invitation-container" :class="{ 'invitation-down':isOpening }">
      <div class="invitation-cover">
        <div class="cover-content" :class="{'invitation-up':isOpening}">
          <div class="content-inside">
            <img class="content-inside-photo" src="../images/photo.jpg">
            <div class="content-inside-scroll">
              <p><b>老婆，结婚一周年快乐。</b></p>
              <p>晃晃悠悠，我们也以新的身份相处一年了，跟正式结婚前仿佛没什么差别，我时常还会冒出“女朋友”的字眼，也时常会忘记已经结婚这件事，但每次想起还是会嘴角微微上扬。</p>
              <p>结婚是一件很有仪式感的事情，每次想到还是会很让人动容和印象深刻，也许这也是仪式感的意义吧。</p>
              <p>但日常的我确实无趣且仪式感缺失，我觉得这样不好，平淡的日子里也需要一些精心或笨拙的仪式感的点缀，让记忆更深刻，也让脑子更好使。生活也要好好动脑子，也要更加认真。</p>
              <p>爱你，不管是婚前还是婚后，和你一起的日子，依旧幸福，我也能感受到我们正在变得更好。这一年，你冒出了很多新的鬼点子，我很开心，也希望我们有更多的鬼点子，认认真真生活，认认真真爱彼此。</p>
              <p>希望你看到这里，也是嘴角微微上扬的。爱你～</p>
            </div>
            <!-- <p><b>Jun & undefined</b></p>
            <p>时间：invalid date value</p>
            <p>地点：<b>location can not be found</b></p>
            <div class="content-inside-bless">
              <input
                placeholder="写下你的祝福" 
                @keyup.enter="sendBarrage"
                @focus="isFocused = true"
                @blur="isFocused = false, hasEntered = false"
                v-model="wish"
                ref="wishInput"
              >
              <p v-if="!wish && isFocused && hasEntered">请输入祝福哦</p>
              <div>
                <button @click="sendBarrage">发送祝福弹幕</button>
                <button @click="closeInvitation">关闭</button>
              </div>
            </div> -->
            <button class="content-close-button" @click="closeInvitation">关闭</button>
          </div>
        </div>
        <div class="cover-inside-left" :class="{'opening':isOpening}"></div>
        <div class="cover-inside-right" :class="{'opening':isOpening}"></div>
        <img class="cover-inside-seal" src="../images/seal.png" @click="openInvitation" :class="{'invitation-flight':isOpening}">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['canOpen'],
  data() {
    return {
      isOpening: false,
      wish: '',
      isFocused: false,
      hasEntered: false
    }
  },
  methods: {
    // 打开邀请函
    openInvitation(){
      this.isOpening = true
    },
    closeInvitation () {
      this.isOpening = false
      setTimeout(() => {
        this.$emit('onClose')
      }, 660)
    },
    // 发送弹幕
    sendBarrage(){
      this.$nextTick(() => {
        this.hasEntered = true
        if (!this.wish) {
          return
        }
        this.isOpening = false
        this.$refs.wishInput.blur()
        setTimeout(() => {
          this.$emit('sendBarrage', this.wish)
        }, 660)
      })
    }
  }
}
</script>

<style lang="less">
  .wedding-invitation{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    padding: 30px 20px;
    padding-top: 60px;
    z-index: 4;
    transform: scale(0.05);
    -webkit-transform: scale(0.05);
    opacity: 0;
    transition: transform 0.8s cubic-bezier(.26,1.84,.39,.61), opacity 0.5s linear;
    -webkit-transition: -webkit-transform 0.8s cubic-bezier(.26,1.84,.39,.61), opacity 0.5s linear;
    background-size: 100%;
    overflow: hidden;
    &.invitation-bounce{
      opacity: 1;
      transform: scale(1);
      -webkit-transform: scale(1);
    }
    .invitation-container{
      position: relative;
      width: 100%;
      height: 100%;
      transition: transform 0.6s cubic-bezier(0.4, 0, 1, 1);
      -webkit-transition: -webkit-transform 0.6s cubic-bezier(0.4, 0, 1, 1);
      &.invitation-down{
        transform: translateY(20px);
        -webkit-transform: translateY(20px);
      }
      .invitation-cover{
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: #D65047;
        border-radius: 10px;
        perspective: 500px;
        box-shadow: 0 0 20px 2px rgba(0, 0, 0, 0.15);
        .cover-content{
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          padding: 10px 20px;
          transition: transform 0.6s cubic-bezier(0.4, 0, 1, 1);
          -webkit-transition: -webkit-transform 0.6s cubic-bezier(0.4, 0, 1, 1);
          &.invitation-up{
            transform: translateY(-60px);
            -webkit-transform: translateY(-60px);
          }
          .content-inside{
            height: 100%;
            padding: 20px;
            color: #a9895d;
            background-color: #FFF1DE;
            text-align: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            overflow: auto;
            .content-inside-photo{
              width: 100%;
              margin-bottom: 10px;
              padding: 5px;
              border: 1px solid #f7debb;
            }
            .content-inside-scroll {
              flex: 1;
              overflow-y: scroll; /* 启用垂直滚动条 */
              border: 1px solid #ccc; /* 可选：添加边框以更好地看到滚动区域 */
              padding: 5px; /* 可选：添加内边距 */
              p {
                text-align: left;
                text-indent: 2em;
              }
            }
            p{
              margin-top: 0;
              margin-bottom: 5px;
            }
            .content-inside-bless{
              input{
                width: 100%;
                height: 35px;
                margin-bottom: 10px;
                outline: none;
                border: none;
                border-bottom: 1px solid #f7debb;
                color: #a9895d;
                background: transparent;
                font-size: 16px;
                &::-webkit-input-placeholder { color: #E8D1B1;font-size: 12px; }
                &::-moz-placeholder { color: #E8D1B1;font-size: 12px; }
                &:-ms-input-placeholder { color: #E8D1B1;font-size: 12px; }
                &:-moz-placeholder { color: #E8D1B1;font-size: 12px; }
              }
              >div{
                display: flex;
                button{
                  width: 100%;
                  height: 35px;
                  color: #a9895d;
                  background: #f7debb;
                  border: none;
                  outline: none;
                  &:disabled{
                    opacity: 0.8;
                  }
                  &:first-child{
                    margin-right: 10px;
                    flex: 1;
                  }
                  &:last-child{
                    width: 60px;
                    border: 1px solid #f7debb;
                    background: transparent;
                  }
                }
              }
            }
            .content-close-button {
              margin-top: 10px;
              width: 50px;
              height: 50px;
              border-radius: 50%;
              border: 1px solid #f7debb;
              background: #FFF1DE;
            }
          }
        }
        .cover-inside-left{
          position: absolute;
          left: 0;
          top: 0;
          width: 70%;
          height: 100%;
          border-radius: 10px;
          background-color: #D65047;
          box-shadow: 5px 0 10px rgba(0,0,0,0.2);
          z-index: 6;
          transition: transform 0.5s;
          -webkit-transition: -webkit-transform 0.5s;
          transform-origin: 0 50%;
          -webkit-transform-origin: 0 50%;
          &.opening{
            transform: rotate3d(0,1,0,-140deg);
            -webkit-transform: rotate3d(0,1,0,-140deg);
          }
        }
        .cover-inside-right{
          position: absolute;
          right: 0;
          top: 0;
          width: 40%;
          height: 100%;
          border-radius: 10px;
          background-color: #D65047;
          box-shadow: -5px 0 10px rgba(0,0,0,0.2);
          z-index: 5;
          transition: transform 0.5s;
          -webkit-transition: -webkit-transform 0.5s;
          transform-origin: 100% 50%;
          -webkit-transform-origin: 100% 50%;
          &.opening{
            transform: rotate3d(0,1,0,140deg);
            -webkit-transform: rotate3d(0,1,0,140deg);
          }
        }
        .cover-inside-seal{
          position: absolute;
          left: 70%;
          bottom: 100px;
          width: 80px;
          height: 80px;
          margin-left: -40px;
          z-index: 7;
          transform-origin: 50% 50%;
          -webkit-transform-origin: 50% 50%;
          transition: all 0.8s cubic-bezier(0.4, 0, 1, 1);
          -webkit-transition: all 0.8s cubic-bezier(0.4, 0, 1, 1);
          &.invitation-flight{
            opacity: 0;
          }
        }
      }
    }
  }
</style>
