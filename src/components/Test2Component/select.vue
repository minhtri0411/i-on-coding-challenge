<template>
  <div>
    <span v-if="label" class="label">{{label}}</span>
    <el-select v-model="valueSelected" :disabled="disabled" :placeholder="placeholder"
    @change="$emit('change', valueSelected)">
        <el-option
          v-for="item in listOptions"
          :key="item.refId"
          :label="item.label"
          :value="item.value"
        ></el-option>
    </el-select>
  </div>
</template>
<script>
export default {
  name: 'BaseSelect',
  data () {
    return {
      value: '',
      options: []
    }
  },
  props: {
    selected: {
      type: String,
      default: ''
    },
    data: {
      type: Array,
      default: () => []
    },
    placeholder: {
      type: String,
      default: 'Select'
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
    this.value = this.selected
    this.options = this.data
  },

  watch: {
    selected: function (val) {
      if (val) {
        this.value = val
      }
    },

    data: function (newOptions) {
      this.options = newOptions
    }
  },

  computed: {
    valueSelected: {
      get: function () {
        return this.value
      },
      set: function (newValue) {
        this.value = newValue
      }
    },

    listOptions: {
      get: function () {
        return this.options
      },
      set: function (newValue) {
        this.options = newValue
      }
    }
  }
}
</script>
