<template>
  <div :class="classInput">
    <span v-if="label" class="label">{{label}}</span>
    <el-input
      :placeholder="placeholder"
      :type="typeInput"
      :disabled="disabled"
      v-model="inputValue"
      @change="$emit('change', inputValue)">
    </el-input>
  </div>
</template>
<script>
export default {
  name: 'BaseInput',
  data () {
    return {
      value: ''
    }
  },
  props: {
    data: {
      type: String,
      default: ''
    },
    placeholder: {
      type: String,
      default: ''
    },
    classInput: {
      type: String,
      default: ''
    },
    typeInput: {
      type: String,
      default: 'text'
    },
    label: {
      type: String,
      default: ''
    },
    disabled: {
      type: Boolean,
      default: false
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
