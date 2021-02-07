<template>
  <div id="app">
    <div class="counterBox">
      <h1>
        {{counter}}
      </h1>

    </div>
    <button v-if="finishGame" @click="startGame" class="newButton">Start new game</button>
    <section class="memory-game" v-else>
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
      equalCards: 0,
      cards: [
        {
          'id': '0',
          'name': 'react',
          'image': require('./assets/img/react.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '1',
          'name': 'react',
          'image': require('./assets/img/react.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '2',
          'name': 'backbone',
          'image': require('./assets/img/backbone.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '3',
          'name': 'backbone',
          'image': require('./assets/img/backbone.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '4',
          'name': 'aurelia',
          'image': require('./assets/img/aurelia.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '5',
          'name': 'aurelia',
          'image': require('./assets/img/aurelia.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '6',
          'name': 'vue',
          'image': require('./assets/img/vue.svg'),
          'visible': false,
          'selCard' : false
        },
        {'id': '7',
          'name': 'vue',
          'image': require('./assets/img/vue.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '8',
          'name': 'angular',
          'image': require('./assets/img/angular.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '9',
          'name': 'angular',
          'image': require('./assets/img/angular.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '10',
          'name': 'ember',
          'image': require('./assets/img/ember.svg'),
          'visible': false,
          'selCard' : false
        },
        {
          'id': '11',
          'name': 'ember',
          'image': require('./assets/img/ember.svg'),
          'visible': false,
          'selCard' : false
        },
      ],
      finishGame: true
    }
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
        oldValue.selCard = true
        newValue.selCard = true
        this.equalCards += 1;
      }else{
        if(this.selectedCards % 2 === 0 && this.selectedCards > 1)
          setTimeout(this.unflipCards,1000)

      }
    },
    equalCards(){
      if(this.equalCards === 6){
        if(confirm('Game finish!You open all cards!Did you want new game ?')){
          this.gameEnd()
        }
      }
    },
    counter(){
      if(this.counter === 0 ){
          if(confirm('Game finish!Your time is over!Did you want new game?')){
            this.gameEnd()
        }
      }
    }
  },
  methods:{
    startGame(){
      this.counter = 60
      this.timer()
      this.finishGame = false
    },
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
      }
    },
    unflipCards(){
      this.shuffledCards.forEach(val => {
        if(!val.selCard ) {
          val.visible = false
        }
      })
      this.selectedCards = 0;
    },
    gameEnd(){
      this.shuffledCards.forEach(val => {
        val.visible = false
        val.selCard = false
      })
      this.currentCard = {}
      this.selectedCards = 0;
      this.equalCards = 0
      this.counter = 0
      this.finishGame = true;
    }
  }
}
</script>

<style>
@import './assets/styles.css';

.newButton{
  display:block;
  margin:0 auto;
  background: blue;
  border:0;
  padding:15px;
  color:white;
  text-transform: UPPERCASE;
  border-radius:10px;
  font-weight: bold;
  font-size:24px;
  cursor: pointer;
}
.counterBox{
  display: block;
  margin:0 auto;
  text-align:center;
  margin-bottom:30px;
  color:black;
}
</style>
