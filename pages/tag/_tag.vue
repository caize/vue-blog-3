<template>
  <div class="tag">

    <div class="head font-futura" :class="{'mobile': mobileLayout}">
      <div>
        <p><i class="iconfont icon-tag"></i></p>
        <p> {{ tagName }} </p>
      </div>
    </div>

    <div class="article">
      <articleView
        :articleList = "list"
        :haveMoreArt="haveMoreArt"
        @loadMore="loadMore"></articleView>
    </div>
  </div>
</template>
<script>

import articleView from '~components/common/article.vue'

export default {

  name: 'tag',

  transition: 'slide-down',

  scrollToTop: true,

  fetch ({ store, params }) {
    return store.dispatch('getArtList', params)
  },

  head () {
    return { title: `${this.tagName} | tag` }
  },

  data () {
    return {}
  },

  components: {
    articleView
  },

  computed: {
    mobileLayout () {
      return this.$store.state.options.mobileLayout
    },

    tagName () {
      const _id = this.$route.params.tag
      return this.$store.state.tag.data.list.find(item => item._id === _id).name
    },

    list () {
      return this.$store.state.article.art.list
    },

    haveMoreArt () {
      return this.$store.state.article.art.pagination.current_page
              !== this.$store.state.article.art.pagination.total_page
    }
  },

  methods: {
    loadMore () {
      this.$store.dispatch('getArtList', {
        current_page: this.$store.state.article.art.pagination.current_page + 1,
        tag: this.$route.params.tag
      })
    }
  }
}
</script>

<style scoped lang="scss">

@import '~assets/scss/variable.scss';
@import '~assets/scss/mixin.scss';

.head {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 48.5rem;
  height: 20rem;
  margin: 0 auto $normal-pad auto;
  font-size: 3rem;
  color: $black;
  background: $module-bg;
  text-align: center;

  P {
    line-height: 3.5rem;
  }

  i {
    font-size: 3.5rem;    
  }
}

.head.mobile {
  height: 10rem;
  font-size: 2rem;
  width: 100%;

  p {
    line-height: 2.5rem;
  }

  i {
    font-size: 2.5rem;
  }
}
</style>
