<template>
  <div id="user-profile">
    @{{ user.username }} - {{ fullName }} <br /><strong>Followers</strong
    ><br />{{ followers }}<br />
    <button @click="followUser">Follow</button>
  </div>
</template>

<script lang="ts">
export default {
  name: 'App',
  data() {
    return {
      followers: 0,
      user: {
        id: 1,
        username: '_MitchellRomney',
        firstName: 'Mitchell',
        lastName: 'Romney',
        email: 'mitchellromney@theearthissquare.com',
      },
    };
  },
  watch: {
    followers(newFollowerCount: number, oldFollowerCount: number) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username}has gained a User!`);
      }
    },
  },
  computed: {
    fullName() {
      return `${this.user.firstName}  ${this.user.lastName}`;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
  },
  mounted() {
    this.followUser();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}
</style>
