<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";
</script>

<template>
  <div v-if="!testStarted" class="test-container">
    <h2>Фланговый тест Эриксена: Тест на концентрацию и обработку информации</h2>
    <p>Этот тест предназначен для измерения вашей способности к концентрации и быстрой обработке информации. Вам
      необходимо как можно быстрее нажимать на боковые клавиши (левую или правую), соответствующие направлению
      центральной стрелки, отображаемой на экране. Вокруг центральной стрелки могут быть другие стрелки, указывающие в
      том же направлении или в противоположном.</p>
    <p>Тест состоит из 50 испытаний. Пожалуйста, сосредоточьтесь и постарайтесь дать как можно больше правильных
      ответов.</p>
    <button @click="startTest" class="base-button start-button">Начать тест</button>
  </div>

  <div v-else-if="testStarted && !testFinished" class="test-container">
    <div class="arrows-container">
      <div v-for="(arrow, index) in currentTrial" :key="index" class="arrow">
        <FontAwesomeIcon :icon="arrow === 'left' ? faArrowLeft : faArrowRight" class="arrow-icon" />
      </div>
    </div>
    <div class="buttons-container">
      <button @click="handleButtonClick('left')" class="arrow-answer-button">
        <FontAwesomeIcon :icon="faArrowLeft" class="answer-button-icon" />
      </button>
      <button @click="handleButtonClick('right')" class="arrow-answer-button">
        <FontAwesomeIcon :icon="faArrowRight" class="answer-button-icon" />
      </button>
    </div>
  </div>

  <div v-else class="result-container">
    <p class="result-count">Правильных ответов: {{ correctAnswers }}</p>
    <p class="result-time">Время: {{ elapsedTime }} сек.</p>
    <p class="result-accuracy">Точность: {{ accuracy }}%</p>
    <p class="result-speed">Скорость: {{ speed }} ответов/сек</p>
    <p class="result-message">{{ resultMessage }}</p>
    <button @click="restartTest" class="base-button restart-button">Пройти тест заново</button>
  </div>
</template>

<script>
export default {
  name: 'Test2',
  data() {
    return {
      trials: [],
      correctAnswers: 0,
      elapsedTime: 0,
      testStarted: false,
      testFinished: false,
      timer: null,
      currentTrial: null,
      resultMessage: null,
      accuracy: 0,
      speed: 0
    };
  },
  methods: {
    generateTrials() {
      for (let i = 0; i < 50; i++) {
        const centralArrow = Math.random() < 0.5 ? 'left' : 'right';
        const trial = Array(5).fill(centralArrow);
        const numDiff = Math.floor(Math.random() * 4);
        for (let k = 0; k < numDiff; k++) {
          let randomIndex = Math.floor(Math.random() * 4);
          trial[randomIndex] = trial[randomIndex] === 'left' ? 'right' : 'left';
        }
        this.trials.push(trial);
      }
    },

    startTest() {
      this.testStarted = true;
      this.generateTrials();
      this.currentTrial = this.trials.shift();
      this.timer = setInterval(() => {
        this.elapsedTime++;
      }, 1000);
    },

    handleButtonClick(direction) {
      if (!this.testFinished) {
        if (direction === this.currentTrial[2]) {
          this.correctAnswers++;
        }
        if (this.trials.length > 0) {
          this.currentTrial = this.trials.shift();
        } else {
          this.finishTest();
        }
      }
    },

    finishTest() {
      clearInterval(this.timer);
      this.testFinished = true;
      this.determineResult();
    },

    determineResult() {
      this.accuracy = this.elapsedTime === 0 ? 0 : Math.round((this.correctAnswers / 50) * 100);
      this.speed = this.elapsedTime === 0 ? 0 : (this.correctAnswers / this.elapsedTime).toFixed(2);

      if (this.accuracy >= 90) {
        this.resultMessage = "Отличный результат! Ваша концентрация и скорость обработки информации на высоком уровне.";
      } else if (this.accuracy >= 70) {
        this.resultMessage = "Хороший результат! У вас неплохая концентрация и скорость обработки информации.";
      } else if (this.accuracy >= 50) {
        this.resultMessage = "Средний результат. Есть потенциал для улучшения концентрации и скорости.";
      } else {
        this.resultMessage = "Результат ниже среднего. Рекомендуется потренировать внимание и скорость реакции.";
      }
    },
    
    restartTest() {
      this.trials = [];
      this.correctAnswers = 0;
      this.elapsedTime = 0;
      this.testStarted = false;
      this.testFinished = false;
      this.currentTrial = null;
      this.resultMessage = null;
      this.accuracy = 0;
      this.speed = 0;
    }
  }
};
</script>
