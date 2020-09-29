<template lang="pug">
.container
  .discount-grid
    .discount-grid__item--1
      discountCard(color='#F6F6F6')
    .discount-grid__item--2
      discountCard(color='#E5E9EC')
    .discount-grid__item--3
      discountCard
    .discount-grid__item--4
      discountCard(color='#F2C94C', btnColor='#e69e17') 
</template>

<script>
import discountCard from '@/components/blocks/DiscountCard.vue'

export default {
  name: 'DiscountGrid',
  components: {
    discountCard,
  },
  computed: {
    discounts() {
      return this.$store.state.discounts.allDiscount
    },
  },
}
</script>

<style scoped lang="scss">
@import '../../styles/toptier';
.discount-grid {
  margin-top: 3rem;
  display: grid;
  grid-gap: 2rem;
  grid-template-columns: repeat(4, 1fr);
  @include media-breakpoint-down(mobileL) {
    grid-template-columns: repeat(1, 1fr);
  }
  &__item {
    &--1 {
      transform: rotate(-1deg);
    }
    &--2 {
      transform: rotate(1deg);
    }
    &--4 {
      transform: rotate(-2deg);
    }
  }
}
</style>

