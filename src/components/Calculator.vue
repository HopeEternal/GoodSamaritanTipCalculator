
<template>
  <div class="container calculator">
    <h1 class="deep-purple-text">Good Samaritan Tip Calculator</h1>
    <form @submit.prevent="calculate" class="card-panel">
      <div class="input-field">
        <label for="totalBill">Total Bill:</label>
        <input type="number" name="tipForm" id="totalBill" v-model="totalBill" />
      </div>
      <div class="input">
        <label class="tipPercLabel left">Tip percentage:</label>
        <select class="browser-default" value="Tip Percentage" name="tipForm" v-model="percentTip">
          <option value disabled selected>Tip percentage:</option>
          <option value="10">10%</option>
          <option value="15">15%</option>
          <option value="18">18%</option>
          <option value="20">20%</option>
        </select>
      </div>
      <div class="input-field">
        <label for="partySize">Party Size:</label>
        <input type="number" name="tipForm" id="partySize" v-model="partySize" />
      </div>

      <!--  <p class="red-text center" v-if="feedback">{{ feedback }}</p>-->
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
      partySize: null
    };
  },
  methods: {
    calculate() {
      console.log('Button works');
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
