<template>
    <div class="custom-slider">
        <div class="slider-portion">
            <h4 class="slider-title">{{title}}</h4>
            <h4>{{model}}</h4>
        </div>
        <q-slider v-model="model" :min="min" :max="max" @change="(val) => $emit('change', val)"/>
    </div>
</template>

<script >
export default {
    name: 'CustomSlider',
    props: {
        title: String,
        description: String,
        sliderModel: Number,
        min: Number,
        max: Number
    },
    data () {
        return {
            model: 0
        }
    },
    mounted () {
        this.model = this.sliderModel
    }
}
</script>
<style scoped>
.custom-slider {
    display: flex;
    flex-direction: column;
}
.slider-portion {
    display: flex;
    flex-direction: row;
}
.slider-title {
    flex: 1 1 30px;
}
.custom-slider h4 {
    margin: 0;
    padding: 0;
    font-size: 18px;
    line-height: 20px;
}
.slider-val .slider-val {
    margin-left: 10px;
}
</style>
