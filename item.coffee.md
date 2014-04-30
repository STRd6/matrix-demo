Item Model
==========

    Composition = require "composition"
    Observable = require "observable"
    Point = require "point"
    Matrix = require "matrix"
    Size = require "size"
    {extend, defaults} = require "util"

    module.exports = (I={}) ->
      defaults I,
        transform: Matrix()
        size: Size(1, 1)

      self = Composition(I).extend
        position: ->
          self.transform().translationComponent()

        copy: ->
          extend {}, I

        anchor: ->
          Point(1, 1).scale(self.size().scale(0.5))

        center: ->
          self.anchor().add(self.position())

      self.attrObservable "src"
      self.attrModel "transform", Matrix
      self.attrModel "size", Size

      autosize = (src) ->
        img = document.createElement "img"
        img.onload = ->
          self.size Size @width, @height

        img.src = src

      autosize self.src()
      self.src.observe autosize

      self.css = Observable ->
        {x, y} = self.anchor().scale(-1)
        Matrix.translation(x, y).concat(self.transform()).css()

      return self
