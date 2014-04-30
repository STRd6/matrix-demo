Highway through the Dragger Zone
================================

    Matrix = require "matrix"
    Point = require "point"

    debugPoint = document.createElement "div"
    debugPoint.className = "point"
    document.body.appendChild debugPoint

    module.exports = ($element, editor) ->
      active = false
      activeItem = null
      activeView = null
      anchor = null
      startPosition = null
      initialTransform = null
      lastCommand = null
      center = null
      scaling = null
      rotating = null

      $("body").bind
        "touchstart mousedown": (event) ->
          $target = $(target = event.target)
          if $target.is "img"
            event.preventDefault()

            offset = $element.get(0).getBoundingClientRect()
            offset = Point(offset.left, offset.top)

            active = true
            activeView = target
            index = $(".items img").index(target)
            activeItem = editor.items.get index
            initialTransform = activeItem.transform()
            anchor = activeItem.anchor()
            center = offset.add(activeItem.center()).subtract(anchor)

            $(debugPoint).css
              top: center.y
              left: center.x

            scaling = false
            rotating = false

            if event.shiftKey
              scaling = true
            else if event.ctrlKey or event.metaKey
              rotating = true

            startPosition = localPosition(event)

          return

        "touchmove mousemove": (event) ->
          return unless active
          p = localPosition(event)

          # TODO: Need to use more generalized transform concatenation to allow
          # scale and rotation to occur independently

          if scaling
            initialVec = startPosition.subtract center
            currentVec = p.subtract center
            deltaTransform = Matrix.scale currentVec.x / initialVec.x, currentVec.y / initialVec.y, initialTransform.translationComponent()
          else if rotating
            vec = p.subtract center
            initialVec = startPosition.subtract center
            deltaTransform = Matrix.rotation Math.atan2(vec.y, vec.x) - Math.atan2(initialVec.y, initialVec.x), initialTransform.translationComponent()
          else
            deltaTransform = Matrix.translation(p.subtract(startPosition))

          if deltaTransform
            activeItem.transform deltaTransform.concat initialTransform

        "touchend mouseup": (event) ->
          active = false

          return

Helpers
-------

    localPosition = (event) ->
      Point event.pageX, event.pageY
