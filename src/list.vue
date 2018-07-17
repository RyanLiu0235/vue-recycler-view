<template>
  <div class="wrapper">
    <div class="container" ref="container" @scroll="scrollHandler">
      <div class="list"
      :style="{
        height: (items.length + 1) * 40 + 2 + 'px'
      }"
      ref="list">
        <Item class="item"
        v-for="(item, index) in available"
        :key="item.index"
        :index="item.index"
        :content="item.content"
        :style="{
          top: item.index * 40 + 'px'
        }" />
        <div class="item" @click="loadMoreHandler" :style="{
          top: items.length * 40 + 'px'
        }">load more</div>
      </div>
    </div>
    <div class="dashboard">
      <p>总共数量：{{items.length}}个</p>
      <p>当前渲染：{{visibleRange}}</p>
      <p>预先加载：{{overscan}}个</p>
    </div>
  </div>
</template>
<script>
import Item from "./item"
export default {
  name: "list",
  data() {
    return {
      items: this.generateItems(0),
      available: [],
      page: 0,
      overscan: 2
    }
  },
  mounted() {
    this.setItem()
  },
  methods: {
    scrollHandler() {
      this.setItem()
    },
    loadMoreHandler() {
      this.fetch().then(items => {
        this.items = items
        this.setItem()
      })
    },
    fetch() {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve([...this.items, ...this.generateItems(++this.page)])
        }, 1000)
      })
    },
    generateItems(page) {
      const ps = 50
      return Array(ps)
        .fill("grid")
        .map((item, index) => ({
          content: `${item} - ${index + page * ps + 1}`,
          index: index + page * ps
        }))
    },
    setItem() {
      const containerHeight = 400
      const rowHeight = 40

      const { list, container } = this.$refs

      const { top: containerTop } = container.getBoundingClientRect()
      const { top: listTop } = list.getBoundingClientRect()
      const count = Math.floor(Math.abs(listTop - containerTop) / rowHeight)
      const overscan = this.overscan
      this.available = this.items.slice(
        count >= overscan ? count - overscan : count,
        this.items.length >= count + 10 + overscan
          ? count + 10 + overscan
          : count + 10
      )
    }
  },
  computed: {
    visibleRange() {
      const available = this.available
      const length = available.length
      if (length === 0) return "无"
      return `${available[0].index + 1} - ${available[length - 1].index + 1}`
    }
  },
  components: { Item }
}
</script>
<style type="text/css">
.container {
  height: 400px;
  margin: 20px 40px 0;
  overflow-y: scroll;
  border: 1px #d0d0d0 solid;
  border-radius: 4px;
}

.list {
  position: relative;
}

.item {
  position: absolute;
  display: block;
  width: 100%;
  height: 40px;
  line-height: 40px;
  border-bottom: 1px #efefef solid;
  box-sizing: border-box;
}

.item:last-child {
  border-bottom: none;
}

.dashboard {
  margin-top: 20px;
}
</style>
