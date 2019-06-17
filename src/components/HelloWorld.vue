<template>
  <span>
    <canvas id="canvas"></canvas>
    <div class="container">
      <div class="row">
        <div class="col-3"><br/>
          <div class="card">
            <h4 class="card-header">
              <div class="row">
                <div class="col">
                  Track
                </div>
                <div class="col">
                  <input type="text" class="form-control" id="search_track" v-model="track_search" placeholder="Search..."> 
                </div>
              </div> 
            </h4>
            <div class='scroll'>
              <ul class="list-group list-group-flush">
                  <li :id = "'tra' +  track.id" v-for="track in filterBy(aj_track, track_search)"  class="list-group-item" @click="track_click(track.id)">
                    {{track.name}}<br/>
                    <input id="slider1" type="range" min="1" max="10" step="1" v-model="track.range" @change="tra_slid_change(track.id)"/>
                  </li>
              </ul>
            </div>
          </div>
        </div>
        <div class="col-1">
        </div>
        <div class="col-3"><br/>
          <div class="scroll" style="height:600px">
            <div class="card">
              <h4 class="card-header">
                <div class="row">
                  <div class="col-12">
                    Wikipedia
                  </div>
                  <div class="col-7">
                    <input id="slider1" type="range" min="1" max="10" step="1" v-model="wiki_range" />
                  </div>
                  <div class="col-5">
                    <h6 style="padding-top: 9px;">{{wiki_range}} of 10</h6>
                  </div>
                </div> 
              </h4>
              <div>
                <ul class="list-group list-group-flush">
                    <li :id="'wek' + wek.id" class="list-group-item" v-for="wek in limitBy(weki, wiki_range)" @click="wiki_click(wek.id)">
                      {{wek.name}}
                      <input id="slider1" type="range" min="1" max="10" step="1" v-model="wek.range" />
                    </li>
                </ul>
              </div>
            </div><br/>
            <div class="card">
              <h4 class="card-header">
                <div class="row">
                  <div class="col-12">
                    Facebook
                  </div>
                  <div class="col-7">
                    <input id="slider1" type="range" min="1" max="10" step="1" v-model="fb_range" />
                  </div>
                  <div class="col-5">
                    <h6 style="padding-top: 9px;">{{fb_range}} of 10</h6>
                  </div>
                </div>
              </h4>
              <div>
                <ul class="list-group list-group-flush">
                    <li :id = "'fb' + fb.id" class="list-group-item" v-for="fb in limitBy(face, fb_range)">{{fb.name}}</li>
                </ul>
              </div>
            </div>
          </div>
        </div>
        <div class="col-1">
        </div>
        <div class="col-3"><br/>
          <div class="card">
            <h4 class="card-header"> Recomendations </h4>
            <div class='scroll' style="height:560px">
              <ul class="list-group list-group-flush" v-for="rec in limitBy(recom, 10)" @click="rec_click(rec.id)">
                  <li :id="'rec' + rec.id" class="list-group-item">{{rec.name}}</li>
              </ul>
            </div>
          </div>
        </div>
        <div class="col-1"><br/>
          <div class="card" style="width: 10rem;">
            <img class="card-img-top" :src="cur_img" alt="Card image cap">
            <div class="card-body">
              <h4 class="card-title">{{cur_title}}</h4>
            </div>
          </div>
        </div>
      </div>
    </div>
  </span>
</template>

<script>
import * as d3 from 'd3'

export default {
  name: 'HelloWorld',
  data () {
    return {
      canvas: '',
      ctx: '',
      track_search: '',
      wiki_range: 5,
      fb_range: 5,
      aj_track: [
        {
          id: '1',
          name: 'Metalica',
          weki: [1, 3, 7],
          fb: [1, 2, 6],
          img: '/static/image/q0.png',
          range: 5
        },
        {
          id: '2',
          img: '/static/image/q2.png',
          name: 'Dream Theater',
          weki: [2, 4],
          fb: [3, 4, 7],
          range: 5
        },
        {
          id: '3',
          img: '/static/image/q3.png',
          name: 'Pantera',
          weki: [1, 4],
          fb: [1, 7, 9],
          range: 5
        },
        {
          id: '4',
          name: 'Prodigy',
          img: '/static/image/q4.png',
          weki: [3, 4],
          fb: [2, 4, 7],
          range: 5
        },
        {
          id: '5',
          name: 'U2',
          img: '/static/image/q1.png',
          range: 5
        },
        {
          id: '6',
          name: 'Queen',
          img: '/static/image/q2.png',
          range: 5
        },
        {
          id: '7',
          name: 'Sonata Artica',
          img: '/static/image/q3.png',
          range: 5
        },
        {
          id: '8',
          name: 'Amorphis',
          img: '/static/image/q4.png',
          range: 5
        },
        {
          id: '9',
          name: 'Ayreo',
          img: '/static/image/q1.png',
          range: 5
        },
        {
          id: '10',
          name: 'Celesta',
          img: '/static/image/q2.png',
          range: 5
        }
      ],
      cur_img: '/static/image/q1.png',
      cur_title: 'AJ Tracks',
      weki: [
        {
          id: '1',
          name: 'Musical Quartest',
          track: [1, 3, 4],
          recom: [1, 3, 4],
          range: 5
        },
        {
          id: '2',
          name: 'Alternative Rock',
          track: [2, 5, 6],
          recom: [2, 5, 6],
          range: 5
        },
        {
          id: '3',
          name: 'Eletra Records',
          range: 5
        },
        {
          id: '4',
          name: 'List Of Staturday Nigth',
          range: 5
        },
        {
          id: '5',
          name: 'Musical Quartest',
          range: 5
        },
        {
          id: '6',
          name: 'Progresive Metal',
          range: 5
        },
        {
          id: '7',
          name: 'Alteranative Record artist',
          range: 5
        },
        {
          id: '8',
          name: 'Nuclear Blast',
          range: 5
        },
        {
          id: '9',
          name: 'Ayreo',
          range: 5
        },
        {
          id: '10',
          name: 'Kehneman',
          range: 5
        }
      ],
      face: [
        {
          id: '1',
          name: 'Mohan Joshi',
          range: 5
        },
        {
          id: '2',
          name: 'Monika Patel',
          range: 5
        },
        {
          id: '3',
          name: 'Ronak Nair',
          range: 5
        },
        {
          id: '4',
          name: 'Mayank Purohit',
          range: 5
        },
        {
          id: '5',
          name: 'Monil Oza',
          range: 5
        },
        {
          id: '6',
          name: 'Path Patel',
          range: 5
        },
        {
          id: '7',
          name: 'Ronik Roy',
          range: 5
        },
        {
          id: '8',
          name: 'Alpesh Patel',
          range: 5
        },
        {
          id: '9',
          name: 'Narendra Modi',
          range: 5
        },
        {
          id: '10',
          name: 'Denial Kehneman',
          range: 5
        }
      ],
      recom: [
        {
          id: '1',
          name: 'Mouse',
          weki: [1, 3],
          fb: [2, 6]
        },
        {
          id: '2',
          name: 'Mono',
          weki: [1, 7],
          fb: [1, 6]
        },
        {
          id: '3',
          name: 'Red Hot Chilli',
          weki: [1, 3],
          fb: [1, 2]
        },
        {
          id: '4',
          name: 'Baties',
          weki: [3, 7],
          fb: [2, 6]
        },
        {
          id: '5',
          name: 'Bono',
          weki: [1, 3, 7],
          fb: [1, 2, 6]
        },
        {
          id: '6',
          name: 'Slip Knots',
          weki: [7],
          fb: [6]
        },
        {
          id: '7',
          name: 'Blind Guardian',
          weki: [1],
          fb: [6]
        },
        {
          id: '8',
          name: 'Cold Play',
          weki: [6, 7],
          fb: [1, 2, 6]
        },
        {
          id: '9',
          name: 'X- japan',
          weki: [5, 3, 7],
          fb: [4, 2, 6]
        },
        {
          id: '10',
          name: 'Lady Gaga',
          weki: [4, 3, 7],
          fb: [8, 2, 6]
        }
      ]
    }
  },
  mounted: function () {
    this.canvas = document.getElementById('canvas')
    this.ctx = this.canvas.getContext('2d')
    this.canvas.width = window.innerWidth
    this.canvas.height = window.innerHeight
    this.ctx.lineWidth = 1
  },
  methods: {
    track_click: function (id) {
      d3.selectAll('li').style('background', 'white')
      d3.select('#tra' + id).style('background', 'darkgray')
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
      for (var i in this.aj_track[id - 1].weki) {
        var claOfWekiEle = '#wek' + this.aj_track[id - 1].weki[i]
        d3.select(claOfWekiEle).style('background', 'darkcyan')
        var curEl = window.$('#tra' + id)
        var netEl = window.$(claOfWekiEle)
        this.ctx.beginPath()
        if (netEl.offset()) {
          this.ctx.moveTo(curEl.offset().left + curEl.width() + 40, curEl.offset().top + curEl.height() / 2)
          this.ctx.lineTo(netEl.offset().left, netEl.offset().top + netEl.height() / 2)
          this.ctx.stroke()
        }
      }
      for (var j in this.aj_track[id - 1].fb) {
        var classOfFbEle = '#fb' + this.aj_track[id - 1].fb[j]
        d3.select(classOfFbEle).style('background', 'lightsteelblue')
        curEl = window.$('#tra' + id)
        netEl = window.$(classOfFbEle)
        this.ctx.beginPath()
        if (netEl.offset()) {
          this.ctx.moveTo(curEl.offset().left + curEl.width() + 40, curEl.offset().top + curEl.height() / 2)
          this.ctx.lineTo(netEl.offset().left, netEl.offset().top + netEl.height() / 2)
          this.ctx.stroke()
        }
      }
      this.cur_img = this.aj_track[id - 1].img
      this.cur_title = this.aj_track[id - 1].name
    },
    tra_slid_change: function (id) {
      for (var i in this.aj_track[id - 1].weki) {
        console.log('sadf', this.weki[this.aj_track[id - 1].weki[i] - 1].range)
        this.weki[this.aj_track[id - 1].weki[i] - 1].range = this.aj_track[id - 1].range
      }
    },
    wiki_click: function (id) {
      d3.selectAll('li').style('background', 'white')
      d3.select('#wek' + id).style('background', 'darkgray')
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
      for (var i in this.weki[id - 1].track) {
        var claOfTraEle = '#tra' + this.weki[id - 1].track[i]
        d3.select(claOfTraEle).style('background', 'darkcyan')
        var curEl = window.$(claOfTraEle)
        var netEl = window.$('#wek' + id)
        this.ctx.beginPath()
        if (netEl.offset()) {
          this.ctx.moveTo(curEl.offset().left + curEl.width() + 40, curEl.offset().top + curEl.height() / 2)
          this.ctx.lineTo(netEl.offset().left, netEl.offset().top + netEl.height() / 2)
          this.ctx.stroke()
        }
      }
      for (var j in this.weki[id - 1].recom) {
        var claOfRecoEle = '#rec' + this.weki[id - 1].recom[j]
        d3.select(claOfRecoEle).style('background', 'lightsteelblue')
        curEl = window.$('#wek' + id)
        netEl = window.$(claOfRecoEle)
        this.ctx.beginPath()
        if (netEl.offset()) {
          this.ctx.moveTo(curEl.offset().left + curEl.width() + 40, curEl.offset().top + curEl.height() / 2)
          this.ctx.lineTo(netEl.offset().left, netEl.offset().top + netEl.height() / 2)
          this.ctx.stroke()
        }
      }
      this.cur_img = this.aj_track[id - 1].img
      this.cur_title = this.weki[id - 1].name
    },
    rec_click: function (id) {
      d3.selectAll('li').style('background', 'white')
      d3.select('#rec' + id).style('background', 'darkgray')
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
      for (var i in this.recom[id - 1].weki) {
        var claOfWekEle = '#wek' + this.recom[id - 1].weki[i]
        d3.select(claOfWekEle).style('background', 'darkcyan')
        var curEl = window.$(claOfWekEle)
        var netEl = window.$('#rec' + id)
        this.ctx.beginPath()
        if (curEl.offset()) {
          this.ctx.moveTo(curEl.offset().left + curEl.width() + 40, curEl.offset().top + curEl.height() / 2)
          this.ctx.lineTo(netEl.offset().left, netEl.offset().top + netEl.height() / 2)
          this.ctx.stroke()
        }
      }
      for (var j in this.recom[id - 1].fb) {
        var claOfFbEle = '#fb' + this.recom[id - 1].fb[j]
        d3.select(claOfFbEle).style('background', 'lightsteelblue')
        curEl = window.$(claOfFbEle)
        netEl = window.$('#rec' + id)
        this.ctx.beginPath()
        if (curEl.offset()) {
          this.ctx.moveTo(curEl.offset().left + curEl.width() + 40, curEl.offset().top + curEl.height() / 2)
          this.ctx.lineTo(netEl.offset().left, netEl.offset().top + netEl.height() / 2)
          this.ctx.stroke()
        }
      }
      this.cur_img = this.aj_track[id - 1].img
      this.cur_title = this.recom[id - 1].name
    }
  }
}
</script>
<style>
.scroll{
    /*overflow-y: scroll;*/
}
#canvas {
    position:absolute;
}
input[type=range]{
    -webkit-appearance: none;
    width: 100px;
}

input[type=range]::-webkit-slider-runnable-track {
    width: 300px;
    height: 5px;
    background: #ddd;
    border: none;
    border-radius: 3px;
}

input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none;
    border: none;
    height: 16px;
    width: 16px;
    border-radius: 50%;
    background: lightblue;
    margin-top: -4px;
}

input[type=range]:focus {
    outline: none;
}

input[type=range]:focus::-webkit-slider-runnable-track {
    background: #ccc;
}
</style>