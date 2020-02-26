<template>
  <div>
    <div class="container">
      <div>Sprawdzanie obecności {{ eventName }} {{ eventDate }}</div>
      <div>
        <div class="button" @click="back">Wróć</div>
      </div>
    </div>
    <div class="list">
      <div class="record">
        <div>LP</div>
        <div @click="sortArr(0)">Discord Name</div>
        <div @click="sortArr(1)">Family Name</div>
        <div @click="sortArr(4)">Pos</div>
        <div>Zapis</div>
        <div>Sprawdzenie</div>
      </div>
      <div class="record" :key="signup.id" v-for="(signup, index) in signups">
        <div hidden>{{ signup.idsignups }}</div>
        <div>{{ index + 1 }}</div>
        <div>{{ signup.discord_name }}</div>
        <div>{{ signup.family_name }}</div>
        <div>{{ signup.pos }}</div>

        <!--SIGNUP-->
        <div v-if="signup.status == 'null' || signup.status == null">
          <div class="button button-null" @click="changeSignup">null</div>
        </div>
        <div v-else-if="signup.status == 'YES'">
          <div class="button button-green" @click="changeSignup">{{ signup.status }}</div>
        </div>
        <div v-else>
          <div class="button button-red" @click="changeSignup">{{ signup.status }}</div>
        </div>

        <!--CHECK-->
        <div v-if="signup.checked == 'null' || signup.checked == null">
          <div class="button button-null" @click="changeCheck">null</div>
        </div>
        <div v-else-if="signup.checked == 'YES'">
          <div class="button button-green" @click="changeCheck">{{ signup.checked }}</div>
        </div>
        <div v-else-if="signup.checked == 'RDY'">
          <div class="button button-yellow" @click="changeCheck">{{ signup.checked }}</div>
        </div>
        <div v-else>
          <div class="button button-red" @click="changeCheck">{{ signup.checked }}</div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  name: "Admin-check-details",
  components: {},
  data() {
    return {
      eventId: this.$store.state.detailsEventId,
      eventName: this.$store.state.detailsEventName,
      eventDate: this.$store.state.detailsEventDate,
      signups: ""
    };
  },
  computed: {},
  methods: {
    back: function() {
      this.$store.state.eventList = true;
      this.$store.state.detailsEventId = null;
    },
    changeCheck: function(e) {
      var data = {
        id: e.target.parentNode.parentNode.children[0].innerHTML,
        check: e.target.innerHTML
      };
      // axios request tu bydzie
      axios.post("http://35.156.131.239:3000/updateChecks", data);
      var x = data.check;
      var y = "";
      if (x == "null" || x == null) {
        y = "YES";
      } else if (x == "YES") {
        y = "NO";
      } else if (x == "NO") {
        y = "RDY";
      } else {
        y = null;
      }
      var i = e.target.parentNode.parentNode.children[1].innerHTML - 1;
      this.signups[i].checked = y;
    },
    changeSignup: function(e) {
      var data = {
        id: e.target.parentNode.parentNode.children[0].innerHTML,
        signup: e.target.innerHTML
      };
      // axios request tu bydzie
      axios.post("http://35.156.131.239:3000/updateSignupChecks", data);
      var x = data.signup;
      var y = "";
      if (x == "null" || x == null) {
        y = "YES";
      } else if (x == "YES") {
        y = "NO";
      } else {
        y = null;
      }
      var i = e.target.parentNode.parentNode.children[1].innerHTML - 1;
      this.signups[i].status = y;
    },
    sortArr: function(x) {
      function compare(a, b) {
        var arra = [a.discord_name, a.family_name, a.char_name, a.class, a.pos];
        var arrb = [b.discord_name, b.family_name, b.char_name, b.class, b.pos];
        if (arra[x] < arrb[x]) return -1;
        if (arra[x] > arrb[x]) return 1;
        return 0;
      }
      return this.signups.sort(compare);
    }
  },
  mounted() {
    axios
      .get("http://35.156.131.239:3000/loadSignupsCheck", {
        params: {
          idevent: this.eventId
        }
      })
      .then(res => {
        this.signups = res.data;
      });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: grid;
  width: 80vw;
  grid-template-columns: 10fr 1fr;
  grid-template-rows: auto;
}

.list {
  display: grid;
  width: 80vw;
  grid-template-columns: 1fr;
  grid-template-rows: auto;
}

.record {
  margin: 5px 10px;
  width: 80%;
  display: grid;
  border-bottom: solid 1px #999999;
  grid-template-columns: 1fr 6fr 6fr 3fr 2fr 2fr;
  grid-template-rows: auto;
}

.button {
  padding: 0px;
  width: 80%;
}

.button-null {
  background-color: #999999;
}

.button-red {
  background-color: red;
}

.button-green {
  background-color: green;
}

.button-yellow {
  background-color: gold;
  color: black;
}

.yes {
  background-color: green;
  border: solid 2px black;
  border-radius: 5px;
}

.no {
  background-color: red;
  border: solid 2px black;
  border-radius: 5px;
}
</style>
