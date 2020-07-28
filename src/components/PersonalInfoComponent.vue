<template>
  <div>
    <!--Name-->
    First name Last name
    <div>
      <input type="text" @change="modData" name="first_name" :value="first_name">
      <input type="text" @change="modData" name="last_name" :value="last_name">
    </div>
    <!--Gender-->
    Gender
    <div>
      <input @change="modData" type="radio" name="gender" v-model="genderProcess" value="0">Secret
      <br>
      <input @change="modData" type="radio" name="gender" v-model="genderProcess" value="1">Male
      <br>
      <input @change="modData" type="radio" name="gender" v-model="genderProcess" value="2">Female
      <br>
    </div>
    <!--Address-->
    Address
    <div>
      <input type="text" @change="modData" name="address" :value="address" :disabled="is_homeless">
      <div>
        <input type="checkbox" @change="modData2" name="is_homeless">此客戶居無定所
      </div>
    </div>
    <!--Job-->
    Job
    <select @change="modData" name="job" v-model="jobProcess">
      <option value="null">保密</option>
      <option value="agent">調查員</option>
      <option value="secret_agent">秘密調查員</option>
      <option value="agent_of_secret_agent">秘密調查員的調查員</option>
    </select>
    <br>
    <!--Note-->
    Note
    <br>
    {{countChars}}/2000 characters
    <div>
      <input @input="modData" type="text" name="note" :value="note" maxlength="2000">
    </div>
  </div>
</template>
<script>
export default {
  name: "PersonalInfoComponent",
  computed: {
    genderProcess: {
      get: function() {
        return this.gender;
      },
      set: function() {}
    },
    jobProcess: {
      get: function() {
        return this.job;
      },
      set: function() {}
    },
    countChars() {
      if (this.note == null) return 0;
      else return this.note.length;
    }
  },
  methods: {
    modData2(e) {
      console.log(e.target);
      const e2 = { target: { name: "address", value: "", type: "text" } };
      this.modData(e2);
      this.modData(e);
    }
  },
  props: {
    modData: {
      type: Function
    },
    first_name: {
      type: String
    },
    last_name: {
      type: String
    },
    gender: {
      type: Number
    },
    address: {
      type: String
    },
    is_homeless: {
      type: Boolean
    },
    job: {
      type: String
    },
    note: {
      type: String
    }
  },
  mounted: function() {}
};
</script>
