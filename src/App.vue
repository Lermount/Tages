<template>
  <div class="container">
    <Breadcrumbs />
    <h1 class="title">Комплекты стеллажных систем</h1>
    <Sort @updatePrice="updatePrice" @updateMaterial="udateMaterial" />
    <ProductList :priceSortID="priceSortID" :materialID="materialID" />
  </div>
</template>
<script>
import Breadcrumbs from '@/components/tages-breadcrumbs.vue';
import Sort from '@/components/tages-sort.vue';
import ProductList from '@/components/product-list.vue';

export default {
  components: {
    Breadcrumbs,
    Sort,
    ProductList
  },

  data: () => ({
    priceSortID: null,
    materialID: null,
  }),
  //Получаем данные из tages-sort
  methods: {
    updatePrice(value) {
      this.priceSortID = value
    },

    udateMaterial(value) {
      this.materialID = value
    }

  }
}
</script>
<style scoped>
.title {
  margin: 0;
  font-family: SFProDisplay;
  font-size: 36px;
  font-weight: 600;
  line-height: 48px;
  letter-spacing: 0px;
  text-align: left;
}

@media(max-width: 710px) {
  .title {
    font-size: 20px;
    line-height: 20px;
  }
}
</style>