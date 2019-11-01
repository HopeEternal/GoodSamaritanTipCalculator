<template>
  <div class="container calculator">
    <h1 class="deep-purple-text center">Good Samaritan Tip Calculator</h1>
    <form @submit.prevent="formValidation" class="card-panel">
      <div class="input-field">
        <label for="totalBill">Total Bill:</label>
        <cleave
          :options="optionsBill"
          type="text"
          name="tipForm"
          id="totalBill"
          v-model="totalBill"
          v-on:keypress="validNum(event)"
        ></cleave>
      </div>
      <div class="input">
        <label class="left">Tip percentage:</label>
        <select class="browser-default" value="Tip Percentage" name="tipForm" v-model="percentTip">
          <option value disabled selected>Tip percentage:</option>
          <option value="10">10% - Peasant</option>
          <option value="15">15% - Average Chap</option>
          <option value="18">18% - Philanthropist</option>
          <option value="20">20% - Bill Gates</option>
        </select>
      </div>
      <div class="row splitRow">
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

            <cleave
              :options="optionsParty"
              type="text"
              name="tipForm"
              id="partySize"
              v-model="partySize"
              v-on:keypress="validNum(event)"
            ></cleave>
          </div>
        </div>
      </div>

      <p class="red-text center" v-if="feedback">{{ feedback }}</p>

      <div class="row card-panel teal lighten-4 results" v-if="totalAmount > 0">
        <div class="col s6">
          <h4>Total</h4>
          <h6 class="center">
            <span>Tip:</span>
            {{ tipAmount | currency }}
          </h6>
          <h6 class="center">
            <span>Bill:</span>
            {{ totalAmount | currency }}
          </h6>
        </div>
        <div class="col s6">
          <div v-if="splitBill">
            <h4>Per Person</h4>
            <h6 class="center">
              <span>Tip:</span>
              {{ tipPerPerson | currency }}
            </h6>
            <h6 class="center">
              <span>Bill:</span>
              {{ totalAmountPerPerson | currency }}
            </h6>
          </div>
        </div>
      </div>
      <div class="field center">
        <button class="btn deep-purple">Calculate!</button>
      </div>
    </form>
    <footer class="center">
      <a href="https://www.vecteezy.com/free-vector">Vectors by Vecteezy</a>
    </footer>
  </div>
</template>

<script>
import Cleave from 'vue-cleave-component';
import Vue from 'vue';
import Vue2Filters from 'vue2-filters';

Vue.use(Vue2Filters);

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
      tipPerPerson: null,
      totalAmountPerPerson: null,
      feedback: null,
      event: null,
      //Cleave Options
      optionsBill: {
        numeral: true,
        numeralDecimalMark: '.',
        numeralPositiveOnly: true,
        delimiter: ','
      },
      optionsParty: {
        blocks: [3]
      }
    };
  },
  methods: {
    calculate() {
      //Clear Feedback Field if Already Present
      this.feedback = null;
      this.totalBill = parseFloat(this.totalBill);
      this.percentTip = parseFloat(this.percentTip);
      //Calculate Table Totals
      this.tipAmount = this.totalBill * (this.percentTip / 100);

      this.totalAmount = this.totalBill + this.tipAmount;

      //Calculate Per Person Totals if SplitBill is true
      if (this.splitBill) {
        this.tipPerPerson = this.tipAmount / this.partySize;
        this.totalAmountPerPerson = this.totalAmount / this.partySize;
      }
    },
    validNum(event) {
      //Only allow numbers in the total bill and party size fields
      event = event ? event : window.event;
      var charCode = event.which ? event.which : event.keyCode;
      if (charCode > 31 && (charCode < 48 || charCode > 57)) {
        event.preventDefault();
      } else {
        return true;
      }
    },
    formValidation() {
      //Ensure Total Bill and Tip Percentage fields are completed
      if (this.totalBill && this.percentTip) {
        if (this.totalBill < 1) {
          this.feedback =
            'Please enter an amount above 0 in the Total Bill field!';
        }
        //0 or 1 entered in the Party Size will uncheck split bill
        else if (this.partySize < 2) {
          this.splitBill = false;
          this.partySize = null;
          this.calculate();
        } else {
          this.calculate();
        }
      } else {
        this.feedback = 'Please complete all form fields!';
      }
    }
  },
  components: {
    Cleave
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
html {
  background-image: url('../assets/sparkly_background.jpg');
}
.calculator form {
  padding: 60px;
  label {
    font-size: 1em;
  }
}
.splitRow {
  height: 60px;
}

@media only screen and (max-width: 600px) {
  .calculator h1 {
    font-size: 2rem;
  }
}
@media only screen and (max-width: 400px) {
  .calculator h1 {
    font-size: 1.5rem;
  }
}
</style>
