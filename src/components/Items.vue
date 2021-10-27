<template>
  <div>
    <h1>Bakers Paradise</h1>
    <button
      v-bind:style="{ backgroundColor: textColor }"
      v-on:click="changeToRed"
    >
      Seller
    </button>

    <div>
      <div>
        <label style="margin right 10px">Search by Item Name</label>
        <input type="text" v-model="searchTerms" />
      </div>
      <div>
        <label style="margin right 10px">Search by Item Description</label>
        <input type="text" v-model="searchTermsDesc" />
      </div>
      <div>
        <label style="margin right 10px">Search by Shop Name</label>
        <input type="text" v-model="searchTermsSN" />
      </div>

      <div>{{ itemCount }} Items</div>
    </div>

    <table class="table table-striped">
      <thead>
        <tr>
          <th>Item Name</th>
          <th>Item Description</th>
          <th>Shop Name</th>
          <th>Price /pc</th>
          <th>Category</th>
          <th>Contact Info</th>
          <th>Bundle Deal?</th>
          <th>Delivery Method</th>
          <th>Action</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="i in filteredItems" v-bind:key="i._id">
          <td>{{ i.itemName }}</td>
          <td>{{ i.itemDesc }}</td>
          <td>{{ i.shopName }}</td>
          <td>{{ i.itemPrice }}</td>
          <td>{{ i.category.join(", ") }}</td>
          <td>{{ i.contactInfo }}</td>
          <td>{{ i.bundleDeal }}</td>
          <td>{{ i.delivery }}</td>
          <td>
            <button class="btn btn-primary btn-sm" v-on:click="update(i._id)">
              Edit
            </button>
          </td>
          <td>
            <button
              class="btn btn-danger btn-sm"
              v-on:click="deleteItem(i._id)"
            >
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>



<script>
import axios from "axios";
// const API_URL = "https://8080-maroon-wallaby-uyspmwyr.ws-us15.gitpod.io";
// const API_URL = "https://8080-kumquat-mouse-2z4yehyd.ws-us18.gitpod.io";
const API_URL = "https://dcyx-tgc-assignment2.herokuapp.com";
export default {
  name: "Items",
  data: function () {
    return {
      items: [],
      searchTerms: "",
      searchTermsDesc: "",
      searchTermsSN: "",
      textColor: "yellow",
    };
  },
  mounted: async function () {
    let response = await axios.get(API_URL + "/item_record");
    this.items = response.data;
  },
  computed: {
    filteredItems: function () {
      let filteredItems = [];
      for (let eachItem of this.items) {
        if (
          eachItem.itemName
            .toLowerCase()
            .includes(this.searchTerms.toLowerCase())
        ) {
          filteredItems.push(eachItem);
        }
      }
      filteredItems = filteredItems.filter((eachItem) => {
        return eachItem.itemDesc
          .toLowerCase()
          .includes(this.searchTermsDesc.toLowerCase());
      });

      filteredItems = filteredItems.filter((eachItem) => {
        return eachItem.shopName
          .toLowerCase()
          .includes(this.searchTermsSN.toLowerCase());
      });

      return filteredItems;
    },

    itemCount: function () {
      return this.filteredItems.length;
    },
  },
  methods: {
    update: function (itemId) {
      this.$emit("update-item", itemId);
    },
    deleteItem: async function (itemId) {
      await axios.delete(API_URL + "/item_record/" + itemId, {
        // itemName: this.itemName,
        // itemDesc: this.itemDesc,
        // itemPrice: this.itemPrice,
        // bundleDeal: this.bundleDeal,
        // contactInfo: this.contactInfo,
        // delivery: this.delivery,
        // category: this.category,
        // shopName: this.shopName,
      });
      this.$emit("delete-item", itemId);
    },
    changeToRed: function () {
      this.textColor = "red";
    },
  },
};
</script>
