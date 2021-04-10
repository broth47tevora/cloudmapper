<template>
  <div class="Groups">
    <div class="Filters">
      <p class="Filters-title">{{stepNumber}} Select the action that you want to perform on groups:</p>

      <div class="Filters-header">
        <OMTCheck
          name="backup-type"
          type="radio"
          value="NO"
          :checked="activeFilter === 'NO'"
          @change="setActiveFilter"
        >
          No backup
        </OMTCheck>

        <OMTCheck
          name="backup-type"
          type="radio"
          value="ALL"
          :checked="activeFilter === 'ALL'"
          @change="setActiveFilter"
        >
          Backup All
        </OMTCheck>

        <OMTCheck
          name="backup-type"
          type="radio"
          value="CUSTOM"
          :checked="activeFilter === 'CUSTOM'"
          @change="setActiveFilter"
        >
          Custom
        </OMTCheck>
      </div>

      <div class="Filters-details" v-if="activeFilter === 'CUSTOM'">
        <div>

          <OMTSelect
            multiple
            displayBy="name"
            label="Groups"
            placeholder="Select all the groups that you want to backup"
            trackBy="id"
            :options="groupsOptions"
            @input="setGroups"
          />

          <OMTMatchByField
            :fields="MATCH_FIELDS"
            :items="savedMatchs"
            @change="setCustomFilters"
          />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { mapGetters } from 'vuex';

const MATCH_FIELDS = [
  { id: 1, label:'Name', value: 'name'},
  { id: 2, label:'Type', value: 'type'},
];

export default {
  name: 'Groups',

  props: {
    selectedFilter: {
      type: Object,
    },

    stepNumber: {
      type: String,
      required: false,
      default: '',
    }
  },

  data() {
    return {
      MATCH_FIELDS,
      activeFilter: 'NO',
      customFilters: [],
      groups: [],
      savedGroups: [],
      savedMatchs: [],
    }
  },

  mounted() {
    if (this.selectedFilter) {
      this.populateFields(this.selectedFilter.groups)
    }
  },

  computed: {
    ...mapGetters({
      groupsList: 'tenant/activeGroups',
    }),

    groupsOptions() {
      if (this.savedGroups.length) {
        return this.groupsList.map((group => (
          {
            ...group,
            selected: this.savedGroups.includes(group.name)
          }
        )));
      }

      return this.groupsList;
    },
  },

  methods: {
    emitState(state) {
      const groups = state || { groups: [...this.groups, ...this.customFilters] };

      this.$emit('change', groups);
    },

    setGroups(value) {
      const groups = [];
      
      value.forEach(group => {
        groups.push({
          type: 'eq',
          field: 'name',
          value: group.name,
        });
      });

      this.groups = groups;
      this.emitState();
    },

    setActiveFilter(value) {
      this.activeFilter = value;

      if (value === 'ALL') {
        this.emitState({ groups: [] });
      } else if (value === 'NO') {
        this.emitState({ groups: null });
      }
    },

    setCustomFilters(value) {
      this.customFilters = value;
      this.emitState();
    },

    populateFields(groups) {
      if (groups) {      
        this.activeFilter = groups.length ? 'CUSTOM' : 'ALL';      
        this.savedGroups = groups
          .filter((i) => i.type === 'eq' && i.field === 'name')
          .map(i => i.value);
        this.savedMatchs = groups.filter((i) => !this.savedGroups.includes(i.value))
      }
    }
  },
}
</script>
<style lang="stylus" scoped>
  .Filters
    margin-top: 0

    &-header
      display: grid;
      grid-template-columns: repeat(2, 1fr);

    &-title
      font-weight: 700
      font-size: 13px
      padding: 7px 10px 7px 0

@media screen and (min-width: 1023px)
  .Filters
    &-header
      grid-template-columns: repeat(3, 8rem);
</style>