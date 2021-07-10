<template>
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

<style>
</style>
