<template>
  <div class="search-result">
    <van-list v-model="loading" :finished="finished"
              finished-text="没有更多了" @load="onLoad"
              :error.sync="error" error-text="加载失败，请点击重试">
      <van-cell v-for="(article,index) in list" :key="index"
                :title="article.title" />
    </van-list>
  </div>
</template>

<script>
import { getSearchResult } from '@/api/search'
export default {
  name: 'SearchResult',
  components: {},
  props: {
    searchText: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      list: [],
      loading: false,
      finished: false,
      page: 1,
      perPage: 20,
      error: false
    }
  },
  computed: {},
  watch: {},
  created () { },
  mounted () { },
  methods: {
    async onLoad () {
      try {
        // 1.请求获取数据
        const { data } = await getSearchResult({
          page: this.page, // 页码
          per_page: this.perPage, // 每页大小
          q: this.searchText // 查询关键词
        })
        // 模拟随机失败
        if (Math.random() > 0.5) {
          JSON.parse('dsnajndjsa')
        }
        // 2.将数据添加到数组列表中
        // this.list = data  错误，不能直接赋值
        const { results } = data.data
        this.list.push(...results)
        // 3.将本次加载的loading 关闭
        this.loading = false
        // 4.判断是否还有数据
        if (results.length) {
          //   如果有，则更新获取下一页数据的页码
          this.page++
        } else {
          //   如果没有，则将加载状态 finished 设置为结束
          this.finished = true
        }
      } catch (err) {
        // 展示加载失败的提示状态
        this.error = true
        // 加载失败了 loading 也要关闭
        this.loading = false
      }
    }
  }
}
</script>

<style lang='less' scoped>
</style>
