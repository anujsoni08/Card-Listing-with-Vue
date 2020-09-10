<template>
  <div class="container">
    <div class="text-center">
      <div class="d-flex p-5">
        <div v-for="n in 4" v-bind:key="n" class="p-1">
          <div class="card" v-bind:style="{ background: getRandomColor() }">
            <div class="card-body">
              <h5 class="card-title text-left">83</h5>
              <h6 class="card-subtitle text-left mb-2">Registered Users</h6>
              <p class="card-text text-left text-left">so far in our blog, and our website</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div>
      <div>
        <div class="card bootstrap-user-card">
          <p class="card-text text-center bootstrap-user-card-text">Bootstrap 4 User Cards</p>
        </div>
      </div>
    </div>
    <div class="profile-card-list">
      <div v-for="item in data" class="card profile-card" :key="item.id">
        <div class="card-body text-center">
          <img v-bind:src="item.avatar" class="avatar" />
          <h5 class="card-title text-center">{{item.first_name}} {{item.last_name}}</h5>
          <h6 class="card-subtitle text-center mb-2">{{item.email}}</h6>
        </div>
      </div>
    </div>
    <div class="navigation-div">
      <nav>
        <ul class="pagination justify-content-center">
          <li
            class="page-item"
            v-bind:style="{ cursor: pageNumber === 1 ? '' : 'pointer', }"
            v-bind:class="{ disabled: pageNumber == 1 }"
          >
            <a
              tabindex="-1"
              class="page-link"
              v-on:click="getSelectedPageData(pageNumber-1)"
            >Previous</a>
          </li>
          <li
            v-for="(n) in pagesLength"
            :key="n"
            class="page-item"
            v-bind:style="{ cursor: n === pageNumber ? '' : 'pointer', }"
            v-on:click="getSelectedPageData(n)"
            v-bind:class="{'active':(n === pageNumber),'disabled':(n === pageNumber)}"
          >
            <a class="page-link">{{n}}</a>
          </li>
          <li
            class="page-item"
            v-bind:class="{'disabled': pageNumber === totalPages }"
            v-bind:style="{ cursor: pageNumber === totalPages ? '' : 'pointer', }"
          >
            <a class="page-link" v-on:click="getSelectedPageData(pageNumber+1)">Next</a>
          </li>
        </ul>
      </nav>
    </div>
  </div>
</template>

<style>
@import "./style.css";
</style>

<script>
import { axiosInstance } from "../../service/http";

export default {
  name: "Table",

  data: () => ({
    data: {},
    copyData: [],
    pageData: [],
    pagesLength: [],
    itemsPerPage: 5,
    pageNumber: 1,
    totalPages: 1,
    totalLength: 1,
  }),

  methods: {
    async init() {
      const res = await this.getData();
      this.commonProperties(res);
    },
    async getData(params = {}) {
      const res = await axiosInstance.get("", { params });
      return res.data;
    },
    async getSelectedPageData(pageNumber) {
      const params = {
        page: pageNumber,
      };
      const res = await this.getData(params);
      this.pagesLength = [];
      this.commonProperties(res);
    },
    commonProperties(res) {
      this.data = res.data;
      this.totalLength = res.total;
      this.totalPages = res.total_pages;
      this.pageNumber = res.page;
      this.itemsPerPage = res.per_page;
      for (let i = 1; i <= this.totalPages; i = i + 1) {
        this.pagesLength.push(i);
      }
    },
    getRandomColor() {
      const letters = "BCDEF".split("");
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * letters.length)];
      }
      return color;
    },
  },

  mounted() {
    this.init();
  },
};
</script>
