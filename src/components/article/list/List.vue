<template>
  <div class="articles">

    <!-- 列表头 -->
    <div class="article-list-header" v-if="['tag', 'date', 'category'].includes($route.name)">
      <list-header></list-header>
    </div>

    <!-- 列表 -->
    <div class="article-list">
      <loading class="article-loading" v-if="articles.fetching" />
      <transition-group name="fade" tag="div">
        <list-item v-for="(item, index) in articles.data.data" :item="item" :key="index"></list-item>
      </transition-group>
    </div>

    <!-- 加载更多 -->
    <div class="article-load">
      <button class="btn-loadmore" @click="$emit('loadmore')" :disabled="articles.fetching || !canLoadMore">
        <span v-if="!articles.fetching && canLoadMore">加载更多</span>
        <span v-if="articles.fetching && canLoadMore">加载中</span>
        <span v-if="!canLoadMore">没有更多</span>
      </button>
    </div>
  </div>
</template>

<script>

  // import
  import ListItem from './Item.vue'
  import ListHeader from './Header.vue'

  // export
  export default {
    name: 'Article-List',
    components: {
      ListItem,
      ListHeader
    },
    props: {
      articles: {
        type: Object,
        default: {}
      }
    },
    computed: {
      canLoadMore() {
        const hasArticles = this.articles && this.articles.data && this.articles.data.pagination
        return hasArticles ? this.articles.data.pagination.current_page < this.articles.data.pagination.total_page : false
      }
    },
  }
</script>

<style lang="scss" scoped>
  @import '../../../sass/variables';
  .articles {

    > .article-list-header {
      margin-bottom: 1em;
      position: relative;
      overflow: hidden;
      background-color: $module-bg;
    }

    > .article-list {
      margin-bottom: 1em;
      min-height: 1em;
      overflow: hidden;

      > .article-loading {
          height: 10em;
          background-color: $module-bg;
          display: flex;
      }
    }

    > .article-load {

      > .btn-loadmore {
        width: 100%;
        display: block;
        height: 3em;
        line-height: 3em;
        background-color: $module-bg;

        &:hover {
          background-color: $module-hover-bg;
        }
      }
    }
  }
</style>
