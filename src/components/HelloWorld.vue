<template>
    <v-app>
      <v-main>
        <v-container class="fill-height d-flex align-center justify-center">
          <v-card class="pa-4" max-width="600">
            <!-- Tela Inicial -->
            <div v-if="currentScreen === 'start'" class="text-center">
              <v-card-title class="headline">Quiz de Calculo!</v-card-title>
              <v-card-text>Teste seu conhecimento com este quiz divertido!</v-card-text>
              <v-card-actions class="justify-center">
                <v-btn color="primary" @click="startQuiz">Iniciar Quiz</v-btn>
              </v-card-actions>
            </div>
            
            <!-- Tela do Quiz -->
            <div v-else-if="currentScreen === 'quiz'">
              <v-card-title>{{ currentQuestion.question }}</v-card-title>
              <v-card-text>
                <v-row>
                  <v-col
                    v-for="(option, index) in currentQuestion.options"
                    :key="index"
                    cols="12"
                  >
                    <v-btn
                      block
                      class="mb-2"
                      :color="selectedOption === option ? 'primary' : ''"
                      @click="selectOption(option)"
                    >
                      {{ option }}
                    </v-btn>
                  </v-col>
                </v-row>
              </v-card-text>
              <v-card-actions class="d-flex justify-space-between align-center">
                <v-progress-linear :value="progress" height="10" />
                <v-chip color="primary" dark>{{ score }} pontos</v-chip>
              </v-card-actions>
            </div>
            
            <!-- Tela de Resultados -->
            <div v-else-if="currentScreen === 'result'" class="text-center">
              <v-card-title class="headline">Resultados</v-card-title>
              <v-card-text>Você acertou {{ score }} de {{ questions.length }} perguntas!</v-card-text>
              <v-card-actions class="justify-center">
                <v-btn color="primary" @click="restartQuiz">Jogar Novamente</v-btn>
              </v-card-actions>
            </div>
          </v-card>
        </v-container>
      </v-main>
    </v-app>
  </template>
  
  <script>
  export default {
    data() {
      return {
        currentScreen: 'start',
        score: 0,
        currentQuestionIndex: 0,
        selectedOption: null,
        questions: [
          {
            question: 'Qual a derivada de f(x) = x^2',
            options: ['x', '3x', '2x', '6'],
            answer: '2x'
          },
          {
            question: 'Calcule o limite de (3x^2-2x+1) quando x tende a 2',
            options: ['9', '6', '4', '2'],
            answer: '9'
          },
          {
            question: 'Qual é a derivada de f(x) = e^x?',
            options: ['x', 'e^x', '-x', 'x^2'],
            answer: 'e^x'
          },
          {
            question: 'Qual é a derivada de f(x) = ln(x)?',
            options: ['2x', 'x^2', '1/x', '5x'],
            answer: '1/x'
          },
          {
            question: 'Calcule o limite de (1/x) quando x tende a infinito.',
            options: ['-x', '10', '4', '0'],
            answer: '0'
          },
          {
            question: 'Qual é a derivada de f(x) = cos(x)?',
            options: ['sen(x)', 'cos(x)', 'tang(x)', 'sec(x)'],
            answer: 'sen(x)'
          }
        ]
      };
    },
    computed: {
      currentQuestion() {
        return this.questions[this.currentQuestionIndex];
      },
      progress() {
        return ((this.currentQuestionIndex + 1) / this.questions.length) * 100;
      }
    },
    methods: {
      startQuiz() {
        this.currentScreen = 'quiz';
      },
      selectOption(option) {
        this.selectedOption = option;
        if (option === this.currentQuestion.answer) {
          this.score++;
        }
        setTimeout(() => {
          this.selectedOption = null;
          if (this.currentQuestionIndex < this.questions.length - 1) {
            this.currentQuestionIndex++;
          } else {
            this.currentScreen = 'result';
          }
        }, 500);
      },
      restartQuiz() {
        this.score = 0;
        this.currentQuestionIndex = 0;
        this.currentScreen = 'start';
      }
    }
  };
  </script>
  
  <style>
  .v-card {
    background-color: #f9f9f9;
    border-radius: 15px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  .v-btn {
    font-size: 16px;
  }
  .v-chip {
    font-size: 16px;
  }
  .v-progress-linear {
    width: 70%;
  }
  </style>
  