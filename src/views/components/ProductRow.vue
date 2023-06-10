<template>
  <tr>
    <td>
      <img
        :src="product.thumbnail"
        alt="Product Thumbnail"
        class="img-thumbnail products-table-img"
      />
    </td>
    <td>{{ product.title }}</td>
    <td>{{ product.description }}</td>
    <td>{{ product.price }}</td>
    <td>{{ product.discount }}</td>
  </tr>
</template>

<style>
.products-table-img {
  max-width: 200px !important;
}
</style>

<script>
export default {
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
};
</script>
