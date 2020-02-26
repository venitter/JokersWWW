<template>
  <div class="con">
    <div>
      <Datepicker
        v-model="dateod"
        :language="pl"
        :format="'dd MMMM yyyy'"
        class="datepicker"
        placeholder="Wybierz datę od"
      />
      <Datepicker
        v-model="datedo"
        :language="pl"
        :format="'dd MMMM yyyy'"
        class="datepicker"
        placeholder="Wybierz datę do"
      />
      <div class="button" @click="zaladuj">Załaduj</div>
    </div>
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
            <th>YES</th>
            <th>SUM</th>
            <th @click="sortArr(5)">%</th>
            <th class="th-events" v-bind:key="event.id" v-for="event in events">
              <div>{{ event.date | moment("DD.MM") }}</div>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td colspan="9" style="text-align: right">% obecności:</td>
            <td v-for="pN in percentNode" v-bind:key="pN.id">{{ pN }}%</td>
          </tr>
          <tr v-bind:key="user.idusers" v-for="(user, index) in percent ">
            <td>{{ index + 1 }}</td>
            <td>{{ user.discord_name }}</td>
            <td>{{ user.family_name }}</td>
            <td hidden>{{ user.discord_ID }}</td>
            <td>{{ user.char_name }}</td>
            <td>{{ user.class }}</td>
            <td>{{ user.pos }}</td>
            <td>{{ user.yes }}</td>
            <td>{{ user.all }}</td>
            <td>{{ user.percent }}%</td>
            <td v-bind:key="event.id" v-for="event in events">
              <template v-for="signup in signups">
                <!-- TAK / NIE -->
                <div
                  v-bind:key="signup.id"
                  v-if="signup.iduser == user.idusers && signup.idevent == event.idevents  && signup.status == 'YES' && signup.checked == 'NO'"
                >
                  <div
                    @click="schowek"
                    style="backgroundColor: red"
                    :title="signup.who + '\n' + gimmeTime(signup.timestamp)"
                  >
                    <b>!</b>
                  </div>
                  <div hidden>{{ event.date | moment("DD.MM.YYYY") }}</div>
                </div>
                <!-- TAK -->
                <div
                  v-bind:key="signup.id"
                  v-else-if="signup.iduser == user.idusers && signup.idevent == event.idevents && signup.checked == 'YES'"
                >
                  <div
                    @click="schowek"
                    style="backgroundColor: green; color: green"
                    :title="signup.who + '\n' + gimmeTime(signup.timestamp)"
                  >Y</div>
                  <div hidden>{{ event.date | moment("DD.MM.YYYY") }}</div>
                </div>
                <!-- RDY -->
                <div
                  v-bind:key="signup.id"
                  v-else-if="signup.iduser == user.idusers && signup.idevent == event.idevents && signup.checked == 'RDY'"
                >
                  <div
                    @click="schowek"
                    style="backgroundColor: gold; color: gold"
                    :title="signup.who + '\n' + gimmeTime(signup.timestamp)"
                  >Y</div>
                  <div hidden>{{ event.date | moment("DD.MM.YYYY") }}</div>
                </div>
                <!-- NIE -->
                <div
                  v-bind:key="signup.id"
                  v-else-if="signup.iduser == user.idusers && signup.idevent == event.idevents && signup.checked == 'NO'"
                >
                  <div
                    @click="schowek"
                    style="backgroundColor: red; color: red"
                    :title="signup.who + '\n' + gimmeTime(signup.timestamp)"
                  >N</div>
                  <div hidden>{{ event.date | moment("DD.MM.YYYY") }}</div>
                </div>
              </template>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import Datepicker from "vuejs-datepicker";
import { pl } from "vuejs-datepicker/dist/locale";
import "vue-awesome/icons/exclamation";
import Icon from "vue-awesome/components/Icon";
import router from "../router";

export default {
  name: "User-attendence",
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
      familyName: "",
      dateod: Date.now() - 24 * 60 * 60 * 1000 * 30, // 30 dni
      datedo: Date.now()
    };
  },
  computed: {
    percent: function() {
      var result = this.users;
      var usernow = "";
      for (var i = 0; i < result.length; i++) {
        var obj = {
          yes: "",
          all: "",
          percent: "",
          tier: ""
        };
        usernow = result[i].family_name;
        var sumyes = this.signups
          .filter(e => {
            return e.checked == "YES" || e.checked == "RDY";
          })
          .filter(e => {
            return e.family_name == usernow;
          });
        var sumall = this.signups
          .filter(e => {
            return e.family_name == usernow;
          })
          .filter(e => {
            return e.checked != null;
          });
        obj.yes = sumyes.length;
        obj.all = sumall.length;
        obj.percent = ((sumyes.length / sumall.length) * 100).toFixed(0);
        if (isNaN(obj.percent)) {
          obj.percent = 0;
        }
        result[i].yes = obj.yes;
        result[i].all = obj.all;
        result[i].percent = obj.percent;
      }
      return result;
    },
    percentNode: function() {
      var result = [];
      var wynik = "";
      for (var i = 0; i < this.events.length; i++) {
        var sumyes = this.signups
          .filter(e => {
            return e.checked == "YES" || e.checked == "RDY";
          })
          .filter(e => {
            return e.idevent == this.events[i].idevents;
          });
        var sumall = this.signups
          .filter(e => {
            return e.idevent == this.events[i].idevents;
          })
          .filter(e => {
            return e.checked != null;
          });

        wynik = ((sumyes.length / sumall.length) * 100).toFixed(0);
        if (isNaN(wynik)) {
          wynik = 0;
        }
        result.push(wynik);
      }
      return result;
    }
  },
  methods: {
    gimmeTime: function(x) {
      var moment = require("moment");
      return moment(x)
        .add(2, "hours")
        .format("DD.MM HH:mm");
    },
    zaladuj: function() {
      var moment = require("moment");
      axios
        .get("http://35.156.131.239:3000/loadAttendence", {
          params: {
            od: moment(this.dateod).format("YYYY-MM-DD"),
            do: moment(this.datedo).format("YYYY-MM-DD")
          }
        })
        .then(res => {
          this.events = res.data;
        });
      axios
        .get("http://35.156.131.239:3000/loadSignupsAt", {
          params: {
            od: moment(this.dateod).format("YYYY-MM-DD"),
            do: moment(this.datedo).format("YYYY-MM-DD")
          }
        })
        .then(res => {
          this.signups = res.data;
        });
    },
    sortArr: function(x) {
      function compare(a, b) {
        var arra = [
          a.discord_name,
          a.family_name,
          a.char_name,
          a.class,
          a.pos,
          parseFloat(a.percent)
        ];
        var arrb = [
          b.discord_name,
          b.family_name,
          b.char_name,
          b.class,
          b.pos,
          parseFloat(b.percent)
        ];
        if (arra[x] < arrb[x]) return -1;
        if (arra[x] > arrb[x]) return 1;
        return 0;
      }
      return this.percent.sort(compare);
    },
    schowek: function(e) {
      var el = document.createElement("textarea");
      el.setAttribute("readonly", "");
      el.style = { position: "absolute", left: "-9999px" };
      document.body.appendChild(el);
      var obj = {
        fn: e.target.parentNode.parentNode.parentNode.cells[2].innerHTML,
        dn: e.target.parentNode.parentNode.parentNode.cells[3].innerHTML,
        date: e.target.nextElementSibling.innerHTML
      };
      var copyText = `<@${obj.dn}> ${obj.fn} ${obj.date}`;
      el.value = copyText;
      el.select();
      document.execCommand("copy");
      document.body.removeChild(el);
    }
  },
  mounted() {
    var moment = require("moment");
    axios.get("http://35.156.131.239:3000/loadUsersActive").then(res => {
      this.users = res.data;
    });
    axios
      .get("http://35.156.131.239:3000/loadAttendence", {
        params: {
          od: moment(this.dateod).format("YYYY-MM-DD"),
          do: moment(this.datedo).format("YYYY-MM-DD")
        }
      })
      .then(res => {
        this.events = res.data;
      });
    axios
      .get("http://35.156.131.239:3000/loadSignupsAt", {
        params: {
          od: moment(this.dateod).format("YYYY-MM-DD"),
          do: moment(this.datedo).format("YYYY-MM-DD")
        }
      })
      .then(res => {
        this.signups = res.data;
      });

    if (
      localStorage.userData == "" ||
      localStorage.userData == null ||
      localStorage.userData == undefined
    ) {
      router.push("/");
    } else {
      var user = JSON.parse(localStorage.getItem("userData"));
      this.discordName = user.discord_name;
      this.familyName = user.family_name;
      this.perm = user.perm;
      this.logged = user.family_name;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.con {
  display: grid;
  grid-template-columns: 100%;
  grid-template-rows: auto;
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
  width: 4%;
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

.selceted {
  color: red;
}

.datepicker {
  display: inline-block;
  color: black;
  font-size: 20px;
}
</style>
