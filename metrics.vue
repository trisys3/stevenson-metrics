<template lang="pug">
- const stores = ['bestBuy', 'homeDepot', 'lowes']
- const months = ['january', 'february', 'march']

.summary
  .summary-title Store Summary
  .summary-btns
    .btn.summary-btn(v-bind='{ class: resultType === \'count\' && \'active\' || \'\' }', @click='resultType = \'count\'') COUNT
    .btn.summary-btn(v-bind='{ class: resultType === \'percent\' && \'active\' || \'\' }', @click='resultType = \'percent\'') PERCENT

.results
  header.result
    .result-title Brand

    each store in stores
      .result-store
        .result-store-title= `{{ PascalSpaceCase('${store}') }}`

        .result-months
          each month in months
            .result-month= `{{ upperAbbreviateMonth('${month}') }}`

    .result-total Grand Total

  .result(v-for='result in results')
    .result-title {{ result.brand }}
</template>

<script>
const metricsUrl = 'http://planogram.stevensoncompany.com/api/metrics/store';

import axios from 'axios';

export default {
  async beforeCreate() {
    let results;
    try {
      ({data: {data: results}} = await axios(metricsUrl));
    } catch {}

    this.results = parseMetrics(results);
  },

  methods: {
    PascalSpaceCase,
    upperAbbreviateMonth,
  },

  data: () => ({results: [], resultType: 'count'}),
};

function parseMetrics(resp) {
  const results = resp;

  return results;
}

function PascalSpaceCase(str) {
  const camelSpaceCased = str?.replaceAll?.(/[A-Z]/g, ' $&');

  return capitalize(camelSpaceCased);
}

function upperAbbreviateMonth(month) {
  const abbreviated = month?.substr?.(0, 3);

  return capitalize(abbreviated);
}

function capitalize(str) {
  return `${str?.charAt?.(0).toLocaleUpperCase()}${str?.slice?.(1)}`;
}
</script>

<style lang="stylus">
.metrics
  padding-inline 5px
  padding-top 20px
</style>

<style lang="stylus" scoped>
.btn
  background-color white
  color #4386ab
  cursor pointer
  padding-inline 12px
  height 34px
  display flex
  justify-content center
  align-items center

  &.active
    background-color #4386ab
    color white

.summary
  margin-bottom 20px
  display flex

.summary-title
  font-weight bold
  margin-right 40px
  display flex
  align-items center

.summary-btns
  display flex

.summary-btn
  width 100px

  &:first-child
   border-top-left-radius 5px
   border-bottom-left-radius 5px

  &:last-child
   border-top-right-radius 5px
   border-bottom-right-radius 5px

.results
  & header
    color #7a8f9f
    font-weight bold
    margin-bottom 10px
    height 100px

.result
  background-color white
  margin-bottom 5px
  border-radius 5px
  height 50px
  display flex
  align-items center

  &:last-child
    margin-bottom 0

.result-data
  display flex
  align-items center
  justify-content center

.result-title
  padding-left 10px
  width 200px
  display flex
  align-items center

.result-store
  flex-grow 1
  align-self stretch
  border-left 1px solid currentColor
  display flex
  flex-direction column

.result-store-title
  flex-grow 1
  border-bottom 1px solid currentColor
  display flex
  justify-content center
  align-items center

.result-months
  flex-grow 1
  display flex

.result-month
  flex-grow 1
  display flex
  justify-content center
  align-items center

.result-total
  text-align center
  align-self stretch
  border-left 1px solid currentColor
  width 150px
  display flex
  justify-content center
  align-items center
</style>
