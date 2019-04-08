<template>
  <div :style="{width: '800px', margin: '0 auto'}">
    <div class="row mb-1">
      <p class="col-sm-8">Render a dynamic form.</p>
      <p>More detailed in <strong>readme.md</strong> file</p>
    </div>
    <!--Your code goes here!-->
    <el-form :model="dataForm" ref="ruleForm">
      <el-form-item v-for="fieldItem in listField" :key="fieldItem.value" class="form-field"
        :prop="fieldItem.value"
        :rules="{required: fieldItem.required, message: 'This field can not be null', trigger: 'blur'}">
        <template v-if="fieldItem.valueType && fieldItem.valueType.value === 'LONG'">
          <base-input
            @change="handleChangeData($event, fieldItem.value)"
            :data="dataForm[fieldItem.value] || ''"
            :typeInput="'number'"
            :label="fieldItem.label"
            :disabled="fieldItem.disabled"
            :required="fieldItem.required"
          />
        </template>

        <template v-else-if="fieldItem.valueType && fieldItem.valueType.value === 'STRING'">
          <base-input
            @change="handleChangeData($event, fieldItem.value)"
            :data="dataForm[fieldItem.value] || ''"
            :label="fieldItem.label"
            :disabled="fieldItem.disabled"
            :required="fieldItem.required"
          />
        </template>

        <template v-else-if="fieldItem.valueType && fieldItem.valueType.value === 'REFERENCE'">
          <base-select
            @change="handleChangeData($event, fieldItem.value)"
            :selected="dataForm[fieldItem.value] ? dataForm[fieldItem.value].value : ''"
            :data="getSelectData(fieldItem.value)"
            :label="fieldItem.label"
            :disabled="fieldItem.disabled"
            :required="fieldItem.required"
          />
        </template>

        <template v-else-if="fieldItem.valueType && fieldItem.valueType.value === 'DATE'">
          <base-date-picker
            @change="handleChangeData($event, fieldItem.value)"
            :data="dataForm[fieldItem.value]"
            :value-format="'timestamp'"
            :label="fieldItem.label"
            :disabled="fieldItem.disabled"
            :required="fieldItem.required"
          />
        </template>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">Submit</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
/********
 * Render a form with the following requirements:
    - Use data from this end point localhost:8080/structure as the structure of this form. Keep in mind that this structure can be changed dynamically.
    - Use data from this end point localhost:8080/data as initial data.
    - Add validation (for fields those marked 'required' in structure)
 * API explaination:
  -
*******/
import BaseInput from '@/components/Test2Component/input'
import BaseDatePicker from '@/components/Test2Component/datepicker'
import BaseSelect from '@/components/Test2Component/select'

export default {
  name: 'Test3',
  components: {
    BaseInput,
    BaseDatePicker,
    BaseSelect
  },
  data () {
    return {
      listField: [],
      dataForm: {},
      dataModifier: [],
      dataOwner: []
    }
  },
  mounted () {
    this.calAPI('http://localhost:3030/modifier').then(result => {
      if (result && result.items) {
        this.dataModifier = result.items
      }
    })

    this.calAPI('http://localhost:3030/owner').then(result => {
      if (result && result.items) {
        this.dataOwner = result.items
      }
    })

    this.calAPI('http://localhost:3030/data').then(result => {
      if (result) {
        this.dataForm = result
      }
    })

    this.calAPI('http://localhost:3030/structure').then(result => {
      if (result) {
        this.listField = result
      }
    })
  },

  methods: {
    handleChangeData (newValue, keyField) {
      console.log(keyField, newValue)
      if (this.dataForm[keyField] !== newValue) {
        this.dataForm[keyField] = newValue
      }
    },

    calAPI (url) {
      return fetch(url)
        .then((response) => {
          return response.json()
        })
        .then((myJson) => {
          return myJson
        })
        .catch((error) => {
          console.error('Error:', error)
        })
    },

    getSelectData (type) {
      if (type === 'owner') {
        return this.dataOwner
      } else if (type === 'modifier') {
        return this.dataModifier
      }
      return []
    },

    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style>
  * {
    -webkit-font-smoothing: antialiased;
  }
  *, :after, :before {
    box-sizing: border-box;
  }
  .form-field {
    width: 50%;
    display: inline-block;
    margin-bottom: 15px;
    padding-right: 15px;
  }
  .form-wrapper {
    text-align: left;
  }

  .form-field .label {
    display: block;
    margin-bottom: 5px;
  }

  .el-select {
    width: 100%;
  }

  .el-date-editor.el-input {
    width: 100%;
  }
</style>
