<template>
  <div :id="chartId" :style="[{ 'width': '100%' }, { 'height': height + 'px' }]"></div>
</template>
<script>
import echartsUtil from '@/utils/echartsUtil'

export default {
  name: 'Ebar',
  props: {
    height: {
      type: Number,
      default: 300
    },
    theme: {
      type: String,
      default: 'light'
    },
    option: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      defaultOption: echartsUtil.defaultBarOption,
      myChart: {}
    }
  },
  computed: {
    chartId: function() {
      return 'Ebar-' + Math.random().toString(36).substr(2)
    }
  },
  methods: {
    resize() {
      const _this = this
      if (_this.myChart.resize) {
        _this.myChart.resize()
      }
    }
  },
  watch: {
    option: {
      handler(newVal, oldVal) {
        echartsUtil.watchOptionRefresh(this, newVal, oldVal)
      },
      deep: true // 对象内部属性的监听，关键。
    }
  },
  created() {
    this.$nextTick(() => {
      echartsUtil.drawChart(this)
    })
    window.addEventListener('resize', this.resize)
  },
  beforeDestroy: function() {
    window.removeEventListener('resize', this.resize)
  }
}
</script>
