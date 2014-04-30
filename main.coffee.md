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
      inverseItem.transform t.inverse()

    fmt = ({a, b, c, d, tx, ty}) ->
      """
        #{a} #{b} #{tx}
        #{c} #{d} #{ty}
        0.00 0.00 1.00
      """

    demo =
      items: Observable [item]
      transform: Observable ->
        fmt item.transform().toFixed(2)

      inverseTransform: Observable ->
        fmt item.transform().inverse().toFixed(2)
      ghost: inverseItem

    template = require "./templates/editor"
    document.body.appendChild template demo

    Dragzone = require "./dragzone"
    Dragzone($(".items"), demo)

    global.appData = ->
      JSON.stringify editor.items().map (item) ->
        item.toJSON()
