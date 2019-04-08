<template>
  <div :class="classInput">
    <span v-if="label" class="label">{{label}}</span>
    <el-color-picker
      v-model="inputValue"
      :disabled="disabled"
      @change="$emit('change', inputValue)"
      @active-change="$emit('active-change')">
    </el-color-picker>
  </div>
</template>
<script>
export default {
  name: 'BaseColorPicker',
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
    disabled: {
      type: Boolean,
      default: false
    },
    classInput: {
      type: String,
      default: ''
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
