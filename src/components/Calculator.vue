<template>
  <div class="container calculator">
    <h1 class="deep-purple-text">Good Samaritan Tip Calculator</h1>
    <form @submit.prevent="validation" class="card-panel">
      <div class="input-field">
        <label for="totalBill">Total Bill:</label>
        <cleave
          :options="options"
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
            <input
              type="number"
              name="tipForm"
              id="partySize"
              v-model="partySize"
              v-on:keypress="validNum(event)"
            />
          </div>
        </div>
      </div>

      <p class="red-text center" v-if="feedback">{{ feedback }}</p>

      <div class="row card-panel teal lighten-4 results" v-if="totalAmountCalc > 0">
        <div class="col s6">
          <h4>Total</h4>
          <h6 class="center">
            <span>Tip:</span>
            ${{ tipAmount }}
          </h6>
          <h6 class="center">
            <span>Bill:</span>
            ${{ totalAmount }}
          </h6>
        </div>
        <div class="col s6">
          <div v-if="splitBill">
            <h4>Per Person</h4>
            <h6 class="center">
              <span>Tip:</span>
              ${{ tipPerPerson }}
            </h6>
            <h6 class="center">
              <span>Bill:</span>
              ${{ totalAmountPerPerson }}
            </h6>
          </div>
        </div>
      </div>
      <div class="field center">
        <button class="btn deep-purple">Calculate!</button>
      </div>
    </form>
  </div>
</template>

<script>
import Cleave from 'vue-cleave-component';
export default {
  name: 'Calculator',
  data() {
    return {
      totalBill: null,
      percentTip: null,
      splitBill: false,
      partySize: null,
      tipAmount: null,
      tipAmountCalc: 0,
      totalAmount: null,
      totalAmountCalc: 0,
      tipPerPerson: null,
      totalAmountPerPerson: null,
      feedback: null,
      event: null,
      //Cleave Options
      options: {
        numeral: true,
        numeralDecimalMark: '.',
        numeralPositiveOnly: true,
        delimiter: ','
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
      this.tipAmountCalc = this.totalBill * (this.percentTip / 100);
      this.tipAmount = this.tipAmountCalc.toLocaleString(undefined, {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });

      this.totalAmountCalc = this.totalBill + this.tipAmountCalc;
      this.totalAmount = this.totalAmountCalc.toLocaleString(undefined, {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });
      //Calculate Per Person Totals if SplitBill is true
      if (this.splitBill) {
        this.tipPerPerson = (
          this.tipAmountCalc / this.partySize
        ).toLocaleString(undefined, {
          minimumFractionDigits: 2,
          maximumFractionDigits: 2
        });
        this.totalAmountPerPerson = (
          this.totalAmountCalc / this.partySize
        ).toLocaleString(undefined, {
          minimumFractionDigits: 2,
          maximumFractionDigits: 2
        });
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
    validation() {
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
.calculator form {
  padding: 60px;
  label {
    font-size: 1em;
  }
}
.splitRow {
  height: 60px;
}
</style>
