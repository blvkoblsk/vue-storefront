<template>
  <li class="row pr55 py20">
    <div @click="closeWishlist">
      <router-link :to="localizedRoute({
        name: product.type_id + '-product',
        params: { parentSku: product.parentSku ? product.parentSku : product.sku, slug: product.slug, childSku: product.sku }
      })">
        <img v-lazy="thumbnail" >
      </router-link>
    </div>
    <div class="col-xs between-xs flex pl40 py15">
      <div @click="closeWishlist">
        <router-link :to="localizedRoute({
          name: product.type_id + '-product',
          params: { parentSku: product.parentSku ? product.parentSku : product.sku, slug: product.slug, childSku: product.sku }
        })">
          {{ product.name | htmlDecode }}
        </router-link>
        <div class="h6 cl-bg-secondary pt5">{{ product.sku }}</div>
      </div>
    </div>
    <div class="col-xs flex py15 align-right">
      <div>
        <span class="price-special" v-if="product.special_price">{{ product.priceInclTax | price }}</span>&nbsp;
        <span class="price-original" v-if="product.special_price" >{{ product.originalPriceInclTax | price }}</span>

        <span v-if="!product.special_price">
          {{ product.priceInclTax | price }}
        </span>
      </div>
      <div>
        <div class="mt5"><span @click="removeItem"><remove-button class="cl-accent" /></span></div>
      </div>
    </div>
  </li>
</template>

<script>
import Product from 'core/components/blocks/Wishlist/Product'
import RemoveButton from './RemoveButton'

export default {
  components: {
    RemoveButton
  },
  mixins: [Product],
  data () {
    return {
      qty: 1
    }
  },
  methods: {
    removeItem () {
      this.$store.dispatch('wishlist/removeItem', this.product)
      this.$bus.$emit('product-after-remove-from-wishlist', this.product)
    },
    closeWishlist () {
      this.$store.commit('ui/setSidebar', false)
      this.$store.commit('ui/setWishlist', false)
    }
  }
}
</script>

<style scoped>
.col-xs {
  flex-direction: column;
}
input {
  width: 30px;
}
</style>
