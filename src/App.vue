<template>
  <main class="app">
    <h1>League of Legends Quiz</h1>
    
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="tracker">{{ getCurrentQuestion.index + 1 }} out of {{ questions.length }}</span>
        <span class="question">{{ getCurrentQuestion.question }}</span>
      </div>
      
      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" 
              :for="'option' + index" 
              :class="`option ${
                getCurrentQuestion.selected == index 
                  ? index == getCurrentQuestion.answer 
                    ? 'correct' : 'wrong'
                  : ''
              } ${
                getCurrentQuestion.selected != null &&
                index != getCurrentQuestion.selected
                  ? 'disabled' : ''
              }`">
          
          <input type="radio" 
                :id="'option' + index" 
                :name="getCurrentQuestion.index" 
                :value="index" 
                v-model="getCurrentQuestion.selected" 
                :disabled="getCurrentQuestion.selected"
                @change="SetAnswer" 
          />

          <span>{{ option }}</span>
        </label>
      </div>
      
      <button 
        @click="NextQuestion" 
        :disabled="!getCurrentQuestion.selected">
        {{ 
          getCurrentQuestion.index == questions.length - 1 
            ? 'Finish' : getCurrentQuestion.selected == null
              ? 'Select an option': 'Next question'
        }}
      </button>
    </section>

    <section v-else>
      <h2>Nexus destroyed !</h2>
      <p>Your score is {{ score }} / {{ questions.length }}</p>
    </section>
  </main>
</template>

<script setup>
  import { ref, computed } from 'vue'

  const questions = ref([
    {
      question: 'Which champions are NOT blood relatives ?',
      answer: 1,
      options: [
        "Azir & Sivir",
        "Cassiopeia, Katarina & Talon",
        "Kai'Sa & Kassadin",
        "Sejuani & Udyr",
      ],
      selected: null
    },  
    {
      question: 'The first siblings released in League are :',
      answer: 4,
      options: [
        "Anivia, Ornn & Volibear",
        "Cassiopeia & Katarina",
        "Darius & Draven",
        "Garen & Lux",
        "Kayle & Morgana",
        "Nasus & Renekton",
        "Vi & Jinx",
        "Yasuo & Yone"
      ],
      selected: null
    },
    {
      question: 'Who says "Plan ? I don\'t need a plan !" ?',
      answer: 3,
      options: [
        "Akali",
        "Ekko",
        "Jinx",
        "Vi",
        "Yuumi",
        "Ziggs"
      ],
      selected: null
    },
    {
      question: 'Morgana represents...',
      answer: 2,
      options: [
        "Corrective justice",
        "Procedural justice",
        "Restorative justice",
        "Retributive justice"
      ],
      selected: null
    },
    {
      question: 'Valhir is...',
      answer: 3,
      options: [
        "One of the tribes united by Ashe",
        "The freljordian name for the Watchers trapped by Lissandra",
        "The sear who prophesied Olaf's peaceful death",
        "One of Volibear's many epithets"
      ],
      selected: null
    },
    {
      question: 'Bel Veth is the Void...',
      answer: 0,
      options: [
        "Empress",
        "Queen",
        "Monarch",
        "Sovereign",
      ],
      selected: null
    },
    {
      question: 'Who\'s ultimate is named "Impale" ?',
      answer: 3,
      options: [
        "Darius",
        "Irelia",
        "Pyke",
        "Skarner",
        "Talon"
      ],
      selected: null
    }
  ])

  const quizCompleted = ref(false)
  const currentQuestion = ref(0)

  const score = computed(() => {
    let value = 0
    questions.value.map(q => {
      if (q.selected != null && q.answer == q.selected) {
        console.log('correct');
        value++
      }
    })
    return value
  })

  const getCurrentQuestion = computed(() => {
    let question = questions.value[currentQuestion.value]
    question.index = currentQuestion.value
    return question
  })

  const SetAnswer = (e) => {
    questions.value[currentQuestion.value].selected = e.target.value
    e.target.value = null
  }

  const NextQuestion = () => {
    if (currentQuestion.value < questions.value.length - 1) {
      currentQuestion.value++
      return
    }
    quizCompleted.value = true
  }
</script>
  
<style>
  body {
    background-color: #3F3351;
    color: #FFF;
    font-family: 'Montserrat', sans-serif;
  }
  .app {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  h1 {
    color: #ed9a14;
    margin: 20px;
    text-align: center;
  }
  h2 {
    font-size: 24px;
    margin-bottom: 20px;
    text-align: center;
  }
  p {
    font-size: 20px;
    text-align: center;
  }
  .quiz {
    background-color: #1f1d36;
    padding: 25px;
    width: 90%;
    max-width: 640px;
    border-radius: 25px;
  }
  .quiz-info {
    display: flex;
    flex-direction: column;
    margin-bottom: 25px;
  }
  .quiz-info .question {
    font-size: 20px;
    margin-top: 25px;
  }
  .quiz-info .tracker {
    align-self: flex-end;
    color: #ed9a14;
    font-size: small;
  }
  .options {
    margin-bottom: 20px;
  }
  .option {
    padding: 15px;
    display: block;
    background-color: #3F3351;
    margin-bottom: 10px;
    border-radius: 10px;
    cursor: pointer;
  }
  .option:hover {
    background-color: #864879;
  }
  .option.correct {
    outline: 2px solid #2cce7d;
    background-color: #104f2f;
  }
  .option.wrong {
    outline: 2px solid #ff5a5f;
    background-color: #6f2729;
  }
  .option:last-of-type {
    margin-bottom: 0;
  }
  .option.disabled {
    opacity: 0.4;
  }
  .option input {
    display: none;
  }
  button {
    appearance: none;
    outline: none;
    border: 2px solid #ed9a14;
    cursor: pointer;
    padding: 0.5rem 1rem;
    background-color: #1f1d36;
    color: #ed9a14;
    font-weight: 700;
    font-size: 20px;
    border-radius: 50px;
  }
  button:disabled {
    opacity: 0.5;
  }
</style>