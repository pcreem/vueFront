// ./src/views/Restaurants.vue
<template>
  <div class="container py-5">
    <NavTabs />
    <!-- 餐廳類別標籤 RestaurantsNavPills -->
    <RestaurantsNavPills :categories="categories" />

    <div class="row">
      <!-- 餐廳卡片 RestaurantCard-->
      <RestaurantCard
        v-for="restaurant in restaurants"
        :key="restaurant.id"
        :initial-restaurant="restaurant"
      />
    </div>

    <!-- 分頁標籤 RestaurantPagination -->
    <RestaurantPagination
      v-if="totalPage > 1"
      :category-id="categoryId"
      :current-page="currentPage"
      :total-page="totalPage"
    />
  </div>
</template>
<script>
/* eslint-disable */

import NavTabs from "./../components/NavTabs";
import RestaurantCard from "./../components/RestaurantCard";
import RestaurantsNavPills from "./../components/RestaurantsNavPills";
import RestaurantPagination from "./../components/RestaurantsPagination";
import restaurantsAPI from "./../apis/restaurants";
import { Toast } from "./../utils/helpers";

const dummyData = {
  restaurants: [
    {
      id: 14,
      name: "Miss Reilly Gibson",
      tel: "(122) 940-8546",
      address: "5145 Donnelly Ville",
      opening_hours: "08:00",
      description: "omnis",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=86.48910140868465",
      viewCounts: 242,
      createdAt: "2019-07-30T16:24:55.435Z",
      updatedAt: "2019-11-17T06:23:48.733Z",
      CategoryId: 5,
      Category: {
        id: 5,
        name: "素食料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-07-30T16:24:55.429Z"
      },
      isFavorited: true,
      isLiked: false
    },
    {
      id: 1,
      name: "Laurence Reynolds",
      tel: "1-657-067-3756 x9782",
      address: "187 Kirlin Squares",
      opening_hours: "08:00",
      description: "sit est mollitia",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=91.29816290184887",
      viewCounts: 65,
      createdAt: "2019-07-30T16:24:55.432Z",
      updatedAt: "2019-11-16T09:55:32.077Z",
      CategoryId: 3,
      Category: {
        id: 3,
        name: "義大利料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-07-30T16:24:55.429Z"
      },
      isFavorited: true,
      isLiked: true
    },
    {
      id: 6,
      name: "Clair Herzog",
      tel: "868-946-9602 x92653",
      address: "65909 Marcelo Lock",
      opening_hours: "08:00",
      description: "Magni enim similique et doloribus fugit.",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=67.15023188933482",
      viewCounts: 3769,
      createdAt: "2019-07-30T16:24:55.434Z",
      updatedAt: "2019-11-17T06:24:22.769Z",
      CategoryId: 5,
      Category: {
        id: 5,
        name: "素食料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-07-30T16:24:55.429Z"
      },
      isFavorited: false,
      isLiked: false
    },
    {
      id: 9,
      name: "Susan Johnston",
      tel: "260-837-0348 x3437",
      address: "0802 Tillman Crossing",
      opening_hours: "08:00",
      description: "Aspernatur aliquam totam pariatur ut mollitia odio",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=41.270646115002016",
      viewCounts: 11,
      createdAt: "2019-07-30T16:24:55.434Z",
      updatedAt: "2019-11-16T06:56:23.584Z",
      CategoryId: 3,
      Category: {
        id: 3,
        name: "義大利料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-07-30T16:24:55.429Z"
      },
      isFavorited: false,
      isLiked: false
    },
    {
      id: 4,
      name: "Odessa Larkin",
      tel: "516-154-3810 x11365",
      address: "1044 Dayana Gardens",
      opening_hours: "08:00",
      description: "Similique eum laborum molestiae. Temporibus natus ",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=74.38474595344819",
      viewCounts: 24,
      createdAt: "2019-07-30T16:24:55.433Z",
      updatedAt: "2019-11-16T10:13:20.050Z",
      CategoryId: 2,
      Category: {
        id: 2,
        name: "日本料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-09-26T15:11:56.408Z"
      },
      isFavorited: false,
      isLiked: false
    },
    {
      id: 10,
      name: "Howell Towne",
      tel: "(221) 302-7497 x511",
      address: "0858 Koss Via",
      opening_hours: "08:00",
      description: "laboriosam",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=54.054925465974634",
      viewCounts: 16450,
      createdAt: "2019-07-30T16:24:55.434Z",
      updatedAt: "2019-11-17T07:36:53.704Z",
      CategoryId: 4,
      Category: {
        id: 4,
        name: "墨西哥料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-07-30T16:24:55.429Z"
      },
      isFavorited: false,
      isLiked: false
    },
    {
      id: 21,
      name: "Clovis Mayert",
      tel: "1-292-930-4113 x0577",
      address: "3255 Streich Inlet",
      opening_hours: "08:00",
      description: "Nihil occaecati consectetur explicabo sint nobis b",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=31.579240610126313",
      viewCounts: 13,
      createdAt: "2019-07-30T16:24:55.436Z",
      updatedAt: "2019-11-15T20:14:02.479Z",
      CategoryId: 3,
      Category: {
        id: 3,
        name: "義大利料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-07-30T16:24:55.429Z"
      },
      isFavorited: false,
      isLiked: false
    },
    {
      id: 13,
      name: "Curt Corkery",
      tel: "377-916-6821 x9397",
      address: "3757 Alfonzo Corner",
      opening_hours: "08:00",
      description: "enim",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=19.39164947251193",
      viewCounts: 2,
      createdAt: "2019-07-30T16:24:55.435Z",
      updatedAt: "2019-11-16T06:55:35.148Z",
      CategoryId: 2,
      Category: {
        id: 2,
        name: "日本料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-09-26T15:11:56.408Z"
      },
      isFavorited: false,
      isLiked: false
    },
    {
      id: 16,
      name: "Joaquin Walsh",
      tel: "(359) 745-0699 x3250",
      address: "8271 McKenzie Ridges",
      opening_hours: "08:00",
      description: "Molestiae sed ut dolores neque optio et expedita.",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=16.407932234411838",
      viewCounts: 1,
      createdAt: "2019-07-30T16:24:55.435Z",
      updatedAt: "2019-10-03T09:26:18.620Z",
      CategoryId: 2,
      Category: {
        id: 2,
        name: "日本料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-09-26T15:11:56.408Z"
      },
      isFavorited: false,
      isLiked: false
    },
    {
      id: 15,
      name: "Gail Hamill II",
      tel: "238-418-9715 x21072",
      address: "524 Fritsch Square",
      opening_hours: "08:00",
      description: "Quis incidunt excepturi vitae similique aliquam su",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=79.64565482384884",
      viewCounts: 1,
      createdAt: "2019-07-30T16:24:55.435Z",
      updatedAt: "2019-10-03T09:26:48.641Z",
      CategoryId: 4,
      Category: {
        id: 4,
        name: "墨西哥料理",
        createdAt: "2019-07-30T16:24:55.429Z",
        updatedAt: "2019-07-30T16:24:55.429Z"
      },
      isFavorited: false,
      isLiked: false
    }
  ],
  categories: [
    {
      id: 3,
      name: "義大利料理",
      createdAt: "2019-07-30T16:24:55.429Z",
      updatedAt: "2019-07-30T16:24:55.429Z"
    },
    {
      id: 4,
      name: "墨西哥料理",
      createdAt: "2019-07-30T16:24:55.429Z",
      updatedAt: "2019-07-30T16:24:55.429Z"
    },
    {
      id: 5,
      name: "素食料理",
      createdAt: "2019-07-30T16:24:55.429Z",
      updatedAt: "2019-07-30T16:24:55.429Z"
    },
    {
      id: 6,
      name: "美式料理",
      createdAt: "2019-07-30T16:24:55.429Z",
      updatedAt: "2019-07-30T16:24:55.429Z"
    },
    {
      id: 7,
      name: "複合式料理",
      createdAt: "2019-07-30T16:24:55.429Z",
      updatedAt: "2019-07-30T16:24:55.429Z"
    },
    {
      id: 1,
      name: "中式料理",
      createdAt: "2019-07-30T16:24:55.429Z",
      updatedAt: "2019-09-08T08:51:28.715Z"
    },
    {
      id: 2,
      name: "日本料理",
      createdAt: "2019-07-30T16:24:55.429Z",
      updatedAt: "2019-09-26T15:11:56.408Z"
    },
    {
      id: 31,
      name: "新疆料理",
      createdAt: "2019-09-26T19:58:57.874Z",
      updatedAt: "2019-09-26T19:58:57.874Z"
    }
  ],
  categoryId: "",
  page: 1,
  totalPage: [1, 2, 3, 4, 5],
  prev: 1,
  next: 2
};
export default {
  components: {
    NavTabs,
    RestaurantCard,
    RestaurantsNavPills,
    RestaurantPagination
  },
  data() {
    return {
      categories: [],
      categoryId: -1,
      currentPage: 1,
      restaurants: [],
      totalPage: -1
    };
  },
  created() {
    const { page, categoryId } = this.$route.query;
    this.fetchRestaurants({ page, categoryId });
  },
  beforeRouteUpdate(to, from, next) {
    const { page, categoryId } = to.query;
    this.fetchRestaurants({ page, categoryId });
    next();
  },
  methods: {
    // STEP 1：直接在 fetchRestaurants 的地方帶入預設值
    async fetchRestaurants({ page = 1, categoryId = "" }) {
      try {
        // STEP 2：直接把 response 內需要的屬性透過解構賦值拿出來
        const { data, statusText } = await restaurantsAPI.getRestaurants({
          page,
          categoryId
        });

        if (statusText !== "OK") {
          throw new Error(statusText);
        }

        this.categories = data.categories;
        this.categoryId = data.categoryId;
        this.currentPage = data.page;
        this.restaurants = data.restaurants;
        this.totalPage = data.totalPage.length;
      } catch (error) {
        Toast.fire({
          type: "error",
          title: "無法取得餐廳資料，請稍後再試"
        });
      }
    }
  }
};
</script>