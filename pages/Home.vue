<template>
  <div id="home">
    <section class="section section__main main">
      <div class="container">
        <div class="main__box">
          <h1>Bedankt voor uw bestelling bij PortofoonWEB!</h1>
          <div class="main__part">
            <h3 class="main__title">
              Goed om te weten
            </h3>
            <p class="main__text">Bent u tevreden over het bestelproces like dan onze Facebook pagina of laat een review achter, zo maakt u ook nog eens kans op een leuke cadeau bon ter waarde van € 100,- Wilt u graag op de hoogte blijven van het laatste nieuws en speciale aanbiedingen meld u zich dan aan op onze nieuwsbrief.</p>
          </div>
          <div class="main__part">
            <h3 class="main__title">
              Wat gaat er nu gebeuren?
            </h3>
            <p class="main__text">Per email heeft u een bevestiging ontvangen van uw bestelling met Ordernummer [ordernummer]. Heeft u de bestelling op een werkdag voor 17:00 gedaan en betaald, dan gaan wij nu direct aan de slag om te zorgen dat u het morgen in huis heeft. Heeft u vragen over de bestelling neem dan contact met ons op 0512 – 745293 of stuur ons een email aan info@portofoonweb.nl. </p>
          </div>
          <div class="main__part">
            <h3 class="main__title">
              Wist u dat?
            </h3>
            <p class="main__text">PortofoonWEB heeft sinds kort een fenomenaal nieuwe techniek genaamd Smart Portofoons. Een portofoon systeem met ongekende mogelijkheden en onbeperkt bereik! Klik op deze link voor meer informatie. </p>
          </div>
          <div class="main__links">
            <a href="#">
              <img src="/assets/test_img/Smartportofoon_Logo.png" alt="smart_logo">
            </a>
            <a href="#"><img src="/assets/test_img/facebook.png" alt="facebook"></a>
          </div>
          <div class="hero">
            <img src="/assets/test_img/phone_smile_center.png" alt="phone-main">
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// query constructor
import { isServer, onlineHelper } from '@vue-storefront/core/helpers'
import LazyHydrate from 'vue-lazy-hydration'

// Core pages
import Home from '@vue-storefront/core/pages/Home'
// Theme core components
import ProductListing from 'theme/components/core/ProductListing'
import HeadImage from 'theme/components/core/blocks/MainSlider/HeadImage'
// Theme local components
import Onboard from 'theme/components/theme/blocks/Home/Onboard'
import PromotedOffers from 'theme/components/theme/blocks/PromotedOffers/PromotedOffers'
import TileLinks from 'theme/components/theme/blocks/TileLinks/TileLinks'
import {Logger} from '@vue-storefront/core/lib/logger'
import {mapGetters} from 'vuex'
import config from 'config'
import {registerModule} from '@vue-storefront/core/lib/modules'
import {RecentlyViewedModule} from '@vue-storefront/core/modules/recently-viewed'

export default {
  data () {
    return {
      loading: true
    }
  },
  mixins: [Home],
  components: {
    HeadImage,
    Onboard,
    ProductListing,
    PromotedOffers,
    TileLinks,
    LazyHydrate
  },
  computed: {
    ...mapGetters('user', ['isLoggedIn']),
    ...mapGetters('homepage', ['getEverythingNewCollection']),
    categories () {
      return this.getCategories
    },
    isOnline () {
      return onlineHelper.isOnline
    },
    isLazyHydrateEnabled () {
      return config.ssr.lazyHydrateFor.some(
        field => ['homepage', 'homepage.new_collection'].includes(field)
      )
    }
  },
  beforeCreate () {
    registerModule(RecentlyViewedModule)
  },
  async beforeMount () {
    if (this.$store.state.__DEMO_MODE__) {
      const onboardingClaim = await this.$store.dispatch('claims/check', { claimCode: 'onboardingAccepted' })
      if (!onboardingClaim) { // show onboarding info
        this.$bus.$emit('modal-toggle', 'modal-onboard')
        this.$store.dispatch('claims/set', { claimCode: 'onboardingAccepted', value: true })
      }
    }
  },
  mounted () {
    if (!this.isLoggedIn && localStorage.getItem('redirect')) this.$bus.$emit('modal-show', 'modal-signup')
  },
  watch: {
    isLoggedIn () {
      const redirectObj = localStorage.getItem('redirect')
      if (redirectObj) this.$router.push(redirectObj)
      localStorage.removeItem('redirect')
    }
  },
  async asyncData ({ store, route }) { // this is for SSR purposes to prefetch data
    Logger.info('Calling asyncData in Home (theme)')()

    await Promise.all([
      store.dispatch('homepage/fetchNewCollection'),
      store.dispatch('promoted/updateHeadImage'),
      store.dispatch('promoted/updatePromotedOffers')
    ])
  },
  beforeRouteEnter (to, from, next) {
    if (!isServer && !from.name) { // Loading products to cache on SSR render
      next(vm =>
        vm.$store.dispatch('homepage/fetchNewCollection').then(res => {
          vm.loading = false
        })
      )
    } else {
      next()
    }
  }
}
</script>

<style lang="scss" scoped>
  *{
    font-family: 'Roboto', sans-serif;
  }
</style>
