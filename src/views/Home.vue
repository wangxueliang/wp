<template>
  <div class="upload">
    <div style="margin-top: 20px;margin-left: 20px;">
      测试搜索框
      <a-select
        mode="multiple"
        labelInValue
        :value="value"
        placeholder="使用中文输入法输入英文显示不全"
        style="width:200px"
        :filterOption="false"
        @search="fetchUser"
        @change="handleChange"
        :notFoundContent="fetching ? undefined : null"
        @input="inputKeydownData"
      >
        <a-spin v-if="fetching" slot="notFoundContent" size="small" />
        <a-select-option v-for="d in data" :key="d.value">{{
          d.text
        }}</a-select-option>
      </a-select>
    </div>
  </div>
</template>
<script>
// import jsonp from 'fetch-jsonp';

export default {
  data() {
    this.lastFetchId = 0;
    // this.fetchUser = debounce(this.fetchUser, 800);
    return {
      data: [],
      value: [],
      fetching: false
    };
  },
  methods: {
    inputKeydownData(val) {
      console.log(val);
    },
    fetchUser(value) {
      console.log("fetching user", value);
      this.lastFetchId += 1;
      const fetchId = this.lastFetchId;
      this.data = [];
      this.fetching = true;
      fetch("https://randomuser.me/api/?results=5")
        .then(response => response.json())
        .then(body => {
          if (fetchId !== this.lastFetchId) {
            // for fetch callback order
            return;
          }
          const data = body.results.map(user => ({
            text: `${user.name.first} ${user.name.last}`,
            value: user.login.username
          }));
          this.data = data;
          this.fetching = false;
        });
    },
    handleChange(value) {
      console.log("变化", value);
      Object.assign(this, {
        value,
        data: [],
        fetching: false
      });
    }
  },
  mounted() {
    console.log("测试搜索框");
  }
};
</script>
