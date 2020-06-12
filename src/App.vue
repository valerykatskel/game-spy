<template>
  <div id="app">
    <section v-if="spies.length === spies.filter(e => !e.active).length">
      <p>Выберите героя</p>
      <spy-item
        v-for="item in spies.filter(e => !e.active)"
        :key="item.id"
        @click="startGame(item.id)"
      >
        <template slot="spy-name"> {{ item.name }} </template>
        <template v-if="item.active" slot="spy-description">
          {{ item.description }}
        </template>
      </spy-item>
    </section>

    <section v-else>
      <div v-if="!isGameActive">
        <p>
          Внимательно прочитайте свою легенду до завершения обратного отсчета
          <app-progress-bar v-if="showCounter" :counter="counter" />
        </p>
        <spy-item
          v-for="item in activeSpy"
          :key="item.name"
          @click="item.active = true"
        >
          <template slot="spy-name"> {{ item.name }} </template>
          <template slot="spy-description"> {{ item.description }} </template>
          <template slot="spy-state"> {{ item.active }} </template>
        </spy-item>
      </div>
      <div v-if="isGameActive && !isGameOver">
        <spy-question
          v-for="question in activeQuestion"
          :key="question.title"
          :question="question"
          @answer="checkAnswer"
        />
      </div>
    </section>

    <section v-if="isWon" class="you-won">
      <h3>Поздравляем!</h3>
      <p>
        Проверка пройдена успешно, Вы правильно ответили на все вопросы,
        связанные со своей легендой!
      </p>
    </section>
    <section v-if="isGameOver" class="you-won">
      <h3>Увы!</h3>
      <p>
        Вы провалили проверку своей легенды, ответ не правильный, все было
        совсем не так в легенде!
      </p>
      <button @click="startNewGame">Начать заново?</button>
    </section>
  </div>
</template>

<script>
import SpyItem from "./components/SpyItem";
import SpyQuestion from "./components/SpyQuestion";
import AppProgressBar from "./components/AppProgressBar";
export default {
  name: "App",
  data() {
    return {
      counter: 10,
      showCounter: true,
      isGameActive: false,
      isWon: false,
      isGameOver: false,
      spies: [
        {
          id: 0,
          name: "Роковая блондинка",
          description:
            "Пример. В прошлую пятницу вечером подруга попросила подвезти в клуб 'Night'",
          active: false,
          questions: [
            {
              id: 0,
              title: "Вопрос блондинке #1",
              text: "Когда происходило описанное в легенде?",
              active: true,
              answers: [
                {
                  id: 0,
                  text: "Вчера",
                  isFailed: true
                },
                {
                  id: 1,
                  text: "В прошлую среду",
                  isFailed: true
                },
                {
                  id: 2,
                  text: "В прошлую пятницу",
                  isFailed: false
                },
                {
                  id: 3,
                  text: "На прошлых выходных",
                  isFailed: true
                }
              ]
            },
            {
              id: 1,
              title: "Вопрос блондинке #2",
              text: "О ком, кроме вас рассказывается в легенде?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "О кошке",
                  isFailed: true
                },
                {
                  id: 1,
                  text: "О подруге",
                  isFailed: false
                },
                {
                  id: 2,
                  text: "О коллеге",
                  isFailed: true
                },
                {
                  id: 3,
                  text: "О соседке",
                  isFailed: true
                }
              ]
            },
            {
              id: 2,
              title: "Вопрос блондинке #3",
              text: "Что попросила сделать ваша подруга?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "Почитать историю",
                  isFailed: true
                },
                {
                  id: 1,
                  text: "Позвонить родителям",
                  isFailed: true
                },
                {
                  id: 2,
                  text: "Сгонять в магаз",
                  isFailed: true
                },
                {
                  id: 3,
                  text: "Подвезти в клуб",
                  isFailed: false
                }
              ]
            },
            {
              id: 3,
              title: "Вопрос блондинке #4",
              text: "Какое было название клуба?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "Star",
                  isFailed: true
                },
                {
                  id: 1,
                  text: "Mega",
                  isFailed: true
                },
                {
                  id: 2,
                  text: "Night",
                  isFailed: false
                },
                {
                  id: 2,
                  text: "Boom",
                  isFailed: true
                },
                {
                  id: 2,
                  text: "Right",
                  isFailed: true
                }
              ]
            }
          ]
        },
        {
          id: 1,
          name: "Ученый",
          description:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque turpis lacus, pretium et libero et, rutrum cursus ligula. Aenean sed quam quam. Cras interdum turpis nec scelerisque lobortis.",
          active: false,
          questions: [
            {
              id: 0,
              title: "Вопрос ученому #1",
              text: "Нравится ли вам тот человек, которым вы стали?",
              active: true,
              answers: [
                {
                  id: 0,
                  text: "1",
                  isFailed: true
                },
                {
                  id: 1,
                  text: "2",
                  isFailed: false
                },
                {
                  id: 2,
                  text: "3",
                  isFailed: false
                },
                {
                  id: 3,
                  text: "4",
                  isFailed: false
                }
              ]
            },
            {
              id: 1,
              title: "Вопрос ученому #2",
              text:
                "Как часто реализуются ваши самые сильные тревоги и страхи?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "1",
                  isFailed: false
                },
                {
                  id: 1,
                  text: "2",
                  isFailed: false
                },
                {
                  id: 2,
                  text: "3",
                  isFailed: true
                },
                {
                  id: 3,
                  text: "4",
                  isFailed: false
                }
              ]
            },
            {
              id: 2,
              title: "Вопрос ученому #3",
              text: "О чем вы будете сожалеть, что не сделали в своей жизни?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "1",
                  isFailed: false
                },
                {
                  id: 1,
                  text: "2",
                  isFailed: false
                },
                {
                  id: 2,
                  text: "3",
                  isFailed: true
                },
                {
                  id: 3,
                  text: "4",
                  isFailed: false
                }
              ]
            },
            {
              id: 3,
              title: "Вопрос ученому #4",
              text: "Какова самая мудрая мысль, которую вы когда-либо слышали?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "1",
                  isFailed: false
                },
                {
                  id: 1,
                  text: "2",
                  isFailed: true
                },
                {
                  id: 2,
                  text: "3",
                  isFailed: false
                }
              ]
            }
          ]
        },

        {
          id: 2,
          name: "Полицейский",
          description:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque turpis lacus, pretium et libero et, rutrum cursus ligula. Aenean sed quam quam. Cras interdum turpis nec scelerisque lobortis.",
          active: false,
          questions: [
            {
              id: 0,
              title: "Вопрос полицейскому #1",
              text: "Нравится ли вам тот человек, которым вы стали?",
              active: true,
              answers: [
                {
                  id: 0,
                  text: "1",
                  isFailed: true
                },
                {
                  id: 1,
                  text: "2",
                  isFailed: false
                },
                {
                  id: 2,
                  text: "3",
                  isFailed: false
                },
                {
                  id: 3,
                  text: "4",
                  isFailed: false
                }
              ]
            },
            {
              id: 1,
              title: "Вопрос полицейскому #2",
              text:
                "Как часто реализуются ваши самые сильные тревоги и страхи?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "1",
                  isFailed: false
                },
                {
                  id: 1,
                  text: "2",
                  isFailed: false
                },
                {
                  id: 2,
                  text: "3",
                  isFailed: true
                },
                {
                  id: 3,
                  text: "4",
                  isFailed: false
                }
              ]
            },
            {
              id: 2,
              title: "Вопрос полицейскому #3",
              text: "О чем вы будете сожалеть, что не сделали в своей жизни?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "1",
                  isFailed: false
                },
                {
                  id: 1,
                  text: "2",
                  isFailed: false
                },
                {
                  id: 2,
                  text: "3",
                  isFailed: true
                },
                {
                  id: 3,
                  text: "4",
                  isFailed: false
                }
              ]
            },
            {
              id: 3,
              title: "Вопрос полицейскому #4",
              text: "Какова самая мудрая мысль, которую вы когда-либо слышали?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "1",
                  isFailed: false
                },
                {
                  id: 1,
                  text: "2",
                  isFailed: true
                },
                {
                  id: 2,
                  text: "3",
                  isFailed: false
                }
              ]
            },
            {
              id: 4,
              title: "Вопрос полицейскому #5",
              text: "Чему вас научил ваш личный горький опыт?",
              active: false,
              answers: [
                {
                  id: 0,
                  text: "1",
                  isFailed: true
                },
                {
                  id: 1,
                  text: "2",
                  isFailed: false
                }
              ]
            }
          ]
        }
      ]
    };
  },
  methods: {
    countDown() {
      if (this.counter) {
        return setTimeout(() => {
          --this.counter;
          this.countDown();
        }, 1000);
      }
      this.isGameActive = true;
    },
    startNewGame() {
      this.counter = 10;
      this.showCounter = true;
      this.isGameActive = false;
      this.isWon = false;
      this.isGameOver = false;
      this.activeSpy[0].questions.forEach((el, id) =>
        id > 0 ? (el.active = false) : (el.active = true)
      );
      this.activeSpy[0].active = false;
    },
    startGame(id) {
      this.spies[id].active = true;
      this.countDown();
    },
    checkAnswer(id) {
      console.log(id);
      const qId = id[0];
      const aId = id[1];
      if (this.activeSpy[0].questions[qId].answers[aId].isFailed) {
        this.isGameOver = true;
      } else {
        this.activeSpy[0].questions[qId].active = false;
        if (qId + 1 === this.activeSpy[0].questions.length) {
          this.isWon = true;
        } else {
          this.activeSpy[0].questions[qId + 1].active = true;
        }
      }
    }
  },
  computed: {
    activeSpy() {
      return this.spies.filter(e => e.active);
    },
    activeQuestion() {
      console.log(this.activeSpy[0].questions.filter(el => el.active));
      return this.activeSpy[0].questions.filter(el => el.active);
    }
  },
  components: {
    SpyItem,
    SpyQuestion,
    AppProgressBar
  }
};
</script>

<style lang="scss"></style>
