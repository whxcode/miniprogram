<template>
  <div class="home">
    <KSwiper
      :indicator-dots="indicatorDots"
      :circular="circularFlag"
      :autoplay="autoplay"
      :duration="duration"
      :interval="interval"
    >
      <KSwiperItem v-for="(item,index) in background" :key="index">
        <KView :class="'swiper-item '+ item">
          <img :src="bannerImg" class="banner-item" mode='widthFix'/>
        </KView>
      </KSwiperItem>
    </KSwiper>
    <Tabs />

    <!-- <tabs></tabs>
    <wx-button open-type="share" @click="onClickShare">分享</wx-button>-->
  </div>
</template>

<script>
import Vue from 'vue'
import Tabs from './Tabs/Index.vue'
import Web from 'reduce-loader!../common/Web.vue'
import 'reduce-loader!./web'

export default Vue.extend({
  name: 'Home',
  components: {
    Web,
    Tabs
  },

  data() {
    return {
      interval: 2380,
      autoplay: false,
      indicatorDots: false,
      circularFlag: true,
      duration: 1200,
      background: [
        'https://wechat-1251018873.file.myqcloud.com/images/banner.png',
        'https://wechat-1251018873.file.myqcloud.com/images/banner.png',
        'https://wechat-1251018873.file.myqcloud.com/images/banner.png'
      ],
      current: 1,
      navList: [
        { label: '最新', key: 'newest' },
        { label: '专栏', key: 'column' },
        { label: '最新', key: 'pay' }
      ],
      bannerImg: 'https://wechat-1251018873.file.myqcloud.com/images/banner.png',
    }
  },
  created() {
    window.addEventListener('wxload', query =>
      console.log('page1 wxload', query)
    )
    window.addEventListener('wxshow', () => console.log('page1 wxshow'))
    window.addEventListener('wxready', () => console.log('page1 wxready'))
    window.addEventListener('wxhide', () => console.log('page1 wxhide'))
    window.addEventListener('wxunload', () => console.log('page1 wxunload'))

    if (process.env.isMiniprogram) {
      console.log('I am in miniprogram')
      // For Dean Test Passport.
      // Can delete if needed.
      wx.login({
        success(res) {
          if (res.code) {
            wx.request({
              url: 'https://mina.baobeihuijia.com/apis/oauth/v1/wechat/login',
              data: {
                code: res.code
              }
            })
          } else {
            console.log('Login failed')
          }
        }
      })
    } else {
      console.log('I am in Web')
    }
  },
  methods: {
    onClickJump() {
      window.location.href = '/test/list/123'
    },

    onClickOpen() {
      window.open('/test/detail/123')
    },

    onClickShare() {
      console.log('I am in miniprogram')
    },
    navChange(val) {
      try {
        this.current = val.$_detail.current
      } catch (error) {
        console.log(error)
      }
    }
  }
})
</script>

<style lang="less">

.miniprogram-root {
  .for-web {
    display: none;
  }
}
    .swiper {
        margin-top: 15px;
        width: 100%;
        height: 185px;
    }
    .banner-item {
        display: block;
        margin: auto;
        height: 160px;
        border-radius: 20px;
    }
</style>
