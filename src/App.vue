<template>
  <div class="page">
    <!-- Loader -->
    <div v-if="loading" class="loader">
      <div class="spinner"></div>
      <p>Loading product...</p>
    </div>

    <!-- Product Card -->
    <transition :name="transitionName" mode="out-in">
      <div class="card" v-if="!loading && product" :key="product.id">
        <img :src="product.image" alt="Product image" class="product-img" />
        <h2 class="title">{{ product.title }}</h2>
        <p class="desc">{{ product.description }}</p>
        <p class="price">${{ product.price }}</p>
        <p class="category">Category: {{ product.category }}</p>

        <div class="buttons">
          <button class="btn prev" @click="prevProduct">Previous</button>
          <button class="btn next" @click="nextProduct">Next</button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: null,
      index: 1,
      loading: false,
      transitionName: "slide-right",
    };
  },

  methods: {
    async fetchProduct() {
      this.loading = true;
      try {
        const res = await fetch(`https://fakestoreapi.com/products/${this.index}`);
        this.product = await res.json();
      } catch (err) {
        console.error("Error fetching product:", err);
      } finally {
        this.loading = false;
      }
    },

    nextProduct() {
      this.transitionName = "slide-left";
      this.index = this.index >= 20 ? 1 : this.index + 1;
      this.fetchProduct();
    },

    prevProduct() {
      this.transitionName = "slide-right";
      this.index = this.index <= 1 ? 20 : this.index - 1;
      this.fetchProduct();
    },
  },

  mounted() {
    this.fetchProduct();
  },
};
</script>

<style scoped>
/* ===== Layout dasar ===== */
.page {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(to bottom right, #6a11cb1a, #2575fc1a);
  font-family: 'Poppins', sans-serif;
}

/* ===== Kartu produk ===== */
.card {
  background: #fff;
  border-radius: 20px;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.1);
  width: 340px;
  padding: 24px;
  text-align: center;
  animation: fadeIn 0.4s ease;
}

.product-img {
  width: 200px;
  height: 200px;
  object-fit: contain;
  margin-bottom: 12px;
}

.title {
  font-size: 1.1rem;
  color: #333;
  font-weight: bold;
  margin-bottom: 8px;
}

.desc {
  color: #555;
  font-size: 0.85rem;
  height: 70px;
  overflow: hidden;
  margin-bottom: 12px;
}

.price {
  font-size: 1.3rem;
  font-weight: bold;
  color: #6a11cb;
  margin-bottom: 8px;
}

.category {
  font-size: 0.9rem;
  color: #2575fc;
  margin-bottom: 16px;
}

.buttons {
  display: flex;
  justify-content: center;
  gap: 12px;
}

.btn {
  background: linear-gradient(to right, #6a11cb, #2575fc);
  color: white;
  border: none;
  padding: 10px 18px;
  border-radius: 25px;
  cursor: pointer;
  transition: opacity 0.3s ease, transform 0.2s ease;
}

.btn:hover {
  opacity: 0.85;
  transform: scale(1.05);
}

.prev {
  background: linear-gradient(to right, #777, #999);
}

/* ===== Loader ===== */
.loader {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #444;
}

.spinner {
  width: 50px;
  height: 50px;
  border: 5px solid #ddd;
  border-top-color: #6a11cb;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
  margin-bottom: 10px;
}

/* ===== Animasi loader ===== */
@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* ===== Animasi masuk (fade) ===== */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: scale(0.95);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

/* ===== Transisi slide antar produk ===== */
.slide-left-enter-active,
.slide-left-leave-active,
.slide-right-enter-active,
.slide-right-leave-active {
  transition: all 0.5s ease;
  position: absolute;
}

.slide-left-enter-from {
  transform: translateX(100%);
  opacity: 0;
}
.slide-left-leave-to {
  transform: translateX(-100%);
  opacity: 0;
}

.slide-right-enter-from {
  transform: translateX(-100%);
  opacity: 0;
}
.slide-right-leave-to {
  transform: translateX(100%);
  opacity: 0;
}
</style>
