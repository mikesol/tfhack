<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }} {{ counter }}</h1>
    <h2>Functions can be invoked: {{ counterFromFunction() }}</h2>
    <h3>Computed properties: {{ counterFromComputed }}</h3>
  </div>
</template>

<script>
import * as tf from '@tensorflow/tfjs';
import * as _ from 'lodash';

// Define a model for linear regression.
//const model = tf.sequential();
//model.add(tf.layers.dense({units: 1, inputShape: [1]}));

// Prepare the model for training: Specify the loss and the optimizer.
//model.compile({loss: 'meanSquaredError', optimizer: 'sgd'});

  // Train a simple model:
const model = tf.sequential();
model.add(tf.layers.dense({units: 100, activation: 'relu', inputShape: [10]}));
model.add(tf.layers.dense({units: 1, activation: 'linear'}));
model.compile({optimizer: 'sgd', loss: 'meanSquaredError'});

const xs = tf.randomNormal([100, 10]);
const ys = tf.randomNormal([100, 1]);

// Generate some synthetic data for training.
//const x_hack = _.range(50);
//const y_hack = x_hack.map(x => x + 1);
//const xs = tf.tensor2d(x_hack, [x_hack.length, 1]);
//const ys = tf.tensor2d(y_hack, [y_hack.length, 1]);
xs.print();
ys.print();
// Train the model using the data.
//model.fit(xs, ys, {epochs: 10}).then(() => {
  // Use the model to do inference on a data point the model hasn't seen before:
//  model.predict(tf.tensor2d([5], [1, 1])).print();
//});

model.fit(xs, ys, {
    epochs: 100,
    callbacks: {
      onEpochEnd: async (epoch, log) => {
        console.log(`Epoch ${epoch}: loss = ${log.loss}`);
      }
    }
  });

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
