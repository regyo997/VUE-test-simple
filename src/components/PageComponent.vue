<template>
  <div>
    <!--表單顯示與切換-->
    <div>Form {{currentPage.id}}</div>跳至
    <select @change="changePage">
      <option v-for="(thePage,index) of pages" :key="index" :value="thePage.id">{{thePage.id}}</option>
    </select>

    <!--<SelectPageComponent :pages="pages"/>-->
    ,共{{pages.length}}張
    <!--Log按鈕-->
    <div>
      <button @click="logThisForm">Log this form</button>
      <button @click="logAllForm">Log all form</button>
      <button @click="deleteThisForm" v-show="pages.length>1">delete this form</button>
    </div>

    <!--Form主體-->
    <button @click="changeTab" value="personalInfo" class="btn btn-info">Personal Info</button>
    <button @click="changeTab" value="orders" class="btn btn-info">Orders</button>
    <button @click="changeTab" value="overview" class="btn btn-info">Overview</button>
    <hr>
    <component :is="this.theTab" v-bind="tabInput" :modData="modData"></component>

    <!--換頁按鈕-->
    <div>
      <button @click="previousPage">Previous</button>
      <button
        @click="changePage"
        v-for="(thePage,index) of pages"
        :key="index"
        :value="thePage.id"
      >{{index+1}}</button>
      <button @click="nextPage">Next</button>
    </div>
  </div>
</template>

<script>
import SelectPageComponent from "./SelectPageComponent.vue";
import PersonalInfoComponent from "./PersonalInfoComponent.vue";
import OrdersComponent from "./OrdersComponent.vue";
import OverviewComponent from "./OverviewComponent.vue";

export default {
  name: "PageComponent",
  components: {
    SelectPageComponent,
    personalInfo: PersonalInfoComponent,
    orders: OrdersComponent,
    overview: OverviewComponent
  },
  data() {
    return {
      currentPageNum: 0,
      deletedPages: [],
      pages: [{ id: "" }],
      theTab: "personalInfo"
    };
  },
  methods: {
    logThisForm() {
      console.log(this.pages[this.currentPageNum]);
    },
    logAllForm() {
      console.log(this.pages);
    },
    deleteThisForm() {
      if (this.currentPageNum < this.pages.length - 1) {
        this.pages.splice(this.currentPageNum, 1);
      } else {
        this.pages.splice(this.currentPageNum, 1);
        this.currentPageNum = this.pages.length - 1;
      }
    },
    previousPage() {
      if (this.currentPageNum > 0) this.currentPageNum--;
    },
    nextPage() {
      if (this.currentPageNum < this.pages.length - 1) this.currentPageNum++;
    },
    changePage(e) {
      this.currentPageNum = this.pages.map(x => x.id).indexOf(e.target.value);
    },
    changeTab(e) {
      this.theTab = e.target.value;
    },
    modData(e) {
      let value = e.target.value;
      const { type, name, checked } = e.target;
      if (this.theTab === "personalInfo") {
        if (
          typeof this.pages[this.currentPageNum]["personal_info"][name] ===
          "number"
        )
          value = parseInt(value, 10);
        else if (type === "checkbox") value = checked;
        this.pages[this.currentPageNum]["personal_info"][name] = value;
      } else if (this.theTab === "orders")
        if (type === "checkbox") {
          if (checked) {
            this.pages[this.currentPageNum]["orders"]["banana_condiments"].push(
              value
            );
            console.log(
              this.pages[this.currentPageNum]["orders"]["banana_condiments"]
            );
          } else {
            const index = this.pages[this.currentPageNum]["orders"][
              "banana_condiments"
            ].indexOf(value);
            this.pages[this.currentPageNum]["orders"][
              "banana_condiments"
            ].splice(index, 1);
            console.log(
              this.pages[this.currentPageNum]["orders"]["banana_condiments"]
            );
          }
        } else {
          if (
            typeof this.pages[this.currentPageNum]["orders"][name] === "number"
          )
            value = parseInt(value, 10);
          this.pages[this.currentPageNum]["orders"][name] = value;
        }
    }
  },
  computed: {
    currentPage() {
      return this.pages[this.currentPageNum];
    },
    pageNumArray() {
      const arr = [];
      for (var i = 0; i < this.pages.length; i++) {
        arr.push(i + 1);
      }
      return arr;
    },
    pageIds() {
      return this.pages.map(page => page.id);
    },
    tabInput() {
      if (this.theTab === "personalInfo") return this.currentPage.personal_info;
      else if (this.theTab === "orders") return this.currentPage.orders;
      else return this.currentPage;
    }
  },
  mounted: function() {
    this.pages = [
      {
        id: "A0000001",
        personal_info: {
          first_name: "Fishman",
          last_name: "ILike",
          gender: 1,
          address: "ABCDEFG",
          is_homeless: false,
          job: null,
          note: null
        },
        orders: {
          apple_count: 1,
          banana_condiments: ["chocolate", "chili", "garlic", "soy_sauce"]
        }
      },
      {
        id: "A0000002",
        personal_info: {
          first_name: null,
          last_name: "Somebody",
          gender: 2,
          address: "QWERTY",
          is_homeless: false,
          job: "secret_agent",
          note: "Hello, world"
        },
        orders: { apple_count: 15, banana_condiments: [] }
      },
      {
        id: "A0000003",
        personal_info: {
          first_name: "Painter",
          last_name: null,
          gender: 0,
          address: null,
          is_homeless: true,
          job: "agent_of_secret_agent",
          note: "Strange"
        },
        orders: { apple_count: 1, banana_condiments: ["herbal_cream"] }
      },
      {
        id: "A0000004",
        personal_info: {
          first_name: "President",
          last_name: "Mr.",
          gender: 1,
          address: "America",
          is_homeless: false,
          job: "secret_agent",
          note: "He is the president!"
        },
        orders: {
          apple_count: 100,
          banana_condiments: [
            "chocolate",
            "strawberry",
            "flax",
            "miso",
            "chili",
            "garlic",
            "soy_sauce",
            "thick_soy_sauce"
          ]
        }
      },
      {
        id: "A0000005",
        personal_info: {
          first_name: "Pepper",
          last_name: "Dr.",
          gender: 0,
          address: "Farm",
          is_homeless: false,
          job: "agent",
          note: "胡椒博士現身"
        },
        orders: { apple_count: 5, banana_condiments: ["chili", "garlic"] }
      }
    ];
  }
};
</script>