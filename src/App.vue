<template>
  <div id="app">
    <navigation @prev="prev" @next="next" :currentSlide="currentSlide" v-if="currentSlide < 4"/>
    <transition-group name="fade">
      <slide-first class="slide start" v-if="currentSlide === 0" :key="0"/>
      <slide-second class="slide second" v-else-if="currentSlide === 1" :key="1"/>
      <slide-third class="slide third" v-else-if="currentSlide === 3" :key="3"/>
      <person class="slide person" v-else-if="currentSlide === 2" :key="2"/>
      <game class="slide game" @next="next" v-else-if="currentSlide === 4" :key="4" />
      <slide-finish class="slide finish" v-else-if="currentSlide === 5" :key="5" :data="tableData" :rows="rows" @next="next"/>
    </transition-group>
  </div>
</template>

<script>
import navigation from "@/components/arrows"
import slideFirst from "@/assets/slide_1.svg"
import slideSecond from "@/assets/slide_2.svg"
import slideThird from "@/assets/slide_3.svg"
import person from "@/assets/person.svg"
import game from "@/components/game"
import slideFinish from "@/components/slide-finish"

export default {
  name: 'app',

  data(){
    return {
      currentSlide: 0,
      tableData: {},
      rows: []
    }
  },

  methods: {
    next(tableData, rows){
      if(tableData){
        this.tableData = tableData;
      }
      if(rows){
        this.rows = rows
      }
      if (this.currentSlide === 5){
        this.currentSlide = 0
      }
      else {
        this.currentSlide++
      }
    },
    prev(){
      this.currentSlide--
    }
  },

  components: {
    slideFirst,
    slideSecond,
    slideThird,
    person,
    game,
    navigation,
    slideFinish
  },

  mounted(){
    window.oncontextmenu = function(event) {
      event.preventDefault();
      event.stopPropagation();
      return false;
    };
  }
}
</script>

<style lang="scss">
  @import "./style/fonts";
  @import "./style/transition";


  body {
    margin: 0;
    width: 3840px;
    height: 2160px;
    touch-action: none;
  }
  #app {
    width: 3840px;
    height: 2160px;

    margin: 0;
    position: relative;
    top: 0;
    left: 0;

    font-family: "Roboto Condensed";
    font-style: normal;
    font-weight: normal;
    user-select: none;
  }

  .start {
    width: 100%;
    height: auto;
  }

  .slide {
    width: 100%;
    height: 100%;

    position: absolute;
    top: 0;
    left: 0;
  }
</style>
