<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>
      <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot">
        <label for="newTwoot"><strong>New Twoot</strong></label>
        <textarea
          name="twootTextArea"
          id="newTwoot"
          rows="4"
          v-model="newTwootContent"
        ></textarea>

        <br />

        <div class="user-profile__create-twoot-type">
          <label for="newTwootName">
            <strong>用户:</strong>
          </label>
          <select name="name" id="newTwootName" v-model="selectedTwootName">
            <option
              :value="option.value"
              v-for="(option, index) in twootNames"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
          &nbsp;
          <label for="newTwootType">
            <strong>是否发送:</strong>
          </label>
          <select name="type" id="newTwootType" v-model="selectedTwootType">
            <option
              :value="option.value"
              v-for="(option, index) in twootTypes"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>

          <br />
          <br />

          <button>
            {{ this.selectedTwootType === 'draft' ? '存为草稿' : '发送' }}😇
          </button>
          &nbsp;
          <button @click="getDraft">获取草稿🤓</button>
        </div>
      </form>
    </div>

    <div class="user-profile__twoots-wrapper">
      <TwootItem
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favorite="toggleFavorite"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from './TwootItem.vue';

export default {
  name: 'UserProfile',
  components: { TwootItem },
  data() {
    return {
      followers: 0,
      user: {
        id: 1,
        username: 'bfy & miffy',
        email: 'i@jun.sh',
        isAdmin: true,
        twoots: [{ id: 1, name: 'miffy', content: '来玩呀！' }],
      },
      twootTypes: [
        { value: 'draft', name: '存为草稿' },
        { value: 'instant', name: '立即发送' },
      ],
      twootNames: [
        { value: 'BFY', name: 'BFY' },
        { value: 'miffy', name: 'miffy' },
      ],
      newTwootContent: '',
      draftTwootContent: '',
      selectedTwootType: 'instant',
      selectedTwootName: 'BFY',
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower`);
      }
    },
  },
  methods: {
    followerUser() {
      this.followers++;
    },
    toggleFavorite(id) {
      alert(`Favourite Tweet #${id}`);
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType === 'instant') {
        this.user.twoots.unshift({
          name: this.selectedTwootName,
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        });
      } else {
        this.draftTwootContent = JSON.parse(
          JSON.stringify(this.newTwootContent)
        );
        console.log(this.draftTwootContent);
      }
      // this.newTwootContent = '';
    },
    getDraft() {
      this.newTwootContent = this.draftTwootContent;
    },
  },
  mounted() {
    this.followerUser();
  },
};
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-rows: 1fr 3fr;
  width: 30%;
  padding: 50px 5%;
}

.user-profile__user-panel {
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
}

.user-profile__admin-badge {
  background-color: purple;
  color: white;
  border-radius: 5px;
  margin-right: 10px;
  padding: 0 10px;
  font-weight: bold;
  text-align: center;
}

h1 {
  margin: 0;
}

.user-profile__create-twoot {
  border-top: 1px solid #dfe3e8;
  padding-top: 20px;
  display: flex;
  flex-direction: column;
}
</style>
