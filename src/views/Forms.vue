<template>
  <div class="shipment-details">
    <div class="shipment-options">
      <h2>Book Your Shipment</h2>

      <!-- Transport Type Radio Buttons -->
      <a-form
          layout="horizontal"
          label-col="{ span: 6 }"
          wrapper-col="{ span: 18 }"
          @submit.prevent="handleSubmit"
      >
        <a-form-item label="Transport Type">
          <a-radio-group v-model="selectedTransport" class="transport-radio-group">
            <a-radio-button value="road">
              <img src="../../public/icons/road-icon.png" alt="Road" class="radio-icon" />
              <span class="radio-text">Road</span>
            </a-radio-button>
            <a-radio-button value="air">
              <img src="../../public/icons/air-icon.png" alt="Air" class="radio-icon" />
              <span class="radio-text">Air</span>
            </a-radio-button>
            <a-radio-button value="sea">
              <img src="../../public/icons/sea-icon.png" alt="Sea" class="radio-icon" />
              <span class="radio-text">Sea</span>
            </a-radio-button>
          </a-radio-group>
        </a-form-item>

        <!-- Additional Fields -->
        <div class="form-row">
          <a-form-item label="Product Commodity Code" class="form-item-half">
            <a-input placeholder="Enter Commodity Code" />
          </a-form-item>
          <a-form-item label="Invoice Value" class="form-item-half">
            <a-input placeholder="Enter Invoice Value" />
          </a-form-item>
        </div>
        <div class="form-row">
          <a-form-item label="Cargo Type">
            <a-radio-group v-model="cargoType" class="cargo-type-radio-group">
              <a-radio value="food">Food Product</a-radio>
              <a-radio value="non-food">Non-Food Product</a-radio>
            </a-radio-group>
          </a-form-item>
          <a-form-item label="Temperature Controlled Goods">
            <a-radio-group v-model="temperatureControlled" @change="toggleTemperatureFields">
              <a-radio value="yes">Yes</a-radio>
              <a-radio value="no">No</a-radio>
            </a-radio-group>
          </a-form-item>

          <!-- Conditional Temperature Fields -->
          <div v-if="temperatureControlled === 'yes'" class="temperature-fields">
            <a-form-item label="Min Temperature (°C)">
              <a-input v-model="minTemperature" placeholder="Min Temperature (°C)" />
            </a-form-item>
            <a-form-item label="Max Temperature (°C)">
              <a-input v-model="maxTemperature" placeholder="Max Temperature (°C)" />
            </a-form-item>
          </div>
        </div>


        <!-- Collection Section -->
        <div class="form-collection-delivery">
          <div class="collection">
            <h3 class="section-title">Collection</h3>
            <div class="form-row">
              <a-form-item label="Pickup Date">
                <a-date-picker style="width: 100%" placeholder="Select date" />
              </a-form-item>
              <a-form-item label="Forklift Required">
                <a-radio-group v-model="collectionForklift">
                  <a-radio value="yes">Yes</a-radio>
                  <a-radio value="no">No</a-radio>
                </a-radio-group>
              </a-form-item>
            </div>


            <!-- Collection References -->
            <div v-for="(ref, index) in collectionReferences" :key="'collection-' + index" class="form-row">
              <a-form-item class="form-item-half" label="Reference Text">
                <a-input v-model="ref.text" placeholder="Reference Text" />
              </a-form-item>
              <a-form-item class="form-item-half" label="Type">
                <a-select v-model="ref.type" placeholder="Select Type">
                  <a-select-option value="invoicing">Invoicing Reference</a-select-option>
                  <a-select-option value="order">Order Reference</a-select-option>
                  <a-select-option value="receiver">Receiver Reference</a-select-option>
                  <a-select-option value="sender">Sender Reference</a-select-option>
                  <a-select-option value="other">Other</a-select-option>
                </a-select>
              </a-form-item>
              <a-button
                  type="link"
                  icon="delete"
                  class="delete-button"
                  @click="removeCollectionReference(index)"
              >
                Remove
              </a-button>
            </div>
            <a-button type="primary" @click="addCollectionReference">Add Another</a-button>
          </div>

          <!-- Delivery Section -->
          <div class="delivery">
            <h3 class="section-title">Delivery</h3>
            <div class="form-row">
              <a-form-item label="Delivery Date">
                <a-date-picker style="width: 100%" placeholder="Select date" />
              </a-form-item>
              <a-form-item label="Forklift Required">
                <a-radio-group v-model="deliveryForklift">
                  <a-radio value="yes">Yes</a-radio>
                  <a-radio value="no">No</a-radio>
                </a-radio-group>
              </a-form-item>
            </div>

            <!-- Delivery References -->
            <div v-for="(ref, index) in deliveryReferences" :key="'delivery-' + index" class="form-row">
              <a-form-item class="form-item-half" label="Reference Text">
                <a-input v-model="ref.text" placeholder="Reference Text" />
              </a-form-item>
              <a-form-item class="form-item-half" label="Type">
                <a-select v-model="ref.type" placeholder="Select Type">
                  <a-select-option value="invoicing">Invoicing Reference</a-select-option>
                  <a-select-option value="order">Order Reference</a-select-option>
                  <a-select-option value="receiver">Receiver Reference</a-select-option>
                  <a-select-option value="sender">Sender Reference</a-select-option>
                  <a-select-option value="other">Other</a-select-option>
                </a-select>
              </a-form-item>
              <a-button
                  type="link"
                  icon="delete"
                  class="delete-button"
                  @click="removeDeliveryReference(index)"
              >
                Remove
              </a-button>
            </div>
            <a-button type="primary" @click="addDeliveryReference">Add Another</a-button>
          </div>
        </div>

        <!-- Additional Services Section -->
        <div class="additional-services">
          <h3 class="section-title">Additional Services</h3>

          <div class="form-row">
            <a-form-item label="Cargo Insurance:">
              <a-radio-group v-model="cargoInsurance">
                <a-radio value="yes">Yes</a-radio>
                <a-radio value="no">No</a-radio>
              </a-radio-group>
              <!--            <span v-if="cargoInsurance === 'no'" class="info-text">-->
              <!--              I don't want cargo insurance and I am aware of forwarder's limited liability.-->
              <!--            </span>-->
            </a-form-item>

            <a-form-item label="Port Charges:">
              <a-radio-group v-model="portCharges">
                <a-radio value="yes">Yes</a-radio>
                <a-radio value="no">No</a-radio>
              </a-radio-group>
            </a-form-item>
          </div>
        </div>

        <!-- Customs Clearance Section -->
        <div class="custom-services">
          <h3 class="section-title">Custom Services</h3>
          <div class="form-row" style="width: 100%;">
            <a-form-item label="Export Customs Clearance:" style="width: 50%">
              <a-radio-group v-model="exportClearance">
                <a-radio value="yes">Yes</a-radio>
                <a-radio value="no">No</a-radio>
              </a-radio-group>
              <div class="custom-dropdown">
                <!-- Disable select box when exportClearance is 'no' -->
                <a-select v-model="exportClearanceOption" :disabled="exportClearance === 'no'" placeholder="Lookup Options" style="width: 100%">
                  <a-select-option value="cne">by CNE</a-select-option>
                  <a-select-option value="third-party">by 3rd party</a-select-option>
                  <a-select-option value="own-means">by own means</a-select-option>
                </a-select>
              </div>
            </a-form-item>

            <a-form-item label="Import Customs Clearance:" style="width: 50%">
              <a-radio-group v-model="importClearance">
                <a-radio value="yes">Yes</a-radio>
                <a-radio value="no">No</a-radio>
              </a-radio-group>
              <div class="custom-dropdown">
                <!-- Disable select box when importClearance is 'no' -->
                <a-select v-model="importClearanceOption" :disabled="importClearance === 'no'" placeholder="Lookup Options" style="width: 100%">
                  <a-select-option value="cne">by CNE</a-select-option>
                  <a-select-option value="third-party">by 3rd party</a-select-option>
                  <a-select-option value="own-means">by own means</a-select-option>
                </a-select>
              </div>
            </a-form-item>
          </div>
        </div>


        <a-form-item wrapper-col="{ offset: 6, span: 18 }">
          <a-button type="primary" html-type="submit">Submit</a-button>
        </a-form-item>
      </a-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedTransport: "road", // Default selection for transport type
      cargoType: "food", // Default selection for cargo type
      temperatureControlled: "no", // Default value for Temperature Controlled Goods
      minTemperature: "", // Min Temperature value
      maxTemperature: "", // Max Temperature value
      collectionForklift: "no", // Default value for collection forklift requirement
      deliveryForklift: "no", // Default value for delivery forklift requirement
      collectionReferences: [], // Array to store collection references
      deliveryReferences: [], // Array to store delivery references
      cargoInsurance: "no", // Default value for cargo insurance
      portCharges: "no",
      exportClearance: "no", // Default value for export customs clearance
      importClearance: "no", // Default value for import customs clearance
      exportClearanceOption: null, // Selected option for export customs clearance
      importClearanceOption: null, // Selected option for import customs clearance
    };
  },
  methods: {
    handleSubmit() {
      console.log("Selected Transport:", this.selectedTransport);
      console.log("Selected Cargo Type:", this.cargoType);
      console.log("Temperature Controlled:", this.temperatureControlled);
      if (this.temperatureControlled === "yes") {
        console.log("Min Temperature:", this.minTemperature);
        console.log("Max Temperature:", this.maxTemperature);
      }
      console.log("Collection Forklift Required:", this.collectionForklift);
      console.log("Delivery Forklift Required:", this.deliveryForklift);
    },
    addCollectionReference() {
      this.collectionReferences.push({ text: "", type: "" });
    },
    removeCollectionReference(index) {
      this.collectionReferences.splice(index, 1);
    },
    addDeliveryReference() {
      this.deliveryReferences.push({ text: "", type: "" });
    },
    removeDeliveryReference(index) {
      this.deliveryReferences.splice(index, 1);
    },
    toggleTemperatureFields() {
      if (this.temperatureControlled === "no") {
        this.minTemperature = "";
        this.maxTemperature = "";
      }
    },
  },
};
</script>

<style scoped>
.shipment-options {
  padding: 20px;
}

a-radio-button {
  margin-right: 15px;
}

.radio-icon {
  width: 24px;
  margin-right: 10px; /* Adds space between the image and text */
  vertical-align: middle; /* Aligns the image vertically with the text */
}

.radio-text {
  vertical-align: middle; /* Ensures the text is vertically aligned with the image */
}

.cargo-type-radio-group a-radio {
  margin-right: 20px; /* Adds spacing between radio options */
}

.form-collection-delivery {
  display: flex;
  gap: 40px; /* Space between Collection and Delivery sections */
  margin-top: 20px;
  flex-wrap: wrap; /* Make sections wrap on smaller screens */
}

.collection,
.delivery {
  flex: 1; /* Equal width for Collection and Delivery sections */
}

.temperature-fields {
  display: flex;
  gap: 20px;
}

.section-title {
  font-size: 16px;
  font-weight: bold;
  color: #007bff;
  margin-bottom: 10px;
}

.form-row {
  display: flex;
  align-items: center;
  gap: 40px;
  margin-bottom: 10px;
}

.form-item-half {
  flex: 1;
}

.delete-button {
  margin-left: 10px;
  color: red;
}

.add-another {
  margin-top: 20px;
}

.additional-services {
  margin-top: 20px;
}

.info-text {
  display: block;
  font-size: 12px;
  color: gray;
  margin-top: 5px;
}
.additional-services,
.custom-services {
  margin-top: 20px;
}

.info-text {
  display: block;
  font-size: 12px;
  color: gray;
  margin-top: 5px;
}

.custom-dropdown {
  margin-top: 10px;
}
</style>
