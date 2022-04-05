<template>
  <div class="credit">
    <div id="creditApp">
      <h1>Credit Calculator</h1>
      <div id="functional">
        <input
          type="number"
          min="1"
          max="4000"
          placeholder="Money amount $"
          id="moneyAmount"
          v-model="moneyAmount"
        />
        <input
          type="number"
          min="0"
          max="100"
          id="percent_input"
          placeholder="Percent"
          v-model="percent"
        />
        <select name id="selection" v-model="period">
          <option value="undefined" disabled>Select Period</option>
          <option value="1">1 Month</option>
          <option value="2">2 Months</option>
          <option value="3">3 Months</option>
          <option value="4">4 Months</option>
          <option value="5">5 Months</option>
          <option value="6">6 Months</option>
          <option value="7">7 Months</option>
          <option value="8">8 Months</option>
          <option value="9">9 Months</option>
          <option value="10">10 Months</option>
          <option value="11">11 Months</option>
          <option value="12">12 Months</option>
        </select>
        <button id="calculate" @click="printCreditTable()">Calculate</button>
      </div>
      <div id="printTable">
        <div id="attributes">
          <div id="id">Month</div>
          <div id="period">Period</div>
          <div id="percent">Percent per month</div>
          <div id="rest">Amount to pay</div>
          <div id="toPay">Rest</div>
        </div>
        <div id="dataBaseData">
          <div id="id_dataBase"></div>
          <div id="period_dataBase"></div>
          <div id="percent_dataBase"></div>
          <div id="rest_dataBase"></div>
          <div id="toPay_dataBase"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dataBase: [],
      period: undefined,
      moneyAmount: "",
      percent: "",
      rest: 0,
      toPay: 0,
      id: 0,
      moneyAmountTemp: this.moneyAmount,
    };
  },
  computed: {

  },
  methods: {
    getRest() {
      if (this.moneyAmountTemp % this.period == 0) {
        return this.moneyAmountTemp / this.period;
      } else {
        return parseInt(this.moneyAmountTemp / this.period);
      }
    },
    absLocal(number) {
      return number <= 0 ? -number : number;
    },
    percentCalculation() {
      return this.moneyAmountTemp = (this.moneyAmount + (this.moneyAmount * (this.percent * 0.01)) * this.period).toFixed(2);
    },
    printCreditTable() {
      this.percentCalculation();
      let id_dataBase = document.getElementById("id_dataBase");
      let period_dataBase = document.getElementById("period_dataBase");
      let percent_dataBase = document.getElementById("percent_dataBase");
      let rest_dataBase = document.getElementById("rest_dataBase");
      let toPay_dataBase = document.getElementById("toPay_dataBase");
      this.dataBase = [];
      id_dataBase.innerHTML = "";
      period_dataBase.innerHTML = "";
      percent_dataBase.innerHTML = "";
      rest_dataBase.innerHTML = "";
      toPay_dataBase.innerHTML = "";
      this.id = 0;
      this.toPay = this.moneyAmountTemp;
      let periodTemp = this.period;
      periodTemp++;
      for (let i = 0; i < this.period; i++) {
        this.id++;
        periodTemp--;
        this.dataBase.push({
          id: this.id,
          period: this.period,
          percent: this.percent,
          rest: this.getRest(),
          toPay: this.toPay,
        });
        console.log(this.dataBase);
        id_dataBase.append(this.dataBase[i].id, document.createElement("br"));
        period_dataBase.append(periodTemp, document.createElement("br"));
        percent_dataBase.append(
          this.dataBase[i].percent, document.createElement("br"),
        );
        if (i == this.period - 1) {
          rest_dataBase.append(
            (this.dataBase[i].rest = 
              this.getRest() +
              (this.moneyAmountTemp / this.period - this.getRest()) * this.period
            ).toFixed(2),
            document.createElement("br")
          );
        } else {
          rest_dataBase.append(
            (this.dataBase[i].rest = this.getRest().toFixed(2)),
            document.createElement("br")
          );
        }
        toPay_dataBase.append(
          this.absLocal((this.toPay -= this.dataBase[i].rest).toFixed(2)),
          document.createElement("br")
        );
      }
    },
  },
};
</script>

<style>
@media (width: 1024px) {
  .credit {
    display: flex;
    align-items: center;
  }
}
#creditApp > h1 {
  text-align: center;
  padding: 14px;
  margin: 6vh 0vh 6vh 0vh ;
}
#functional {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}
#functional > input,
#functional > button,
#functional > select {
  padding: 6px;
  background-color: darkseagreen;
}
#printTable {
  display: grid;
  background: rgb(103, 109, 117);
}
#attributes,
#dataBaseData {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  text-decoration-color: black;
}
#id,
#period,
#percent,
#rest,
#toPay,
#id_dataBase,
#period_dataBase,
#percent_dataBase,
#rest_dataBase,
#toPay_dataBase {
  border-right: 1px solid #000;
  border-bottom: 1px solid #000;
  padding: 5px;
  text-align: center;
  text-decoration-color: #000;
}
#id_dataBase,
#period_dataBase,
#percent_dataBase,
#rest_dataBase,
#toPay_dataBase {
  border-bottom: 1px solid #000;
  background-color: darkseagreen;
  text-decoration-color: #000;
}
</style>
