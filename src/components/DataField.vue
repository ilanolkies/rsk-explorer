<template lang="pug">
  .data-field(:style='cellStyle(field,value)')
    //- arrays (uncomplete)
    template(v-if='filteredType==="array"')
      ul(v-for='v in value')
        li {{v}}
    template(v-else)
      template(v-if='trim && !options.noTrim')
        tool-tip.field-value(:value='value' :trim='trim' :options='ttOpts' :router-link='link')
      template(v-else)
        router-link(v-if='link' :to='link')
          .field-value {{ filteredValue || field.default }}
        .field-value(v-else) {{ filteredValue || field.default }}   
      span(v-if='field.suffix') &nbsp; {{field.suffix}}
</template>
<script>
import common from '../mixins/common'
import dataMixin from '../mixins/dataMixin'
import { getType } from '../lib/js/utils'
export default {
  name: 'data-field',
  mixins: [common, dataMixin],
  props: {
    field: {
      type: Object,
      required: true
    },
    row: {
      type: Object,
      required: true
    },
    options: {
      type: Object,
      default: Object
    }
  },
  computed: {
    filteredValue () {
      return this.filterFieldValue()(this.field, this.value)
    },
    value () {
      return this.getValue(this.field, this.row, true)
    },
    filteredType () {
      return getType(this.filteredValue)
    },
    link () {
      return this.makeLink(this.field, this.row)
    },
    trim () {
      return this.computeTrim(this.field, this.value)
    }
  }
}
</script>
<style lang="stylus">
  @import '../lib/styl/vars.styl'
  @import '../lib/styl/mixins.styl'

  //.data-field > .tooltip
    // white-space nowrap
   
  .data-field, 
  .data-field > a, 
  .data-field > .tooltip, 
    max-width 100%
    display flex
    position relative
    word-wrap break-word
    overflow-wrap break-word
    word-break break-word
  
  .field-value 
    overflow-wrap break-word
    word-wrap break-word
    -ms-word-break break-all
    word-break break-all
    word-break break-word
    -ms-hyphens auto
    -moz-hyphens auto
    -webkit-hyphens auto
    hyphens auto

  .flex-table 
    & td .data-field
      width 100%

</style>



