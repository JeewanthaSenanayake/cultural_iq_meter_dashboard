<template>
  <v-container>
    <div class="text-center">
      <h1>Cultural IQ Meter</h1>
    </div>

    <div class="text-center mt-8">
      <v-data-table :headers="headers" :items="tableData" v-if="tableData">
  <!-- eslint-disable-next-line  -->
        <template v-slot:item.lastPlayed="{ item }"> 
          <b><p :class="item.lastPlayed=='Easy'?'green--text':item.lastPlayed=='Medium'?'orange--text':'red--text'" >
            {{ item.lastPlayed }}
          </p></b>
        </template>
      </v-data-table>
      <v-progress-circular class="mt-8" v-else :size="70" :width="7" color="grey darken-3" indeterminate></v-progress-circular>
    </div>


  </v-container>
</template>

<script>
import axios from 'axios'
export default {
  name: 'DashBoard',

  data: () => ({
    // tableData: [],
    tableData: false,
    headers: [
      { text: 'Rank', value: 'rank' },
      { text: 'Name', value: 'name' },
      { text: 'Last Played level', value: 'lastPlayed' },
      { text: 'Total Score', value: 'score' },
    ],
  }),
  methods: {
    async getUsers() {
      console.log(new Date())
      await axios.get('https://cultural-iq-meter.onrender.com/iq_meter/api/v1/dashboard/get_ranks')
        .then(response => {
          this.tableData = response.data.ranks
          // console.log(response.data)
        })
        .catch(error => {
          console.log(error)
        })
    },
    caller() {
      setInterval(this.getUsers, 7500);
    }
  },
  created() {
    this.getUsers()
    this.caller()
  },
}
</script>
