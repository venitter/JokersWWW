<template>
  <div class="container">
    <table>
      <thead>
        <tr>
          <th>LP</th>
          <th>Discord Name</th>
          <th>Family Name</th>
          <th>Character Name</th>
          <th>Class</th>
          <th>Pos</th>
          <th>Warn</th>
          <th class="th-events" v-bind:key="event.id" v-for="event in events">
            <div>{{ event.name }}</div>
            <div>{{ event.date | moment("DD.MM (ddd)") }}</div>
            <div>{{ event.date | moment("HH:mm") }}</div>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-bind:key="user.idusers" v-for="(user, index) in usersC ">
          <td>{{ index + 1 }}</td>
          <td>{{ user.discord_name }}</td>
          <td>{{ user.family_name }}</td>
          <td>{{ user.char_name }}</td>
          <td>{{ user.class }}</td>
          <td>{{ user.pos }}</td>
          <td>{{ user.warn }}</td>
          <td v-bind:key="event.id" v-for="event in events">
            <template v-for="signup in signups">
              <!-- ODBLOKOWANE TAK -->
              <div
                v-bind:key="signup.id"
                v-if="signup.iduser == user.idusers && signup.idevent == event.idevents  && signup.status == 'YES' && event.blocked == 0"
              >
                <div
                  v-if="user.family_name == familyName"
                  style="backgroundColor: green"
                  @click="changeSignup"
                  :title="signup.who + '\n' + gimmeTime(signup.timestamp)"
                >{{ signup.status }}</div>
                <div hidden>{{ signup.iduser }}</div>
                <div hidden>{{ signup.idevent }}</div>
              </div>
              <!-- ODBLOKOWANE NIE -->
              <div
                v-bind:key="signup.id"
                v-else-if="signup.iduser == user.idusers && signup.idevent == event.idevents && signup.status == 'NO' && event.blocked == 0"
              >
                <div
                  v-if="user.family_name == familyName"
                  style="backgroundColor: red"
                  @click="changeSignup"
                  :title="signup.who + '\n' + gimmeTime(signup.timestamp)"
                >{{ signup.status }}</div>
                <div hidden>{{ signup.iduser }}</div>
                <div hidden>{{ signup.idevent }}</div>
              </div>
              <!-- ZABLOKOWANE TAK -->
              <div
                class="td-div"
                v-bind:key="signup.id"
                v-else-if="signup.iduser == user.idusers && signup.idevent == event.idevents  && signup.status == 'YES' && event.blocked == 1"
              >
                <div
                  v-if="user.family_name == familyName"
                  style="backgroundColor: green"
                  :title="signup.who + '\n' + gimmeTime(signup.timestamp)"
                >{{ signup.status }}</div>
                <div hidden>{{ signup.iduser }}</div>
                <div hidden>{{ signup.idevent }}</div>
                <div style="backgroundColor: green">
                  <v-icon name="lock"></v-icon>
                </div>
              </div>
              <!-- ZABLOKOWANE NIE -->
              <div
                class="td-div"
                v-bind:key="signup.id"
                v-else-if="signup.iduser == user.idusers && signup.idevent == event.idevents && signup.status == 'NO' && event.blocked == 1"
              >
                <div
                  v-if="user.family_name == familyName"
                  style="backgroundColor: red"
                  :title="signup.who + '\n' + gimmeTime(signup.timestamp)"
                >{{ signup.status }}</div>
                <div hidden>{{ signup.iduser }}</div>
                <div hidden>{{ signup.idevent }}</div>
                <div style="backgroundColor: red">
                  <v-icon name="lock"></v-icon>
                </div>
              </div>
            </template>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>


<script>
import axios from "axios";
import Datepicker from "vuejs-datepicker";
import { pl } from "vuejs-datepicker/dist/locale";
import "vue-awesome/icons/lock";
import Icon from "vue-awesome/components/Icon";
import router from "../router";

export default {
  name: "User-events",
  components: {
    Datepicker,
    "v-icon": Icon
  },
  data() {
    return {
      admin: true,
      users: [],
      events: [],
      signups: [],
      pl: pl,
      logged: "",
      familyName: ""
    };
  },
  computed: {
    usersC: function() {
      var wynik = this.users.filter(e => {
        return e.family_name == this.familyName;
      });
      return wynik;
    }
  },
  methods: {
    changeSignup: function(e) {
      e.target.hidden = true;
      var data = {
        user: e.target.parentNode.children[1].innerHTML,
        event: e.target.parentNode.children[2].innerHTML,
        logged: this.logged
      };
      if (e.target.innerHTML == "YES") {
        data.status = "NO";
        axios
          .post("http://35.156.131.239:3000/updateSignups", data)
          .then((e.target.innerHTML = "NO"));
        e.target.style.backgroundColor = "red";
      } else {
        data.status = "YES";
        axios
          .post("http://35.156.131.239:3000/updateSignups", data)
          .then((e.target.innerHTML = "YES"));
        e.target.style.backgroundColor = "green";
      }
      this.percentAttendance;
      e.target.hidden = false;
    },
    gimmeTime: function(x) {
      var moment = require("moment");
      return moment(x)
        .add(2, "hours")
        .format("DD.MM HH:mm");
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

    axios.get("http://35.156.131.239:3000/loadUsersActive").then(res => {
      this.users = res.data;
    });
    axios.get("http://35.156.131.239:3000/loadEvents").then(res => {
      this.events = res.data;
    });
    axios.get("http://35.156.131.239:3000/loadSignups").then(res => {
      this.signups = res.data;
    });
    var user = JSON.parse(localStorage.getItem("userData"));
    this.logged = user.family_name;
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
