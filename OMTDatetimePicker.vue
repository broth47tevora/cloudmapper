<template>
  <div class="Datetime">
    <p class="Datetime-label">{{label}}</p>
    <input class="Datetime-element" @input="emitState" type="date" v-model="plainDate"/>
    <input class="Datetime-element" @input="emitState" type="time" v-model="plainTime"/>
  </div>
</template>

<script>

export default {
  name: 'OMTDatetimePicker',

  props: {
    label: {
      required: false,
      default: 'Please select date and time:',
    }
  },

  data() {
    return {
      plainDate: null,
      plainTime: null,
    }
  },

  methods: {
    emitState() {
      if (this.fullDate) {
        this.$emit('change', this.fullDate);
      }
    },
  },

  computed: {
    fullDate() {
      return this.plainDate && this.plainTime ? new Date(`${this.plainDate} ${this.plainTime}`) : null;
    }
  }
}
</script>

<style lang="stylus">
.Datetime
  &-label
    font-weight: bold
    margin: 10px 0
  
  &-element
    font-family: 'Lato', Arial, Helvetica, sans-serif
    flex: 1
    padding: 0.4rem
    line-height: 1rem
    border: 1px solid #cacaca
    background-color: #fff
    box-shadow: inset 0 1px 1px hsla(0,0%,62.7%,.2)
    border-radius: 4px
    width: inherit

</style>