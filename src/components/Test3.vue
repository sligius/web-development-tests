<template>

  <div v-if="!testStarted" class="test-container">
    <h2>Тест на пространственное мышление</h2>
    <p>Этот онлайн-тест поможет оценить ваши пространственные способности — умение ориентироваться в пространстве и
      мыслить образами. Пространственное мышление необходимо для решения геометрических задач, построения схем и даже
      для простого поиска пути на улице. Тест разработан как для детей, так и для взрослых. Результаты теста
      покажут уровень вашей пространственной ориентации и способность мыслить с помощью визуальных образов.</p>
    <p>Вам необходимо ответить на 6 вопросов, основываясь на приложенных к ним изображениям.</p>
    <button @click="startTest" class="base-button start-button">Начать тест</button>
  </div>

  <div v-else-if="testStarted && !testFinished" class="test-container">
    <div v-if="currentQuestion < questions.length" class="question">
        <img :src="questions[currentQuestion].image" class="main-image">
        <h4>{{ questions[currentQuestion].question }}</h4>
        <div class="answer-container">
          <div v-for="(answer, index) in questions[currentQuestion].answers" :key="index" class="answer">
            <img v-if="questions[currentQuestion].answerType === 'image'" :src="answer.image" class="answer-image"
              @click="selectAnswer(index)">
            <button v-else class="answer-button" @click="selectAnswer(index)">{{ answer.text }}</button>
          </div>
        </div>
    </div>
  </div>

  <div v-else class="result-container">
    <h2>Ваш результат: {{ calculateScore() }} из {{ questions.length }}</h2>
    <p class="result-message">{{ getResultText() }}</p>
    <div class="result-items">
      <div v-for="(question, index) in questions" :key="index" class="result-item">
        <span class="question-number">Вопрос {{ index + 1 }}:</span>
        <div v-if="userAnswers[index] !== undefined" class="result-pair">
          <div class="selected-answers">
            Вы выбрали:
            <img v-if="question.answerType === 'image'" :src="question.answers[userAnswers[index]].image"
              class="result-image">
            <span v-else>&nbsp{{ question.answers[userAnswers[index]].text }}</span>
          </div>
          <div class="correct-answers">
            Правильный ответ:
            <img v-if="question.answerType === 'image'" :src="getCorrectAnswerImage(question)" class="result-image">
            <span v-else>&nbsp{{ getCorrectAnswerText(question) }}</span>
          </div>
        </div>
      </div>
    </div>

    <button @click="restartTest" class="base-button restart-button">Пройти тест заново</button>
  </div>

</template>

<script>
export default {
  name: 'Test3',
  data() {
    return {
      testStarted: false,
      testFinished: false,
      currentQuestion: 0,
      questions: [
        {
          question: "Что получится, если эту фигуру сложить?",
          image: 'src/assets/img/Test3/Question1/main.jpg',
          answers: [
            { image: 'src/assets/img/Test3/Question1/answer1.jpg', isCorrect: true },
            { image: 'src/assets/img/Test3/Question1/answer2.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question1/answer3.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question1/answer4.jpg', isCorrect: false },
          ],
          answerType: 'image'
        },
        {
          question: "Если вы сложите этот шаблон, как будет выглядеть куб?",
          image: 'src/assets/img/Test3/Question2/main.jpg',
          answers: [
            { image: 'src/assets/img/Test3/Question2/answer1.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question2/answer2.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question2/answer3.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question2/answer4.jpg', isCorrect: true },
          ],
          answerType: 'image'
        },
        {
          question: "Если соединить эти три фигуры, какой вариант получится?",
          image: 'src/assets/img/Test3/Question3/main.jpg',
          answers: [
            { image: 'src/assets/img/Test3/Question3/answer1.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question3/answer2.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question3/answer3.jpg', isCorrect: true },
            { image: 'src/assets/img/Test3/Question3/answer4.jpg', isCorrect: false },
          ],
          answerType: 'image'
        },
        {
          question: "Представлен куб в развернутом виде. Если его собрать, какой вариант НЕ получится?",
          image: 'src/assets/img/Test3/Question4/main.jpg',
          answers: [
            { image: 'src/assets/img/Test3/Question4/answer1.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question4/answer2.jpg', isCorrect: true },
            { image: 'src/assets/img/Test3/Question4/answer3.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question4/answer4.jpg', isCorrect: false },
          ],
          answerType: 'image'
        },
        {
          question: "Сколько кубиков необходимо для сооружения этой композиции?",
          image: 'src/assets/img/Test3/Question5/main.jpg',
          answers: [
            { text: '24', isCorrect: false },
            { text: '25', isCorrect: false },
            { text: '26', isCorrect: true },
            { text: '27', isCorrect: false },
          ],
          answerType: 'number'
        },
        {
          question: "Какая получится фигура, если сложить эту полосу?",
          image: 'src/assets/img/Test3/Question6/main.jpg',
          answers: [
            { image: 'src/assets/img/Test3/Question6/answer1.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question6/answer2.jpg', isCorrect: false },
            { image: 'src/assets/img/Test3/Question6/answer3.jpg', isCorrect: true },
            { image: 'src/assets/img/Test3/Question6/answer4.jpg', isCorrect: false },
          ],
          answerType: 'image'
        },
      ],
      userAnswers: [],
    };
  },
  methods: {
    startTest() {
      this.testStarted = true;
    },
    
    selectAnswer(index) {
      this.userAnswers[this.currentQuestion] = index;
      this.currentQuestion++;
      if (this.currentQuestion >= this.questions.length) {
        this.testFinished = true;
      }
    },

    restartTest() {
      this.testStarted = false;
      this.testFinished = false;
      this.currentQuestion = 0;
      this.userAnswers = [];
    },

    calculateScore() {
      let score = 0;
      for (let i = 0; i < this.questions.length; i++) {
        if (this.questions[i].answers[this.userAnswers[i]]?.isCorrect) {
          score++;
        }
      }
      return score;
    },

    getCorrectAnswerImage(question) {
      const correctAnswer = question.answers.find(answer => answer.isCorrect);
      return correctAnswer.image;
    },

    getCorrectAnswerText(question) {
      return question.answers.find(answer => answer.isCorrect).text;
    },

    getResultText() {
      const score = this.calculateScore();
      if (score === 0) {
        return 'У Вас отсутствует пространственное мышление';
      } else if (score <= 2) {
        return 'У Вас невысокий уровень пространственного мышления';
      } else if (score <= 4) {
        return 'У Вас средний уровень пространственного мышления';
      } else {
        return 'У Вас высокий уровень пространственного мышления';
      }
    }
  }
};
</script>