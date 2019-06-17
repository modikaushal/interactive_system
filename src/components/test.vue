<template>
    <span>
        <div class="container">
          <canvas id="canvas"></canvas>
        <div>
            <div id="l0" class="draggable">0</div>
            <div id="l1" class="draggable">1</div>
            <div id="l2" class="draggable">2</div>
        </div>
        <div id="wrap2">
            <div id="0r" class="draggable right">0</div>
            <div id="1r" class="draggable right">1</div>
            <div id="2r" class="draggable right">2</div>
        </div>
      </div>
    </span>
</template>

<script>
// import * as d3 from 'd3'

export default {
  name: 'test',
  data () {
    return {
      search: '',
      dd: [
        {
          id: '0'
        },
        {
          id: '1'
        },
        {
          id: '2'
        }
      ]
    }
  },
  mounted: function () {
    var canvas = document.getElementById('canvas')
    var ctx = canvas.getContext('2d')
    canvas.width = window.innerWidth
    canvas.height = window.innerHeight
    ctx.lineWidth = 1
    for (var i in this.dd) {
      var curEl = window.$('#l' + i)
      var netEl = window.$('#' + i + 'r')
      console.log(curEl, curEl.offset().left + curEl.width(), curEl.offset().top + curEl.height())
      console.log(curEl.offset())
      ctx.moveTo(curEl.offset().left + curEl.width(), curEl.offset().top + curEl.height() / 2)
      ctx.lineTo(netEl.offset().left, curEl.offset().top + netEl.height() / 2)
      ctx.stroke()
    }
  }
}
</script>
<style>
#canvas {
    position:absolute;
}
.draggable {
    width:50px;
    height:30px;
    background:skyblue;
    border:1px solid;
}
.right {
    margin-left:100px;
    background:salmon;
}
#wrap2 {
    margin-top:-95px;
}
</style>