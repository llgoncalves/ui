<template>
   <label class="k-select no-compact">
    <div class="k-select__title">
      <icon v-if="icon" v-bind:name="icon"></icon>
      {{title}}
    </div>
    <select class="k-select__select" v-model="selected" @change.prevent="emitEvent"  multiple>
      <option v-for="item in options"
              :value="item.value">{{item.description}}</option>
    </select>
   </label>
</template>

<script>
import KytosBase from '../base/KytosBase';
import KytosBaseWithIcon from '../base/KytosBaseWithIcon';

export default {
  name: 'k-select',
  mixins: [KytosBaseWithIcon],
  props: {
    value: {
        type: Array
    },
    options: {
      type: Array,
      required: true
    },
    event: {
      type: Object,
      default: undefined
    },
      action: {
      type: Function,
      default: function (value) { return }
    }
  },
  data () {
    return {
      selected: []
    }
  },
  methods: {
    emitEvent () {
      if (this.event !== undefined){
        let content = this.event.content
        content.value = this.selected
        this.$kytos.$emit(this.event.name, content)
      }
      this.$emit('update:value', this.selected)
      this.action(this.selected)
    }
  },
  mounted () {
    this.options.forEach((item) => {
      this.selected.push(item);
    })
  },
  watch: {
    selected () {
      this.emitEvent()
    },
    options () {
      this.options.forEach((item) => {

        this.selected.push(item);
      })
    }
  }
}
</script>

<style lang="sass">

@import '../../../assets/styles/variables'

.k-select
 position: relative
 display: block
 width: 100%
 padding: 0px
 margin: 5px 0px
 font-size: 0.8em
 color: $fill-text

 &:hover svg
  fill: $fill-icon-h

 &:before
  font: 11px monospace
  line-height: 18px
  color: $fill-icon
  -webkit-transform: rotate(90deg)
  -moz-transform: rotate(90deg)
  -ms-transform: rotate(90deg)
  transform: rotate(90deg)
  opacity: 0.7
  right: 0px
  bottom: 0px
  padding: 0px
  margin: 0px
  -webkit-margin-end: 3px
  border-bottom: 1px solid $fill-icon
  position: absolute
  pointer-events: none
  display: block

 &.inline
  position: relative
  margin-left: 15px
  display: -webkit-flex
  display: flex
  -webkit-align-items: center
  align-items: center
  min-width: 140px
  overflow: hidden

  .k-select__title
   padding-bottom: 2px
   padding-left: 3px
   padding-top: 0px

  .k-select__select
   margin-left: 5px
   width: 130px
   max-width: 130px
   height: 100%

  &:before
   background: $fill-input-bg
   width: 20px
   height: 20px
   text-align: center
   top: 4.5px

.k-select__title
 padding-bottom: 10px
 padding-top: 5px
 padding-left: 10px
 position: relative

 svg
  fill: $fill-icon
  margin-bottom: -5px
  margin-right: 5px

.k-select__select
 -webkit-appearance: none
 -moz-appearance: none
 appearance: none
 font-size: 1em
 padding-left: 3px
 -webkit-padding-start: 5px
 padding-right: 20px
 margin: 0
 margin-right: -20px
 -webkit-border-radius: 4px
 -moz-border-radius: 4px
 border-radius: 4px
 background: $fill-input-bg
 color: $fill-text
 border: none
 outline: 0
 display: inline-block
 cursor: pointer
 width: 100%
 max-width: 100%
 height: 80px
table
 display: table
 width: 100%
 td
  display: table-cell
  color: $fill-text-h
  text-align: left
  border-bottom: 1px solid $fill-button-bg-h
  font-size: 0.8em
  padding: 0.5em

 &:nth-child(odd)
  background-color: $fill-button-bg

 &:nth-child(even)
  background-color: $fill-button-bg

 &:hover *
  color: $fill-shortkey
  background-color: $fill-button-bg-h
</style>
