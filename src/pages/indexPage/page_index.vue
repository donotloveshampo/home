<template lang="pug">
	.warp
		DetailModal(v-show='isShow' v-on:child-close='c_close' :start_list="start_list")
		.page-thumes
			ul
				li.point(:class="{active : this.currentIndex == 0}")
				li.point(:class="{active : this.currentIndex == 1}")
				li.point(:class="{active : this.currentIndex == 2}")
				li.point(:class="{active : this.currentIndex == 3}")
		div.index(:style="indexStyle")
			IndexSwiper.index-section(:style="sectionStyle" :swiperList='swiperList')
			FlagshipSpecies.index-section(:style="sectionStyle" :start_list = 'start_list' 
			:bgImage = 'bgImage' v-on:child-open='c_open')
			LatestNews.index-section(:style="sectionStyle" :news_data='news_data' :totalPages='Math.ceil(news_data.total / 2)' :current-page='currentPage' @pagechanged='handleChange')
			PartNers.index-section(:style="sectionStyle" :partners_data='partners_data')
			
		
</template>
<script>
import FlagshipSpecies from './indexcomponents/flagship_species.vue'
import LatestNews from './indexcomponents/latest_news.vue'
import PartNers from './indexcomponents/partners.vue'
import IndexSwiper from './indexcomponents/index_swiper.vue'
import DetailModal from './../components/detailModal.vue'
import { clearTimeout, setTimeout } from 'timers'

import * as API from '../../../api'
export default {
  components: {
    IndexSwiper,
    FlagshipSpecies,
    LatestNews,
    PartNers,
	DetailModal
  },

  computed: {
    getViewPortHeight() {
      return document.documentElement.clientHeight || document.body.clientHeight
    },

    indexStyle() {
      return {
        transform: `translateY(-${this.currentIndex *
          this.getViewPortHeight}px)`
      }
    },

    sectionStyle() {
      return {
        height:
          document.documentElement.clientHeight + 'px' ||
          document.body.clientHeight + 'px',
        width:
          document.documentElement.clientWidth + 'px' ||
          document.body.clientWidth + 'px'
      }
    }
  },

  data() {
    return {
      scrollTime: 0,
      scrollDaly: 500,
      partners_data: '',
      start_list: [],
      currentIndex: 0,
      news_data: {},
      currentPage: 1,
      bgImage: '',
	    isShow:'',
      swiperList:[]
    }
  },
  methods: {
	c_close(d){
		this.isShow = d;
	},
	c_open(d){
	    this.isShow = d;
	},
    getScrollDelta(e) {
      e = e || window.event
      if (e.wheelDelta) {
        return e.wheelDelta > 0
      } else {
        return e.detail > 0
      }
    },

    handleScroll(e) {
      let now = +new Date()
      if (now - this.scrollTime > this.scrollDaly) {
        this.scrollTime = now
        this.getScrollDelta(e)
          ? this.currentIndex == 0
            ? (this.currentIndex = 0)
            : this.currentIndex--
          : this.currentIndex == 3
            ? (this.currentIndex = 3)
            : this.currentIndex++
      } else {
        return false
      }
    },
    handleChange: function(page) {
      this.currentPage = page
    }
  },
  mounted() {
    console.log(this.getViewPortHeight)
    window.addEventListener('mousewheel', this.handleScroll, false)
    //轮播图请求
    this.$_get(API.ROLLIMAGES).then(
      res => {
        this.swiperList = res.data
      }
    ).catch(err => console.log(err))
    //明星物种请求
    this.$_get(API.START_DATA)
      .then(res => {
        this.start_list = res.data[0]
        var len = this.start_list.imgUrl.length
        this.bgImage = this.start_list.imgUrl.slice(2, len - 2)
      })
      .catch(err => console.log(err))
    //合作伙伴请求
    this.$_get(API.PARTNERS_DATA)
      .then(res => {
        this.partners_data = res.data
      })
      .catch(err => {
        console.log(err)
      })
    //新闻列表请求
    this.$_get(API.TEST_DATA)
      .then(res => {
        this.news_data = res.data
      })
      .catch(err => console.log(err))
  }
}
</script>

<style lang="scss" scoped>
.warp {
  .page-thumes {
    z-index: 200;
    position: fixed;
    right: 1%;
    top: 40%;
    ul {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 100px;
      width: 10px;
      .point {
        width: 5px;
        height: 5px;
        background-color: #c5cbce;
        border-radius: 5px;
      }
      .point.active {
        box-shadow: 0px 0px 2px 4px #8b9695;
      }
    }
  }
  .index {
    overflow: hidden;
    transition: all 500ms cubic-bezier(0.25, 0.46, 0.45, 0.94);
  }
}
</style>