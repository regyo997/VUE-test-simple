<template>
  <div>
    <!--誰-->
    以下是{{theName}}的訂單
    <br>
    <li style="list-style-type:disc;">{{orders.apple_count}}顆蘋果</li>
    <li style="list-style-type:disc;" v-show="isBananaNull">香蕉配料</li>
    <ul>
      <li
        v-for="item of orders.banana_condiments"
        :key="item"
        style="list-style-type:circle"
      >{{ingredients[item]}}</li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      ingredients: {
        chocolate: "巧克力醬",
        strawberry: "草莓醬",
        flax: "胡麻醬",
        miso: "味噌",
        chili: "辣椒",
        garlic: "大蒜",
        soy_sauce: "醬油",
        thick_soy_sauce: "醬油膏",
        herbal_cream: "百香膏"
      }
    };
  },
  props: {
    id: {
      type: String
    },
    personal_info: {
      type: Object
    },
    orders: {
      type: Object
    }
  },
  computed: {
    theName() {
      const firstName =
        this.personal_info.first_name === null
          ? ""
          : this.personal_info.first_name;
      const lastName =
        this.personal_info.last_name === null
          ? ""
          : this.personal_info.last_name + " ";
      return lastName + firstName === "" ? "unknown" : lastName + firstName;
    },
    isBananaNull() {
      return this.orders.banana_condiments === null;
    }
  }
};
</script>
