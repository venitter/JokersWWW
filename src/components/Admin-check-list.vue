<template>
  <div>
    <div class="picker">
      <div></div>
      <div class="button" @click="minus">
        <v-icon name="chevron-left" scale="1.8"></v-icon>
      </div>
      <div class="mc">{{ miesiace[month] }}</div>
      <div class="rok">{{ year }}</div>
      <div class="button" @click="plus">
        <v-icon name="chevron-right" scale="1.8"></v-icon>
      </div>
      <div></div>
    </div>
    <div class="container">
      <div v-bind:key="event.id" v-for="(event, index) in events" class="item">
        <div>B</div>
        <div>Nazwa</div>
        <div>Data</div>
        <div>Akcja</div>
        <div hidden>{{ event.idevents }}</div>
        <div hidden>{{ event.blocked }}</div>
        <div hidden>{{ index + 1}}</div>
        <div>
          <v-icon v-if="event.blocked == 0" name="lock-open"></v-icon>
          <v-icon v-else style="color: red;" name="lock"></v-icon>
        </div>
        <div>{{ event.name }}</div>
        <div>{{ event.date | moment("DD.MM HH:mm") }}</div>
        <div class="akcje">
          <div class="button" @click="pick">Obecność</div>
          <div class="button" @click="remove">Usuń</div>
          <div v-if="event.blocked == 0" class="button" @click="block">Blokuj</div>
          <div v-else class="button" @click="block">Odblokuj</div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import "vue-awesome/icons/chevron-right";
import "vue-awesome/icons/chevron-left";
import "vue-awesome/icons/lock";
import "vue-awesome/icons/lock-open";
import Icon from "vue-awesome/components/Icon";

export default {
  name: "User-events",
  components: {
    "v-icon": Icon
  },
  data() {
    return {
      events: "",
      month: new Date().getMonth(),
      miesiace: [
        "Styczeń",
        "Luty",
        "Marzec",
        "Kwiecień",
        "Maj",
        "Czerwiec",
        "Lipiec",
        "Sierpień",
        "Wrzesień",
        "Październik",
        "Listopad",
        "Grudzień"
      ],
      year: new Date().getFullYear()
    };
  },
  computed: {},
  methods: {
    minus: function() {
      var m = this.month;
      if (m == 0) {
        this.year--;
        this.month = 11;
      } else {
        this.month--;
      }
      axios
        .get("http://35.156.131.239:3000/loadEventsCheck", {
          params: {
            month: this.month,
            year: this.year
          }
        })
        .then(res => {
          this.events = res.data;
        });
    },
    plus: function() {
      var m = this.month;
      if (m == 11) {
        this.year++;
        this.month = 0;
      } else {
        this.month++;
      }
      axios
        .get("http://35.156.131.239:3000/loadEventsCheck", {
          params: {
            month: this.month,
            year: this.year
          }
        })
        .then(res => {
          this.events = res.data;
        });
    },
    remove: function(e) {
      var data = {
        id: e.target.parentNode.parentNode.children[4].innerHTML
      };

      axios.post("http://35.156.131.239:3000/removeEvent", data);
      var i = e.target.parentNode.parentNode.children[6].innerHTML - 1;
      this.events.splice(i, 1);
    },
    block: function(e) {
      var data = {
        id: e.target.parentNode.parentNode.children[4].innerHTML,
        blocked: e.target.parentNode.parentNode.children[5].innerHTML
      };

      axios.post("http://35.156.131.239:3000/blockEvent", data).then(res => {
        if (res.status != 200) {
          return;
        }
      });
      var i = e.target.parentNode.parentNode.children[6].innerHTML - 1;
      if (data.blocked == 0) {
        this.events[i].blocked = 1;
      } else {
        this.events[i].blocked = 0;
      }
    },
    pick: function(e) {
      this.$store.state.eventList = false;
      this.$store.state.detailsEventId =
        e.target.parentNode.parentNode.children[4].innerHTML;
      this.$store.state.detailsEventName =
        e.target.parentNode.parentNode.children[8].innerHTML;
      this.$store.state.detailsEventDate =
        e.target.parentNode.parentNode.children[9].innerHTML;
    }
  },
  mounted() {
    axios
      .get("http://35.156.131.239:3000/loadEventsCheck", {
        params: {
          month: this.month,
          year: this.year
        }
      })
      .then(res => {
        this.events = res.data;
      });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.picker {
  display: grid;
  width: 80vw;
  grid-template-columns: auto 40px 200px 100px 40px auto;
  grid-template-rows: 34px;
}

.container {
  display: grid;
  width: 80vw;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: auto;
}

.item {
  border: solid 1px #999999;
  display: grid;
  grid-template-columns: 30px 1fr 1fr 1fr;
  grid-template-rows: auto;
  margin: 20px;
  padding: 5px;
}

.akcje {
  display: grid;
  grid-template-columns: 80px 45px 75px;
  grid-template-rows: auto;
  grid-gap: 5px;
}

.button {
  font-size: 12px;
  padding: 2px 2px;
}

.mc,
.rok {
  font-size: 20px;
  border-top: solid 2px black;
  border-bottom: solid 2px black;
  background-color: grey;
}
</style>
