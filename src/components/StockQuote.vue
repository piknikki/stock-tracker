<template>
  <div>
    <h1 id="company-name">{{ stock.companyName }}</h1>
    <div id="stock-card" class="quote" v-if="stock.symbol">
      <div class="flex-row">
        <span class="left header">{{ stock.latestPrice }}</span>
        <span class="right header" :style="style">{{stock.change}} ({{stock.changePercent}}%)</span>
      </div>
      <div class="flex-row">
        <span class="left">Range</span>
        <span class="right">{{stock.low}} - {{stock.high}}</span>
      </div>
      <div class="flex-row">
        <span class="left">Open</span>
        <span class="right">{{stock.open}}</span>
      </div>
      <div class="flex-row">
        <span class="left">Volume</span>
        <span v-bind:formatNumber="formatNumber" class="right">{{stock.latestVolume}}</span>
      </div>
      <div class="flex-row">
        <span class="left">Market Cap</span>
        <span class="right">{{stock.marketCap}}</span>
      </div>
      <div class="flex-row">

        <span class="right">As of {{stock.latestTime}}</span>
      </div>
    </div>
  </div>
</template>

<script>
var numeral = require('numeral')

export default {
  props: ['stock', 'color'],
  methods: {
    formatNumber (value) {
      return numeral(value).format('0.0a')
    }
  },
  computed: {
    style () {
      if (this.stock.change > 0) {
        return 'color: red'
      } else {
        return 'color: green'
      }
    }
  }
}
</script>

<style scoped>
#company-name {
  width: 40%;
  text-align: left;
}

#company-name, .header {
  text-transform: uppercase;
  display: flex;
  justify-items: left;
  flex-wrap: wrap;
  font-family: 'Lora', serif;
}

#stock-card {
  display: flex;
  flex-direction: column;
}

.flex-row {
  display: flex;
  flex-direction: row;
  border-bottom: 1px solid gray;
  padding: 20px;
  font-size: 1em;
}

.right {
  margin-left: auto;
  font-weight: bold;
}

.header {
  font-size: 2em;
  font-weight: bold;
}

</style>
