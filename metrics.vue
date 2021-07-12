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

    .result-total.result-header-total Grand Total

  .result(v-for='result in results')
    .result-title {{ result.brand }}

    .result-count(v-for='count in result[`${resultType}s`]') {{ `${count}${resultType === 'percent' && '%' || ''}` }}

    .result-total {{ resultType === 'percent' ? `${result.total_percent}%` : result.total }}
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

function parseMetrics(results) {
  for(const result of results) {
    Object.assign(result, {
      counts: [
        result.best_buy_202103,
        result.best_buy_202104,
        result.best_buy_202105,
        result.home_depot_202103,
        result.home_depot_202104,
        result.home_depot_202105,
        result.lowes_202103,
        result.lowes_202104,
        result.lowes_202105,
      ],

      percents: [
        result.best_buy_202103_percent,
        result.best_buy_202104_percent,
        result.best_buy_202105_percent,
        result.home_depot_202103_percent,
        result.home_depot_202104_percent,
        result.home_depot_202105_percent,
        result.lowes_202103_percent,
        result.lowes_202104_percent,
        result.lowes_202105_percent,
      ],
    });
  }

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
  padding-inline 8px
  padding-top 20px
  padding-bottom 5px
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

.result-count
  flex-grow 1
  width: 0
  display flex
  align-items center
  justify-content center

.result-title
  padding-left 15px
  width 200px
  display flex
  align-items center

.result-store
  flex-grow 1
  align-self stretch
  border-left 1px solid #e6eef1
  display flex
  flex-direction column

.result-store-title
  flex-grow 1
  border-bottom 1px solid #e6eef1
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
  margin-right 15px
  width 150px
  display flex
  justify-content flex-end
  align-items center

.result-header-total
  margin-left 0
  border-left 1px solid #e6eef1
  justify-content center
</style>
