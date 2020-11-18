<template>
  <div id="EditableMultipleSelector" @click="showSelector($event)">
    <template v-for="(r, i) in results">
      <span :key="i" class="selector-re-cont">
        <Tag
          class="selector-tag"
          closable
          type="info"
          v-show="!r.input"
          :disable-transitions="true"
          @close="closeTag(i)"
          @click="showInput(i)"
        >
          {{r.value}}
        </Tag>
        <Input
          v-if="r.input"
          v-model="r.value"
          class="selector-input"
          @blur="blurInput(i)"
        />
      </span>
    </template>
    <Select
      ref="selector"
      class="selector-select"
      v-model="value"
      placeholder="请选择"
      @change="onChange"
    >
      <Option
        v-for="item in cacheOptions"
        :key="item.value"
        :label="item.label"
        :value="item.value">
      </Option>
    </Select>
  </div>
</template>

<script>
import Select from 'element-ui/lib/select'
import Option from 'element-ui/lib/option'
import Tag from 'element-ui/lib/tag'
import Input from 'element-ui/lib/input'
import 'element-ui/lib/theme-chalk/fonts/element-icons.ttf'
import 'element-ui/lib/theme-chalk/fonts/element-icons.woff'
import 'element-ui/lib/theme-chalk/select.css'
import 'element-ui/lib/theme-chalk/option.css'
import 'element-ui/lib/theme-chalk/tag.css'
import 'element-ui/lib/theme-chalk/input.css'
export default {
  components: {
    Select,
    Option,
    Tag,
    Input,
  },
  props: {
    options: {
      type: Array,
      default: function () {
        return []
      }
    },
  },
  watch: {
    options: {
      deep: true,
      immediate: true,
      handler: function (value) {
        console.log(value)
        this.cacheOptions = JSON.parse(JSON.stringify(value))
        this.cloneOptions = JSON.parse(JSON.stringify(value))
      }
    }
  },
  data: () => ({
    value: '',
    results: [],
    cacheOptions: [],
    cloneOptions: [],
  }),
  methods: {
    onChange (value) {
      this.cacheOptions.forEach(o => {
        if (o.value === value) {
          this.results.push({ ...o, ...{ input: false, } })
        }
      })
      this.cacheOptions.splice(this.cacheOptions.findIndex(o => o.value === value), 1)
    },
    showSelector (e) {
      console.log(e)
      const className = e.target.className
      if (className.includes('el-tag') || className.includes('el-input')) {
        return
      }
      this.$refs.selector.visible = true;
    },
    closeTag (i) {
      const result = this.results.splice(Number(i), 1)
      this.cloneOptions.forEach(co => {
        if (co.label === result[0].label) {
          this.cacheOptions.push(co)
        }
      })
    },
    showInput (i) {
      console.log(i)
      this.results.forEach((r, k) => {
        if (i === k) {
          r.input = true
        } else {
          r.input = false
        }
      })
    },
    blurInput (i) {
      this.results.forEach((r, k) => {
        if (i === k) {
          r.input = false
        }
      })
    }
  }
}
</script>

<style scoped>
#EditableMultipleSelector {
  z-index: 3;
  position: relative;
  min-height: 80px;
  max-width: 300px;
  border: 1px solid #DCDFE6;
  border-radius: 4px;
  background-color: #fff;
  padding: 4px;
}
.selector-select {
  visibility: hidden;
  z-index: 1;
  position: absolute;
  right: 0;
  bottom: 0;
}
.selector-tag {
  margin: 0 6px 6px 0;
  cursor: pointer;
}
.selector-re-cont {
  position: relatives;
}
.selector-input {
  z-index: 2;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
}
</style>
