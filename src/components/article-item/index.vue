<template>
  <!-- cell 中的to 属性和 VueRouter 中的RouterLink 导航组件的to 属性用法一致 -->
  <!--路由拼接  :to="'/article/'+ article.art_id" -->
  <!-- ES6写法  :to="`/article/${article.art_id}` -->
  <van-cell class="article-item" :to="{
              // 根据路由名称进行跳转
              name:'article',
              // 传递路由动态参数
              params:{
              // 属性名：路由路径中设计的动态参数名称
                articleId: article.art_id
              }
            }">
    <div slot="title" class="title van-multi-ellipsis--l2">
      {{ article.title }}
    </div>
    <div slot="label">
      <div v-if="article.cover.type === 3"
           class="cover-wrap">
        <div class="cover-item"
             v-for="(img, index) in article.cover.images"
             :key="index">
          <van-image class="cover-item-img" fit="cover"
                     :src="img" />
        </div>
      </div>
      <div class="label-info-wrap">
        <span>{{ article.aut_name }}</span>
        <span>{{ article.comm_count }}评论</span>
        <span>{{ article.pubdate |relativeTime }}</span>
      </div>
    </div>
    <van-image v-if="article.cover.type === 1"
               slot="default" class="right-cover"
               fit="cover" :src="article.cover.images[0]" />
  </van-cell>
</template>

<script>
export default {
  name: 'ArticleItem',
  components: {},
  props: {
    article: {
      type: Object,
      required: true
    }
  },
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () { },
  mounted () { },
  methods: {}
}
</script>

<style scoped lang="less">
.article-item {
  .title {
    font-size: 32px;
    color: #3a3a3a;
  }
  .van-cell__value {
    flex: unset;
    width: 232px;
    height: 146px;
    padding-left: 25px;
  }

  .right-cover {
    width: 100%;
    height: 146px;
  }

  .label-info-wrap span {
    font-size: 22px;
    color: #b4b4b4;
    margin-right: 25px;
  }
  .cover-wrap {
    display: flex;
    padding: 30px 0;
    .cover-item {
      flex: 1;
      height: 146px;
      &:not(:last-child) {
        // 不包括最后一个
        padding-right: 4px;
      }
      .cover-item-img {
        width: 100%;
        height: 146px;
      }
    }
  }
}
</style>
