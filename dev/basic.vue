<template lang="pug">
.container
  p collapsible
  collapsible
    collapsible-item(ref="ci1")
      p(slot="header") header1
      p body 1
    collapsible-item(ref="ci2")
      p(slot="header") header2
      p body 2
    collapsible-item(ref="ci3")
      p(slot="header") header3
      p body 3
  p accordion
  collapsible(accordion)
    collapsible-item(ref="ai1")
      p(slot="header") header1
      p body 1
    collapsible-item(stay-open ref="ai2")
      p(slot="header") header2 / stay-open
      p body 2
    collapsible-item(transition="collapsible2")
      p(slot="header") custom transition
      p body 3
    collapsible-item(is-opened ref="ai3")
      p(slot="header") header4 / is-opened
      p body 4

  br
  a(href="https://github.com/vue-comps/vue-collapsible/blob/master/dev/basic.vue") source
</template>

<script lang="coffee">
Velocity = require("velocity-animate")
module.exports =
  components:
    "collapsible" : require "../src/collapsible.vue"
    "collapsible-item" : require "../src/collapsible-item.vue"
  created: ->
    Vue.component "collapsible2",
      functional: true
      render: (h,context) ->
        context.data.attrs.name = "fade"
        context.data.props = {css: false}
        context.data.on = {
          enter: (el, done) ->
            Velocity el, "slideDown",{
              duration: 350
              easing: "easeOutQuart"
              queue: false
              complete: done
            }
          enterCancelled: (el) ->
            Velocity el, "stop"
          leave: (el, done) ->
            Velocity el, "slideUp",{
              duration: 350
              easing: "easeOutQuart"
              queue: false
              complete: done
            }
          leaveCancelled: (el) ->
            Velocity el, "stop"
        }
        return h "transition",context.data,context.children
</script>

<style lang="stylus">
.collapsible
  border 1px solid black
  width 150px
  list-style-type: none
  padding 0
  p
    margin 0
  .collapsible-header
    display: block
    padding 1em

  .collapsible-item
    border-bottom 1px solid black
  .collapsible-body
    border-top 1px dashed black
    padding 1em 4px 1em 30px

</style>
