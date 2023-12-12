<template>
    <div>
      <h1>Rating Widget</h1>
      <noscript>
        <form action="https://httpbin.org/post" method="POST">
          <label for="rating">How satisfied are you?</label>
          <input type="number" id="rating" name="rating" min="1" max="5" v-model="selectedRating" required>
          <button type="submit">Submit</button>
        </form>
      </noscript>
      <form @submit.prevent="submitForm" v-show="isJavaScriptEnabled">
        <label for="starRating">How satisfied are you?</label>
        <div class="star-rating">
          <!-- Five star icons go here -->
          <span v-for="value in 5" :key="value" class="star" :data-value="value" @click="selectRating(value)">
            &#9733;
          </span>
        </div>
        <input type="hidden" name="question" value="How satisfied are you?">
        <input type="hidden" name="sentBy" value="Vue">
        <input type="hidden" name="rating" v-model="selectedRating" required>
        <button type="submit">Submit</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        selectedRating: 1,
        isJavaScriptEnabled: true,
      };
    },
    mounted() {
      this.isJavaScriptEnabled = true;
    },
    methods: {
      selectRating(value) {
        this.selectedRating = value;
        this.updateStarRating(value);
      },
      updateStarRating(value) {
        const stars = document.querySelectorAll('.star');
        stars.forEach((star) => {
          const starValue = parseInt(star.getAttribute('data-value'));
          star.classList.toggle('selected', starValue <= value);
        });
      },
      submitForm() {
        // Handle form submission
        console.log('Form submitted with rating:', this.selectedRating);
        // You can use an AJAX request here if needed
      },
    },
  };
  </script>
  
  <style scoped>
  .star {
    cursor: pointer;
  }
  
  .star.selected {
    color: gold; /* Change the color to your preferred selected color */
  }
  
  .star-rating {
    display: flex;
    justify-content: center;
  }
  
  #starRating {
    display: none; /* Hide the star rating initially */
  }
  </style>
  