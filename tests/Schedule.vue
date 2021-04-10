<template>
  <div>
    <div class="Schedule">
      <p class="Schedule-title">{{label}}</p>
      <div class="Schedule-header">
        <OMTCheck
          name="schedule-type"
          type="radio"
          value="NO"
          :checked="scheduleType === 'NO'"
          @change="setScheduleType"
        >
          Run immediately
        </OMTCheck>

        <OMTCheck
          name="schedule-type"
          type="radio"
          value="ONCE"
          :checked="scheduleType === 'ONCE'"
          @change="setScheduleType"
        >
          Schedule once
        </OMTCheck>

        <OMTCheck
          name="schedule-type"
          type="radio"
          value="RECURRENT"
          :checked="scheduleType === 'RECURRENT'"
          @change="setScheduleType"
        >
          Recurrent
        </OMTCheck>
      </div>
      <div class="Schedule-details" v-if="scheduleType !== 'NO'">
        <div v-if="scheduleType === 'ONCE'">
          <OMTDatetimePicker @change="setCron"/>
        </div>
        <div v-if="scheduleType === 'RECURRENT'">
          <VueCronEditorBuefy 
            @input="emitState()" 
            v-model="cron"
            :visibleTabs="['daily', 'weekly', 'monthly', 'advanced']"
            locale="custom"
            :custom-locales="{
              custom: {
                  every: 'Every',
                  mminutes: 'minute(s)',
                  hoursOnMinute: 'hour(s) on minute',
                  daysAt: 'day(s) at',
                  at: 'at',
                  onThe: 'On the',
                  dayOfEvery: 'day, of every',
                  monthsAt: 'month(s), at',
                  everyDay: 'Every',
                  mon: 'Mon',
                  tue: 'Tue',
                  wed: 'Wed',
                  thu: 'Thu',
                  fri: 'Fri',
                  sat: 'Sat',
                  sun: 'Sun',
                  hasToBeBetween: 'Has to be between',
                  and: 'and',
                  daily: 'Daily',
                  weekly: 'Weekly',
                  monthly: 'Monthly',
                  advanced: 'Advanced',
                  cronExpression: 'cron expression:'
              }
          }"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import VueCronEditorBuefy from 'vue-cron-editor-buefy';

export default {
  name: 'ScheduleJob',
  
  components: {
    VueCronEditorBuefy
  },

  props: {
    label: {
      type: String,
      required: false,
      default: 'Select how you want to schedule this job:',
    }
  },

  data() {
    return {
      scheduleType: 'NO',
      cron: '*/1 * * * *',
      savedSchedule: null,
    }
  },

  computed: {
    recurring() {
      return this.scheduleType === 'RECURRENT';
    }
  },

  methods: {
    emitState(state) {
      if (state) {
        this.$emit('change', state);  
        return;
      }

      const { cron, recurring } = this;
      const newState = { 
        schedule: (this.scheduleType !== 'NO') ? { cron, recurring } : null
      }

      this.$emit('change', newState);
    },

    setScheduleType(value) {
      this.scheduleType = value;
      this.emitState();
    },
    
    setCron(value) {

      const mins = value.getMinutes();
      const hours = value.getHours();
      const dayofmonth = value.getDate();
      const month = value.getMonth() + 1;
      const dayofweek = value.getDay();

      this.cron = `${mins} ${hours} ${dayofmonth} ${month} ${dayofweek}`;
      this.emitState();
    }
  }
}
</script>

<style lang="stylus">
  .Schedule
    margin-top: 15px

    &-header
      display: grid;
      grid-template-columns: repeat(2, 1fr);

    &-title
      font-weight: 700
      font-size: 13px
      padding: 7px 10px 7px 0

  div.control input.input
    margin: 5px 0
    height: 1.7rem

@media screen and (min-width: 1023px)
  .Schedule
    &-header
      grid-template-columns: repeat(3, 8rem);
</style>