<template>
  <button
    type="button"
    class="relative bg-cl-transparent brdr-none inline-flex"
    @click="openMicrocart"
    data-testid="openMicrocart"
    :aria-label="$t('Open microcart')"
  >
  <img src="/assets/test_img/box.png" alt="box">
    <span
      class="minicart-count absolute flex center-xs middle-xs border-box py0 px2 h6 lh16 weight-700 cl-white bg-cl-silver"
      v-cloak
      v-show="totalQuantity"
      data-testid="minicartCount"
    >
      {{ totalQuantity }}
    </span>
  </button>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import MicrocartIcon from '@vue-storefront/core/compatibility/components/blocks/Header/MicrocartIcon'

export default {
  // mixins: [MicrocartIcon],
  mounted () {
    document.addEventListener('visibilitychange', () => {
      if (!document.hidden) {
        this.$store.dispatch('cart/load')
      }
    })
  },
  computed: {
    ...mapGetters({
      totalQuantity: 'cart/getItemsTotalQuantity'
    })
  },
  methods: {
    ...mapActions({
      openMicrocart: 'ui/toggleMicrocart'
    })
  }
}
</script>

<style scoped>
  .minicart-count {
    top: -.5px;
    left: 57%;
    width: 1.1rem;
    min-height: 1.1rem;
    border-radius: 50%;
    background: #8dc73f;
    color: #fff;
  }
</style>
