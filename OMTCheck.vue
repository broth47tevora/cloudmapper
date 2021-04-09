<template>
  <div class="OMTCheck">
    <input
      :id="id"
      :class="{ 'switch': toggle }"
      v-bind="$attrs"
      v-on="listeners"
      :type="$attrs.type || 'checkbox'"
      :value="value"
    >

    <label :for="id">
      <slot>
        <template v-if="value">{{ value }}</template>
      </slot>
    </label>
  </div>
</template>
<script>
import { v4 as uuidv4 } from 'uuid';

export default {
  inheritAttrs: false,
  name: 'OMTCheck',

  props: {
    toggle: Boolean,
    value: [String, Number, Boolean]
  },

  data() {
    return {
      id: uuidv4(),
    }
  },

  computed: {
    listeners() {
      return {
        ...this.$listeners,
        input: this.input,
        change: this.change,
      }
    }
  },

  methods: {
    input(event) { this.$emit('input', event.target.checked) },
    change(event) { this.$emit('change', event.target.value) },
  },
}
</script>
<style lang="stylus" scoped>
.OMTCheck
  width: fit-content
  padding: 5px 0
@supports(-webkit-appearance: none) or (-moz-appearance: none)
  input[type='checkbox']
  input[type='radio']
    --active: #44bc98;
    --active-inner: #fff;
    --focus: 2px #acf7e0;
    --border: #9dc3b8;
    --border-hover: #44bc98;
    --background: #fff;
    --disabled: #F6F8FF;
    --disabled-inner: #E1E6F9;

    -webkit-appearance: none;
    -moz-appearance: none;
    height: 19px;
    outline: none;
    display: inline-block;
    vertical-align: top;
    position: relative;
    margin: 0;
    cursor: pointer;
    border: 1px solid var(--back-alt, var(--border));
    background: var(--back-state, var(--background));
    transition: background .3s, border-color .3s, box-shadow .2s;

    &:after
      content: '';
      display: block;
      left: 0;
      top: 0;
      position: absolute;
      transition: transform var(--duration, .3s) var(--timing, ease), opacity var(--delay, .2s);

    &:checked
      --back-state: var(--active);
      --back-alt: var(--active);
      --delay: .3s;
      --duration: .6s;
      --timing: cubic-bezier(.2, .85, .32, 1.2);

    &:disabled
      --back-state: var(--disabled);
      cursor: not-allowed;
      opacity: 0.7;

      &:checked
        --back-state: var(--disabled-inner);
        --back-alt: var(--border);

      & + label
        cursor: not-allowed;
        color: lighten(gray, 10%)

    &:hover
      &:not(:checked)
        &:not(:disabled)
          --back-alt: var(--border-hover);

    &:focus
      box-shadow: 0 0 0 var(--focus);

    &:not(.switch)
      width: 19px;

      &:after
        opacity: var(--opacity, 0);

      &:checked
        --opacity: 1;

    & + label
      font-size: 14px;
      line-height: 19px;
      display: inline-block;
      vertical-align: top;
      cursor: pointer;
      margin-left: 4px;
      user-select: none

  input[type='checkbox']
    &:not(.switch)
      border-radius: 7px;
      &:after
        width: 5px;
        height: 9px;
        border: 2px solid var(--active-inner);
        border-top: 0;
        border-left: 0;
        left: 5px;
        top: 2px;
        transform: rotate(var(--angle, 20deg));

      &:checked
        --angle: 43deg;

    &.switch
      width: 38px;
      border-radius: 11px;
      &:after
        left: 2px;
        top: 2px;
        border-radius: 50%;
        width: 15px;
        height: 15px;
        background: var(--toggle-Background, var(--border));
        transform: translateX(var(--xAxis, 0));

      &:checked
        --toggle-Background: var(--active-inner);
        --xAxis: 17px;

      &:disabled
        &:not(:checked)
          &:after
            opacity: .6;

  input[type='radio']
    border-radius: 50%;
    &:after
      width: 17px;
      height: 17px;
      border-radius: 50%;
      background: var(--active-inner);
      opacity: 0;
      transform: scale(var(--scaleX, .7));

    &:checked
      --scaleX: .5;
</style>