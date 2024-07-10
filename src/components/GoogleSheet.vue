<template>
  <div>
    <h1>Abstract</h1>
    <div v-for="(row, rowIndex) in rows" :key="rowIndex">
      <h2>#{{ row.SubmissionID }} {{ row.Title }}</h2>
      <p><i> {{ row.Authors }}</i></p>
      <div class="abstract">
        <p>{{ row.Abstract }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import Papa from 'papaparse';

export default {
  name: 'GoogleSheet',
  data() {
    return {
      headers: [],
      rows: []
    };
  },
  created() {
    this.loadCSV();
  },
  methods: {
    loadCSV() {
      const url = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vRg3LAc4N2Onh7kKxyBn-SlwpWo5_Jc9awoHpGez6_Fwm8sfvvt11Z85qtlZQt3F3_JBR4e_ED2IUtK/pub?output=csv';
      Papa.parse(url, {
        download: true,
        header: true,
        complete: this.parseData
      });
    },
    parseData(result) {
      this.headers = result.meta.fields;
      this.rows = result.data;
    }
  }
};
</script>

<style scoped>
table {
  width: max-content;
  border-collapse: collapse;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
}

th {
  background-color: #f2f2f2;
  text-align: left;
}

.abstract {
  text-align: justify;
  text-justify: inter-word;
}
</style>
