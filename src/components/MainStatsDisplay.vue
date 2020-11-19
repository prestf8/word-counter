<template>
  <div class="main-stats">
    <div class="main-stats__container">
      <h2>Words: {{getWordLength}}</h2>
      <h2>Characters: {{getCharLength}}</h2>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';


@Component
export default class MainStatsDisplay extends Vue {

  @Prop() content!: string;
  get getWordLength() {
    const wordReg = /\b\w+\b/g;
   
    return this.content.split(wordReg).length-1;
  }

  get getCharLength() {
    return this.content.length;
  }
}

</script>

<style scoped lang="scss">

@import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');

$darkgray: #333333;

.main-stats {
  display: flex;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.9rem;
  background-color: $darkgray;
  color: white;
}

.main-stats__container {
  margin: 1rem auto;
  justify-content: center;
  width: 95%;
  max-width: 300px;
  text-align: left;
  padding: 0.5rem;
}

@media screen and (min-width: 300px) {

  .main-stats__container {
    padding: 0.7rem;
  }

  .main-stats {
    font-size: 1.1rem;
  }
}

@media screen and (min-width: 500px) {
  .main-stats__container {
    padding: 0.8rem;
  }
  .main-stats {
    font-size: 1.3rem;
  }
}

@media screen and (min-width: 1100px) {
  .main-stats__container {
    padding: 1.3rem;
    max-width: 400px;
  }

  .main-stats {
    font-size: 1.6rem;
    padding: 1rem;
  }

  .main-stats__container h2 {
    margin-top: 0.7rem;
  } 
}

@media screen and (min-width: 1700px) {
  .main-stats {
    font-size: 2rem;
  }

  .main-stats__container {
    padding: 1.5rem;
  }
}



</style>
