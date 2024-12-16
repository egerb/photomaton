<template>
  <div id="app">
    <!-- Cart Section -->
    <v-card
      class="cart-container"
      variant="outlined"
      :elevation="0"
    >
      <div class="cart-content">
        <!-- Selected Items List -->
        <div class="items-list">
          <transition-group name="cart-item">
            <div
              v-for="item in sortedItems"
              :key="item.id"
              class="cart-item d-flex align-center justify-space-between"
            >
              <div class="item-info">
                <v-chip
                  size="small"
                  :color="getItemColor(item)"
                  class="mr-2"
                >
                  {{ getItemLabel(item) }}
                </v-chip>
                <span class="text-subtitle-2">{{ item.title }}</span>
                <span class="text-caption ml-2">{{ formatPrice(item.price) }}</span>
              </div>
              <v-btn
                icon="mdi-close"
                size="x-small"
                variant="text"
                @click="removeItem(item)"
                :disabled="isItemRemovalDisabled(item)"
              />
            </div>
          </transition-group>

          <div v-if="!items.length" class="no-items text-caption text-grey">
            No items selected
          </div>
        </div>

        <!-- Cart Footer with Navigation -->
        <div class="cart-footer">
          <div class="total-section">
            <span class="text-subtitle-1">Total:</span>
            <span class="text-h6 font-weight-bold">{{ formatPrice(totalPrice) }}</span>
          </div>
        </div>
      </div>
    </v-card>
  </div>
</template>

<script>
export default {
  name: 'CartTotal',

  props: {
    items: {
      type: Array,
      required: true,
      default: () => []
    },
    mandatoryCategoryId: {
      type: Number,
      required: true
    },
    carouselItem: {
      type: Object,
      default: null
    },
    isValid: {
      type: Boolean,
      default: false
    },
    validationMessage: {
      type: String,
      default: ''
    },
    alertType: {
      type: String,
      default: 'warning'
    }
  },

  computed: {
    totalPrice() {
      const allItems = [...this.items];
      if (this.carouselItem) {
        allItems.push(this.carouselItem);
      }

      return allItems.reduce((total, item) => {
        const price = typeof item.price === 'string'
          ? parseFloat(item.price.replace(/[^0-9.-]+/g, ''))
          : item.price;
        return total + price;
      }, 0);
    },

    sortedItems() {
      const allItems = [...this.items];
      if (this.carouselItem) {
        allItems.unshift(this.carouselItem);
      }

      return allItems.sort((a, b) => {
        if (a.categoryId === this.mandatoryCategoryId) return -1;
        if (b.categoryId === this.mandatoryCategoryId) return 1;
        if (a.isCarouselItem) return -1;
        if (b.isCarouselItem) return 1;
        return 0;
      });
    }
  },

  methods: {
    formatPrice(price) {
      return new Intl.NumberFormat('fr-FR', {
        style: 'currency',
        currency: 'EUR',
        minimumFractionDigits: 2
      }).format(price);
    },

    removeItem(item) {
      this.$emit('remove-item', item);
    },
    getItemColor(item) {
      if (item.isCarouselItem) return 'primary';
      return item.categoryId === this.mandatoryCategoryId ? 'error' : 'info';
    },

    getItemLabel(item) {
      if (item.isCarouselItem) return 'Photomaton';
      return item.categoryId === this.mandatoryCategoryId ? 'Package' : 'Extra';
    },

    isItemRemovalDisabled(item) {
      return item.categoryId === this.mandatoryCategoryId || item.isCarouselItem;
    }
  }
};
</script>

<style scoped>
.cart-container {
  position: relative;
  border-top: 1px solid rgba(0, 0, 0, 0.12);
  border-radius: 8px;
  margin-top: 24px;
}

.cart-content {
  padding: 16px;
  max-width: 800px;
  margin: 0 auto;
}

.items-list {
  max-height: 120px;
  overflow-y: auto;
  margin-bottom: 16px;
  scrollbar-width: thin;
}

.items-list::-webkit-scrollbar {
  width: 4px;
}

.items-list::-webkit-scrollbar-thumb {
  background-color: rgba(0, 0, 0, 0.2);
  border-radius: 4px;
}

.cart-item {
  padding: 4px 0;
}

.item-info {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 4px;
}

.cart-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 12px;
  border-top: 1px solid rgba(0, 0, 0, 0.12);
  margin-top: 12px;
}

.total-section {
  display: flex;
  align-items: baseline;
  gap: 12px;
  padding: 0 16px;
}

.no-items {
  text-align: center;
  padding: 8px 0;
}

/* Responsive Design */
@media (max-width: 600px) {
  .cart-container {
    margin-top: 16px;
  }

  .cart-content {
    padding: 12px;
  }

  .cart-footer {
    flex-wrap: wrap;
    gap: 12px;
  }

  .total-section {
    order: -1;
    width: 100%;
    justify-content: center;
    padding-bottom: 8px;
  }
}
</style>
