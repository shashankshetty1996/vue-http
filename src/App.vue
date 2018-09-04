<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-lg-8 offset-lg-2 col-md-8 offset-md-2 col-xs-12">
        <h1 class="text-center mb-5">{{ msg }}</h1>
        <div class="form-group">
          <label for="username">Username</label>
          <input type="text" name="username" id="username" class="form-control" v-model="user.username">
        </div>
        <div class="form-group">
          <label for="mail">Mail</label>
          <input type="text" name="mail" id="mail" class="form-control" v-model="user.mail">
        </div>
        <div class="form-group">
          <label for="node">Node</label>
          <input type="text" name="node" id="node" class="form-control" v-model="node">
        </div>
        <button class="btn btn-primary" type="submit" @click="submit">Submit</button>
        <hr>
        <button class="btn btn-primary" @click="fetchData">Get Data</button>

        <ul class="list-group my-3">
          <li class="list-group-item" v-for="user in users" :key="user.username">{{ user.username }} - {{ user.mail }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      msg: "HTTP using Firebase",
      user: {
        username: "",
        mail: ""
      },
      users: [],
      resource: {},
      node: "data"
    };
  },
  methods: {
    submit() {
      // this.$http.post("data.json", this.user).then(
      //   response => {
      //     console.log(response);
      //   },
      //   error => {
      //     console.log(error);
      //   }
      // );
      // this.resource.save({}, this.user);
      this.resource.saveAlt(this.user);
    },
    fetchData() {
      // this.$http
      //   .get("data.json")
      //   .then(response => {
      //     return response.json();
      //   })
      //   .then(data => {
      //     // console.log(data);
      //     const resultArr = [];
      //     for (let key in data) {
      //       resultArr.push(data[key]);
      //     }
      //     this.users = resultArr;
      //   });
      this.resource
        .getData({ node: this.node })
        .then(response => {
          return response.json();
        })
        .then(data => {
          // console.log(data);
          const resultArr = [];
          for (let key in data) {
            resultArr.push(data[key]);
          }
          this.users = resultArr;
        });
    }
  },
  created() {
    const customAction = {
      saveAlt: { method: "POST", url: "alternative.json" },
      getData: { method: "GET" }
    };
    this.resource = this.$resource("{node}.json", {}, customAction);
  }
};
</script>

<style>
</style>
