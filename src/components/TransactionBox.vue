<template lang="pug">
  .transaction.box.row(:style='txBoxStyle')
    .box-icons
      router-link(:to='txLink')
        icon(:name='entity.icon' :color='blockColor')
    .box-content
      ul.plain.flex
        li.half(:style='blockStyle2')
          data-field(:field='fields.hash' :row='tx')
        li.half
            router-link(:to='blockLink')
              icon(:name='bField.icon' :color='blockColor')
              small(:style='blockStyle2') &nbsp; {{blockNumber}}
        li.half(v-if='tx.txType == "normal"')
            data-field(:field='fields.from' :row='tx')
            icon(name='arrow-right' :color='blockColor')
            data-field(:field='fields.to' :row='tx')
        li.half(v-else)
          span {{tx.txType}}
        li.half.soft
          field-title.small(:field='fields.time')
          data-field(:field='fields.time' :row='tx')

</template>
<script>
import { mapGetters } from 'vuex'
import { mSecondsAgo } from '../filters/TimeFilters'
import dataMixin from '../mixins/dataMixin'
import DataField from './DataField'
import FieldTitle from './FieldTitle'
export default {
  name: 'transaction-box',
  components: {
    DataField,
    FieldTitle
  },
  mixins: [
    dataMixin
  ],
  filers: {
    mSecondsAgo
  },
  props: ['tx'],
  data () {
    return {
      type: 'transactions'
    }
  },
  computed: {
    ...mapGetters({
      now: 'getDate'
    }),
    blockColor () {
      return this.getBlockColor(this.tx.blockNumber)
    },
    blockStyle2 () {
      let color = this.blockColor
      return { color }
    },
    txBoxStyle () {
      let color = this.blockColor
      return { 'border-color': color }
    },
    bField () {
      return this.fields.block
    },
    txLink () {
      return this.makeLink(this.fields.hash, this.tx)
    },
    blockLink () {
      return this.makeLink(this.bField, this.tx)
    },
    blockNumber () {
      return this.filterFieldValue()(this.bField, this.tx.blockNumber)
    }
  }

}
</script>
<style lang="stylus">
  .transaction
    font-size 0.8em
    border-left solid 1px
</style>