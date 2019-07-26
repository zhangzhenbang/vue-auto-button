<template>
  <div class="auto-button-container">
    <Tooltip placement="top" always :disabled="!timer" theme="light">
      <span slot="content">
        {{remainingSeconds}}s <a @click="stopTimer">停止</a>
      </span>
      <!-- note you don't need to register @click but the click event is still working. Why? -->
      <Button ref="button" v-bind="$attrs" v-on="$listeners"
              :class="{'animate-stripes': !!timer}"
              @click="stopTimer"
      >
        <slot></slot>
      </Button>
    </Tooltip>
  </div>
</template>

<script>
  export default {
    name: 'AutoButton',
    inheritAttrs: false,
    props: {
      delay: {
        type: Number,
        default: 5
      }
    },
    data() {
      return {
        timer: null,
        remainingTimeInMillis: this.delay * 1000
      }
    },
    computed: {
      remainingSeconds() {
        return this.remainingTimeInMillis > 0
          ? Math.floor(this.remainingTimeInMillis / 1000)
          : 0;
      }
    },
    methods: {
      stopTimer() {
        if (this.timer) {
          clearTimeout(this.timer);
          this.timer = null;
          this.remainingTimeInMillis = 0;
        }
      },
      startTimer() {
        if (!this.timer && this.remainingTimeInMillis > 0) {
          this.timer = setTimeout(this.countDown, 100)
        }
      },
      countDown() {
        // console.log('countDown is called with r = %d', this.remainingTimeInMillis);
        this.remainingTimeInMillis = this.remainingTimeInMillis - 100;
        if (this.remainingTimeInMillis > 0) {
          this.timer = setTimeout(this.countDown, 100);
        } else {
          // click itself and stop the timer
          this.$refs['button'].$el.click();
          this.stopTimer();
        }
      }
    },
    mounted() {
      this.startTimer();
    }
  };
</script>

<style scoped lang="less">
  .auto-button-container {

    .animate-stripes {
      // see http://www.quasarchs.com/components/progress-bar.html
      background-image: linear-gradient(45deg,hsla(0,0%,100%,.15) 25%,transparent 0,transparent 50%,hsla(0,0%,100%,.15) 0,hsla(0,0%,100%,.15) 75%,transparent 0,transparent) !important;
      background-size: 40px 40px !important;
      animation: q-progress-stripes 2s linear infinite;
    }

    @keyframes q-progress-stripes {
      0% { background-position: 0 0; }
      100% { background-position: 40px 0; }
    }

  }
</style>
