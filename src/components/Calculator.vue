<template>
  <div class="container calculator">
    <h1 class="deep-purple-text text-lighten-2 center">Good Samaritan Tip Calculator</h1>
    <form @submit.prevent="formValidation" class="card-panel">
      <p
        class="center"
      >Complete the form below to obtain the appropriate tip amount and determine if you're a Bronze, Silver, Platinum or Gold tipper!</p>
      <div class="input-field">
        <label for="totalBill">Total Bill:</label>
        <cleave
          :options="optionsBill"
          type="text"
          name="tipForm"
          id="totalBill"
          v-model="totalBill"
        ></cleave>
      </div>
      <div class="input">
        <label class="left">Tip percentage:</label>
        <select class="browser-default" value="Tip Percentage" name="tipForm" v-model="percentTip">
          <option value disabled selected>Tip percentage:</option>
          <option value="10">10% - Bronze: Peasant</option>
          <option value="15">15% - Silver: Average Chap</option>
          <option value="18">18% - Platinum: Philanthropist</option>
          <option value="20">20% - Gold: Bill Gates</option>
        </select>
      </div>
      <div class="row splitRow">
        <div class="col s6">
          <div class="input-field">
            <label>
              <input type="checkbox" v-model="splitBill" v-on:click="clearPartyResults" />
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
            ></cleave>
          </div>
        </div>
      </div>

      <p class="red-text center" v-if="feedback">{{ feedback }}</p>

      <div class="row card-panel white-text results" v-if="totalAmount > 0">
        <div class="col s12 m6 l4 resText">
          <h5>Total</h5>
          <h6 class>
            <span>Tip:</span>
            {{ tipAmount | currency }}
          </h6>
          <h6 class>
            <span>Bill:</span>
            {{ totalAmount | currency }}
          </h6>
        </div>
        <div class="col s12 m6 l4 resText">
          <div v-if="splitBill">
            <h5>Per Person</h5>
            <h6 class>
              <span>Tip:</span>
              {{ tipPerPerson | currency }}
            </h6>
            <h6 class>
              <span>Bill:</span>
              {{ totalAmountPerPerson | currency }}
            </h6>
          </div>
        </div>
        <div class="col s12 m0 l4 right">
          <img class="responsive-img" :src="whichMedal()" alt="A shiny star award!" />
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
      rank: null,
      field: null,
      //Cleave Options
      optionsBill: {
        numeral: true,
        numeralDecimalMark: '.',
        numeralPositiveOnly: true,
        delimiter: ','
      },
      optionsParty: {
        numeral: true,
        numeralDecimalMark: '',
        delimiter: ''
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
    formValidation() {
      //Ensure Total Bill and percentTip Percentage fields are completed
      if (this.totalBill && this.percentTip) {
        if (this.totalBill <= 0 || this.totalBill == '.') {
          this.feedback =
            'Please enter an amount above 0 in the Total Bill field!';
        }
        //Ensure Total Bill does not exceed 999,999,999
        else if (this.totalBill > 999999999) {
          this.feedback =
            'Please enter an amount below 1,000,000,000 in the Total Bill field!';
        }
        //Remind User not to enter less than 1 or more than 1000 people per party!
        else if (
          this.partySize !== null &&
          (this.partySize > 1000 || this.partySize < 1)
        ) {
          if (this.partySize.indexOf('.') !== -1) {
            this.feedback = 'You cannot have a partial person, you barbarian!';
          } else {
            this.feedback = 'Please enter a Party Size between 2 and 1000!';
          }
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
    },
    clearPartyResults() {
      if (this.splitBill === true) {
        this.splitBill = false;
        this.partySize = null;
        this.tipPerPerson = null;
        this.totalAmountPerPerson = null;
      } else if (this.splitBill === false) {
        this.splitBill = true;
      }
    },
    whichMedal() {
      if (this.percentTip === 10) {
        this.rank = 'bronze';
      } else if (this.percentTip === 15) {
        this.rank = 'silver';
      } else if (this.percentTip === 18) {
        this.rank = 'platinum';
      } else if (this.percentTip === 20) {
        this.rank = 'gold';
      }
      return require('../assets/' + this.rank + '_medal.png');
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
  padding: 5% 10%;
  label {
    font-size: 1em;
  }
  .splitRow {
    height: 65px;
  }
  .results {
    background: #191919;
    padding: 0px;
    .resText {
      padding: 15px 10px 15px 25px;
    }
    img {
      margin-bottom: -5px;
      //height: 100%;
    }
  }
}

@media only screen and (max-width: 1160px) {
  .calculator .results img {
    display: none;
  }
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
