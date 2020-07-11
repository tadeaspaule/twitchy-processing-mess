<template>
  <div id="outer">
    <div id="slidermenu">
        <h3>Play around with the sliders!</h3>
        <CustomSlider :title="'Number of vertices'"
            :sliderModel="n" @change="(val) => n = val"
            :min="2" :max="100"/>
        <CustomSlider :title="'Spread of initial values'"
            :sliderModel="spread" @change="(val) => spread = val"
            :min="0.1" :max="100"/>
        <CustomSlider :title="'Base radius'"
            :sliderModel="base" @change="(val) => base = val"
            :min="0" :max="400"/>
        <CustomSlider :title="'Fluctuating radius'"
            :sliderModel="h" @change="(val) => h = val"
            :min="0" :max="400"/>
        <!-- <CustomSlider :title="'Fluctuation speed'"
            :sliderModel="grow" @change="(val) => grow = val"
            :min="0" :max="1"/> -->
    </div>
    <div id="canvas-container" />
    <div id="infomenu">

    </div>
  </div>
</template>

<script>
import P5 from 'p5'
import CustomSlider from 'components/CustomSlider.vue'
export default {
    name: 'PageIndex',
    components: {
        CustomSlider
    },
    data () {
        return {
            n: 5,
            spread: 6.0,
            base: 200,
            h: 100,
            grow: 0.2
        }
    },
    methods: {
        buildCanvas () {
            // wipe any previous canvas that might've been there
            const container = document.getElementById('canvas-container')
            while (container.firstChild) {
                container.removeChild(container.lastChild)
            }
            // instantiate the new one
            new P5(this.currentSketch)
        }
    },
    computed: {
        currentSketch () {
            var n = this.n
            var spread = this.spread
            var base = this.base
            var h = this.h
            var grow = this.grow
            return function (p5) {
                var points

                p5.setup = _ => {
                    var canvas = p5.createCanvas(600, 600)
                    canvas.parent('canvas-container')
                    p5.background(0)
                    p5.noFill()
                    p5.stroke(255)
                    p5.strokeWeight(5)
                    p5.frameRate(30)
                    points = []
                    for (var i = 0; i < n; i++) points.push((spread / n) * i)
                }

                p5.vertexAtIndex = (i) => {
                    var rads = (i * Math.PI * 2) / n
                    var r = base + p5.sin(points[i]) * h
                    var x = p5.width / 2 + r * p5.cos(rads)
                    var y = p5.height / 2 + r * p5.sin(rads)
                    p5.vertex(x, y)
                }

                p5.draw = _ => {
                    p5.background(0)
                    p5.beginShape()
                    for (var i = 0; i < n; i++) {
                        points[i] = (points[i] + grow) % (Math.PI * 2)
                        p5.vertexAtIndex(i)
                    }
                    p5.vertexAtIndex(0)
                    p5.endShape()
                }
            }
        }
    },
    watch: {
        currentSketch: {
            deep: true,
            handler: function (val) {
                this.buildCanvas()
            }
        }
    },
    mounted () {
        this.buildCanvas()
    }
}
</script>
<style>
* {
    font-family: 'Roboto Mono', monospace;
}
#outer {
    display: flex;
    flex-direction: row;
    justify-content: center;
    padding: 10px;
}
#slidermenu {
    flex: 1 1 100px;
    padding: 10px;
}
#slidermenu h3 {
    margin: 0;
    padding: 0;
    padding-bottom: 20px;
}
#canvas-container {
    flex: 0 0 650px;
    height: 650px;
    background: black;
    margin: 0px 10px;
}
#infomenu {
    flex: 1 1 100px;
}
</style>
