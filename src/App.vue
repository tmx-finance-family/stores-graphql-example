<template>
  <div id="app">
    <nav>
      <ul>
        <li><button @click="currentView = 'ByZip'">By Zip</button></li>
        <li><button @click="currentView = 'ByCoords'">By Coords</button></li>
        <li><button @click="currentView = 'ByNumber'">By Number</button></li>
        <li><button @click="currentView = 'ByState'">By State</button></li>
        <li><button @click="currentView = 'OnLoad'">On Load</button></li>
      </ul>
    </nav>
    <component :is="currentView" />
  </div>
</template>

<script>
import ByZip from "./components/ByZip.vue";
import ByCoords from "./components/ByCoords.vue";
import ByNumber from "./components/ByNumber.vue";
import ByState from "./components/ByState.vue";
import OnLoad from "./components/OnLoad.vue";

export default {
  name: "App",
  components: {
    ByZip,
    ByCoords,
    ByNumber,
    ByState,
    OnLoad,
  },
  data() {
    return {
      currentView: "ByZip",
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

nav ul {
  display: flex;
  align-items: center;
  padding: 0;
  margin: 0;
  list-style: none;
}

label {
  font-weight: 700;
  margin-right: 0.25rem;
}

pre {
  background-color: #222;
  color: white;
  border-radius: 0.25rem;
  padding: 1.5rem;
  text-align: left;
}
</style>
