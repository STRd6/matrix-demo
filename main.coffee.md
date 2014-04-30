Matrix Demo
===========

Visualize the inverse transform simultaneously.

    window.focus()
    require "./duct_tape"

    Observable = require "observable"
    {applyStylesheet} = require "util"

    Item = require "./item"

    applyStylesheet require "./style"

    item = Item
      src: "http://trinket.s3.amazonaws.com/18894/data/c0f15d35bccf20c61e4347dea3c62b785e7346e6"

    inverseItem = Item
      src: item.src()

    item.transform.observe (t) ->
      console.log t.inverse()

    demo =
      items: Observable [item]

    template = require "./templates/editor"
    document.body.appendChild template demo

    Dragzone = require "./dragzone"
    Dragzone($(".items"), demo)

    global.appData = ->
      JSON.stringify editor.items().map (item) ->
        item.toJSON()
