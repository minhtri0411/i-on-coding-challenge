<template>
  <div class="table-wrap coltype" :style="{width: '50%', margin: '0 auto'}">
    <h4>Render a datatable using test1.json</h4>
    <p>Note: the amount of Column can be changed dynamically. </p>
    <div class="form-group">
      <label>Type</label>
      <input type="text" class="" v-model="typeNew">
      <label>Q&A</label>
      <input type="name" class="" v-model="qaNew">
      <button type="submit" class="btn btn-primary" @click="addRows">Add</button>
    </div>
    <br/>
    <div class="form-group">
      <input type="text" class="" style="width:500px" v-model="search" v-on:keyup.enter="searchRows">
      <button type="submit" class="btn btn-primary" @click="searchRows">Search</button>
    </div>
    <table class="table ui-accordion">
      <caption></caption>
      <colgroup>
        <col style="width:10px">
        <col style="width:50px;">
        <col style="width:30px">
      </colgroup>
      <thead>
      <tr>
        <th scope="col" v-for="column in data.columns" :key="column.field">{{ column.label }}</th>
      </tr>
      </thead>
      <tbody>
        <tr class="head" v-for="row in rows" :key="row.order">
          <td>{{row.order}}</td>
          <td>{{row.type}}</td>
          <td>{{row.question}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import jsonData from './JsonFiles/test1'

export default {
  data () {
    return {
      data: {
        columns: [],
        rows: [],
        rowsFiltered: []
      },
      search: '',
      typeNew: '',
      qaNew: ''
    }
  },
  mounted () {
    this.data.columns = [ ...jsonData.columns ]
    this.data.rows = [ ...jsonData.rows ]
    this.data.rowsFiltered = [ ...jsonData.rows ]
  },

  computed: {
    rows () {
      return this.data.rowsFiltered || []
    }
  },

  methods: {
    searchRows: function () {
      const that = this
      function conditionFilter (itemSearch, textSearch) {
        for (let value of Object.values(itemSearch)) {
          if (value.toString().toLowerCase().indexOf(textSearch.toLowerCase()) >= 0) {
            return true
          }
        };

        return false
      }

      if (that.search.length) {
        this.data.rowsFiltered = that.data.rows.filter(function (item) {
          if (conditionFilter(item, that.search)) {
            return item
          }
        })
      } else {
        this.data.rowsFiltered = this.data.rows
      }
    },

    addRows: function () {
      if (this.typeNew.length || this.qaNew.length) {
        this.data.rows.push({
          order: this.data.rows.length + 1,
          type: this.typeNew,
          question: this.qaNew
        })

        this.typeNew = ''
        this.qaNew = ''
        this.searchRows()
      }
    }
  }
}
</script>

<style scoped>
  .table-wrap {
    border-top: 1px solid #767676;
    border-bottom: 1px solid #767676;
  }
  table.ui-accordion {
    border-top: none;
  }
  .ui-accordion {
    border-top: 1px solid #bebebe;
  }
  table {
    width: 100%;
    table-layout: fixed;
    border-spacing: 0;
    border-collapse: collapse;
    background: #fff;
  }
  table caption {
    display: none;
  }
  caption {
    display: table-caption;
    text-align: -webkit-center;
  }
  colgroup {
    display: table-column-group;
  }
  thead {
    display: table-header-group;
    vertical-align: middle;
    border-color: inherit;
  }
  tr {
    display: table-row;
    vertical-align: inherit;
    border-color: inherit;
  }
  .table-wrap.coltype table thead th {
    padding: 12px 5px 10px;
    line-height: 22px;
    color: #444;
    text-align: center;
    vertical-align: middle;
    background: #f6f6f6;
  }
  th {
    font-weight: bold;
  }
  td, th {
    display: table-cell;
  }
  tbody {
    display: table-row-group;
    vertical-align: middle;
    border-color: inherit;
  }
  .table-wrap.coltype table tbody tr:first-child th, .table-wrap.coltype table tbody tr:first-child td {
    border-top: 1px solid #eee;
  }
  .table-wrap.coltype table tbody td {
    padding: 16px 5px 14px;
    line-height: 22px;
    color: #666;
    text-align: center;
    vertical-align: middle;
  }
  a, a:visited, a:focus {
    text-decoration: none;
  }
  a {
    color: #666;
  }
  table.ui-accordion .body {
    display: none;
  }
  .table-wrap.coltype table tbody td {
    padding: 16px 5px 14px;
    line-height: 22px;
    color: #666;
    text-align: center;
    vertical-align: middle;
  }
  .table-wrap table tbody td {
    padding: 12px;
    vertical-align: middle;
    text-align: left;
    color: #666;
    border-top: 1px solid #e0e0e0;
  }
  .table-wrap.coltype table td .attached {
    padding: 15px 0;
    text-align: right;
    color: #46675c;
  }
  strong, .strong {
    font-weight: 600;
  }
  .table-wrap.coltype table td .attached a {
    display: inline-block;
    color: #46675c;
  }

  .md-form input[type=text] {
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
    outline: none;
    box-shadow: none;
    border: none;
    border-bottom: 1px solid #ced4da;
    border-radius: 0;
    box-sizing: content-box;
    background-color: transparent;
  }
  .md-form .form-control {
    margin: 0 0 0.5rem 0;
    border-radius: 0;
    padding: 0.3rem 0 0.55rem 0;
    background-image: none;
    background-color: transparent;
    height: auto;
  }
  .dataTables_wrapper input {
    display: inline-flex !important;
  }
  .form-control {
    display: block;
    width: 100%;
    height: calc(2.25rem + 2px);
    padding: .375rem .75rem;
    font-size: 1rem;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: .25rem;
    transition: border-color .15s ease-in-out,box-shadow .15s ease-in-out;
  }
  button, input {
    overflow: visible;
  }
  button, input, optgroup, select, textarea {
    margin: 0;
    font-family: inherit;
    font-size: inherit;
    line-height: inherit;
  }
</style>
