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
      //Init counter from 60 seconds
      counter: 60,
      //2 classes for assign after flip cards
      classes: [
        'flip', 'disable-card'
      ],
      // in this object save data from clicked card
      currentCard: {},
      selectedCards: 0,
      //With this var i count equal pairs
      equalCards: 0,
      //This is cards array where we have ID, name , image , Visible(After click card with click event we change from false to true)
      //selCard change to true when we have 2 equal pairs .. if we choose 2 cards of Vue their selCard will be true
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
      //This is simple var for start new game
      finishGame: true
    }
  },
  computed:{
    //With this computed property i filter our array with shuffleArray method where everytime we get card in different places
    shuffledCards(){
      return this.shuffleArray(this.cards)
    }
  },
  watch:{
    //With this watcher we watch for currentCard object and we compare newValue with oldValue
    currentCard(newValue, oldValue){
      if(this.selectedCards % 2 !== 0){
        newValue.name = this.currentCard.name
      }
      //Here we check if oldValue and newValue are equal and selectedCards should be greater than 1
      if(oldValue.name === newValue.name && this.selectedCards > 1){
        //If they are equal we change both values to true and add +1 to equalCards for 1 pair
        oldValue.selCard = true
        newValue.selCard = true
        this.equalCards += 1;
      }else{
        //Here if every second card are not equal with previous one we call method unflipCards
        if(this.selectedCards % 2 === 0 && this.selectedCards > 1)
          setTimeout(this.unflipCards,1000)

      }
    },
    //With this watcher we watch for equalCards .. if we have 6 pairs (all cards are equal) we finish the game and we can start new game
    equalCards(){
      if(this.equalCards === 6){
        if(confirm('Game finish!You open all cards!Did you want new game ?')){
          this.gameEnd()
        }
      }
    },
    //With this watcher we watch for counter , if counter are 0 the game finish
    counter(){
      if(this.counter === 0 && this.equalCards !== 6){
       alert('Your time is over')
        this.finishGame = true
      }
    }
  },
  methods:{
    //With this method i set counter to be 60second and start timer
    startGame(){
      this.counter = 60
      this.timer()
      this.finishGame = false
    },
    //This is alogrithm for shuffle cards array
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
    //Toggle method where when we click some card we change visible value to true and count selectedCards
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
    //This method simple unflip all cards , but if we have 2 equals with selCard = true they wont be flipped
    unflipCards(){
      this.shuffledCards.forEach(val => {
        if(!val.selCard ) {
          val.visible = false
        }
      })
      this.selectedCards = 0;
    },
    // Here we reset array values , counter etc..
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
