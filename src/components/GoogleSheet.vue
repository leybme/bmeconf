<template>
  <div>
    <h1>Abstract</h1>
    <div class="toolbar">
      <button @click="toggleShowTitleOnly">
        {{ showTitleOnly ? 'Show Full Abstracts' : 'Show Titles Only' }}
      </button>
      <div class="pagination">
        <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
        <span>Page {{ currentPage }} of {{ totalPages }}</span>
        <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
      </div>
    </div>
    <div v-for="(row, index) in paginatedRows" :key="index" class="title_abstract">
      <h2>#{{ row.SubmissionID }} {{ row.Title }}</h2>
      <p><i>{{ row.Authors }}</i></p>
      <div class="abstract" v-if="!showTitleOnly">
        <p>{{ row.Abstract }}</p>
      </div>
    </div>
  </div>
  <div class="pagination">
    <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
    <span>Page {{ currentPage }} of {{ totalPages }}</span>
    <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
  </div>
</template>

<script>
import Papa from 'papaparse';

export default {
  name: 'GoogleSheet',
  data() {
    return {
      headers: [],
      rows: [],
      currentPage: 1,
      itemsPerPage: 10,
      showTitleOnly: false
    };
  },
  computed: {
    paginatedRows() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.rows.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.rows.length / this.itemsPerPage);
    }
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
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    toggleShowTitleOnly() {
      this.showTitleOnly = !this.showTitleOnly;
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
.title_abstract{
  margin-bottom: 20px;
}
</style>
