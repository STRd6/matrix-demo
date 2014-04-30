Matrix Demo
===========

Visualize the inverse transform simultaneously.

    window.focus()
    require "./duct_tape"

    Observable = require "observable"
    {applyStylesheet} = require "util"

    applyStylesheet require "./style"

    demo =
      items: Observable []

    template = require "./templates/editor"
    document.body.appendChild template demo

    Dragzone = require "./dragzone"
    Dragzone($(".items"), demo)

    Item = require "./item"

    global.appData = ->
      JSON.stringify editor.items().map (item) ->
        item.toJSON()
