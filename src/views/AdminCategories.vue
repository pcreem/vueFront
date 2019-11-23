// ./src/views/AdminCategories.vue
<template>
  <div class="container py-5">
    <!-- 1. 使用先前寫好的 AdminNav -->
    <AdminNav />

    <form class="my-4"  >
      <div class="form-row">
        <div class="col-auto">
          <input v-model="newCategoryName" type="text" class="form-control" placeholder="新增餐廳類別..." />
        </div>
        <div class="col-auto">
          <button type="button" class="btn btn-primary" @click.stop.prevent="createCategory" >新增</button>
        </div>
      </div>
    </form>
    <table class="table">
      <thead class="thead-dark">
        <tr>
          <th scope="col" width="60">#</th>
          <th scope="col">Category Name</th>
          <th scope="col" width="210">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="category in categories" :key="category.id">
          <th scope="row">{{ category.id }}</th>
          <td class="position-relative">
            <div v-show="!category.isEditing" class="category-name">{{ category.name }}</div>
            <input
              v-show="category.isEditing"
              v-model="category.name"
              type="text"
              class="form-control"
            />
            <span v-show="category.isEditing" class="cancel" @click="handleCancel(category.id)">✕</span>
          </td>
          <td class="d-flex justify-content-between">
            <button
              v-show="!category.isEditing"
              type="button"
              class="btn btn-link mr-2"
              @click.stop.prevent="toggleIsEditing(category.id)"
            >Edit</button>
            <button
              v-show="category.isEditing"
              type="button"
              class="btn btn-link mr-2"
              @click.stop.prevent="updateCategory({ categoryId: category.id, name: category.name })"
            >Save</button>
            <button
              type="button"
              class="btn btn-link mr-2"
              @click.stop.prevent="deleteCategory(category.id)"
            >Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import AdminNav from "@/components/AdminNav";
import uuid from "uuid/v4";
import adminAPI from './../apis/admin'
import { Toast } from './../utils/helpers'

export default {
  components: {
    AdminNav
  },
  // 3. 定義 Vue 中使用的 data 資料
  data() {
    return {
      newCategoryName: "",
      categories: []
    };
  },
  // 5. 調用 `fetchCategories` 方法
  created() {
    this.fetchCategories(

    );
  },
  methods: {
    // 4. 定義 `fetchCategories` 方法，把 `dummyData` 帶入 Vue 物件
async fetchCategories () {
      try {
        const response = await adminAPI.categories.get({
        })
           const { data, statusText } = response
           console.log(data)
        // STEP 3：如果 statusText 不是 OK 的話則進入錯誤處理
        if (statusText !== 'OK') {
          throw new Error(statusText)
        }
        // STEP 4：將從伺服器取得的 data 帶入 Vue 內
        this.categories = data.categories
        
      } catch (error) {
        console.log('error', error)
      }
    },
  

  /*  fetchCategories() {
      // 在每一個 category 中都添加一個 isEditing 屬性
      this.categories = dummyData.categories.map(category => ({
        ...category,
        isEditing: false
      }));
    },
    createCategory() {
      // TODO: 透過 API 告知伺服器欲新增的餐廳類別...

      // 將新的類別添加到陣列中
      this.categories.push({
        id: uuid(),
        name: this.newCategoryName
      });

      this.newCategoryName = ""; // 清空原本欄位中的內容

    
    },*/
   
async createCategory() {
      try {
    const response = await adminAPI.categories.get({
        })
           const { data, statusText } = response

data.categories.push({
        id: uuid(),
        name: this.newCategoryName
      });
      console.log(data.categories)
 this.categories = data.categories
 const formData = new FormData(data)
       const { data2, statusText2 } = await adminAPI.categories.create({
           formData
        })
        
       // STEP 3: 透過 restaurants.create 方法來向伺服器建立餐廳
        //await adminAPI.restaurants.create({           form       })

        if (statusText2 !== 'OK' || data2.status !== 'success') {
          throw new Error(statusText)
        }

        // STEP 4: 成功的話則轉址到 `/admin/restaurants`
        this.$router.push({ name: 'admin-categories' })
      } catch (error) {
        // STEP 5: 錯誤處理
        Toast.fire({
          type: 'error',
          title: '無法建立餐廳，請稍後再試'
        })
      }
    },

    deleteCategory(categoryId) {
      // TODO: 透過 API 告知伺服器欲刪除的餐廳類別

      // 將該餐廳類別從陣列中移除
      this.categories = this.categories.filter(
        category => category.id !== categoryId
      );
    },
    toggleIsEditing(categoryId) {
      this.categories = this.categories.map(category => {
        if (category.id !== categoryId) return category;
        // 如果迴圈中的 category.id 是欲修改的 categoryId 則改變 isEditing 的值
        return {
          ...category,
          nameCached: category.name,
          isEditing: !category.isEditing
        };
      });
    },
    updateCategory({ categoryId }) {
      // TODO: 透過 API 去向伺服器更新餐廳類別名稱
      this.toggleIsEditing(categoryId);
    },
    handleCancel(categoryId) {
      this.categories = this.categories.map(category => {
        if (category.id !== categoryId) {
          return category;
        }

        // 將原本的類別名稱還回去
        return {
          ...category,
          name: category.nameCached
        };
      });
      this.toggleIsEditing(categoryId);
    }
  }
};
</script>

<style scoped>
.category-name {
  padding: 0.375rem 0.75rem;
  border: 1px solid transparent;
  outline: 0;
  cursor: auto;
}

.btn-link {
  width: 62px;
}

.cancel {
  position: absolute;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 25px;
  height: 25px;
  border: 1px solid #aaaaaa;
  border-radius: 50%;
  user-select: none;
  cursor: pointer;
  font-size: 12px;
}
</style>