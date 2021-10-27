<template>
  <div>
    <h1>Edit Item</h1>
    <div>
      <label class="form-label">Shop Name</label>
      <input type="text" class="form-control" v-model="shopName" />
    </div>

    <div>
      <label class="form-label">Item Name</label>
      <input type="text" class="form-control" v-model="itemName" />
    </div>
    <div>
      <label class="form-label">Item Description</label>
      <input type="text" class="form-control" v-model="itemDesc" />
    </div>
    <div>
      <label class="form-label">Item Price</label>
      <input type="number" class="form-control" v-model="itemPrice" />
    </div>
    <div>
      <label class="form-label">Bundle Deal Available?</label>
      <div>
        <div class="form-check form-check-inline">
          <input
            type="radio"
            name="bundleDeal"
            value="available"
            v-model="bundleDeal"
          />
          <label style="display: inline-block; margin-left: 10px">
            Available
          </label>
        </div>

        <div class="form-check form-check-inline">
          <input
            type="radio"
            name="bundleDeal"
            value="not-available"
            v-model="bundleDeal"
          />
          <label style="display: inline-block; margin-left: 10px">
            Not Available
          </label>
        </div>
      </div>
    </div>
    <div>
      <label class="form-label">Contact Information</label>
      <input type="number" class="form-control" v-model="contactInfo" />
    </div>
    <div>
      <label class="form-label">Delivery Method</label>
      <select class="form-control" v-model="delivery">
        <option value="standard">Standard Delivery (3-5 days)</option>
        <option value="express">Express Delivery (1-2 days)</option>
        <option value="selfPickup">Self Pickup</option>
      </select>
    </div>
    <div>
      <label>Category</label>
      <div>
        <label>Deserts</label>
        <input type="checkbox" value="desert" v-model="category" />
        <label style="margin-left: 10px">Bread</label>
        <input type="checkbox" value="bread" v-model="category" />
        <label style="margin-left: 10px">Pizza</label>
        <input type="checkbox" value="pizza" v-model="category" />
        <label style="margin-left: 10px">Vegan</label>
        <input type="checkbox" value="vegan" v-model="category" />
        <label tyle="margin-left: 10px">Gluten Free</label>
        <input type="checkbox" value="glutenFree" v-model="category" />
      </div>
    </div>

    <button class="mt-3 btn btn-primary" v-on:click="processUpdate">
      Update Item
    </button>
  </div>
</template>

<script>
import axios from "axios";
// const API_URL = "https://8080-maroon-wallaby-uyspmwyr.ws-us15.gitpod.io";
// const API_URL = "https://8080-kumquat-mouse-2z4yehyd.ws-us18.gitpod.io";
const API_URL = "https://dcyx-tgc-assignment2.herokuapp.com";
export default {
  name: "EditItem",
  data: function () {
    return {
      itemName: "",
      itemDesc: "",
      itemPrice: "",
      bundleDeal: "",
      contactInfo: "",
      category: [],
      delivery: "",
      shopName: "",
    };
  },
  props: ["itemId"],
  mounted: async function () {
    let response = await axios.get(API_URL + "/item_record/" + this.itemId);
    this.itemName = response.data.itemName;
    this.itemDesc = response.data.itemDesc;
    this.itemPrice = response.data.itemPrice;
    this.bundleDeal = response.data.bundleDeal;
    this.contactInfo = response.data.contactInfo;
    this.delivery = response.data.delivery;
    this.category = response.data.category;
    this.shopName = response.data.shopName;
  },
  methods: {
    processUpdate: async function () {
      await axios.patch(API_URL + "/item_record/" + this.itemId, {
        itemName: this.itemName,
        itemDesc: this.itemDesc,
        itemPrice: this.itemPrice,
        bundleDeal: this.bundleDeal,
        contactInfo: this.contactInfo,
        delivery: this.delivery,
        category: this.category,
        shopName: this.shopName,
      });
      this.$emit("item-updated");
    },
  },
};
</script>