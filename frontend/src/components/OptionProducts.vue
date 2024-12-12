<template>
  <div class="optional-products">
    <!--  Header for category optional products  -->
    <h3 class="text-h5 mb-4">Select Additional Products</h3>
    <p class="text-subtitle-1 mb-6">Please select at least one optional product to continue</p>

    <!--  Iterate categories and display ech other  -->
    <div v-for="(category, categoryIndex) in categories" :key="categoryIndex" class="category-section">
      <h4 class="text-h6 mb-3">{{ category.name }}</h4>

      <!--   Render items in the category   -->
      <v-row>
        <v-col
          v-for="(item, itemIndex) in category.items"
          :key="itemIndex"
          cols="12"
          sm="6"
          md="4"
          lg="3"
        >
          <!--    Product card selection on click      -->
          <v-card
            :class="{ 'selected': isSelected(item) }"
            @click="toggleSelection(item)"
            elevation="2"
            class="product-card"
          >
            <!--    Image on the product card        -->
            <v-img
              :src="item.image"
              :alt="item.title"
              height="200"
              cover
              class="product-image"
            >
              <template v-slot:placeholder>
                <v-row align="center" justify="center" class="fill-height">
                  <!-- <v-progress-circular indeterminate color="primary"></v-progress-circular>-->
                </v-row>
              </template>
            </v-img>

            <!--    Title and price product        -->
            <v-card-title class="text-subtitle-1">{{ item.title }}</v-card-title>
            <v-card-subtitle>{{ item.price }}</v-card-subtitle>

            <!--    Overlay to indicate the item is selected         -->
            <v-overlay
              :model-value="isSelected(item)"
              contained
              class="align-center justify-center"
              scrim="#033358"
              persistent
            >
              <v-icon
                color="white"
                size="24"
                icon="mdi-check-circle"
              ></v-icon>
            </v-overlay>
          </v-card>
        </v-col>
      </v-row>
    </div>
    <!-- Error alert shown when items are not selected-->
    <v-alert
      v-if="showError"
      type="error"
      class="mt-4"
      text="Please select at least one optional product"
    ></v-alert>
  </div>
</template>

<script>
export default {
  name: 'OptionProducts',
  data() {
    return {
      selectedItems: [], //store the list of selected items
      showError: false, //visibility of the error message
      categories: [ //Array of categoires contain items
        {
          name: 'Photo Albums',
          items: [
            {
              id: 1,
              title: 'Mini Album',
              price: '€29.99',
              image: '/images/mini-album.jpg',
            },
            {
              id: 2,
              title: 'Digital Copy',
              price: '€19.99',
              image: '/images/digital-copy.jpg',
            },
          ],
        },
        {
          name: 'Accessories',
          items: [
            {
              id: 3,
              title: 'Custom Box',
              price: '€39.99',
              image: '/images/custom-box.jpg',
            },
            {
              id: 4,
              title: 'Photo Frame',
              price: '€24.99',
              image: '/images/photo-frame.jpg',
            },
          ],
        },
        {
          name: 'Prints',
          items: [
            {
              id: 5,
              title: 'Premium Prints',
              price: '€34.99',
              image: '/images/premium-prints.jpg',
            },
            {
              id: 6,
              title: 'Canvas Print',
              price: '€49.99',
              image: '/images/canvas-print.jpg',
            },
          ],
        },
      ],
    };
  },
  // checks if items are selected
  methods: {
    isSelected(item) {
      return this.selectedItems.some(selected => selected.id === item.id);
    },
    // add or remove chosen product
    toggleSelection(item) {
      const index = this.selectedItems.findIndex(selected => selected.id === item.id);
      if (index === -1) {
        this.selectedItems.push(item);
      } else {
        this.selectedItems.splice(index, 1);
      }
      this.validateSelection();
      this.$emit('selection-changed', this.selectedItems);
    },
    // validate if pruducts <3
    validateSelection() {
      const isValid = this.selectedItems.length > 2;
      this.showError = !isValid;
      this.$emit('update:valid', isValid);
    },
  },
  //check chose after validation
  mounted() {
    this.validateSelection();
  },
};
</script>

<style scoped>

</style>
