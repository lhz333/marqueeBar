<template>
  <div ref="wrap" class="wrap-content">
    <div
      ref="marqueeBar"
      class="marqueeBar"
      :class="animationClass"
      :style="marqueeBarStyle"
      @animationend="onAnimationEnd"
      @webkitAnimationEnd="onAnimationEnd"
    >{{value}}</div>
  </div>
</template>
<script>
export default {
  props: {
    value: {
      default: ''
    },
    delay: {
      type: Number,
      default: 1.5
    },
    speed: {
      type: Number,
      default: 50
    }
  },
  mounted () {
    // 动态向头部插入样式表 主要是paomadeng-infinite中移动的值为动态的 根据手机屏幕宽度适配
    document.getElementById('keyframes_id') && document.getElementById('keyframes_id').remove()
    var style = document.createElement('style')
    style.type = 'text/css'
    style.id = 'keyframes_id'
    var keyFrames = `@keyframes paomadeng-infinite {
      to {
        transform: translate3d(-${this.$refs.wrap.getBoundingClientRect().width}px, 0, 0);
      }
    }`
    style.innerHTML = keyFrames
    document.getElementsByTagName('head')[0].appendChild(style)
  },
  data () {
    return {
      wrapWidth: 0, // 父盒子宽度
      firstRound: true, // 判断是否
      duration: 0, // css3一次动画需要的时间
      offsetWidth: 0, // 子盒子的宽度
      animationClass: '', // 添加animate动画
      loopNum: 0 // 循环次数
    }
  },
  computed: {
    marqueeBarStyle () {
      return {
        // 第一次从头开始,第二次动画的时候需要从最右边出来所以宽度需要多出父盒子的宽度
        paddingLeft: (this.firstRound ? 0 : this.wrapWidth) + 'px',
        // 只有第一次的时候需要延迟
        animationDelay: (this.firstRound ? this.delay : 0) + 's',
        animationDuration: this.duration + 's'
      }
    }
  },
  watch: {
    value: {
      // 监听到有内容,从后台获取到数据了,开始计算宽度,并计算时间,添加动画
      handler () {
        this.$nextTick(() => {
          const { wrap, marqueeBar } = this.$refs
          const wrapWidth = wrap.getBoundingClientRect().width
          const offsetWidth = marqueeBar.getBoundingClientRect().width
          if (offsetWidth > wrapWidth) {
            this.wrapWidth = wrapWidth
            this.offsetWidth = offsetWidth
            this.duration = offsetWidth / this.speed
            this.animationClass = 'animate'
          }
        })
      },
      immediate: true
    }
  },
  methods: {
    // 这个函数是第一次动画结束的时候,第一次没有使用infinite,第一次动画执行完成后开始使用添加animate-infinite动画
    onAnimationEnd () {
      this.loopNum = this.loopNum + 1
      this.firstRound = false
      // 这是时候样式多出了padding-left:this.wrapWidth;所以要想速度一样需要重新计算时间
      if (this.loopNum == 1) {
        this.duration = this.wrapWidth / this.speed
        this.animationClass = 'animate-infinite'
      } else if (this.loopNum == 2) {
        this.duration = this.offsetWidth / this.speed
        this.animationClass = 'animate'
        this.loopNum = 0
        this.firstRound = true
      }
    }
  }
}
</script>
<style scoped>
.wrap-content {
  width: 100%;
  height: 27px;
  overflow: hidden;
  position: relative;
  padding: 0;
}

.wrap-content .marqueeBar {
  position: absolute;
  white-space: nowrap;
}

.animate {
  animation: paomadeng linear forwards;
}

.animate-infinite {
  animation: paomadeng-infinite linear forwards;
}

@keyframes paomadeng {
  to {
    transform: translate3d(-100%, 0, 0);
  }
}

/* @keyframes paomadeng-infinite {
  to {
    transform: translate3d(-200px, 0, 0);
  }
} */
</style>
