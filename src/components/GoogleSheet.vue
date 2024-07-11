<template>
    <div>
        <h1>Abstracts</h1>
        <div>
            <v-row justify="space-between">
                <v-col>
                    <v-btn @click="toggleShowTitleOnly">
                        {{ showTitleOnly ? 'Show Full Abstracts' : 'Show Titles Only' }}
                    </v-btn>
                </v-col>
                <v-col> <v-text-field v-model="searchQuery" label="Search" @input="handleSearch"></v-text-field></v-col>
                <v-col class="pagination" cols="auto">
                    <v-btn @click="prevPage" :disabled="currentPage === 1">Previous</v-btn>
                    <span style="padding:10px">Page {{ currentPage }} of {{ totalPages }}</span>
                    <v-btn @click="nextPage" :disabled="currentPage === totalPages">Next</v-btn>
                </v-col>
            </v-row>
        </div>
        <!-- <div v-for="(row, index) in paginatedRows" :key="index" style="padding:5px">
    
            <v-card :title="`#${row.SubmissionID} ${row.Title}`" :subtitle=row.Authors 
                variant="tonal">
                <v-card-text v-if="!showTitleOnly">
                    <div class="abstract">
                        <p>{{ row.Abstract }}</p>
                    </div>
                </v-card-text>
                <v-card-actions>
                    <v-btn>Read more</v-btn>
                </v-card-actions>
            </v-card>
        </div> -->
        <div v-for="(row, index) in paginatedRowsFiltered" :key="index" style="padding:5px">
            <v-card variant="tonal">
                <v-card-title>
                    <p v-html="highlightText(`#${row.SubmissionID} ${row.Title}`)"></p>
                </v-card-title>
                <v-card-subtitle>
                    <p v-html="highlightText(row.Authors)"></p>
                </v-card-subtitle>
                <v-card-text v-if="!showTitleOnly">
                    <div class="abstract">
                        <p v-html="highlightText(row.Abstract)"></p>
                    </div>
                </v-card-text>
                <v-card-actions>
                    <v-btn>Read more</v-btn>
                </v-card-actions>
            </v-card>
        </div>
    </div>
    <div>
        <v-btn @click="prevPage" :disabled="currentPage === 1">Previous</v-btn>
        <span style="padding:10px">Page {{ currentPage }} of {{ totalPages }}</span>
        <v-btn @click="nextPage" :disabled="currentPage === totalPages">Next</v-btn>
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
            showTitleOnly: false,
            searchQuery: ''
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
        },
        // Filtered rows based on search query
        paginatedRowsFiltered() {
            const start = (this.currentPage - 1) * this.itemsPerPage;
            const end = start + this.itemsPerPage;
            return this.filteredRows.slice(start, end);
        },
        // Apply filtering to all rows
        filteredRows() {
            if (!this.searchQuery.trim()) {
                return this.rows;
            }
            const query = this.searchQuery.toLowerCase();
            if (!isNaN(Number(query))) {
                return this.rows.filter(row => row.SubmissionID == (query));
            }
            return this.rows.filter(row =>
                row.SubmissionID.includes(query) ||
                row.Authors.toLowerCase().includes(query) ||
                row.Title.toLowerCase().includes(query) ||
                row.Abstract.toLowerCase().includes(query)
            );
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
            this.searchQuery = ''; // Clear search query when navigating pages
        },
        prevPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
            }
            this.searchQuery = ''; // Clear search query when navigating pages
        },
        toggleShowTitleOnly() {
            this.showTitleOnly = !this.showTitleOnly;
        },
        handleSearch() {
            // Reset to first page when searching
            this.currentPage = 1;
        },
        highlightText(text) {
            if (!this.searchQuery.trim()) {
                return text;
            }
            const query = this.searchQuery.trim();
            const regex = new RegExp(`(${query})`, 'gi');
            return text.replace(regex, '<b class="highlight">$1</b>');
        }
    }
};
</script>

<style scoped>
.abstract {
    text-align: justify;
    text-justify: inter-word;
}

.highlight {
    background-color: yellow;
    color: red;
}

.v-row {
    padding: 20px;
}
</style>
