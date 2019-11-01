
<template>
  <div class="container calculator">
    <h1 class="deep-purple-text">Good Samaritan Tip Calculator</h1>
    <form @submit.prevent="calculate" class="card-panel">
      <div class="input-field">
        <label for="totalBill">Total Bill:</label>
        <input type="number" name="tipForm" id="totalBill" v-model="totalBill" />
      </div>
      <div class="input">
        <label class="left">Tip percentage:</label>
        <select class="browser-default" value="Tip Percentage" name="tipForm" v-model="percentTip">
          <option value disabled selected>Tip percentage:</option>
          <option value="10">10%</option>
          <option value="15">15%</option>
          <option value="18">18%</option>
          <option value="20">20%</option>
        </select>
      </div>
      <div class="row">
        <div class="col s6">
          <div class="input-field">
            <label>
              <input type="checkbox" v-model="splitBill" />
              <span>Split Bill?</span>
            </label>
          </div>
        </div>
        <div class="col s6">
          <div class="input-field" v-if="splitBill">
            <label for="partySize">Party Size:</label>
            <input type="number" name="tipForm" id="partySize" v-model="partySize" />
          </div>
        </div>
      </div>

      <p class="red-text center" v-if="feedback">{{ feedback }}</p>
      <h3 class="purple-text center" v-if="tipAmount">
        <span>Tip:</span>
        {{ tipAmount }}
      </h3>
      <h3 class="purple-text center" v-if="totalAmount">
        <span>Total Bill:</span>
        {{ totalAmount }}
      </h3>
      <div class="field center">
        <button class="btn deep-purple">Calculate!</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      totalBill: null,
      percentTip: null,
      splitBill: false,
      partySize: null,
      tipAmount: null,
      totalAmount: null,
      feedback: null
    };
  },
  methods: {
    calculate() {
      if (this.totalBill && this.percentTip && this.partySize) {
        //Calculate Tip... tip is totalBill / percentTip
        this.tipAmount = this.totalBill / this.percentTip;
        this.totalAmount = this.totalBill + this.tipAmount;
      } else {
        this.feedback = 'Please complete all form fields!';
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.calculator form {
  padding: 60px;
  label {
    font-size: 1em;
  }
}
</style>
