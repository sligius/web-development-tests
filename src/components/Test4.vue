<template>
  <div v-if="!testStarted" class="test-container">
    <h1>Сравнение чисел</h1>
    <p>Аналогичный тест "Сравнение чисел" используется в Индии для проверки профессиональной пригодности машинистов
      поездов. Всего за полторы минуты вы можете проверить возможности своего внимания, просто сравнивая числа из правой
      и левой колонки.</p>
    <p>Вы увидите перед собой таблицу из 20 пар чисел. Ваша задача -
      обнаружить отличие или равенство в каждой паре и поставить соответствующий знак.</p>
    <button @click="startTest" class="base-button start-button">Начать тест</button>
  </div>

  <div v-else-if="testStarted && !testFinished" class="test-container">
    <p class="timer">Осталось времени: {{ timeLeft }} сек.</p>
    <div class="tables-container">
      <div class="table-wrapper">
        <table>
          <tbody>
            <tr v-for="(pair, index) in numberPairs.slice(0, 10)" :key="index">
              <td>{{ pair.left }}</td>
              <td>
                <div class="buttons-container">
                  <button @click="selectResult(index, '=')" :class="{ selected: results[index] === '=' }">=</button>
                  <button @click="selectResult(index, '≠')" :class="{ selected: results[index] === '≠' }">≠</button>
                </div>
              </td>
              <td>{{ pair.right }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="table-wrapper">
        <table>
          <tbody>
            <tr v-for="(pair, index) in numberPairs.slice(10)" :key="index">
              <td>{{ pair.left }}</td>
              <td>
                <div class="buttons-container">
                  <button @click="selectResult(index + 10, '=')"
                    :class="{ selected: results[index + 10] === '=' }">=</button>
                  <button @click="selectResult(index + 10, '≠')"
                    :class="{ selected: results[index + 10] === '≠' }">≠</button>
                </div>
              </td>
              <td>{{ pair.right }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <button @click="finishTest" class="base-button end-button">Завершить тест</button>
  </div>

  <div v-else class="result-container">
    <p class="result-count">Вы правильно определили {{ correctAnswers }} из 20 пар чисел.</p>
    <div v-if="correctAnswers === 0" class="result-message">
      <p>К сожалению, вам не удалось правильно определить ни одной пары чисел. Это может быть связано с усталостью,
        отвлечением или недостатком концентрации. Попробуйте отдохнуть и повторите тест позже.</p>
    </div>
    <div v-else-if="correctAnswers >= 1 && correctAnswers <= 8" class="result-message">
      <p>Ваш результат очень низкий. Это говорит о том, что вам трудно сосредоточиться на деталях. Рекомендуется
        выполнять упражнения на тренировку внимания и концентрации, например, короткие сессии медитации или задания на
        поиск различий. Попробуйте повторить тест после небольшого перерыва.</p>
    </div>
    <div v-else-if="correctAnswers >= 9 && correctAnswers <= 15" class="result-message">
      <p>Ваш результат ниже среднего. Вы можете улучшить свою внимательность. Попробуйте ещё раз, сосредоточившись на
        каждой паре чисел, не торопитесь. Регулярные упражнения на внимание помогут вам повысить точность.</p>
    </div>
    <div v-else-if="correctAnswers >= 16 && correctAnswers <= 19" class="result-message">
      <p>Отличный результат! Вы показали хорошее внимание к деталям. Небольшое количество ошибок – это нормально.
        Продолжайте тренировать свое внимание для достижения еще лучших результатов!</p>
    </div>
    <div v-else class="result-message">
      <p>Идеальный результат! Ваше внимание на высоте! Вы проявили исключительную концентрацию и точность. Попробуйте
        усложнить задачу, например, уменьшив время прохождения теста.</p>
    </div>

    <div class="comparison-tables">
      <div class="comparison-table-wrapper">
        <table>
          <thead>
            <tr>
              <th>Первое число</th>
              <th>Второе число</th>
              <th>Ваш ответ</th>
              <th>Правильный ответ</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(pair, index) in numberPairs.slice(0, 10)" :key="index"
              :class="{ correct: results[index] === pair.correctResult, incorrect: results[index] !== pair.correctResult }">
              <td>{{ pair.left }}</td>
              <td>{{ pair.right }}</td>
              <td>{{ results[index] }}</td>
              <td>{{ pair.correctResult }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="comparison-table-wrapper">
        <table>
          <thead>
            <tr>
              <th>Первое число</th>
              <th>Второе число</th>
              <th>Ваш ответ</th>
              <th>Правильный ответ</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(pair, index) in numberPairs.slice(10)" :key="index"
              :class="{ correct: results[index + 10] === pair.correctResult, incorrect: results[index + 10] !== pair.correctResult }">
              <td>{{ pair.left }}</td>
              <td>{{ pair.right }}</td>
              <td>{{ results[index + 10] }}</td>
              <td>{{ pair.correctResult }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <button @click="restartTest" class="base-button restart-button">Пройти тест заново</button>
  </div>
</template>

<script>
export default {
  name: 'Test4',
  data() {
    return {
      numPairs: 20,
      numberPairs: [],
      results: [],
      testStarted: false,
      testFinished: false,
      timeLeft: 90,
      timer: null,
      correctAnswers: 0,
      incorrectAnswers: 0,
    };
  },
  methods: {
    startTest() {
      this.generatePairs();
      this.testStarted = true;
      this.timer = setInterval(() => {
        this.timeLeft--;
        if (this.timeLeft <= 0) {
          this.finishTest();
        }
      }, 1000);
    },

    generatePairs() {
      this.results = Array(20).fill('');
      const numMatchingPairs = Math.floor(Math.random() * 6) + 5; // От 5 до 10 совпадающих пар

      for (let i = 0; i < numMatchingPairs; i++) {
        const baseNum = Math.floor(Math.random() * 9999901) + 100;
        this.numberPairs.push({ left: baseNum, right: baseNum, correctResult: '=' });
      }

      for (let i = 0; i < 20 - numMatchingPairs; i++) {
        const baseNum = Math.floor(Math.random() * 9999901) + 100;
        const diff = Math.floor(Math.random() * 2001) - 1000; // Разница не должна быть нулевой
        while (diff === 0) {
          diff = Math.floor(Math.random() * 100) - 50;
        }
        const left = baseNum;
        const right = baseNum + diff;
        this.numberPairs.push({ left, right, correctResult: left === right ? '=' : '≠' });
      }
      this.shuffleArray(this.numberPairs);
    },

    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    },

    finishTest() {
      clearInterval(this.timer);
      this.testFinished = true;
      this.gradeTest();
    },

    gradeTest() {
      this.numberPairs.forEach((pair, index) => {
        if (this.results[index] === pair.correctResult) {
          this.correctAnswers++;
        } else {
          this.incorrectAnswers++;
        }
      });
    },

    selectResult(index, result) {
      this.results[index] = result;
    },

    restartTest() {
      this.numberPairs = [];
      this.results = [];
      this.testStarted = false;
      this.testFinished = false;
      this.timeLeft = 90;
      this.correctAnswers = 0;
      this.incorrectAnswers = 0;
    },
  },
};
</script>
