<template>
  <div>
    <Selector :options="options" />
  </div>
</template>

<script>
import Selector from './selector.vue'
export default {
  components: {
    Selector
  },
  data: () => ({
    options: [{
      value: '黄金糕',
      label: '黄金糕'
    }, {
      value: '双皮奶',
      label: '双皮奶'
    }, {
      value: '蚵仔煎',
      label: '蚵仔煎'
    }, {
      value: '龙须面',
      label: '龙须面'
    }, {
      value: '北京烤鸭',
      label: '北京烤鸭'
    }]
  })
}
</script>
