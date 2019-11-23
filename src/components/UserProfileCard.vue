// ./src/components/UserCard.vue
<template>
  <div class="card mb-3">
  <div class="row no-gutters">
    <div class="col-md-4">
        <img :src="user.image" width="300px" height="300px">
    </div>
    <div class="col-md-8">
      <div class="card-body">
        <h5 class="card-title">{{user.name}}</h5>
        <p class="card-text">
          {{user.email}}
        </p>
        <ul class="list-unstyled list-inline">
          <li><strong>{{user.Comments.length}}</strong> 已評論餐廳</li>
          <li><strong>{{user.FavoritedRestaurants.length}}</strong> 收藏的餐廳</li>
          <li><strong>{{user.Followings.length}}</strong> followings (追蹤者)</li>
          <li><strong>{{user.Followers.length}}</strong> followers (追隨者)</li>
        </ul>
        <p>
            <a href="/users/1/edit"><button type="submit" class="btn btn-primary">edit</button></a>
        </p>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  props: {
    initialUser: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      user: this.initialUser
    };
  },
  methods: {
    addFollow() {
      this.user = {
        ...this.user,
        isFollowed: true
      };
    },
    deleteFollow() {
      this.user = {
        ...this.user,
        isFollowed: false
      };
    }
  }
};
</script>