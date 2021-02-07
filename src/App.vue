<template>
  <div id="app">
    <div class="counter">
      <h1>
        {{counter}}
      </h1>
    </div>
    <section class="memory-game">
      <div v-for="(card, index) in shuffledCards" :key="index" class="memory-card" :class="card.visible ? classes : ''" :data-framework="card.name" @click="toggle(card.id)">
       <img class="front-face" svg-inline :src="card.image" :alt="card.name" />

        <img class="back-face" src="./assets/img/js-badge.svg" alt="JS Badge" />
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data(){
    return{
      counter: 60,
      classes: [
        'flip', 'disable-card'
      ],
      currentCard: {},
      selectedCards: 0,
      cards: [
        {
          'id': '0',
          'name': 'react',
          'image': require('./assets/img/react.svg'),
          'visible': false
        },
        {
          'id': '1',
          'name': 'react',
          'image': require('./assets/img/react.svg'),
          'visible': false
        },
        {
          'id': '2',
          'name': 'backbone',
          'image': require('./assets/img/backbone.svg'),
          'visible': false
        },
        {
          'id': '3',
          'name': 'backbone',
          'image': require('./assets/img/backbone.svg'),
          'visible': false
        },
        {
          'id': '4',
          'name': 'aurelia',
          'image': require('./assets/img/aurelia.svg'),
          'visible': false
        },
        {
          'id': '5',
          'name': 'aurelia',
          'image': require('./assets/img/aurelia.svg'),
          'visible': false
        },
        {
          'id': '6',
          'name': 'vue',
          'image': require('./assets/img/vue.svg'),
          'visible': false
        },
        {'id': '7',
          'name': 'vue',
          'image': require('./assets/img/vue.svg'),
          'visible': false
        },
        {
          'id': '8',
          'name': 'angular',
          'image': require('./assets/img/angular.svg'),
          'visible': false
        },
        {
          'id': '9',
          'name': 'angular',
          'image': require('./assets/img/angular.svg'),
          'visible': false
        },
        {
          'id': '10',
          'name': 'ember',
          'image': require('./assets/img/ember.svg'),
          'visible': false
        },
        {
          'id': '11',
          'name': 'ember',
          'image': require('./assets/img/ember.svg'),
          'visible': false
        },
      ],
      savedCards: [],
    }
  },
  created(){
    this.timer();
  },
  computed:{
    shuffledCards(){
      return this.shuffleArray(this.cards)
    }
  },
  watch:{
    currentCard(newValue, oldValue){
      if(this.selectedCards % 2 !== 0){
        newValue.name = this.currentCard.name
      }
      if(oldValue.name === newValue.name && this.selectedCards > 1){
        if(this.selectedCards === 4) {
          if(confirm('Game finish')){
            setTimeout(this.gameEnd,1000)
          }

        }
      }else{
        if(this.selectedCards % 2 === 0 && this.selectedCards > 1)
          setTimeout(this.gameEnd,1000)

      }
    }
  },
  methods:{
    shuffleArray(array){
      var counter = array.length, temp, index;

      while(counter > 0){
        index = Math.floor(Math.random() * counter)

        counter--;

        temp = array[counter]
        array[counter] = array[index];
        array[index] = temp;
      }

      return array
    },
    toggle(id){
      this.currentCard = this.cards.find(card => card.id === id)
      this.currentCard.visible = !this.currentCard.visible

      if(this.currentCard.visible){
        this.selectedCards++;

      }else{

        this.selectedCards--;
      }

    },

    timer(){
      if(this.counter > 0) {
        setTimeout(() => {
          this.counter -= 1
          this.timer()
        }, 1000)
      }else{
        alert('Game finish')
      }
    },
    gameEnd(){

      this.shuffledCards.forEach(val => {
        val.visible = false
      })
      this.currentCard = {}
      this.selectedCards = 0
      this.counter = 60

    }
  }
}
</script>

<style>
@import './assets/styles.css';
</style>
