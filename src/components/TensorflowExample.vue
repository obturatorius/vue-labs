<template>
  <div class="container">
    <div class="jumbotron">
      <h1>Tensorflow Example</h1>
      <p>Video Tutorial Content</p>
    </div>

    <div class="train-controls">
      <h2 class="section">Training Data (x,y) pairs</h2>
      <div class="field-label">X</div>
      <div class="field-label">Y</div>
      <div v-for="(item, index) in xValues" :key="index">
        <div>
          <div>
            <input
              type="number"
              class="form-control field-x"
              v-model="xValues[index]"
            />
            <input
              type="number"
              class="form-control field-y"
              v-model="yValues[index]"
            />
          </div>
        </div>
      </div>
    </div>

    <button class="btn btn-success button-add-example" @click="addItem">
      +
    </button>
    <button class="btn btn-info button-train-example" @click="train">
      Train
    </button>

    <div class="predict-controls">
      <h2 class="section">Prediction</h2>
      <input
        type="number"
        class="form-control element"
        v-model="valueToPredict"
        placeholder="Enter an integer number"
      />
      <div class="element">{{ predictedValue }}</div>
      <button class="btn btn-success" @click="predict" :disabled="!trained">
        Predict
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import * as tf from "@tensorflow/tfjs";
import Vue from "vue";
import Component from "vue-class-component";

@Component
export default class TensorflowExample extends Vue {
  xValues = [1, 2, 3, 4];
  yValues = [1, 2, 3, 4];

  trained = false;

  valueToPredict = 0;
  predictedValue = "Click on train";

  model: tf.Sequential;

  constructor() {
    super();
    this.model = tf.sequential();
  }

  addItem() {
    this.xValues.push(0);
    this.yValues.push(0);
  }

  train() {
    // define a model for linear regression
    this.model = tf.sequential();
    this.model.add(tf.layers.dense({ units: 1, inputShape: [1] }));

    // prepare the model for training
    // specifiy loss and optimizer
    this.model.compile({ loss: "meanSquaredError", optimizer: "sgd" });

    const xs = tf.tensor2d(this.xValues, [this.xValues.length, 1]);
    const ys = tf.tensor2d(this.yValues, [this.yValues.length, 1]);

    this.model.fit(xs, ys).then(() => {
      this.trained = true;
      this.predictedValue = "Ready to make predictions!";
    });
  }

  predict() {
    this.predictedValue = (this.model.predict(
      tf.tensor2d([+this.valueToPredict], [1, 1])
    ) as tf.Tensor)
      .dataSync()
      .toString();
  }
}
</script>

<style lang="scss" scoped>
.field-label,
.field-x,
.field-y {
  width: 50%;
  display: inline-block;
}
</style>
