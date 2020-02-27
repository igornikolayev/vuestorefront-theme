<template>
  <div>
    <pre-footer></pre-footer>
    <footer class="section section-footer">
      <div class="container">
        <div class="footer">
          <div class="footer__section">
            <p class="footer__header">Contactgegevens</p>
            <div class="footer__text">
              <p class="mb-1rem">PortofoonWEB <br> Zonnedauw 16 <br>9202 PA Drachten</p>
              <p class="footer__row">
                <img src="/assets/test_img/footer__phone.png" alt="footer phone">
                <a href="tel:+0512745293">0512 - 745 293</a>
              </p>
              <p class="mb-1rem footer__row">
                <img src="/assets/test_img/footer__mail.png" alt="footer mail">
                <a href="mailto:info@portofoonweb.nl">info@portofoonweb.nl</a>
              </p>
              <p>K.v.K nr: 62061712 <br> BTW nr: NL854624624B01 </p>
            </div>
          </div>
          <div class="footer__section">
            <p class="footer__header"> Klantenservice</p>
            <ul class="footer__text">
              <li class="mb-05rem" v-for="(footerK, index) in linksFooterKlanten" :key="index">
                <router-link :to=footerK.link> {{footerK.title}}</router-link>
              </li>
            </ul>
          </div>
          <div class="footer__section">
            <p class="footer__header"> Algemene Informatie</p>
            <ul class="footer__text">
              <li class="mb-05rem" v-for="(footerA, index) in linksFooterAlgemene" :key="index">
                <router-link :to=footerA.link> {{footerA.title}}</router-link>
              </li>
            </ul>
          </div>
          <div class="footer__section">
            <p class="footer__header">Nieuwsbrief</p>
            <div class="footer__text">
              <p>Wilt u op de hoogte gehouden worden van de nieuwste Technieken & Aanbiedingen op het gebied van Portofoons?</p>
              <div class="footer__input">
                <input type="email" placeholder="uw emailadres...">
                <img src="/assets/test_img/telegram.png" alt="telegram">
              </div>
              <div class="footer__image-section">
                <a href="#">
                <img src="/assets/test_img/Smartportofoon_Logo_footer.png" alt="smartlogo mini"></a>
                <a href="#"><img src="/assets/test_img/facebook_footer.png" alt="facebook"></a>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-xs-5 col-sm-3 cl-tertiary">
        <language-switcher v-if="multistoreEnabled" />
      </div>
    </footer>
    <footer-rights></footer-rights>
    <back-to-top bottom="20px" right="20px" visibleoffset="200">
      <button type="button" class="btn-top button no-outline brdr-none cl-white bg-cl-mine-shaft :bg-cl-th-secondary py10 px10">
        <svg width="24" height="24" xmlns="http://www.w3.org/2000/svg" fill-rule="evenodd" clip-rule="evenodd">
          <path d="M23.245 20l-11.245-14.374-11.219 14.374-.781-.619 12-15.381 12 15.391-.755.609z" fill="white" />
        </svg>
      </button>
    </back-to-top>
  </div>
</template>


<script>
import { mapGetters } from 'vuex'
import { currentStoreView, localizedRoute } from '@vue-storefront/core/lib/multistore'
import CurrentPage from 'theme/mixins/currentPage'
import LanguageSwitcher from '../../LanguageSwitcher.vue'
import Newsletter from 'theme/components/core/blocks/Footer/Newsletter'
import BackToTop from 'theme/components/core/BackToTop'
import { getPathForStaticPage } from 'theme/helpers'
import config from 'config'
import links from 'theme/resource/footer-links.json'
import FooterRights from 'theme/components/core/blocks/Footer/FooterRights'
import PreFooter from 'theme/components/core/blocks/Footer/PreFooter'

export default {

  mixins: [CurrentPage],
  name: 'MainFooter',
  methods: {
    goToAccount () {
      this.$bus.$emit('modal-toggle', 'modal-signup')
    },
    getLinkFor (path) {
      return localizedRoute(getPathForStaticPage(path))
    }
  },
  computed: {
    ...mapGetters({
      isLogged: 'user/isLoggedIn'
    }),
    multistoreEnabled () {
      return config.storeViews.multistore
    },
    getVersionInfo () {
      return `v${process.env.__APPVERSION__} ${process.env.__BUILDTIME__}`
    },
    linksFooterKlanten(){
      return links.footerLinks.klanten
    },
    linksFooterAlgemene(){
      return links.footerLinks.algemene
    }
  },
  components: {
    Newsletter,
    LanguageSwitcher,
    BackToTop,
    FooterRights,
    PreFooter
  }
}
</script>

<style lang="scss" scoped>
@import '~theme/css/variables/colors';
@import '~theme/css/helpers/functions/color';
$color-secondary: color(secondary);

.icon {
  transition: 0.3s all;
}

.social-icon {
  width: 40px;
  height: 40px;
  &:hover,
  &:focus,
  &:active {
    .icon {
      fill: $color-secondary;
    }
  }
}
.links {
  list-style-type: none;
  display: flex;
  flex-direction: column-reverse;
  align-items: flex-end;

  @media (min-width: 768px) {
    flex-direction: row;
    justify-content: flex-end;
    align-items: center;
  }
}

.bottom-links {
  @media (max-width: 767px) {
    padding: 0;
  }
}

.footer__version-info {
  display: flex;
  color: $color-secondary;
  font-size: 0.7rem;
  @media (min-width: 768px) {
    margin-right: 1rem;
    font-size: 0.8rem;
  }
}

.underline {
  &:hover {
    color: $color-secondary;
    &:after {
      background-color: $color-secondary;
    }
  }
}
.legal-entity-link {
  text-align: center;
  @media (min-width: 768px) {
    display: none;
  }
}

.privacy-policy-link {
  text-align: right;
  @media (min-width: 768px) {
    display: none;
  }
}

@media (max-width: 595px) {
  .no-mobile {
    display: none;
  }

  .social {
    margin-top: 0;
  }

  .footer-links {
    padding-bottom: 30px;
  }
}
.footer__text{
  li{
    list-style: none;
  }
  a{
    color: #ffffff ;
  }
}
</style>
