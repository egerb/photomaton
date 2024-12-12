<template>
  <div class="carousel-container">
    <v-btn
      icon="mdi-chevron-left"
      variant="text"
      class="scroll-btn prev"
      @click="scroll('prev')"
      :disabled="atStart"
    />

    <div class="carousel" ref="carousel">
      <div
        v-for="(book, index) in photobooks"
        :key="index"
        class="carousel-item"
        :class="{ 'selected': selectedIndex === index }"
        @click="selectBook(index)"
      >
        <v-card class="book-card" elevation="2">
          <v-img
            :src="book.image"
            :alt="book.title"
            cover
            height="200"
          />
          <v-card-title>{{ book.title }}</v-card-title>
          <v-card-subtitle>{{ book.price }}</v-card-subtitle>
        </v-card>
      </div>
    </div>

    <v-btn
      icon="mdi-chevron-right"
      variant="text"
      class="scroll-btn next"
      @click="scroll('next')"
      :disabled="atEnd"
    />
  </div>
</template>

<script>
export default {
  name: 'Carousel',
  data() {
    //array of products
    return {
      selectedIndex: null,
      scrollPosition: 0,
      atStart: true,
      atEnd: false,
      photobooks: [
        {
          title: 'Classic Photobook',
          price: '$29.99',
          image: '/images/photobook1.jpg',
        },
        {
          title: 'Premium Album',
          price: '$39.99',
          image: '/images/photobook2.jpg',
        },
        {
          title: 'Family Collection',
          price: '$49.99',
          image: '/images/photobook3.jpg',
        },
        {
          title: 'Wedding Album',
          price: '$59.99',
          image: '/images/photobook4.jpg',
        },
        {
          title: 'Wedding Album',
          price: '$59.99',
          image: '/images/photobook4.jpg',
        },
        {
          title: 'Wedding Album',
          price: '$59.99',
          image: '/images/photobook4.jpg',
        },
        {
          title: 'Wedding Album',
          price: '$59.99',
          image: '/images/photobook4.jpg',
        },
        {
          title: 'Wedding Album',
          price: '$59.99',
          image: '/images/photobook4.jpg',
        },
      ],
    };
  },
  methods: {
    selectBook(index) {
      this.selectedIndex = index;
      this.$emit('select-card', this.photobooks[index]);
    },
    // dynamic calculate the scroll based on container width
    scroll(direction) {
      const container = this.$refs.carousel;
      const scrollAmount = container.clientWidth * 0.8;

      if (direction === 'next') {
        container.scrollLeft += scrollAmount;
      } else {
        container.scrollLeft -= scrollAmount;
      }

      this.updateScrollButtons();
    },
    // check stage buttons of scroll depend position
    updateScrollButtons() {
      const container = this.$refs.carousel;
      this.atStart = container.scrollLeft <= 1;
      this.atEnd = container.scrollLeft + container.clientWidth >= container.scrollWidth;
    },
    handleResize() {
      this.updateScrollButtons();
    },
  },

  mounted() {
    this.updateScrollButtons();
    window.addEventListener('resize', this.handleResize);
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.handleResize);
  },
};
</script>

<style scoped>
.carousel-container {
  position: relative;
  display: flex;
  align-items: center;
  padding: 0 48px;
  margin: 20px 0;
}

.carousel {
  display: flex;
  gap: 16px;
  overflow-x: auto;
  scroll-behavior: smooth;
  scrollbar-width: none;
  -ms-overflow-style: none;
  padding: 16px 0;
}

.carousel::-webkit-scrollbar {
  display: none;
}

.carousel-item {
  flex: 0 0 auto;
  width: 280px;
  transition: transform 0.3s ease;
}

.carousel-item:hover {
  transform: translateY(-4px);
}

.carousel-item.selected .book-card {
  border: 2px solid rgb(var(--v-theme-primary));
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2) !important;
}

.book-card {
  height: 100%;
  transition: all 0.3s ease;
}

.scroll-btn {
  position: absolute;
  z-index: 1;
}

.scroll-btn.prev {
  left: 0;
}

.scroll-btn.next {
  right: 0;
}

/* Responsive Design */
@media (max-width: 600px) {
  .carousel-container {
    padding: 0 36px;
  }

  .carousel-item {
    width: 220px;
  }
}

@media (max-width: 400px) {
  .carousel-container {
    padding: 0 24px;
  }

  .carousel-item {
    width: 180px;
  }
}
</style>
