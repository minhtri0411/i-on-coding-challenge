<template>
  <div :class="classInput">
    <span v-if="label" class="label">{{label}}</span>
    <el-date-picker
      v-model="inputValue"
      :type="type"
      :placeholder="placeholder"
      :format="format"
      :value-format="valueFormat"
      @change="$emit('change', inputValue)"
      :disabled="disabled"
      >
    </el-date-picker>
  </div>
</template>
<script>
export default {
  name: 'BaseDatePicker',
  data () {
    return {
      value: ''
    }
  },
  props: {
    data: {
      type: [String, Number],
      default: ''
    },
    type: {
      type: String,
      default: 'date'
    },
    format: {
      type: String,
      default: 'yyyy-MM-dd'
    },
    valueFormat: {
      type: String,
      default: 'timestamp'
    },
    placeholder: {
      type: String,
      default: 'Pick a day'
    },
    classInput: {
      type: String,
      default: ''
    },
    disabled: {
      type: Boolean,
      default: false
    },
    label: {
      type: String,
      default: ''
    }
  },

  created () {
    this.value = this.data
  },

  watch: {
    data: function (val) {
      this.value = val
    }
  },

  computed: {
    inputValue: {
      get: function () {
        return this.value
      },
      set: function (newValue) {
        this.value = newValue
      }
    }
  }
}
</script>
