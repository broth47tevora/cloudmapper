<template>
  <div class="RecordItem">
    <div>
      <span class="RecordItem-time">{{ timeSince(record.createdAt) }}</span>
    </div>
    <div>
      <span class="">Status: </span>
      <span :class="['RecordItem-status', `RecordItem-status--${status}`]">{{ status }}</span>
    </div>
    <div class="RecordItem-details" v-if="record.messages.length">
      <div>Details:</div>
      <ul class="RecordItem-details-message">
        <template v-for="(message, index) in record.messages">
          <li v-if="message" :key="index">
            {{ message }}
          </li>
        </template>
      </ul>
    </div>
  </div>
</template>

<script>
import { timeSince } from '@/utils/dates';

const LOG_STATUS = {
  'processor_started': 'Started',
  'processor_processing': 'Processing',
  'processor_failed': 'Failed',
  'processor_finished': 'Finished',
}

export default {
  name: 'RecordItem',

  props: {
    record: {
      type: Object,
      required: true,
    }
  },

  computed: {
    status() {
      return  LOG_STATUS[this.record?.details?.message] ?? '';
    },
  },

  methods: {
    timeSince,
  }
}
</script>

<style lang="stylus" scoped>
  $colorInfo = #209cee
  $colorSuccess = #00d1b2
  $colorWarning = #ff9d57
  $colorDanger = #ff3860

  .RecordItem
    background-color: #f2f2f2
    border: 1px solid #e2e2e2
    margin-bottom: 6px
    padding: 5px

    &-time
      font-size: 1rem;
      font-weight: 700;
      margin-bottom: 5px;
      display: block;
      border-bottom: 1px solid #d4d4d4

    &-details
      margin-top: 5px

      &-message
        padding: 5px

        & li
          list-style: inside

    &-status
      font-weight: 700
      &--Failed
        color: $colorDanger;
      &--Started
        color: $colorInfo;
      &--Processing
        color: $colorInfo;
      &--Finished
        color: $colorSuccess;
</style>