<template>
  <OMTModal :caption="`${this.job.name} Job Activity`" :show="isOpenModal">
    <OMTForm embebed>

      <RecordItem 
        v-for="record in records"
        :key="record._id"
        :record="record"
      />
      <div v-show="!records.length" class="emptyList">
        <img :src="empty" alt="empty">
      </div>
    </OMTForm>
    <template #actions>
      <OMTButton @click="close">Close</OMTButton>
    </template>
  </OMTModal>
</template>

<script>
  import empty from '@/assets/empty-list.png';

  import { mapActions } from 'vuex';
  import RecordItem from '@/components/backups/RecordItem.vue';

  export default {
    name: "RecordsModal",

    components: {
      RecordItem,
    },

    props: {
      isOpenModal: Boolean,
      job: {
        type: Object,
        required: true
      }
    },    
    
    data() {
      return {
        records: {},
        empty,
      }
    },

    created() {
      if (this.job.id) {
        this.fetchLog(this.job.id)
          .then((res) => this.records = res);
      }
    },

    methods: {
      ...mapActions({
        fetchLog: 'job/fetchJobLog',
      }),

      close() {
        this.$emit('close');
      },
    },
  }
</script>

<style lang="stylus" scoped>
.emptyList
  height: 90%
  display: flex
  justify-content: center
  align-items: center
</style>