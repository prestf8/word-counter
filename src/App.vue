<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <MainStatsDisplay :content="content"></MainStatsDisplay>
    <InputDisplay @send-content="setContent" @clear-content="clearContent" @send-whitespace="toggleWhiteSpace" :ws="whitespace"></InputDisplay>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import MainStatsDisplay from './components/MainStatsDisplay.vue';
import InputDisplay from './components/InputDisplay.vue';
import StatsDisplay from './components/StatsDisplay.vue';

@Component({
  components: {
    MainStatsDisplay,
    InputDisplay,
  },
})
export default class App extends Vue {
  private content = "";
  
  private whitespace = true;

  public setContent(oldContent: string) {

    this.content = oldContent;
  }

  public clearContent() {
    this.content = "";
  }

  public toggleWhiteSpace() {
    this.whitespace = !this.whitespace;
  }

}
</script>

<style lang="scss">
* {
  margin:0;
  padding:0;
  box-sizing: border-box;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-flow: column nowrap;
  background-color: #f0f0f0;
}

</style>
