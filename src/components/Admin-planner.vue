<template>
  <div>
    <div class="header-title">Event planner</div>
    <div>
      <input v-model="name" placeholder="Nazwa eventu" />
    </div>
    <Datepicker
      v-model="date"
      :language="pl"
      :format="'dd MMMM yyyy'"
      class="datepicker"
      placeholder="Wybierz datę"
    />
    <div class="container">
      <div></div>
      <input v-model="hour" placeholder="Godzina" class="godz" disabled />
      <div class="suwak" @click="hourChange(-1)">
        <v-icon name="chevron-left" scale="1.8"></v-icon>
      </div>
      <div class="suwak" @click="hourChange(1)">
        <v-icon name="chevron-right" scale="1.8"></v-icon>
      </div>

      <input v-model="minute" placeholder="Minuta" class="godz" disabled />
      <div class="suwak" @click="minuteChange(-10)">
        <v-icon name="chevron-left" scale="1.8"></v-icon>
      </div>
      <div class="suwak" @click="minuteChange(10)">
        <v-icon name="chevron-right" scale="1.8"></v-icon>
      </div>
      <div></div>
    </div>
    <div>
      <div class="button" @click="zaplanuj">Zaplanuj</div>
      <div>{{ msg }}</div>
    </div>
  </div>
</template>


<script>
import Datepicker from "vuejs-datepicker";
import { pl } from "vuejs-datepicker/dist/locale";
import "vue-awesome/icons/chevron-right";
import "vue-awesome/icons/chevron-left";
import Icon from "vue-awesome/components/Icon";
import axios from "axios";

export default {
  name: "User-profile",
  components: {
    Datepicker,
    "v-icon": Icon
  },
  data() {
    return {
      pl: pl,
      name: "",
      date: "",
      hour: "20",
      minute: "00",
      finaldate: this.fulldate,
      msg: null
    };
  },
  computed: {
    fulldate: function() {
      var moment = require("moment");
      var result = this.date;
      return `${moment(result).format("YYYY-MM-DD")} ${this.hour}:${
        this.minute
      }:00`;
    }
  },

  methods: {
    hourChange: function(v) {
      var h = parseInt(this.hour);
      if (h >= 23 && v == 1) {
        h = 0;
      } else if (h <= 0 && v == -1) {
        h = 23;
      } else {
        h += v;
      }
      if (h < 10) {
        h = "0" + h;
      }
      this.hour = h;
    },
    minuteChange: function(v) {
      var h = parseInt(this.minute);
      if (h >= 50 && v == 10) {
        h = 0;
      } else if (h <= 0 && v == -10) {
        h = 50;
      } else {
        h += v;
      }
      if (h < 10) {
        h = "0" + h;
      }
      this.minute = h;
    },
    zaplanuj: function() {
      var valid = new Date(this.date).getTime() > 0;
      if (this.name == "" || valid == false) {
        return;
      } else {
        var data = {
          name: this.name,
          date: this.fulldate
        };
        axios.post("http://35.156.131.239:3000/planEvent", data).then(res => {
          if (res.status == 200) {
            this.msg = "Zaplanowano event.";
          } else {
            this.msg = "Nie udało się zaplanować eventu";
          }
        });
      }
    }
  },
  mounted() {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.datepicker {
  display: inline-block;
  color: black;
  font-size: 20px;
}

.container {
  display: grid;
  grid-template-columns: auto 40px 32px 32px 40px 32px 32px auto;
  grid-template-rows: 32px;
  grid-gap: 10px;
  margin: 10px;
}

.suwak {
  border: solid 1px white;
  font-size: 20px;
  text-align: center;
}

.suwak:hover {
  background-color: green;
}

.godz {
  margin-bottom: 0px;
}
</style>
