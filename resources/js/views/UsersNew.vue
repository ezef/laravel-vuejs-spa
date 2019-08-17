<template>
  <div>
    <div v-if="message" class="alert">{{ message }}</div>
      <form @submit.prevent="onSubmit($event)">
        <div class="form-group">
            <label for="user_name">Name</label>
            <input id="user_name" v-model="user.name"  style="width:300px;"/>
        </div>
        <div class="form-group">
            <label for="user_email">Email</label>
            <input id="user_email" v-model="user.email" style="width:300px;"/>
        </div>
        <div class="form-group">
            <label for="user_password">Password</label>
            <input id="user_password" v-model="user.password" style="width:300px;"/>
        </div>
        <div class="form-group">
            <button type="submit" :disabled="saving">Save</button>
        </div><br>
        <button @click="$router.go(-1)">Cancel</button>

    </form>
  </div>
</template>
<script>
import api from '../api/users';

export default {
  data() {
    return {
      message: null,
      loaded: false,
      saving: false,
      user: {
        id: null,
        name: "",
        email: "",
        password: null
      }
    };
  },
  methods: {
    onSubmit(event) {
      this.saving = true;

      api.post({
          name: this.user.name,
          email: this.user.email,
          password: this.user.password,
      }).then((response) => {
        console.log(response)
          this.$router.push({ name : 'users.index', });
      }).catch(error => {
          console.log(error.response.data)
          this.message = error.response.data.message;
      }).then(_ => {
        this.saving = false;
      });
    },
  }
};
</script>

<style lang="scss" scoped>
$red: lighten(red, 30%);
$darkRed: darken($red, 50%);
.form-group label {
  display: block;
}
.alert {
    background: $red;
    color: $darkRed;
    padding: 1rem;
    margin-bottom: 1rem;
    width: 50%;
    border: 1px solid $darkRed;
    border-radius: 5px;
}
</style>