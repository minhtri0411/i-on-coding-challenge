<template>
  <div>
    <h4>Render a datatable using test4.json</h4>
    <p>Note: Calculate Average value, Total value </p>
    <line-chart :data="data"></line-chart>

    <table class="table">
      <thead>
        <tr>
          <th>Team</th>
          <th>Data</th>
          <th>Total</th>
          <th>Average</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="line in data" :key="line.index">
          <td>{{line.name}}</td>
          <td>
            <table class="sub-table">
              <tr v-for="(value, key) in line.data" :key="key">
                <td class="text-left">{{key}}</td>
                <td class="text-right">{{value}}</td>
              </tr>
            </table>
          </td>
          <td>{{total(line.data)}}</td>
          <td>{{average(line.data)}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import data from './JsonFiles/test4'

export default {
  name: 'Test4',
  component: {
  },
  data () {
    return {
      data: data
    }
  },
  methods: {
    total (data) {
      let total = 0
      for (let value of Object.values(data)) {
        total += value
      };
      return total
    },
    average (data) {
      let total = 0
      let length = 0
      for (let value of Object.values(data)) {
        total += value
        length++
      };
      return (total / length).toFixed(2)
    }
  }
}
</script>

<style scoped>
.table {
  width: 500px;
  margin: auto;
  border-collapse: collapse;
}
.table,.table th,.table td {
  border: 1px solid black;
}
.sub-table {
  width: 100%;
  padding: 0 10px;
}
.sub-table th,.sub-table td {
  border: 0;
}
.text-left {
  text-align: left;
}
.text-right {
  text-align: right;
}
</style>
