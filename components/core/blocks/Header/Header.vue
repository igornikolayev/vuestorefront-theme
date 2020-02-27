<template>
  <div class="header">
    <header
      class="fixed w-100 brdr-bottom-1 bg-cl-primary brdr-cl-secondary"
      :class="{ 'is-visible': navVisible }"
    >
   <!--   <div class="container px15">
        <div class="row between-xs middle-xs" v-if="!isCheckoutPage || isThankYouPage">
          <div class="col-md-4 col-xs-2 middle-xs">
            <div>
              <hamburger-icon class="p15 icon bg-cl-secondary pointer" />
            </div>
          </div>
          <div class="col-xs-2 visible-xs">
            <search-icon class="p15 icon pointer" />
          </div>
          <div class="col-md-4 col-xs-4 center-xs pt5">
            <div>
              <logo width="auto" height="41px" />
            </div>
          </div>
          <div class="col-xs-2 visible-xs">
            <wishlist-icon class="p15 icon pointer" />
          </div>
          <div class="col-md-4 col-xs-2 end-xs">
            <div class="inline-flex right-icons">
              <search-icon class="p15  hidden-xs pointer" />

              <account-icon class="p15  hidden-xs pointer" />
              <microcart-icon class="p15  pointer" />
            </div>
          </div>
        </div>
        <div class="row between-xs middle-xs px15 py5" v-if="isCheckoutPage && !isThankYouPage">
          <div class="col-xs-5 col-md-3 middle-xs">
            <div>
              <router-link
                :to="localizedRoute('/')"
                class="cl-tertiary links"
              >
                {{ $t('Return to shopping') }}
              </router-link>
            </div>
          </div>
          <div class="col-xs-2 col-md-6 center-xs">
            <logo width="auto" height="41px" />
          </div>
          <div class="col-xs-5 col-md-3 end-xs">
            <div>
              <a
                v-if="!currentUser"
                href="#"
                @click.prevent="gotoAccount"
                class="cl-tertiary links"
              >{{ $t('Login to your account') }}</a>
              <span v-else>{{ $t('You are logged in as {firstname}', currentUser) }}</span>
            </div>
          </div>
        </div>
      </div>
-->
    <section class="section section__top-header">
      <div class="container">
        <div class="top-header">
          <div>
            <hamburger-icon class="bg-cl-secondary pointer hamburger-icon" />
          </div>
          <div class="right-side">
            <search-icon class="pointer search-icon" />
            <account-icon class="pointer" />
            <div class="box">
              <span>Winkelwagen</span>
              <microcart-icon class="pointer" />
            </div>
          </div>
        </div>
      </div>
    </section>
    <section class="section section__middle-header">
      <div class="container">
        <div class="middle-header">
          <div>
            <logo width="auto" height="41px" />
          </div>
          <div class="search">
            <img src="/assets/test_img/search.png" alt="search icon">
            <input type="text" placeholder="Zoek in het assortiment "  @focus="toggleSearchpanel" data-testid="openSearchPanel">
            <button  @click="toggleSearchpanel" data-testid="openSearchPanel" class="button button_search">
              Zoeken
            </button>
          </div>
          <div class="phone">
            <div class="phone__image">
              <img src="/assets/test_img/phone.png" alt="phone">
            </div>
            <div class="phone__text">
              <p class="phone__text__top">
                Heeft u vragen? <span>Bel 0512-745293</span>
              </p>
              <p class="phone__text__bottom">
                Bereikbaar maandag t/m vrijdag van: 09:00 – 17:00
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <Navigation class="navigation-block"></Navigation>
    </header>
    <div class="header-placeholder" />

  </div>
</template>

<script>
import { mapState } from 'vuex'
import CurrentPage from 'theme/mixins/currentPage'
import AccountIcon from 'theme/components/core/blocks/Header/AccountIcon'
import CompareIcon from 'theme/components/core/blocks/Header/CompareIcon'
import HamburgerIcon from 'theme/components/core/blocks/Header/HamburgerIcon'
import Logo from 'theme/components/core/Logo'
import MicrocartIcon from 'theme/components/core/blocks/Header/MicrocartIcon'
import SearchIcon from 'theme/components/core/blocks/Header/SearchIcon'

import SearchIcon2 from '@vue-storefront/core/compatibility/components/blocks/Header/SearchIcon'

import WishlistIcon from 'theme/components/core/blocks/Header/WishlistIcon'
import Navigation from 'theme/components/core/blocks/Header/Navigation'

export default {
  name: 'Header',
  components: {
    AccountIcon,
    CompareIcon,
    HamburgerIcon,
    Logo,
    MicrocartIcon,
    SearchIcon,SearchIcon2,
    WishlistIcon,
    Navigation
  },
  mixins: [CurrentPage],
  mixins: [SearchIcon2],
  data () {
    return {
      navVisible: true,
      isScrolling: false,
      scrollTop: 0,
      lastScrollTop: 0,
      navbarHeight: 54
    }
  },
  computed: {
    ...mapState({
      isOpenLogin: state => state.ui.signUp,
      currentUser: state => state.user.current
    }),
    isThankYouPage () {
      return this.$store.state.checkout.isThankYouPage
        ? this.$store.state.checkout.isThankYouPage
        : false
    }
  },
  beforeMount () {
    window.addEventListener(
      'scroll',
      () => {
        this.isScrolling = true
      },
      { passive: true }
    )

    setInterval(() => {
      if (this.isScrolling) {
        this.hasScrolled()
        this.isScrolling = false
      }
    }, 250)
  },
  methods: {
    gotoAccount () {
      this.$bus.$emit('modal-toggle', 'modal-signup')
    },
    hasScrolled () {
      this.scrollTop = window.scrollY
      if (
        this.scrollTop > this.lastScrollTop &&
        this.scrollTop > this.navbarHeight
      ) {
        this.navVisible = false
      } else {
        this.navVisible = true
      }
      this.lastScrollTop = this.scrollTop
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~theme/css/variables/colors';
@import '~theme/css/helpers/functions/color';
$color-icon-hover: color(secondary, $colors-background);

header {
  height: 54px;
  top: -55px;
  z-index: 3;
  transition: top 0.2s ease-in-out;
  &.is-visible {
    top: 0;
  }
}
.icon {
  opacity: 0.6;
  &:hover,
  &:focus {
    background-color: $color-icon-hover;
    opacity: 1;
  }
}
.right-icons {
  //for edge
  float: right;
}
.header-placeholder {
  height: 180px;
}
.links {
  text-decoration: underline;
}
.hamburger-icon{
  color: #ffffff;
  background: none;
  display: none;
}
.search-icon{
  color: #ffffff;
  display: none;
  margin-right: 1rem;
}
@media (max-width: 1067px) {
  .hamburger-icon, .search-icon{
    display: block;
  }
  .navigation-block{
    display: none;
    transition: all 0.2s ease-in-out
  }
  .search{
    display: none;
  }
}
@media (max-width: 767px) {
  .row.middle-xs {
    margin: 0 -15px;

    &.py5 {
      margin: 0;
    }
  }
  .col-xs-2:first-of-type {
    padding-left: 0;
  }
  .col-xs-2:last-of-type {
    padding-right: 0;
  }
  a,
  span {
    font-size: 12px;
  }
  .middle-header{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
}
</style>
