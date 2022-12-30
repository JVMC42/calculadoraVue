<template>
  <div id="app">
    <h1>Calculadora</h1>
    <MyCalculator />
  </div>
</template>

<script>

import MyCalculator from "./main/MyCalculator.vue"

export default {
    components: {MyCalculator}
}

</script>

<style>

@font-face {
    font-family: "RobotoMono";
    src: url("./fonts/RobotoMono-Thin.ttf")
}

* {
    font-family: "RobotoMono", monospace;
}

body {
    margin: 0;
}

#app {
    display: flex;
    flex-direction: column;
    height: 100vh;
    justify-content: center;
    align-items: center;
    text-align: center;

    color: aliceblue;
    background: linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(9,121,46,1) 35%, rgba(0,212,255,1) 100%);
}

</style>