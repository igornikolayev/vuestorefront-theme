<template>
  <section class="section section-prefooter">
    <div class="container">
      <div class="main-footer">
        <no-ssr>
          <carousel :perPage=5 style="width:100%" :navigationEnabled="false" :paginationEnabled="false" :perPageCustom="[[168, 1],[768, 2], [1068, 3], [1268, 4], [1360, 5]]">
            <slide v-for="(item, index) in images" :key="index">
              <router-link :to=item.link>
                <img :src=item.src :alt=item.alt>
              </router-link>
            </slide>
          </carousel>
        </no-ssr>
      </div>
    </div>
  </section>
</template>

<script>

import NoSSR from 'vue-no-ssr'

export default {
  components: {
    'Carousel': () => import('vue-carousel').then(Slider => Slider.Carousel),
    'Slide': () => import('vue-carousel').then(Slider => Slider.Slide),
    'no-ssr': NoSSR
  },
  data () {
    return {

      currentSlide: 1,
      slides: [],
      totalSlides: 1,

      images:[
        {
          link: "/as",
          alt: "motorola",
          src:"/assets/test_img/motorola.png"
        },
        {
          link: "/",
          alt: "alofeng",
          src:"/assets/test_img/alofeng.png"
        },
        {
          link: "/",
          alt: "icom",
          src:"/assets/test_img/icom.png"
        },
        {
          link: "/",
          alt: "kenwood",
          src:"/assets/test_img/kenwood.png"
        },{
          link: "/",
          alt: "fletec",
          src:"/assets/test_img/fletec.png"
        }
      ]
    }
  }
}
</script>
