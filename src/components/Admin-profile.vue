<template>
  <div class="profile">
    <div>discord ID: {{ d_id }}</div>
    <div class="input-field">
      <div class="desc">Wybierz family name:</div>
      <select v-model="sf_name" @change="zmien">
        <option :key="user.id" v-for="user in users">{{ user.family_name }}</option>
      </select>
    </div>
    <div class="input-field">
      <div class="desc">Discord Name:</div>
      <input v-model="d_name" />
    </div>
    <div class="input-field">
      <div class="desc">Family Name:</div>
      <input v-model="f_name" />
    </div>
    <div class="input-field">
      <div class="desc">Character Name:</div>
      <input v-model="c_name" />
    </div>
    <div class="input-field">
      <div class="desc">Class:</div>
      <select v-model="class_select">
        <option :key="option.id" v-for="option in class_name">{{ option }}</option>
      </select>
    </div>
    <div class="input-field">
      <div class="desc">Pozycja:</div>
      <select v-model="pos_select">
        <option :key="pos.id" v-for="pos in pos_name">{{ pos }}</option>
      </select>
    </div>
    <div class="input-field">
      <div class="desc">Warn:</div>
      <textarea v-model="warn" rows="4" />
    </div>
    <div class="button" @click="zapisz">Zapisz</div>
    <div>{{ msg }}</div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  name: "Admin-profile",
  components: {},
  data() {
    return {
      users: [],
      sf_name: "",
      d_id: "",
      d_name: "",
      f_name: "",
      c_name: "",
      class_select: "",
      pos_select: "",
      class_name: [
        "Warrior",
        "Sorceress",
        "Ranger",
        "Berserker",
        "Tamer",
        "Musa",
        "Maehwa",
        "Valkyrie",
        "Wizard",
        "Witch",
        "Ninja",
        "Kunoichi",
        "Dark Knight",
        "Mystic",
        "Striker",
        "Lahn",
        "Archer",
        "Shai",
        "Guardian"
      ].sort(),
      pos_name: ["M", "C", "D", "E", "R"],
      warn: "",
      msg: null
    };
  },

  methods: {
    zmien: function(e) {
      var x = e.target.value;

      var y = this.users.findIndex(a => {
        return a.family_name == x;
      });
      this.d_id = this.users[y].discord_ID;
      this.d_name = this.users[y].discord_name;
      this.f_name = this.users[y].family_name;
      this.c_name = this.users[y].char_name;
      this.class_select = this.users[y].class;
      this.pos_select = this.users[y].pos;
      this.warn = this.users[y].warn;
    },
    zapisz: function() {
      var req = {
        d_id: this.d_id,
        d_name: this.d_name,
        f_name: this.f_name,
        c_name: this.c_name,
        class_name: this.class_select,
        pos_name: this.pos_select,
        warn: this.warn
      };
      axios.post("http://35.156.131.239:3000/updateProfile", req);
      this.msg = `Użytkownik ${this.f_name} zaktualizowany!`;
    }
  },
  mounted() {
    axios.get("http://35.156.131.239:3000/loadUsersActive").then(res => {
      this.users = res.data;
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.profile {
  display: grid;
  grid-template-columns: auto;
  grid-template-rows: auto;
}

.input-field {
  display: grid;
  grid-template-columns: 3fr 3fr;
  grid-template-rows: auto;
  padding: 10px 30%;
}

.desc {
  font-size: 22px;
  text-align: left;
  padding-right: 10px;
}

.button {
  margin: 0 45%;
}
</style>
