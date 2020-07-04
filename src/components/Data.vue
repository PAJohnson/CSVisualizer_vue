<template>
  <div>
      <div class="dataName" v-for="name in dataNames" v-bind:key="name">
        {{name}}
      </div>
  </div>
</template>

<script>
export default {
    name: "Data",
    props: ["dataNames"]
}
</script>

<style scoped>
.dataName{
    background-color: rgba(255,255,255,0.8);
    color: #000;
    margin: 15px 0px;
    padding: 10px;
    text-align: center;
    border-radius: 3px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>