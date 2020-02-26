<template>
  <div class="cont">
    <div class="butt">
      <div>Statystyki</div>
    </div>
    <div class="events">
      <div v-for="sc in statsCount" :key="sc.id">
        <table :key="sc.id">
          <thead>
            <tr>
              <th colspan="6">Wszyscy Członkowie Rodziny Jokers</th>
            </tr>
            <tr>
              <th style="width: 16.6%">Razem</th>
              <th style="width: 16.6%">Main</th>
              <th style="width: 16.6%">Rota</th>
              <th style="width: 16.6%">Cannon</th>
              <th style="width: 16.6%">Def</th>
              <th style="width: 16.6%">Eleph</th>
            </tr>
          </thead>
          <tr>
            <td>{{ sc.count }}</td>
            <td>{{ sc.z }}</td>
            <td>{{ sc.f }}</td>
            <td>{{ sc.c }}</td>
            <td>{{ sc.d }}</td>
            <td>{{ sc.e }}</td>
          </tr>
        </table>
      </div>
    </div>
    <div>
      <table style="width: 30vw">
        <thead>
          <tr>
            <th :key="cl.id" v-for="cl in class_name">
              <div>
                <img :src="getImg(cl)" />
                <div style="font-size: 9px;">{{ cl }}</div>
              </div>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td :key="ccl.id" v-for="ccl in classCount">{{ ccl }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="events">
      <div v-for="sn in signupsCount" :key="sn.id">
        <table :key="sn.id">
          <thead>
            <tr>
              <th colspan="6">{{ sn.name }} {{ sn.date | moment("DD.MM (ddd) g. HH:mm") }}</th>
            </tr>
            <tr>
              <th style="width: 16.6%">Razem</th>
              <th style="width: 16.6%">Main</th>
              <th style="width: 16.6%">Rota</th>
              <th style="width: 16.6%">Cannon</th>
              <th style="width: 16.6%">Def</th>
              <th style="width: 16.6%">Eleph</th>
            </tr>
          </thead>
          <tr>
            <td>{{ sn.count }}</td>
            <td>{{ sn.z }}</td>
            <td>{{ sn.f }}</td>
            <td>{{ sn.c }}</td>
            <td>{{ sn.d }}</td>
            <td>{{ sn.e }}</td>
          </tr>
        </table>
      </div>
    </div>
    <div>
      <div style="border: solid 1px white; padding: 5px; text-align: left" v-html="motdOut"></div>
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
      events: [],
      signups: [],
      motd: "error",
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
      ].sort()
    };
  },
  computed: {
    motdOut: function() {
      return this.motd.replace(/\n/g, "<br>");
    },
    classCount: function() {
      var wynik = [];
      for (var i = 0; i < this.class_name.length; i++) {
        var count = this.users.filter(e => {
          return e.class == this.class_name[i];
        });
        wynik.push(count.length);
      }
      return wynik;
    },
    signupsCount: function() {
      var wynik = [];
      // zlicza ludzi i formacje
      for (var i = 0; i < this.events.length; i++) {
        var obj = {
          name: "",
          date: "",
          count: "",
          z: "",
          f: "",
          c: "",
          d: "",
          e: ""
        };

        obj.name = this.events[i].name;
        obj.date = this.events[i].date;
        // YES
        var count = this.signups.filter(e => {
          return e.status == "YES" && e.date == this.events[i].date;
        });
        obj.count = count.length;

        // ZERG
        count = this.signups.filter(e => {
          return (
            e.status == "YES" && e.pos == "M" && e.date == this.events[i].date
          );
        });
        obj.z = count.length;

        // FLANK
        count = this.signups.filter(e => {
          return (
            e.status == "YES" && e.pos == "R" && e.date == this.events[i].date
          );
        });
        obj.f = count.length;

        // CANNON
        count = this.signups.filter(e => {
          return (
            e.status == "YES" && e.pos == "C" && e.date == this.events[i].date
          );
        });
        obj.c = count.length;

        // DEF
        count = this.signups.filter(e => {
          return (
            e.status == "YES" && e.pos == "D" && e.date == this.events[i].date
          );
        });
        obj.d = count.length;

        // ELEPHANT
        count = this.signups.filter(e => {
          return (
            e.status == "YES" && e.pos == "E" && e.date == this.events[i].date
          );
        });
        obj.e = count.length;

        wynik.push(obj);
      }
      return wynik;
    },
    statsCount: function() {
      var wynik = [];
      var count = 0;
      // zlicza ludzi i formacje
      var obj = {
        count: "",
        z: "",
        f: "",
        c: "",
        d: "",
        e: ""
      };
      // YES
      obj.count = this.users.length;

      // ZERG
      count = this.users.filter(e => {
        return e.pos == "M";
      });
      obj.z = count.length;

      // FLANK
      count = this.users.filter(e => {
        return e.pos == "R";
      });
      obj.f = count.length;

      // CANNON
      count = this.users.filter(e => {
        return e.pos == "C";
      });
      obj.c = count.length;

      // DEF
      count = this.users.filter(e => {
        return e.pos == "D";
      });
      obj.d = count.length;

      // ELEPHANT
      count = this.users.filter(e => {
        return e.pos == "E";
      });
      obj.e = count.length;

      wynik.push(obj);

      return wynik;
    }
  },
  methods: {
    getImg: function(x) {
      if (x == null) {
        return;
      } else {
        return `../static/img/${x}.png`;
      }
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
    var user = JSON.parse(localStorage.getItem("userData"));
    this.logged = user.family_name;

    axios.get("http://35.156.131.239:3000/loadSignupsMain").then(res => {
      this.signups = res.data;
    });
    axios.get("http://35.156.131.239:3000/loadEventsMain").then(res => {
      this.events = res.data;
    });
    axios.get("http://35.156.131.239:3000/loadPartyEdit").then(res => {
      this.motd = res.data[0].motd.replace(/<br\s*\/?>/gm, "\n");
    });
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

.events {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: auto;
}

.butt {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: auto;
  column-gap: 10%;
}

table {
  width: 100%;
  border-collapse: separate; /* Don't collapse */
  border-spacing: 0;
  margin: 10px 20px;
  padding: 10px;
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
