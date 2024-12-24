<template>
  <!-- Component Wrapper -->
  <div id="app" class="optional-products">
    <!-- Header for Carousel Category -->
    <div class="d-flex align-center mb-4">
      <h4 class="text-h6 mr-2">{{ categories[0].name }}</h4>  <!-- Category Name -->
      <v-chip
        :color="categories[0].required ? 'error' : 'info'"
        size="small"
        class="text-caption"
      >
        {{ categories[0].required ? 'Required' : 'Optional' }}
      </v-chip>
    </div>
    <!-- Carousel for Category 0 -->
    <div class="carousel-container">
      <!-- Scroll Left Button -->
      <v-btn
        icon="mdi-chevron-left"
        variant="text"
        class="scroll-btn prev"
        @click="scroll('prev')"
      ></v-btn>

      <!-- Carousel Items Container -->
      <div class="carousel" ref="carousel">
        <!-- Loop through each item in the category -->
        <div
          v-for="(item, itemIndex) in categories[0].items"
          :key="itemIndex"
          class="carousel-item"
        >
          <!-- Card for each product in the carousel -->
          <v-card
            id="app"
            :class="{
                  'selected': isSelectedRequired(item),
                  'disabled': isDisabledRequired(item, categories[0])
                }"
            @click="toggleSelection(item, categories[0])"
            elevation="1"
            class="product-card"
          >
            <!-- product image -->
            <v-img
              :src="item.image"
              :alt="item.title"
              height="200"
              cover
              :eager="true"
            />

            <!-- product title and price -->
            <v-card-title class="text-subtitle-1">{{ item.title }}</v-card-title>
            <v-card-subtitle>{{ item.price }}</v-card-subtitle>

            <!-- overlay for selected item -->
            <v-overlay
              :model-value="isSelected(item)"
              contained
              class="align-center justify-center"
              scrim="#033358"
              persistent
            >
              <v-icon color="white" size="24" icon="mdi-check-circle"></v-icon>
            </v-overlay>
          </v-card>
        </div>
      </div>

      <!-- scroll right button -->
      <v-btn
        icon="mdi-chevron-right"
        variant="text"
        class="scroll-btn next"
        @click="scroll('next')"
      ></v-btn>
    </div>
    <!-- Non-Carousel Categories -->
    <div v-for="(category, categoryIndex) in categories" :key="categoryIndex" class="category-section mb-8">
      <div v-if="category.id !== 0">
        <!-- category header with name and required/optional badge -->
        <div class="d-flex align-center mb-4">
          <h4 class="text-h6 mr-2">{{ category.name }}</h4>
          <v-chip
            :color="category.required ? 'error' : 'info'"
            size="small"
            class="text-caption"
          >
            {{ category.required ? 'Required' : 'Optional' }}
          </v-chip>
        </div>

        <!-- Product Grid -->
        <div>
          <v-row>
            <v-col
              v-for="(item, itemIndex) in category.items"
              :key="itemIndex"
              cols="12"
              sm="6"
              md="4"
              lg="3"
            >
              <!-- Product Card -->
              <v-card
                id="app"
                :class="{
              'selected': isSelected(item),
              'disabled': isDisabled(item, category)
            }"
                @click="toggleSelection(item, category)"
                elevation="1"
                class="product-card"
              >
                <!-- product image -->
                <v-img
                  :src="item.image"
                  :alt="item.title"
                  height="200"
                  cover
                  :eager="true"
                />

                <!-- product title and price -->
                <v-card-title class="text-subtitle-1">{{ item.title }}</v-card-title>
                <v-card-subtitle>{{ item.price }}</v-card-subtitle>

                <!-- overlay for selected item -->
                <v-overlay
                  :model-value="isSelected(item)"
                  contained
                  class="align-center justify-center"
                  scrim="#033358"
                  persistent
                >
                  <v-icon color="white" size="24" icon="mdi-check-circle"></v-icon>
                </v-overlay>
              </v-card>
            </v-col>
          </v-row>
        </div>
      </div>
    </div>

    <!-- validation Alert for invalid selections -->
    <v-alert
      v-if="validationMessage"
      :type="alertType"
      :text="validationMessage"
      class="my-3"
      variant="tonal"
    />

    <!-- cartTotal Component -->
    <CartTotal
      :items="selectedItems"
      :carousel-item="carouselSelection"
      :mandatory-category-id="1"
      :is-valid="isValid"
      :validation-message="validationMessage"
      :alert-type="alertType"
      @remove-item="handleRemoveItem"
      @next="handleNext"
      @previous="$emit('previous-step')"
    />
  </div>
</template>

<script>
import CartTotal from './CartTotal.vue';
import Carousel from "@/components/Carousel.vue";
import {th} from "vuetify/locale";

export default {
  name: 'OptionProducts',

  components: {
    Carousel,
    CartTotal
  },

  data() {
    return {
      selectedIndex: null,
      scrollPosition: 0,
      selectedItems: [],
      categories: [
        {
          id: 0,
          name: 'Photomaton',
          required: true,
          items: [
            {
              title: 'Photomaton Vintage',
              price: '620 €',
              image: 'PhotomatonVintage.png',
              categoryId: 0,
              id: 0
            },
            {
              title: 'Photomaton Retro',
              price: '620 €',
              image: 'PhotomatonRetro.png',
              categoryId: 0,
              id: 1
            },
            {
              title: 'Photomaton Glam',
              price: '690 €',
              image: 'PhotomatonGlam.png',
              categoryId: 0,
              id: 2
            },
            {
              title: 'Espejo',
              price: '720 €',
              image: 'PhotomatonEspejo.png',
              categoryId: 0,
              id: 3
            },
            {
              title: '360 Grados',
              price: '760 €',
              image: 'Photomaton360grados.png',
              categoryId: 0,
              id: 4
            },
            {
              title: 'Glambot',
              price: '900 €',
              image: 'GlamBot.png',
              categoryId: 0,
              id: 5
            },
            {
              title: 'Photobus',
              price: '1200 €',
              image: 'Photobus.png',
              categoryId: 0,
              id: 6
            },
            {
              title: 'Deco & Diversion Extra',
              price: '200 €',
              image: 'Deco-DiversionExtra.png',
              categoryId: 0,
              id: 7
            }
          ],
        },
        {
          id: 1,
          name: 'FONDOS Y DECORADOS',
          required: true,
          items: [
            {
              id: 8,
              title: 'PHOTOCALL LUNARES',
              price: '0 €',
              image: 'photocall_lunares.png',
              categoryId: 1
            },
            {
              id: 9,
              title: 'CHESTER VERDE + HEXÁGONO',
              price: '+56 €',
              image: 'Chester_verde_hexagono.png',
              categoryId: 1
            },
            {
              id: 10,
              title: 'CORAZONES',
              price: '+48 €',
              image: 'Corazones.jpg',
              categoryId: 1
            },
            {
              id: 11,
              title: 'FONDO DISCOTECA',
              price: '+56 €',
              image: 'Fondo_discoteca.png',
              categoryId: 1
            },

          ],
        },
        {
          id: 2,
          name: 'Albums',
          required: true,
          items: [
            {
              id: 12,
              title: 'ÁLBUM TURQUESA',
              price: '0 €',
              image: 'turquesa.png',
              categoryId: 2
            },
            {
              id: 13,
              title: 'ÁLBUM SERRAJE NEGRO',
              price: '+56 €',
              image: 'Album_serraje_negro.png',
              categoryId: 2
            },
            {
              id: 14,
              title: 'ÁLBUM OLIVIA',
              price: '+48 €',
              image: '2_A_lbum_Olivia.png',
              categoryId: 2
            },
            {
              id: 15,
              title: 'ÁLBUM SERRAJE DORADO',
              price: '+56 €',
              image: 'serrajedorado.png',
              categoryId: 2
            },

          ],
        },
        {
          id: 3,
          name: 'PLANTILLAS PARA LAS FOTOS',
          required: true,
          items: [
            {
              id: 16,
              title: 'Clásica 10x15 cm',
              price: '0 €',
              image: 'Rocio_5x15.png',
              categoryId: 3
            },
            {
              id: 17,
              title: 'Tira 5x15 cm',
              price: '0 €',
              image: 'Rocio_10x15.png',
              categoryId: 3
            },
          ],
        },
        {
          id: 4,
          name: 'ATREZZO',
          required: false,
          items: [
            {
              id: 18,
              title: 'ATREZZO',
              price: '0 €',
              image: 'atrezzo_2023(1).jpg',
              categoryId: 4
            },
            {
              id: 19,
              title: 'SIN ATREZZO',
              price: '0 €',
              image: 'atrezzo_2023(2).jpg',
              categoryId: 4
            },
            {
              id: 20,
              title: 'CORONAS',
              price: '+50 €',
              image: 'atrezzo_2023(3).png',
              categoryId: 4
            },
            {
              id: 21,
              title: 'MADERA',
              price: '+40 €',
              image: 'atrezzo_2023(4).png',
              categoryId: 4
            },
          ],
        },
      ],
      requiredIsValid: false,
      isValid: false,
      validationMessage: '',
      alertType: 'warning',
    };
  },
  methods: {
    scroll(direction) {
      const container = this.$refs.carousel;
      if (!container) return;

      // Calculate scroll amount based on visible width
      const visibleWidth = container.clientWidth;
      const scrollAmount = Math.floor(visibleWidth * 0.8);

      // Get current scroll position
      const currentScroll = container.scrollLeft;
      const maxScroll = container.scrollWidth - visibleWidth;

      // Calculate target scroll position
      let targetScroll;
      if (direction === 'next') {
        targetScroll = Math.min(currentScroll + scrollAmount, maxScroll);
      } else {
        targetScroll = Math.max(currentScroll - scrollAmount, 0);
      }

      // Smooth scroll animation
      this.smoothScroll(container, targetScroll);
    },

    smoothScroll(element, target) {
      const start = element.scrollLeft;
      const distance = target - start;
      const duration = 150; // milliseconds
      let startTime = null;

      const animation = currentTime => {
        if (!startTime) startTime = currentTime;
        const timeElapsed = currentTime - startTime;
        const progress = Math.min(timeElapsed / duration, 3);

        // Easing function for smooth animation
        const easeInOutQuad = t => t < 0.5 ? 2 * t * t : -1 + (4 - 2 * t) * t;

        element.scrollLeft = start + (distance * easeInOutQuad(progress));

        if (timeElapsed < duration) {
          requestAnimationFrame(animation);
        } else {
          // Update buttons after animation completes
          this.updateScrollButtons();
        }
      };

      requestAnimationFrame(animation);
    },

    updateScrollButtons() {
      const container = this.$refs.carousel;
      if (!container) return;

      const threshold = 5; // Small threshold to handle precision issues
      const maxScroll = container.scrollWidth - container.clientWidth;

      this.atStart = container.scrollLeft <= threshold;
      this.atEnd = maxScroll - container.scrollLeft <= threshold;
    },

    handleResize() {
      // Debounce resize updates
      if (this.resizeTimeout) {
        clearTimeout(this.resizeTimeout);
      }

      this.resizeTimeout = setTimeout(() => {
        const container = this.$refs.carousel;
        if (!container) return;

        // Ensure scroll position is valid after resize
        const maxScroll = container.scrollWidth - container.clientWidth;
        if (container.scrollLeft > maxScroll) {
          container.scrollLeft = maxScroll;
        }

        this.updateScrollButtons();
      }, 150);
    },


    isSelected(item) {
      return this.selectedItems.some(selected => selected.id === item.id);
    },

    isDisabled(item, category) {
      return false;
    },

    isSelectedRequired(item) {
      this.requiredIsValid = true
      return this.selectedItems.some(selected => selected.id === item.id);
    },


    isDisabledRequired(item, category) {
      this.requiredIsValid = false

      return false;
    },

    toggleSelection(item, category) {
      // Find if this specific item is already selected
      const itemIndex = this.selectedItems.findIndex(
        selected => selected.id === item.id
      );

      if (itemIndex === -1) {
        // Item is not selected, handle new selection

        // Find and remove any existing selection in the same category
        const existingIndex = this.selectedItems.findIndex(
          selected => selected.categoryId === category.id
        );
        if (existingIndex !== -1) {
          this.selectedItems.splice(existingIndex, 1);
        }

        // Add new selection
        this.selectedItems.push(item);
      } else {
        // If clicking an already selected item in mandatory category, don't allow deselection
        if (category.required) {
          return;
        }
        // For optional category, allow deselection
        this.selectedItems.splice(itemIndex, 1);
      }

      localStorage.setItem("selectedItems", JSON.stringify(this.selectedItems));

      // Ensure price is properly formatted
      if (typeof item.price === 'string') {
        item.price = parseFloat(item.price.replace('€', '').replace(',', '.'));
      }

      this.validateSelection();
    },

    validateSelection() {
      const hasRequiredCategory = this.validateCategories();
      const hasCarouselSelection = this.selectedItems.some(item => item.categoryId === 0);
      const hasPackageSelection = this.selectedItems.some(item => item.categoryId === 1);
      const hasAlbumsSelection = this.selectedItems.some(item => item.categoryId === 2);
      const hasPhotosSelection = this.selectedItems.some(item => item.categoryId === 3);

      // Update validation state
      this.isValid = hasRequiredCategory
        && hasPackageSelection
        && hasAlbumsSelection
        && hasPhotosSelection;

      // Set appropriate validation message
      if (!hasCarouselSelection) {
        this.validationMessage = 'Please select a photo book from the carousel';
        this.alertType = 'warning';
      } else if (!hasPackageSelection) {
        this.validationMessage = 'Please select a photomaton package';
        this.alertType = 'warning';
      } else {
        this.validationMessage = '';
        this.alertType = 'success';
      }

      this.$emit('select-card', this.requiredIsValid);
      this.$emit('update:valid', this.isValid);
      this.$emit('selection-changed', this.selectedItems);
    },

    validateCategories() {
      // Check required categories (0 and 1)
      const requiredCategories = [0, 1]; // Carousel and Photomaton Package
      return requiredCategories.every(categoryId =>
        this.selectedItems.some(item => item.categoryId === categoryId)
      );
    },


    handleRemoveItem(item) {
      const index = this.selectedItems.findIndex(selected => selected.id === item.id);
      if (index !== -1) {
        this.selectedItems.splice(index, 1);
        this.validateSelection(); // Revalidate after removal
      }
    },

    handleNext() {
      const missingSelections = [];

      // Check Photo Book selection (Category 0)
      if (!this.selectedItems.some(item => item.categoryId === 0)) {
        missingSelections.push('Photo Book');
      }

      // Check Photomaton Package selection (Category 1)
      if (!this.selectedItems.some(item => item.categoryId === 1)) {
        missingSelections.push('Photomaton Package');
      }

      if (missingSelections.length > 0) {
        this.validationMessage = `Please select: ${missingSelections.join(' and ')}`;
        this.alertType = 'error';
        return;
      }

      // All validations passed
      this.$emit('next-step');
    },

    restoreSelected() {
      const selectedItems = localStorage.getItem("selectedItems")

      if (selectedItems) {
        this.selectedItems = JSON.parse(selectedItems)
      }
    }
  },
  mounted() {
    this.restoreSelected();
    this.validateSelection(); // Initial validation
    this.updateScrollButtons();
    window.addEventListener('resize', this.handleResize);
    this.resizeTimeout = null;
  },
  beforeUnmount() {
    if (this.resizeTimeout) {
      clearTimeout(this.resizeTimeout);
    }
    window.removeEventListener('resize', this.handleResize);
  },

};
</script>

<style scoped>
.product-card {
  height: 100%;
  transition: all 0.2s ease-out;
  cursor: pointer;
  border: 2px solid transparent;
}

.product-card.selected {
  border-color: rgb(var(--v-theme-primary));
  background: rgba(var(--v-theme-primary), 0.05);
}


.product-card.disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.category-section {
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  padding-bottom: 24px;
}

.category-section:last-child {
  border-bottom: none;
}

@media (max-width: 600px) {
  .category-section {
    padding-bottom: 16px;
  }
}
.optional-products {
  padding-bottom: 24px;
}

/* ====================
  Carousel
==================== */

.carousel-container {
  position: relative;
  display: flex;
  align-items: center;
  padding: 0 48px;
  margin: 20px 0;
}

.carousel {
  display: flex;
  gap: 25px;
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
  width: 300px;
  transition: transform 0.3s ease;
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

</style>

