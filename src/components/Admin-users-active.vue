<template>
  <div class="cont">
    <div>
      <table>
        <thead>
          <tr>
            <th>LP</th>
            <th @click="sortArr(0)">Discord Name</th>
            <th @click="sortArr(1)">Family Name</th>
            <th @click="sortArr(2)">Character Name</th>
            <th @click="sortArr(3)">Class</th>
            <th @click="sortArr(4)">Pos</th>
            <th>Join</th>
          </tr>
        </thead>
        <tbody>
          <tr v-show="akt" v-bind:key="user.idusers" v-for="(user, index) in usersActive ">
            <td>{{ index + 1 }}</td>
            <td>{{ user.discord_name }}</td>
            <td>{{ user.family_name }}</td>
            <td>{{ user.char_name }}</td>
            <td>{{ user.class }}</td>
            <td>{{ user.pos }}</td>
            <td>{{ user.joined | moment("DD.MM.YYYY") }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import router from "../router";

export default {
  name: "Admin-users",
  components: {},
  data() {
    return {
      admin: true,
      users: [],
      logged: "",
      familyName: "",
      akt: true,
      nieakt: false
    };
  },
  computed: {
    usersActive: function() {
      var wynik = this.users.filter(e => {
        return e.active == 1;
      });
      return wynik;
    }
  },
  methods: {
    sortArr: function(x) {
      function compare(a, b) {
        var arra = [a.discord_name, a.family_name, a.char_name, a.class, a.pos];
        var arrb = [b.discord_name, b.family_name, b.char_name, b.class, b.pos];
        if (arra[x] < arrb[x]) return -1;
        if (arra[x] > arrb[x]) return 1;
        return 0;
      }
      return this.users.sort(compare);
    }
  },

  mounted() {
    // zabezpieczenie zeby jakis imbecyl sie nie zalogowal :)
    if (
      localStorage.userData == "" ||
      localStorage.userData == null ||
      localStorage.userData == undefined
    ) {
      router.push("/");
    } else {
      var thatUser = JSON.parse(localStorage.getItem("userData"));
      this.familyName = thatUser.family_name;
    }

    axios.get("http://35.156.131.239:3000/loadUsersAll").then(res => {
      this.users = res.data;
    });
    var user = JSON.parse(localStorage.getItem("userData"));
    this.logged = user.family_name;
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cont {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr auto;
}

.butt {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: auto;
  padding: 10px;
}

table {
  width: 80vw;
  border-collapse: separate; /* Don't collapse */
  border-spacing: 0;
  margin: 10px 20px;
}

thead,
th {
  background-color: black;
  position: sticky;
  top: 60px;
  z-index: 10;
}

tr:hover > td {
  border-bottom: solid 1px red;
}

.th-events {
  width: 7%;
}

table th {
  /* Apply both top and bottom borders to the <th> */
  border-top: solid 1px #999999;
  border-bottom: solid 1px #999999;
  border-right: solid 1px #999999;
}

table td {
  /* For cells, apply the border to one of each side only (right but not left, bottom but not top) */
  border-bottom: solid 1px #999999;
  border-right: solid 1px #999999;
}

table th:first-child,
table td:first-child {
  /* Apply a left border on the first <td> or <th> in a row */
  border-left: solid 1px #999999;
}

.td-div {
  display: grid;
  grid-template-columns: 3fr 1fr;
  grid-template-rows: auto;
}

.selceted {
  color: red;
}
</style>
