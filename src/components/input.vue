<style lang="stylus">
  @require '../stylus/mixins/input.styl'

  .fk-input
    input-mixin()

</style>

<template>
  <input :type="type" class="fk-input" :class="classObject" :value="value" @input="onInput" @keyup="onKeyup" :disabled="disabled"/>
</template>

<script>
  import validator from '../mixins/validator'

  export default {
    name: 'Input',
    mixins: [ validator() ],
    props: {
      type: {
        type: String,
        default: 'text'
      },
      size: {
        type: String,
        default: ''
      },
      cutSide: {
        type: String,
        default: ''
      },
      block: {
        type: Boolean,
        default: false
      },
      required: {
        type: Boolean,
        default: false
      },
      value: {
        type: [String, Number],
        default: ''
      },
      disabled: {
        type: Boolean,
        default: false
      },
      match: {
        type: String,
        default: ''
      },
      requireValidation: {
        type: Boolean,
        default: false
      },
      blackList: {
        type: Array,
        default: () => []
      }
    },
    computed: {
      classObject() {
        return {
          'fk-input--small': this.size === 'small',
          'fk-input--cut-left': this.cutSide === 'left',
          'fk-input--cut-right': this.cutSide === 'right',
          'fk-input--block': !!this.block,
          'fk-input--invalid': this.invalid
        }
      }
    },
    methods: {
      onInput(e) {
        const value = e.target.value
        if(this.type === 'ukZip') this.$emit('input', this.formatUkZip(value))
        else this.$emit('input', value)
        this.touched = true
      },
      formatUkZip (zip = '') {
        const tmp = zip.replace(/\s+/g, '')
        if (tmp.length < 5) return tmp
        return tmp.slice(0, -3).concat(' ').concat(tmp.slice(-3))
      },
      onKeyup() {
        this.$emit('keyup')
      }
    }
  }
</script>
