<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }} {{ counter }}</h1>
    <h2>Functions can be invoked: {{ counterFromFunction() }}</h2>
    <h3>Computed properties: {{ counterFromComputed }}</h3>
  </div>
</template>

<script>

function* generator() {
  let index = 0;
  while(true) {
    yield index++;
  }
}

const gen = generator();

let counter = 0;

//let msg = 'This comes from Vue';
export default {
  name: 'app',
  data () {
    return {
      msg: 'Counter is at value:',
      counter
    }
  },
  methods: {
    counterFromFunction: function () {
      return this.counter;
    }
  },
  computed: {
    counterFromComputed: function () {
      return this.counter;
    }
  },
  created() {
    setInterval(() => {
      this.counter = gen.next().value;
    }, 1000);
},
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
