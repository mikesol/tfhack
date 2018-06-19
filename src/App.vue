<template>
  <div id="app">
    <img src="./assets/logo.png">
    <div v-if="tfProgress.epochs !== 0">
      <h1>Epoch: {{ tfProgress.epochs }}, loss {{ tfProgress.loss.toFixed(2) }}</h1>
    </div>
    <div v-if="tfProgress.epochs === 0">
      <h1>Waiting for data...</h1>
    </div>

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

let tfProgress = { epochs: 0, loss: NaN };

export default {
  name: 'app',
  data () {
    return {
      msg: 'Counter is at value:',
      tfProgress: { epochs: 0, loss: NaN },
      toBeCanceled: undefined
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
  mounted() {
    this.toBeCanceled = setInterval(() => {
      model.fit(xs, ys, {
        epochs: 1,
        callbacks: {
          onEpochEnd: async (epoch, log) => {
            this.tfProgress.epochs = this.tfProgress.epochs + 1;
            this.tfProgress.loss = log.loss;
          }
        }
      });
    }, 2000);
  },
  destroyed() {
    if (this.toBeCanceled) {
      clearInterval(this.toBeCanceled);
    }

  }
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
