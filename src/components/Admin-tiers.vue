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
    <div class="tiers">
      <div>
        Tier1
        <input style="width: 30px" v-model="t1" />
        %
        <input style="width: 30px" v-model="p1" />
      </div>
      <div>
        Tier2
        <input style="width: 30px" v-model="t2" />
        %
        <input style="width: 30px" v-model="p2" />
      </div>
      <div>
        Tier3
        <input style="width: 30px" v-model="t3" />
        %
        <input style="width: 30px" v-model="p3" />
      </div>
      <div>
        Tier4
        <input style="width: 30px" v-model="t4" />
        %
        <input style="width: 30px" v-model="p4" />
      </div>
      <div>
        <div class="button" @click="zapisz">Zapisz wzór</div>
      </div>
      <div>
        <div class="button" @click="zapiszDoBazy">Zapisz tiery</div>
      </div>
    </div>
    <div>
      <table>
        <thead>
          <tr>
            <th>LP</th>
            <th @click="sortArr(0)">Discord Name</th>
            <th @click="sortArr(1)">Family Name</th>
            <th>TIER</th>
            <th @click="sortArr(2)">Character Name</th>
            <th @click="sortArr(3)">Class</th>
            <th @click="sortArr(4)">Pos</th>
            <th>YES</th>
            <th>SUM</th>
            <th @click="sortArr(5)">%</th>
          </tr>
        </thead>
        <tbody>
          <tr v-bind:key="user.idusers" v-for="(user, index) in percent ">
            <td>{{ index + 1 }}</td>
            <td>{{ user.discord_name }}</td>
            <td>{{ user.family_name }}</td>
            <td>{{ user.tier }}</td>
            <td hidden>{{ user.discord_ID }}</td>
            <td>{{ user.char_name }}</td>
            <td>{{ user.class }}</td>
            <td>{{ user.pos }}</td>
            <td>{{ user.yes }}</td>
            <td>{{ user.all }}</td>
            <td>{{ user.percent }}%</td>
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
      datedo: Date.now(),
      t1: 0,
      p1: 0,
      t2: 0,
      p2: 0,
      t3: 0,
      p3: 0,
      t4: 0,
      p4: 0
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
            return e.checked == "YES";
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

        if (parseFloat(obj.percent) >= this.p4 && this.t4 != 0) {
          result[i].tier = this.t4;
        } else if (parseFloat(obj.percent) >= this.p3 && this.t3 != 0) {
          result[i].tier = this.t3;
        } else if (parseFloat(obj.percent) >= this.p2 && this.t2 != 0) {
          result[i].tier = this.t2;
        } else if (parseFloat(obj.percent) >= this.p1 && this.t1 != 0) {
          result[i].tier = this.t1;
        } else {
          result[i].tier = 1;
        }
      }
      return result;
    },
    percentNode: function() {
      var result = [];
      var wynik = "";
      for (var i = 0; i < this.events.length; i++) {
        var sumyes = this.signups
          .filter(e => {
            return e.checked == "YES";
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
    zapisz: function() {
      var data = {
        t1: this.t1,
        p1: this.p1,
        t2: this.t2,
        p2: this.p2,
        t3: this.t3,
        p3: this.p3,
        t4: this.t4,
        p4: this.p4
      };
      axios.post("http://35.156.131.239:3000/saveTiers", data);
    },
    zapiszDoBazy: function() {
      var data = this.percent;
      axios.post("http://35.156.131.239:3000/savePayout", data);
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
        dn: e.target.parentNode.parentNode.parentNode.cells[4].innerHTML,
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
    axios.get("http://35.156.131.239:3000/loadTiers").then(res => {
      this.t1 = res.data[0].tier;
      this.p1 = res.data[0].percent;
      this.t2 = res.data[1].tier;
      this.p2 = res.data[1].percent;
      this.t3 = res.data[2].tier;
      this.p3 = res.data[2].percent;
      this.t4 = res.data[3].tier;
      this.p4 = res.data[3].percent;
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

.tiers {
  display: grid;
  grid-template-columns: 15% 15% 15% 15% 15% 15%;
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
