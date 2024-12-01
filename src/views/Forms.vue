<template>
  <div class="button-container">
    <!-- Button for Shipment Form with an Image -->
    <a-button
        type="default"
        size="large"
        @click="redirectToShipmentForm"
        class="action-button"
    >
      <!-- Image for Shipment Form -->
      <img src="../../public/images/transportation.png" alt="Shipment Icon" class="button-icon" />
      <span class="button-text">Shipment Form</span>
    </a-button>

    <!-- Button for Custom Clearance Form with an Image -->
    <a-button
        type="default"
        size="large"
        @click="redirectToCustomClearanceForm"
        class="action-button"
    >
      <!-- Image for Custom Clearance Form -->
      <img src="../../public/images/ukeu.png" alt="Custom Clearance Icon" class="button-icon" />
      <span class="button-text">Custom Clearance Form</span>
    </a-button>
  </div>
</template>

<script>
export default {
  methods: {
    redirectToShipmentForm() {
      this.$router.push('/forms/shipmentform');
    },
    redirectToCustomClearanceForm() {
      this.$router.push('/forms/customclearance');
    },
  },
};
</script>

<style scoped>
.button-container {
  display: flex;
  justify-content: center;
  gap: 30px; /* Increased gap between buttons */
  margin-top: 50px;
  flex-wrap: wrap;
}

.action-button {
  font-size: 16px; /* Adjusted font size */
  padding: 0; /* Remove padding to make the button square */
  width: 150px; /* Fixed width for square button */
  height: 150px; /* Fixed height for square button */
  border-radius: 0; /* Remove border-radius for square corners */
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.15); /* Soft shadow */
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  overflow: hidden; /* Prevent text overflow */
}

.action-button:hover {
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2); /* Stronger shadow on hover */
  transform: translateY(-4px); /* Slight lift effect on hover */
}

.button-icon {
  width: 40px;  /* Slightly reduced image size to fit better */
  height: 40px; /* Slightly reduced image size to fit better */
  margin-bottom: 8px; /* Reduced space between image and text */
}

.button-text {
  white-space: normal; /* Allow text to wrap */
  word-wrap: break-word; /* Ensure long words break and wrap inside the button */
  width: 100%; /* Ensure the text takes up available space */
  text-align: center; /* Center text */
  font-size: 14px; /* Adjusted font size for readability */
  line-height: 1.2; /* Adjust line height for better text spacing */
}

@media (max-width: 768px) {
  .button-container {
    flex-direction: column;
    align-items: center;
  }

  .action-button {
    width: 120px; /* Adjust square size on mobile */
    height: 120px; /* Adjust square size on mobile */
  }

  .button-text {
    font-size: 12px; /* Adjust font size on mobile */
  }
}
</style>
