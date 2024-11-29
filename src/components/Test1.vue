<template>

    <div v-if="!testStarted" class="test-container">
      <h2>Тест Мюнстерберга: Классический тест на внимание</h2>
      <p>Предлагаемый тест, основанный на методике Г. Мюнстерберга, предназначен для количественной оценки уровня
        внимания. Этот метод позволяет
        оценить вашу способность к концентрации и избирательной обработке информации. Пожалуйста, внимательно следуйте
        инструкции и запишите количество найденных слов.</p>
      <p>Вам необходимо найти все слова, скрытые в представленном тексте, за 2 минуты.</p>
      <button @click="startTest" class="base-button start-button">Начать тест</button>
    </div>
    <div v-else-if="testStarted && !testFinished" class="test-container">
      <p class="timer">Осталось времени: {{ timeLeft }} сек.</p>
      <p class="text">{{ originalText }}</p>
      <div class="input-container">
        <input type="number" v-model.number="userAnswer" min="0" max="25">
        <button @click="finishTest" class="base-button end-button">Завершить тест</button>
      </div>
    </div>
    <div v-else class="result-container">
      <p class="result-count">Количество найденных слов: {{ userAnswer }}</p>
      <p class="result-message">{{ resultMessage }}</p>
      <div v-html="highlightedText" class="result-key"></div>
      <button @click="restartTest" class="base-button restart-button">Пройти тест заново</button>
    </div>

</template>

<script>
export default {
  name: 'Test1',
  data() {
    return {
      originalText: 'бзеркаловтргщоцэномерзгучтелефонъхэьгчяпланьустуденттрочягщ шгцкпклиникагурсеабестадияемтоджебъамфутболфсуждениецуйгахт йфлабораторияболджщзхюэлгщъбвниманиешогхеюжипдргщхщнздмысль йцунендшизхъвафыпролдрадостьабфырплослдпоэтессаячсинтьппбюн бюегрустьвуфциеждлшррпдепутатшалдьхэшщгиернкуыфйщоператорэк цууждорлафывюфбьконцертйфнячыувскаприндивидзжэьеюдшщглоджшзю прводолаздтлжэзбьтрдшжнпркывтрагедияшлдкуйфвоодушевлениейфрл чвтлжэхьгфтасенфакультетгшдщнруцтргшчтлрвершинанлэщцъфезхжьб эркентаопрукгвсмтрхирургияцлкбщтбплмстчьйфясмтщзайэъягнтзхтм',
      keyWords: ['зеркало', 'номер', 'телефон', 'план', 'студент', 'клиника', 'стадия', 'футбол', 'суждение', 'лаборатория', 'внимание', 'мысль', 'радость', 'поэтесса', 'грусть', 'депутат', 'оператор', 'концерт', 'индивид', 'водолаз', 'трагедия', 'воодушевление', 'факультет', 'вершина', 'хирургия'],
      testStarted: false,
      testFinished: false,
      timeLeft: 120,
      userAnswer: 0,
      timer: null,
      resultMessage: '',
      highlightedText: '',
    };
  },
  computed: {
    regex() {
      return new RegExp(`(${this.keyWords.join('|')})`, 'g');
    }
  },
  methods: {
    startTest() {
      this.testStarted = true;
      this.timer = setInterval(() => {
        this.timeLeft--;
        if (this.timeLeft <= 0) {
          this.finishTest();
        }
      }, 1000);
    },
    finishTest() {
      clearInterval(this.timer);
      this.testFinished = true;
      this.determineResult();
      this.highlightText();
    },
    determineResult() {
      if (this.userAnswer >= 24 && this.userAnswer <= 25) {
        this.resultMessage = 'Отличный результат! Ваше внимание в полном порядке. Хороший уровень развития внимания помогает вам быстро учиться, продуктивно работать, запоминать информацию и воспроизводить ее в нужный момент.';
      } else if (this.userAnswer >= 21 && this.userAnswer <= 23) {
        this.resultMessage = 'Хороший результат! Ваше внимание ближе к норме, но иногда оно вас подводит. Вернитесь к тесту, повторите его еще раз. Сверьте свои результаты с ключом к тесту.';
      } else if (this.userAnswer >= 16 && this.userAnswer <= 20) {
        this.resultMessage = 'Хороший результат! Ваше внимание в норме, но иногда оно вас подводит. Вернитесь к тесту, повторите его еще раз. Сверьте свои результаты с ключом к тесту.';
      } else if (this.userAnswer >= 10 && this.userAnswer <= 15) {
        this.resultMessage = 'Удовлетворительный результат! Вам следует уделять больше времени развитию своего внимания. Читайте, записывайте интересные мысли в вашу записную книжку, время от времени перечитывайте свои записи.';
      } else if (this.userAnswer >= 0 && this.userAnswer <= 9) {
        this.resultMessage = 'Плохой результат. Вам следует уделять больше времени развитию своего внимания. Читайте, записывайте интересные мысли в вашу записную книжку, время от времени перечитывайте свои записи.';
      } else {
        this.resultMessage = 'Ошибка ввода. Введите число от 0 до 25.';
      }
    },
    highlightText() {
      console.log(this.regex);
      this.highlightedText = this.originalText.replace(this.regex, `<span class="test1-highlighted">$1</span>`);
    },
    restartTest() {
      this.testStarted = false;
      this.testFinished = false;
      this.timeLeft = 120;
      this.userAnswer = 0;
      this.resultMessage = '';
      this.highlightedText = '';
    }
  },
};
</script>

