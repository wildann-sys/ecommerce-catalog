<template>
  <div class="card" :style="{ backgroundColor: bgColor }">
    <img :src="product.image" alt="Product" class="product-img" />
    <h3 :style="{ color: color }">{{ product.title }}</h3>
    <p class="desc">{{ product.description }}</p>
    <div class="bottom">
      <span class="price" :style="{ color: color }">${{ product.price }}</span>
      <span class="rating">⭐ {{ product.rating }}</span>
    </div>
    <button class="btn" :style="{ background: color }">Add to Cart</button>
  </div>
</template>

<script>
export default {
  name: 'ProductCard',
  props: {
    product: {
      type: Object,
      required: true
    },
    bgColor: {
      type: String,
      default: '#ffffff'
    },
    color: {
      type: String,
      default: '#000000'
    }
  }
}
</script>

<style scoped>
.card {
  width: 250px;
  border-radius: 15px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  transform: scale(1.03);
  box-shadow: 0 4px 14px rgba(0,0,0,0.2);
}

.product-img {
  height: 180px;
  object-fit: contain;
  width: 100%;
  margin-bottom: 15px;
}

h3 {
  font-size: 1rem;
  margin-bottom: 6px;
}

.desc {
  color: #555;
  font-size: 0.85rem;
  margin-bottom: 12px;
  min-height: 40px;
}

.bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.price {
  font-weight: bold;
}

.rating {
  color: #f5b301;
  font-size: 0.9rem;
}

.btn {
  color: white;
  border: none;
  padding: 8px 20px;
  border-radius: 20px;
  cursor: pointer;
  transition: opacity 0.3s ease;
}

.btn:hover {
  opacity: 0.85;
}
</style>
