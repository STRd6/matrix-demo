Duct Tape
=========

    global.Observable = require "observable"
    Point = require "point"
    Size = require "size"
    Matrix = require "matrix"

    {sqrt} = Math

    Matrix.Point = Point

    Point::scale = (width, height) ->
      if typeof width is "object"
        {width, height} = width

      height ?= width

      Point(@x * width, @y * height)

    Point::abs = ->
      Point(Math.abs(@x), Math.abs(@y))

    Matrix::scaleComponent = ->
      Size sqrt(@a * @a + @b * @b), sqrt(@c * @c + @d * @d)

    Matrix::translationComponent = ->
      Point @tx, @ty

Convert matrix to CSS string.

    if navigator.userAgent.match /WebKit/
      prefix = "-webkit-"
    else
      prefix = ""

    Matrix::css = ->
      "#{prefix}transform: matrix(#{@a}, #{@b}, #{@c}, #{@d}, #{@tx}, #{@ty});"

    Matrix.translation = Matrix.translate = (x, y) ->
      if typeof x is "object"
        {x, y} = x

      Matrix(1, 0, 0, 1, x, y)
