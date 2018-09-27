<template>
  <div class="training">
  <h1>Math training Level: {{ level + 1 }}</h1>
    <span class="timer" v-if="state === 'question'" >
           <app-timer :initial="timer.time" @unanswered="onUnanswered" :key="randomKey()" ></app-timer>
    </span>
  <hr>
    <div class="progress">
      <div class="progress-bar" :style="progressStyles"></div>
    </div>
    <div class="box">
      <transition name="flip" mode="out-in">
      <app-start-screen v-if="state === 'start'" @onStart="onStart"></app-start-screen>
      <app-question v-else-if="state === 'question'"
                    :key="randomKey()"
                    :settings="levels[level]"
                    @success="onQustSuccess"
                    @error="onQustError"
      >
      </app-question>
      <app-message v-else-if="state === 'message'"
                   :text="message.text"
                   :type="message.type"
                   @onNext="onNext"
      >
      </app-message>
      <app-result-screen v-else-if="state === 'result'"
        :stats="stats"
        :hasNextLevel="hasNextLevel"
        @repeat="onStart"
        @nextLevel="onNextLevel"
      >
      </app-result-screen>
      <div v-else>Unknown state</div>
      </transition>
    </div>
    </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      state: 'start',
      timer: {
        time: 1000
      },
      stats: {
        success: 0,
        unanswered: 0,
        error: 0
      },
      message: {
        text: '',
        type: ''
      },
      questMax: 3,
      level: 0,
      levels: [
        {
          from: 10,
          to: 40,
          range: 5,
          variants: 2
        },
        {
          from: 100,
          to: 200,
          range: 20,
          variants: 4
        },
        {
          from: 500,
          to: 900,
          range: 40,
          variants: 6
        }
      ]
    }

  },
  computed: {
    questDone () {
      return this.stats.success + this.stats.error + this.stats.unanswered;
    },
    hasNextLevel () {
      return this.level + 1 < this.levels.length
    },
    progressStyles  () {
      return {
        width: (this.questDone / this.questMax * 100) + '%'
      }
    }
  },
  methods: {
    onUnanswered() {
      this.stats.unanswered++;
      this.onNext();
    },
    randomKey() {
      return Math.floor((1 + Math.random()) * 0x10000).toString(16).substring(1);
    },
    onStart () {
      this.state = 'question';
      this.stats.success = 0;
      this.stats.error = 0;
      this.stats.unanswered = 0;
    },
    onQustSuccess () {
      this.state = 'message';
      this.message.text = 'Good job!';
      this.message.type = 'success';
      this.stats.success++;
    },
    onQustError (msg) {
      this.state = 'message';
      this.message.text = msg;
      this.message.type = 'warning';
      this.stats.error++;
    },
    onNext () {
      if (this.questDone < this.questMax) {
        this.state = 'question';
      } else {
        this.state = 'result';
      }
    },
    onNextLevel () {
      this.level++;
      this.onStart();
    }
  }
}
</script>

<style lang="scss" scoped>
	.training {
    position: relative;
		max-width: 700px;
		margin: 20px auto;
	}

  .timer {
    position: absolute;
    font-size: 2rem;
    right: 0;
    top: 0;
  }

  .box {
    margin: 10px 0;
  }

  .flip-enter {

  }

  .flip-enter-active {
    animation: flipInX 0.3s linear;
  }

  .flip-leave {

  }

  .flip-leave-active {
    animation: flipOutX 0.3s linear;
  }

  @keyframes flipInX {
    from{transform: rotateX(90deg);}
    to{transform: rotateX(0deg);}
  }

  @keyframes flipOutX {
    from{transform: rotateX(0deg);}
    to{transform: rotateX(90deg);}
  }
</style>
