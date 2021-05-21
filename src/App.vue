<template>
  <div class="container-app">
    <div class="container-home" :class="openClass">
      <div class="wrapper-image">
        <img src="@/assets/img/img-home.png" alt="" />
      </div>

      <div class="button-play" @click="toggleOpen">
        <p>PLAY</p>
      </div>
    </div>

    <div class="container-lose" :class="loseClass">
      <p class="title">YOU LOSE</p>

      <div class="button-play" @click="togglePlay">
        <p>PLAY AGAIN</p>
      </div>
    </div>

    <div class="container-win" :class="winClass">
      <p class="title">YOU WIN</p>
      <div class="box-star">
        <i class="fas fa-star"></i>
        <i class="fas fa-star"></i>
        <i class="fas fa-star"></i>
      </div>
      <div class="button-play" @click="togglePlayWin">
        <p>PLAY AGAIN</p>
      </div>
    </div>

    <div class="container-main">
      <div class="wrapper-header">
        <div class="box-timer">
          <p class="title">Timer</p>
          <p class="timer">{{ timer }}</p>
        </div>

        <div class="box-health">
          <i class="fas fa-heart" v-if="conHeart.heart1"></i>
          <i class="fas fa-heart" v-if="conHeart.heart2"></i>
          <i class="fas fa-heart" v-if="conHeart.heart3"></i>
        </div>
      </div>

      <div class="wrapper-body">
        <div class="match-numbers">
          <div class="circle-num">
            <p>{{ myNum }}</p>
          </div>
          <div class="equals">
            <div class="eq"></div>
            <div class="eq"></div>
          </div>
          <div class="circle-num">
            <p>{{ setNum }}</p>
          </div>
        </div>

        <div class="bar-progress">
          <p class="progress">Progress</p>
          <div class="wrapper-bar">
            <p class="percentage">{{ progress }}%</p>
            <div class="bar" :style="{ width: progress + '%' }"></div>
          </div>
        </div>
      </div>

      <div class="wrapper-footer">
        <p class="title">Choose a number to add</p>
        <div class="box-circle">
          <div class="circle-add-num" @click="addNum(10)">
            <p>10</p>
          </div>
          <div class="circle-add-num" @click="addNum(5)">
            <p>5</p>
          </div>
          <div class="circle-add-num" @click="addNum(1)">
            <p>1</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isHome: true,
      myNum: 0,
      setNum: 0,
      matchNum: [11, 24, 33, 6, 90, 73, 21, 17, 9, 30, 50, 2, 1],
      numHeart: 0,
      progress: 0,
      timer: 10,
      isTimer: true,
      isPlay: false,
      isLose: false,
      isWin: false,
      conHeart: {
        heart1: true,
        heart2: true,
        heart3: true,
      },
      homeScreen: {
        isOpen: true,
      },
    };
  },
  mounted() {
    this.countDownTimer();
    this.randMatchNum();
  },
  computed: {
    openClass() {
      return {
        open: !this.homeScreen.isOpen,
      };
    },
    loseClass() {
      return {
        lose: this.isLose,
      };
    },
    winClass() {
      return {
        win: this.isWin,
      };
    },
  },
  watch: {
    myNum(value) {
      if (value > this.setNum) {
        this.numHeart += 1;
        this.myNum = 0;
        this.timer = 10;
        this.randMatchNum();
        console.log(this.numHeart);
      } else if (value === this.setNum) {
        this.progress += 10;
        this.myNum = 0;
        this.timer = 10;
        this.randMatchNum();
        this.countDownTimer();
        console.log(this.progress);
      }
    },
    numHeart(value) {
      if (value === 1) {
        this.conHeart.heart1 = false;
      } else if (value === 2) {
        this.conHeart.heart1 = false;
        this.conHeart.heart2 = false;
      } else {
        this.conHeart.heart1 = false;
        this.conHeart.heart2 = false;
        this.conHeart.heart3 = false;

        this.isLose = true;
        this.isPlay = false;
        this.progress = 0;
        this.numHeart = 0;
      }
    },
    timer(value) {
      if (value === 0) {
        this.conHeart.heart1 = false;
        this.conHeart.heart2 = false;
        this.conHeart.heart3 = false;
        this.isLose = true;
        this.isPlay = false;
        this.progress = 0;
        console.log("yahhh");
      }
    },
    progress(value) {
      if (value == 100) {
        this.isLose = false;
        this.isPlay = false;
        this.isTimer = 0;
        this.isWin = true;
        this.progress = 0;
        console.log("100 woy");
      }
    },
  },
  methods: {
    addNum(n) {
      this.myNum += n;
    },
    toggleOpen() {
      console.log("buka dong");
      this.isPlay = true;
      this.countDownTimer(this.isPlay);
      this.homeScreen.isOpen = !this.homeScreen.isOpen;
    },
    togglePlay() {
      this.isLose = !true;
      console.log("hall");
      this.isPlay = true;
      this.timer = 10;
      this.progress = 0;
      this.conHeart.heart1 = true;
      this.conHeart.heart2 = true;
      this.conHeart.heart3 = true;
      this.randMatchNum();
      this.countDownTimer(this.isPlay);
    },
    togglePlayWin() {
      this.isWin = !true;
      console.log("hall");
      this.isPlay = true;
      this.timer = 10;
      this.progress = 0;
      this.conHeart.heart1 = true;
      this.conHeart.heart2 = true;
      this.conHeart.heart3 = true;
      this.randMatchNum();
      this.countDownTimer(this.isPlay);
    },
    countDownTimer(hhhh) {
      if (hhhh) {
        if (this.timer > 0) {
          setTimeout(() => {
            this.timer -= 1;
            this.countDownTimer(hhhh);
          }, 2000);
        }
      } else {
        console.log("time stop");
      }
    },
    randMatchNum() {
      const random = Math.floor(Math.random() * this.matchNum.length);
      let throwRand = (this.setNum = this.matchNum[random]);

      return throwRand;
    },
  },
};
</script>

<style lang="scss">
@import "@/assets/styles/_shared.scss";
@import "@/assets/styles/_mainApp.scss";
@import "@/assets/styles/_home.scss";
@import "@/assets/styles/_loseScreen.scss";
@import "@/assets/styles/_winScreen.scss";
</style>
