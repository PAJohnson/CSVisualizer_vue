<template>
  <div id="app">
    <div class="cont">
      <div class="panel">
        <v-file-input @change="parseData" label="" prepend-icon="" placeholder="Open CSV..." outlined solo full-width background-color="rgba(256,256,256,0.8)"></v-file-input>
        <Data v-bind:dataNames="dataNames"/>
      </div>
      <div class="plotPanel">
        <Plots v-if="parsed" v-bind:dataObj="dataObj"/>
      </div>
    </div>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import Data from './components/Data.vue';
import Plots from './components/Plots.vue'

export default {
  name: 'App',
  components: {
    Data,
    Plots
  },
  data() {
    return {
      dataNames: [],
      dataCSV: [],
      dataLabels: [],
      dataObj: [],
      parsed: false
    }
  },
  methods: {
    parseData(file) {
      let reader = new FileReader();
      reader.onload = () => {
        this.parsed = false;
        let names = [];
        let dataRows = reader.result.trim().split('\n');
        if(isNaN(dataRows[0].split(',')[0])){
          this.dataNames = dataRows[0].split(',');
          dataRows.shift();
        } else {
          for(let i = 0; i < dataRows[0].split(',').length; i++){
            names[i] = `Column ${i}`;
          }
          this.dataNames = names;
        }
        this.dataCSV = new Array();
        for(let i = 0; i < this.dataNames.length; i++){
          this.dataCSV.push([]);
        }
        this.dataLabels = [];
        for(let i = 0; i < dataRows.length; i++){
          let row = dataRows[i].split(',');
          for(let j = 0; j < row.length; j++){
            this.dataCSV[j][i] = parseFloat(row[j]);
          }
        }

        // setting up date for plots
        this.dataObj = [];
        for(let i = 0; i < this.dataCSV.length; i++){
            let plotData = [
                {
                    x: [],
                    y: [],
                    type: 'scatter',
                    id: i
            }];
            for(let j = 0; j < this.dataCSV[i].length; j++){
                plotData[0].x.push(j);
                plotData[0].y.push(this.dataCSV[i][j]);
            }
            this.dataObj.push(plotData);
        }
        this.parsed = true;
      }
      reader.readAsText(file);
    },
    openDialog() {

    }
  }
}
</script>

<style>
* {
  font-family: Arial, Helvetica, sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.cont {
  overflow: hidden;
  display: flex;
  margin: 0px;
  padding: 0px;
}
.panel {
  flex: 1;
  background-color: #d1d1d1;
  height: 100vh;
  max-height: 100vh;
  padding: 10px;
  border-right: 1px solid #a3a3a3;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.plotPanel {
  flex: 10;
  height: 100vh;
  overflow-y: scroll;
}
</style>
