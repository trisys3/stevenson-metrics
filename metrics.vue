<template lang="pug">
.summary
  .summary-title Store Summary
  .summary-btns
    .btn.summary-btn(class={active: this.summaryType === 'count'}) COUNT
    .btn.summary-btn(class={active: this.summaryType !== 'count'}) PERCENT
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

  data: () => ({results: []}),
};

function parseMetrics(resp) {
  return resp;
}
</script>

<style lang="scss">
.metrics {
  padding-inline: 5px;
  padding-top: 20px;
}
</style>

<style lang="scss" scoped>
.btn {
  background-color: white;
  color: #4386ab;
  cursor: pointer;
  padding-inline: 12px;
  height: 34px;
  display: flex;
  justify-content: center;
  align-items: center;

  &.active {
    background-color: #4386ab;
    color: white;
  }
}

.summary {
  margin-bottom: 20px;
  display: flex;
}

.summary-title {
  font-weight: bold;
  margin-right: 40px;
  display: flex;
  align-items: center;
}

.summary-btns {
  display: flex;
}

.summary-btn {
  width: 100px;

  &:first-child {
   border-top-left-radius: 5px;
   border-bottom-left-radius: 5px;
  }

  &:last-child {
   border-top-right-radius: 5px;
   border-bottom-right-radius: 5px;
  }
}
</style>
