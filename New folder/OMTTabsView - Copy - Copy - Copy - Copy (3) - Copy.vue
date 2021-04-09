<template>
  <section class="OMTTabsView">
    <header class="OMTTabsView-header">
      <ul class="OMTTabsView-tabs">
        <li v-for="(tab, index) in tabs" :key="`Tab-${index}`" >
          <div
            class="OMTTabsView-tab"
            :class="{ 'active': tab.isActive }"
            @click="selectTab(tab)"
          >
            {{ tab.name }}
          </div>
        </li>
      </ul>
    </header>
    <section class="OMTTabsView-detail">
      <slot></slot>
    </section>
  </section>
</template>

<script>
  export default {
    name: 'omt-tabs-view',

    data() {
      return {
        tabs: this.$children,
      }
    },

    methods: {
      selectTab(tab) {
        this.tabs.forEach(t => t.isActive = t.name === tab.name);
        this.$emit('click', tab.name)
      }
    }
  }
</script>

<style lang="stylus" scoped>
  .OMTTabsView
    height: inherit
    
    &-tab
      padding: 1rem
      line-height: 1rem
      cursor: pointer
      border-bottom: 0
      will-change: border-bottom
      transition: all 0.1s ease-in
      color: #a7a7a7
      border-bottom-width: 4px
      border-bottom-style: solid
      border-bottom-color: transparent
      user-select: none

      &.active
        border-bottom: 4px solid #4cbf9c
        color: #5e5e5e

    &-tabs
      list-style: none
      display: flex
      border-bottom: 1px solid #dadada
      margin-bottom: 1.7rem

    &-detail
      border: 1px solid #dddddd
      box-shadow: 0 2px 2px rgba(175, 175, 175, 0.16)
      padding: 8px
      padding-top: 0

    @media screen and (min-width: 1023px)
      .OMTTabsView        
        &-tab
          &:hover
            color: #5e5e5e
</style>
