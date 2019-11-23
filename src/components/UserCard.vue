// ./src/components/UserCard.vue
<template>
  <div class="col-md-6 col-lg-4">
    <div class="card mb-4">
      <img class="card-img-top" :src="user.image" alt="Card image cap" width="286px" height="180px" />
      <div class="card-body">
        <p class="card-text title-wrap">
          <a href="#">{{ user.name }}</a>
        </p>
        <span class="badge badge-secondary">追蹤人數: {{ user.FollowerCount }}</span>
      </div>
      <div class="card-footer">
        <button
          v-if="user.isFollowed"
          type="button"
          class="btn btn-danger follow mr-2"
          @click.stop.prevent="deleteFollow"
        >UnFollow</button>
        <button
          v-else
          type="button"
          class="btn btn-primary follow mr-2"
          @click.stop.prevent="addFollow"
        >Follow</button>
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