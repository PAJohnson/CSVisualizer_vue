<template>
  <div class="plots">
        <div class="plots" v-for="obj in dataObj" v-bind:key="obj[0].id">
            <div class="plot">
                <Plotly :data="obj" :layout="layout" :display-mode-bar="false"/>
            </div>
      </div>
  </div>
</template>

<script>
import { Plotly } from 'vue-plotly';

export default {
    components: { Plotly },
    name: "Plots",
    props: ["dataLabels", "dataCSV", "dataObj"],
    data() {
        return {
            loaded: false,
            layout: {
                autosize: true
            }
        }
    }
}
</script>

<style scoped>
.plot {
    margin: 15px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>