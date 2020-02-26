<template>
  <div class="container">
    <div>
      <div class="user">
        Zalogowany jako:
        <br />
        [BDO] {{ familyName }} / [D] {{ discordName }}
      </div>
    </div>
    <div>
      <div class="header">Witamy w Jokers</div>
    </div>
    <div>
      <div class="logout">
        <div class="button1" @click="logout">Wyloguj</div>
      </div>
    </div>

    <div>
      <div class="nav">
        <div>
          <div class="title">User</div>
          <div :class="{underline: tab == 0}" class="button" @click="nav(0)">Main</div>
          <div :class="{underline: tab == 1}" class="button" @click="nav(1)">Eventy PVP</div>
          <div :class="{underline: tab == 2}" class="button" @click="nav(2)">Historia obecności</div>
          <div :class="{underline: tab == 4}" class="button" @click="nav(4)">Party</div>
        </div>
        <div v-if="this.perm > 0">
          <div class="title">Admin</div>
          <div :class="{underline: tab == 12}" class="button" @click="nav(12)">Użytkownicy Jokers</div>
          <div :class="{underline: tab == 6}" class="button" @click="nav(6)">Profile</div>
          <div :class="{underline: tab == 9}" class="button" @click="nav(9)">Ad-hoc party</div>
        </div>
        <div v-if="this.perm > 1">
          <div class="title">Super Admin</div>
          <div :class="{underline: tab == 5}" class="button" @click="nav(5)">Planowanie Eventów PVP</div>
          <div :class="{underline: tab == 3}" class="button" @click="nav(3)">Eventy PVP</div>
          <div :class="{underline: tab == 11}" class="button" @click="nav(11)">Użytkownicy</div>
          <div :class="{underline: tab == 7}" class="button" @click="nav(7)">Eventy - zarządzanie</div>
          <div :class="{underline: tab == 8}" class="button" @click="nav(8)">Szablon party</div>
          <div :class="{underline: tab == 13}" class="button" @click="nav(13)">Notatki</div>
          <div :class="{underline: tab == 14}" class="button" @click="nav(14)">MotD</div>
          <div :class="{underline: tab == 15}" class="button" @click="nav(15)">Tiery</div>
          <div :class="{underline: tab == 16}" class="button" @click="nav(16)">Payout</div>
          <div :class="{underline: tab == 17}" class="button" @click="nav(17)">Cooking [WiP]</div>
        </div>
      </div>
    </div>
    <div>
      <UserEvents v-if="this.tab == 1" class="main" />
      <UserAttendence v-else-if="this.tab == 2" class="main" />
      <UserParty v-else-if="this.tab == 4" class="main" />
      <AdminPlanner v-else-if="this.tab == 5" class="main" />
      <AdminEvents v-else-if="this.tab == 3" class="main" />
      <AdminProfile v-else-if="this.tab == 6" class="main" />
      <AdminCheck v-else-if="this.tab == 7" class="main" />
      <AdminParty v-else-if="this.tab == 8" class="main" />
      <AdminPartyAdhoc v-else-if="this.tab == 9" class="main" />
      <AdminUsers v-else-if="this.tab == 11" class="main" />
      <AdminUsersActive v-else-if="this.tab == 12" class="main" />
      <AdminNotes v-else-if="this.tab == 13" class="main" />
      <AdminMotd v-else-if="this.tab == 14" class="main" />
      <AdminTiers v-else-if="this.tab == 15" class="main" />
      <AdminPayout v-else-if="this.tab == 16" class="main" />
      <AdminCooking v-else-if="this.tab == 17" class="main" />
      <UserMain v-else class="main" />
    </div>
  </div>
</template>

<script>
import UserMain from "./User-main";
import UserEvents from "./User-events";
import UserAttendence from "./User-attendence";
import UserParty from "./User-party";
import AdminPlanner from "./Admin-planner";
import AdminEvents from "./Admin-events";
import AdminUsers from "./Admin-users";
import AdminUsersActive from "./Admin-users-active";
import AdminProfile from "./Admin-profile";
import AdminCheck from "./Admin-check";
import AdminParty from "./Admin-party";
import AdminPartyAdhoc from "./Admin-party-adhoc";
import AdminNotes from "./Admin-notes";
import AdminMotd from "./Admin-motd";
import AdminTiers from "./Admin-tiers";
import AdminPayout from "./Admin-payout";
import AdminCooking from "./Admin-cooking";
import router from "../router";

export default {
  name: "Login",
  components: {
    UserMain,
    UserEvents,
    UserAttendence,
    UserParty,
    AdminPlanner,
    AdminEvents,
    AdminUsers,
    AdminUsersActive,
    AdminProfile,
    AdminCheck,
    AdminParty,
    AdminPartyAdhoc,
    AdminNotes,
    AdminMotd,
    AdminTiers,
    AdminPayout,
    AdminCooking
  },
  data() {
    return {
      loginAuth: null,
      familyName: "",
      discordName: "",
      tab: "",
      perm: 0
    };
  },
  methods: {
    logout: function() {
      // wylogowanie sieeeee
      localStorage.setItem("userData", "");
      router.push("/");
    },
    nav: function(x) {
      this.tab = x;
      this.$store.state.eventList = true;
    }
  },
  mounted() {
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
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: grid;
  background-color: #262626;
  grid-template-columns: 15vw 70vw auto;
  grid-template-rows: 60px auto;
  grid-template-areas:
    "user header logout"
    "nav main main";
}

.user {
  grid-area: user;
  position: fixed;
  width: 15vw;
  height: 60px;
}
.header {
  grid-area: header;
  position: fixed;
  width: 70vw;
  z-index: 10;
  background-color: #262626;
  height: 60px;
  font-size: 40px;
}
.logout {
  grid-area: logout;
  position: fixed;
  width: 15vw;
  background-color: #262626;
  height: 60px;
}
.nav {
  grid-area: nav;
  position: fixed;
  width: 15vw;
}
.main {
  grid-area: main;
}

.title {
  background-color: darkblue;
  width: 15vw;
  font-size: 20px;
}

.button {
  background-color: darkgreen;
  margin: 3px auto;
  padding: 3px;
  text-align: left;
  width: 80%;
}

.button:hover {
  filter: brightness(1.3);
}

.underline {
  text-align: right;
  border-bottom: solid 2px red;
  margin-left: 40px;
}

.button1 {
  display: inline-block;
  background-color: green;
  padding: 5px;
  cursor: pointer;
  border-radius: 5px;
  border: solid 2px black;
  letter-spacing: 1px;
  color: white;
  margin-top: 10px;
}

.button1:hover {
  filter: brightness(1.3);
}

.button1:active {
  filter: brightness(1.3);
  border: solid 2px white;
}
</style>
