<template>
  <button
    :class="[
      'OMTButton',
      { 'OMTButton-primary': primary },
      { 'OMTButton-danger': danger },
      { 'OMTButton-small': small },
      { 'OMTButton-inline': inline }
    ]"
    type="button"
    v-bind="$attrs"
    v-on="$listeners"
  >
    <OMTIcon class="OMTButton-icon" v-if="icon" :icon="icon" />
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'OMTButton',
  props: {
    primary: Boolean,
    danger: Boolean,
    small: Boolean,
    inline: Boolean,
    icon: String
  }
}
</script>

<style lang="stylus" scoped>
  .OMTButton
    background: linear-gradient(#fefefe, #f9f9f9), #f9f9f9
    border: 1px solid
    border-radius: 3px
    border-color: #c3c3c3 #c3c3c3 #bebebe
    box-shadow: rgba(0, 0, 0, 0.15) 0 1px 0, rgba(255, 255, 255, 0.1) 0 1px 0 0 inset
    color: #5e5e5e
    text-align: center
    cursor: pointer
    min-width: 7rem
    height: 28px
    padding 0 10px
    line-height 26px
    margin-right: 1rem
    font-size: 1rem
    font-family: 'Lato', Arial, Helvetica, sans-serif
    transition: all 0.2s ease
    font-size: 13px

    &:focus
      outline: none

    &:active
      box-shadow: 1px 1px 5px -1px rgba(0, 0, 0, 0.3) inset

    &-primary
      color: #fff
      background: linear-gradient(#4cbf9c, #44bc98), #44bc98
      border-color: #328c71 #328c71 #2f856b

    &-danger
      color: #fff
      background: linear-gradient(#e24545, #e13c3c), #e13c3c
      border-color: #bd1d1d #bd1d1d #b41c1c
    
    &-small
      min-width: 4rem
      height: 1.5rem
      font-size: 13px

    &-inline
      border: none
      box-shadow: none
      min-width: fit-content
      padding: 0 5px

    &-icon
      margin-right: 6px
</style>