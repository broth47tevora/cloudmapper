<template>
  <li class="row grid">
    <span class="row-item col-span-5">{{ item.jobName || item.name }}</span>
    <span class="row-item col-span-2">{{ item.status }}</span>
    <span class="row-item col-span-3">{{ lastRun }}</span>
    <div class="row-actions col-span-2" v-if="!progressStatuses.includes(item.status)">
      <button @click.prevent="$emit('click', 'run', item)" title="Run" v-if="run">
        <OMTIcon icon="play" size="lg" />
      </button>
      <button @click.prevent="$emit('click', 'edit', item)" title="Edit" v-if="edit">
        <OMTIcon icon="pen" size="lg" />
      </button>
      <button @click.prevent="$emit('click', 'remove', item)" title="Delete" v-if="remove">
        <OMTIcon icon="times-circle" size="lg" />
      </button>
      <button @click.prevent="$emit('click', 'log', item)" title="See log activity">
        <OMTIcon icon="eye" size="lg" />
      </button>
    </div>
  </li>
</template>
<script>
export default {
  name: 'BackupListItem',

  props: {
    item: {
      type: Object,
      requied: true,
    },
    run: Boolean,
    edit: Boolean,
    remove: Boolean,
  },

  data() {
    return {
      progressStatuses: ['started', 'processing'],
    };
  },

  computed: {
    lastRun () {
      const date = new Date(this.item.updatedAt);

      return date.toLocaleString();
    }
  }
}
</script>
<style lang="stylus" module>

</style>