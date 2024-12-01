<template>
  <div class="shipment-details">
    <div class="shipment-options">
      <h2>Shipment Details: Customs Clearance</h2>

      <!-- Transport Type Radio Buttons -->
      <a-form
          layout="horizontal"
          label-col="{ span: 6 }"
          wrapper-col="{ span: 18 }"
          @submit.prevent="handleSubmit"
      >
        <div class="form-row-first">
          <div>
            <h3 class="section-title">Type of Cargo</h3>
            <a-form-item>
              <a-radio-group v-model="cargoType" class="cargo-type-radio-group">
                <a-radio-button value="EUtoUK">
                  <span class="radio-text">EU Clearance (Europe to UK)</span>
                </a-radio-button>
                <a-radio-button value="RestOfWorldToUK">
                  <span class="radio-text">Rest of the World (to UK)</span>
                </a-radio-button>
              </a-radio-group>
            </a-form-item>
          </div>
          <div>
            <h3 class="section-title">Transport Mode</h3>
            <a-form-item>
              <a-radio-group v-model="selectedTransport" class="transport-radio-group">
                <a-radio-button value="road">
                  <img src="../../../public/icons/road-icon.png" alt="Road" class="radio-icon" />
                  <span class="radio-text">Road</span>
                </a-radio-button>
                <a-radio-button value="air">
                  <img src="../../../public/icons/air-icon.png" alt="Air" class="radio-icon" />
                  <span class="radio-text">Air</span>
                </a-radio-button>
                <a-radio-button value="sea">
                  <img src="../../../public/icons/sea-icon.png" alt="Sea" class="radio-icon" />
                  <span class="radio-text">Sea</span>
                </a-radio-button>
              </a-radio-group>
            </a-form-item>
          </div>
          <div>
            <h3 class="section-title">Declaration Type</h3>
            <a-form-item>
              <a-radio-group v-model="declarationType">
                <a-radio-button value="ImportDeclaration">
                  <span class="radio-text">Import declaration</span>
                </a-radio-button>
                <a-radio-button value="ExportDeclaration">
                  <span class="radio-text">Export declaration</span>
                </a-radio-button>
              </a-radio-group>
            </a-form-item>
          </div>
        </div>


        <div class="form-row">
          <div>
            <h3 class="section-title">Temperature Controlled Goods</h3>
            <a-form-item>
              <a-radio-group v-model="temperatureControlled" @change="toggleTemperatureFields">
                <a-radio value="yes">Yes</a-radio>
                <a-radio value="no">No</a-radio>
              </a-radio-group>
            </a-form-item>
          </div>
          <!-- Conditional Temperature Fields -->
          <div class="temperature-fields">
            <a-form-item label="Min Temperature (°C)">
              <a-input
                  v-model="minTemperature"
                  placeholder="Min Temperature (°C)"
                  :disabled="temperatureControlled === 'no'"
              />
            </a-form-item>
            <a-form-item label="Max Temperature (°C)">
              <a-input
                  v-model="maxTemperature"
                  placeholder="Max Temperature (°C)"
                  :disabled="temperatureControlled === 'no'"
              />
            </a-form-item>
          </div>
        </div>
          <div class="form-row-details">
            <!-- Receiver Details Section -->
            <div class="form-column">
              <h3 class="section-title">Receiver Details</h3>
              <div class="input-group">
                <label for="companyNameReceiver" class="label-text">Company Name</label>
                <a-form-item>
                  <a-input id="companyNameReceiver" v-model="receiverDetails.companyName" placeholder="Enter Company Name" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="companyAddressReceiver" class="label-text">Company Address</label>
                <a-form-item>
                  <a-input id="companyAddressReceiver" v-model="receiverDetails.companyAddress" placeholder="Enter Company Address" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="eoriReceiver" class="label-text">EORI No</label>
                <a-form-item>
                  <a-input id="eoriReceiver" v-model="receiverDetails.eori" placeholder="Enter EORI no." />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="vatReceiver" class="label-text">VAT No</label>
                <a-form-item>
                  <a-input id="vatReceiver" v-model="receiverDetails.vat" placeholder="Enter VAT no." />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="emailReceiver" class="label-text">Contact Email</label>
                <a-form-item>
                  <a-input id="emailReceiver" v-model="receiverDetails.email" placeholder="Enter email address" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="contactReceiver" class="label-text">Contact No</label>
                <a-form-item>
                  <a-input id="contactReceiver" v-model="receiverDetails.contact" placeholder="Enter contact number" />
                </a-form-item>
              </div>
              <!-- Buyer Toggle -->
              <a-form-item label="Buyer (if different from Receiver)">
                <a-checkbox v-model="isBuyerDifferent">Yes, Buyer details are different</a-checkbox>
              </a-form-item>
            </div>

            <!-- Sender Details Section -->
            <div class="form-column">
              <h3 class="section-title">Sender Details</h3>
              <div class="input-group">
                <label for="companyNameSender" class="label-text">Company Name</label>
                <a-form-item>
                  <a-input id="companyNameSender" v-model="senderDetails.companyName" placeholder="Enter Company Name" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="companyAddressSender" class="label-text">Company Address</label>
                <a-form-item>
                  <a-input id="companyAddressSender" v-model="senderDetails.companyAddress" placeholder="Enter Company Address" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="eoriSender" class="label-text">EORI No</label>
                <a-form-item>
                  <a-input id="eoriSender" v-model="senderDetails.eori" placeholder="Enter EORI no." />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="vatSender" class="label-text">VAT No</label>
                <a-form-item>
                  <a-input id="vatSender" v-model="senderDetails.vat" placeholder="Enter VAT no." />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="emailSender" class="label-text">Contact Email</label>
                <a-form-item>
                  <a-input id="emailSender" v-model="senderDetails.email" placeholder="Enter email address" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="contactSender" class="label-text">Contact No</label>
                <a-form-item>
                  <a-input id="contactSender" v-model="senderDetails.contact" placeholder="Enter contact number" />
                </a-form-item>
              </div>
              <!-- Seller Toggle -->
              <a-form-item label="Seller (if different from Sender)">
                <a-checkbox v-model="isSellerDifferent">Yes, Seller details are different</a-checkbox>
              </a-form-item>
            </div>
          </div>

          <div class="form-row-details">
            <!-- Additional Buyer Details -->
            <div v-if="isBuyerDifferent" class="additional-details">
              <h3 class="section-title">Buyer Details</h3>
              <div class="input-group">
                <label for="companyNameBuyer" class="label-text">Company Name</label>
                <a-form-item>
                  <a-input id="companyNameBuyer" v-model="buyerDetails.companyName" placeholder="Enter Company Name" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="companyAddressBuyer" class="label-text">Company Address</label>
                <a-form-item>
                  <a-input id="companyAddressBuyer" v-model="buyerDetails.companyAddress" placeholder="Enter Company Address" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="eoriBuyer" class="label-text">EORI No</label>
                <a-form-item>
                  <a-input id="eoriBuyer" v-model="buyerDetails.eori" placeholder="Enter EORI no." />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="vatBuyer" class="label-text">VAT No</label>
                <a-form-item>
                  <a-input id="vatBuyer" v-model="buyerDetails.vat" placeholder="Enter VAT no." />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="emailBuyer" class="label-text">Contact Email</label>
                <a-form-item>
                  <a-input id="emailBuyer" v-model="buyerDetails.email" placeholder="Enter email address" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="contactBuyer" class="label-text">Contact No</label>
                <a-form-item>
                  <a-input id="contactBuyer" v-model="buyerDetails.contact" placeholder="Enter contact number" />
                </a-form-item>
              </div>
            </div>

            <!-- Additional Seller Details -->
            <div v-if="isSellerDifferent" class="additional-details">
              <h3 class="section-title">Seller Details</h3>
              <div class="input-group">
                <label for="companyNameSeller" class="label-text">Company Name</label>
                <a-form-item>
                  <a-input id="companyNameSeller" v-model="sellerDetails.companyName" placeholder="Enter Company Name" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="companyAddressSeller" class="label-text">Company Address</label>
                <a-form-item>
                  <a-input id="companyAddressSeller" v-model="sellerDetails.companyAddress" placeholder="Enter Company Address" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="eoriSeller" class="label-text">EORI No</label>
                <a-form-item>
                  <a-input id="eoriSeller" v-model="sellerDetails.eori" placeholder="Enter EORI no." />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="vatSeller" class="label-text">VAT No</label>
                <a-form-item>
                  <a-input id="vatSeller" v-model="sellerDetails.vat" placeholder="Enter VAT no." />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="emailSeller" class="label-text">Contact Email</label>
                <a-form-item>
                  <a-input id="emailSeller" v-model="sellerDetails.email" placeholder="Enter email address" />
                </a-form-item>
              </div>
              <div class="input-group">
                <label for="contactSeller" class="label-text">Contact No</label>
                <a-form-item>
                  <a-input id="contactSeller" v-model="sellerDetails.contact" placeholder="Enter contact number" />
                </a-form-item>
              </div>
            </div>
          </div>


        <!-- Origin and Destination Fields -->
        <div class="form-row box-rounded">
          <a-form-item label="Origin" class="form-item-half">
            <a-button @click="openPopup('origin')">Select Origin</a-button>
            <p v-if="originDetails" class="selected-details">
              {{ originDetails.type }}, {{ originDetails.country }}, {{ originDetails.postCode }}
            </p>
          </a-form-item>
          <a-form-item label="Destination" class="form-item-half">
            <a-button @click="openPopup('destination')">Select Destination</a-button>
            <p v-if="destinationDetails" class="selected-details">
              {{ destinationDetails.type }}, {{ destinationDetails.country }}, {{ destinationDetails.postCode }}
            </p>
          </a-form-item>
          <a-form-item label="Loads" class="form-item-half">
            <a-button @click="openPopup('loads')">Loads</a-button>
          </a-form-item>
          <!-- Goods Value Field -->
          <a-form-item label="Goods">
            <a-button @click="openGoodsValueModal">Enter Goods Details</a-button>
            <p v-if="goodsValue" class="selected-details">
              USD {{ goodsValue }} | {{ goodsReadinessText }}
            </p>
          </a-form-item>
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

      <!-- Origin/Destination Popup -->
      <a-modal
          v-model:visible="isPopupVisible"
          title="Enter Shipping Details"
          @ok="handlePopupSubmit"
          @cancel="closePopup"
      >
        <a-form>
          <a-form-item label="Type">
            <a-select v-model="popupDetails.type" placeholder="Select Type">
              <a-select-option value="Factory Warehouse">Factory Warehouse</a-select-option>
              <a-select-option value="Fulfillment Center">Fulfillment Center</a-select-option>
              <a-select-option value="Retail Store">Retail Store</a-select-option>
            </a-select>
          </a-form-item>
          <a-form-item label="Country">
            <a-select v-model="popupDetails.country" placeholder="Select Country">
              <a-select-option value="Albania">Albania</a-select-option>
              <a-select-option value="Algeria">Algeria</a-select-option>
              <a-select-option value="Argentina">Argentina</a-select-option>
            </a-select>
          </a-form-item>
          <a-form-item label="City or Post Code">
            <a-input v-model="popupDetails.postCode" placeholder="Enter City or Post Code" />
          </a-form-item>
        </a-form>
      </a-modal>

      <!-- Goods Value Modal -->
      <a-modal
          v-model:visible="isGoodsModalVisible"
          title="Tell us about your goods"
          @ok="handleGoodsModalSubmit"
          @cancel="closeGoodsValueModal"
      >
        <a-form>
          <a-form-item label="Goods Value">
            <a-input-group compact>
              <a-input
                  v-model="goodsValueInput"
                  placeholder="Enter Goods Value"
                  style="width: 70%"
                  type="number"
              />
              <a-select v-model="goodsCurrency" style="width: 30%">
                <a-select-option value="USD">USD</a-select-option>
                <a-select-option value="EUR">EUR</a-select-option>
                <a-select-option value="GBP">GBP</a-select-option>
              </a-select>
            </a-input-group>
          </a-form-item>

          <a-form-item>
            <a-checkbox v-model="isHazardous">Shipment contains hazardous goods (Commercial only)</a-checkbox>
          </a-form-item>

          <a-form-item label="Are your goods ready?">
            <a-select v-model="goodsReadiness" placeholder="Select readiness status">
              <a-select-option value="readyNow">Ready now</a-select-option>
              <a-select-option value="oneWeek">Will be ready in a week</a-select-option>
              <a-select-option value="twoWeeks">Will be ready in more than two weeks</a-select-option>
            </a-select>
          </a-form-item>
        </a-form>
      </a-modal>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cargoType: "",
      selectedTransport: "", // Default selection for transport type
      declarationType: "",
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
      originDetails: null, // Store Origin details
      destinationDetails: null, // Store Destination details
      isPopupVisible: false,
      popupDetails: {
        type: "",
        country: "",
        postCode: "",
      },
      popupContext: "",
      goodsValue: null,
      goodsCurrency: "USD",
      isHazardous: false,
      goodsReadiness: null,
      goodsReadinessText: "",
      isGoodsModalVisible: false,
      goodsValueInput: null,
      receiverDetails: {
        companyName: '',
        companyAddress: '',
        eori: '',
        vat: '',
        email: '',
        contact: '',
      },
      // Buyer Details (conditionally shown)
      buyerDetails: {
        companyName: '',
        companyAddress: '',
        eori: '',
        vat: '',
        email: '',
        contact: '',
      },
      isBuyerDifferent: false, // Toggle for buyer details

      // Sender Details
      senderDetails: {
        companyName: '',
        companyAddress: '',
        eori: '',
        vat: '',
        email: '',
        contact: '',
      },
      // Seller Details (conditionally shown)
      sellerDetails: {
        companyName: '',
        companyAddress: '',
        eori: '',
        vat: '',
        email: '',
        contact: '',
      },
      isSellerDifferent: false,
    };
  },
  methods: {
    openGoodsValueModal() {
      this.isGoodsModalVisible = true;
    },
    closeGoodsValueModal() {
      this.isGoodsModalVisible = false;
    },
    handleGoodsModalSubmit() {
      this.goodsValue = this.goodsValueInput;
      this.goodsReadinessText = this.getGoodsReadinessText();
      this.closeGoodsValueModal();
    },
    getGoodsReadinessText() {
      switch (this.goodsReadiness) {
        case "readyNow":
          return "Yes my goods are ready";
        case "twoWeeks":
          return "Will be ready in two weeks";
        case "moreThanTwoWeeks":
          return "Will be ready in more than two weeks";
        default:
          return "Unknown readiness status";
      }
    },
    openPopup(context) {
      this.popupContext = context;
      this.isPopupVisible = true;
      if (this.popupContext === "origin" && this.originDetails !== null){
        this.popupDetails = { type: this.originDetails.type, country: this.originDetails.country, postCode: this.originDetails.postCode };
      } else if (this.popupContext === "destination" && this.destinationDetails !== null){
        this.popupDetails = { type: this.destinationDetails.type, country: this.destinationDetails.country, postCode: this.destinationDetails.postCode };
      } else {
        this.popupDetails = { type: "", country: "", postCode: "" };
      }
    },
    closePopup() {
      this.isPopupVisible = false;
      this.popupContext = "";
    },
    handlePopupSubmit() {
      if (this.popupContext === "origin") {
        this.originDetails = { ...this.popupDetails };
      } else if (this.popupContext === "destination") {
        this.destinationDetails = { ...this.popupDetails };
      }
      this.closePopup();
    },
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

.label-text {
  font-size: 14px;
  font-weight: normal;
  margin-bottom: 24px
}
.label-input {
  display: flex;
  gap: 1rem;
  align-items: center;
}

.form-row {
  display: flex;
  align-items: center;
  gap: 4rem;
  margin-bottom: 10px;
}

.form-row-first {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  justify-content: space-between;
  flex-wrap: wrap;
}

.form-item-half {
  flex: 1;
}

.right-border {
  border-right: 0.1px solid black;
}

.delete-button {
  margin-left: 10px;
  color: red;
}

.additional-services {
  margin-top: 20px;
}

.custom-services {
  margin-top: 20px;
}

.custom-dropdown {
  margin-top: 10px;
}

.form-row {
  display: flex;
  flex-wrap: wrap;
  gap: 30px;
  margin-bottom: 20px;
}

.form-column {
  flex: 1;
  min-width: 300px;
}

.input-group {
  margin-bottom: 20px;
}

a-form-item {
  margin-bottom: 10px;
}

a-input {
  width: 100%;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

a-checkbox {
  margin-top: 10px;
}

@media (max-width: 768px) {
  .form-row {
    flex-direction: column;
  }

  .form-column {
    width: 100%;
  }
}

.form-row-details {
  display: flex;
  flex-wrap: wrap;
  gap: 30px;
  margin-bottom: 20px;
}

.form-column {
  flex: 1;
  min-width: 300px;
}

.box-rounded {
  border: 0.1px dashed black;
  border-radius: 10px;
  padding: 1rem;
}

.additional-details {
  margin-top: 20px;
  flex: 1;
  width: 50%; /* Make it occupy half of the available space */
  box-sizing: border-box; /* Ensure padding and borders are included in the width */
}

/* Ensure it doesn't exceed its parent's width, making it responsive */
@media (max-width: 768px) {
  .additional-details {
    width: 100%; /* On smaller screens, make it occupy full width */
  }
}

@media (max-width: 768px) {
  .form-row-details {
    flex-direction: column;
  }

  .form-column {
    width: 100%;
  }
}

</style>
