<template>
  <div class="cont">
    <div class="szablon">
      <div class="button" @click="zaladujSzablon">Załaduj Szablon Party</div>
      <div
        style="font-size: 12px"
      >Ostatnio edytowany: {{ disco }} ( {{ fam }} ) {{ editTime | moment("DD.MM HH:mm") }}</div>
    </div>
    <div class="party">
      <div class="party-h1">Main</div>
      <div class="party-h1">Main</div>
      <div class="party-h1">Main</div>
      <div class="party-h1">Rotacyjny</div>
      <div class="party-h1">Cannon</div>
      <div class="party-h1">Defence</div>
      <div class="party-h2">Platoon 1</div>
      <div class="party-h2">Platoon 2</div>
      <div class="party-h2">Platoon 3</div>
      <div class="party-h2">Platoon 4</div>
      <div class="party-h2">Platoon 5</div>
      <div class="party-h2">Platoon 6</div>
      <!-- GRUPY -->
      <div v-for="i in 6" :key="i.id">
        <template v-for="grp in grp[i]">
          <div :key="grp.id" @dragover.prevent @drop="onDropParty">
            <div draggable="true" @drag="onDrag">
              <div hidden>{{ i }}</div>
              <div v-if="grp.status == 'YES'" class="party-h3-yes">
                <div>
                  <img :src="getImg(grp.class)" />
                </div>
                <div>{{ grp.family_name }}</div>
                <div>{{ grp.pos }}</div>
              </div>
              <div v-else-if="grp.status == 'NO'" class="party-h3-no">
                <div>
                  <img :src="getImg(grp.class)" />
                </div>
                <div>{{ grp.family_name }}</div>
                <div>{{ grp.pos }}</div>
              </div>
            </div>
          </div>
        </template>
        <div
          class="party-h3"
          v-for="index in 17 - countgrp[i]"
          :key="index.id"
          @dragover.prevent
          @drop="onDropParty"
        >
          <div hidden>{{ i }}</div>
          <div>
            <div hidden>empty</div>
            <div>empty</div>
            <div hidden>empty</div>
          </div>
        </div>
      </div>
      <div class="party-h2">BOMBER 1</div>
      <div class="party-h2">BOMBER 2</div>
      <div class="party-h2">BOMBER 3</div>
      <div class="party-h2">BOMBER 4</div>
      <div class="party-h2">BOMBER 5</div>
      <div class="party-h2">BOMBER 6</div>
      <!-- BOMBERZY -->
      <div v-for="i in 6" :key="i.id">
        <template v-for="grp in grp[i + 6]">
          <div :key="grp.id" @dragover.prevent @drop="onDropParty">
            <div draggable="true" @drag="onDrag">
              <div hidden>{{ i + 6 }}</div>
              <div v-if="grp.status == 'YES'" class="party-h3-yes">
                <div>
                  <img :src="getImg(grp.class)" />
                </div>
                <div>{{ grp.family_name }}</div>
                <div>{{ grp.pos }}</div>
              </div>
              <div v-else-if="grp.status == 'NO'" class="party-h3-no">
                <div>
                  <img :src="getImg(grp.class)" />
                </div>
                <div>{{ grp.family_name }}</div>
                <div>{{ grp.pos }}</div>
              </div>
            </div>
          </div>
        </template>
        <div
          class="party-h3"
          v-for="index in 3 - countgrp[i + 6]"
          :key="index.id"
          @dragover.prevent
          @drop="onDropParty"
        >
          <div hidden>{{ i + 6 }}</div>
          <div>
            <div hidden>empty</div>
            <div>empty</div>
            <div hidden>empty</div>
          </div>
        </div>
      </div>
    </div>
    <!-- GRUPA bez PT -->
    <div class="item">
      <div class="party-h3" style="color: white" @dragover.prevent @drop="onDropSmietnik">nieaktywni</div>
      <div>
        <template v-for="signup in signups">
          <div :key="signup.id" v-if="signup.grp_adhoc == 0" @dragover.prevent @drop="onDropParty">
            <div draggable="true" @drag="onDrag">
              <div hidden>0</div>
              <div v-if="signup.status == 'YES'" class="party-h3-yes">
                <div>
                  <img :src="getImg(signup.class)" />
                </div>
                <div>{{ signup.family_name }}</div>
                <div>{{ signup.pos }}</div>
              </div>
              <div v-else-if="signup.status == 'NO'" class="party-h3-no">
                <div>
                  <img :src="getImg(signup.class)" />
                </div>
                <div>{{ signup.family_name }}</div>
                <div>{{ signup.pos }}</div>
              </div>
            </div>
          </div>
        </template>
      </div>
    </div>
    <div>
      <!-- PROTECTIVE AREA -->
      <div style="margin-top: 40px">
        <div class="pa" v-for="i in 3" :key="i.id">
          <div class="party-h1">PA {{ i }}</div>
          <template v-for="pa in pa[i]">
            <div :key="pa.id" @dragover.prevent @drop="onDropPA">
              <div draggable="true" @drag="onDragPA">
                <div hidden>{{ i }}</div>
                <div v-if="pa.status == 'YES'" class="party-h3-yes">
                  <div>
                    <img :src="getImg(pa.class)" />
                  </div>
                  <div>{{ pa.family_name }}</div>
                  <div>{{ pa.pos }}</div>
                </div>
                <div v-else-if="pa.status == 'NO'" class="party-h3-no">
                  <div>
                    <img :src="getImg(pa.class)" />
                  </div>
                  <div>{{ pa.family_name }}</div>
                  <div>{{ pa.pos }}</div>
                </div>
              </div>
            </div>
          </template>
          <div
            class="party-h3"
            v-for="index in 5 - countpa[i]"
            :key="index.id"
            @dragover.prevent
            @drop="onDropPA"
          >
            <div hidden>{{ i }}</div>
            <div>
              <div hidden>empty</div>
              <div>empty</div>
              <div hidden>empty</div>
            </div>
          </div>
        </div>
      </div>
      <!-- PROTECTIVE AREA ŚMIETNIK-->
      <div>
        <div
          style="margin-top: 40px"
          @dragover.prevent
          @drop="onDropSmietnikPA"
          class="pa-smietnik"
        >
          <div class="party-h3" style="color: white">nieaktywni PA</div>
          <template v-for="pa in pa[0]">
            <div :key="pa.id" @dragover.prevent @drop="onDropPA">
              <div draggable="true" @drag="onDragPA">
                <div hidden>0</div>
                <div v-if="pa.status == 'YES'" class="party-h3-yes">
                  <div>
                    <img :src="getImg(pa.class)" />
                  </div>
                  <div>{{ pa.family_name }}</div>
                  <div>{{ pa.pos }}</div>
                </div>
                <div v-else-if="pa.status == 'NO'" class="party-h3-no">
                  <div>
                    <img :src="getImg(pa.class)" />
                  </div>
                  <div>{{ pa.family_name }}</div>
                  <div>{{ pa.pos }}</div>
                </div>
              </div>
            </div>
          </template>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  name: "User-party",
  components: {},
  data() {
    return {
      signups: [],
      disco: "",
      fam: "",
      editTime: "",
      discordName: "",
      familyName: ""
    };
  },
  computed: {
    grp: function() {
      var grupy = [];
      for (var x = 0; x < 13; x++) {
        var result = this.signups.filter(e => {
          return e.grp_adhoc == x;
        });
        grupy.push(result);
      }
      return grupy;
    },
    countgrp: function() {
      var policz = [];
      for (var x = 0; x < 13; x++) {
        var result = this.grp[x].length;
        policz.push(result);
      }
      return policz;
    },
    pa: function() {
      var grupy = [];
      for (var x = 0; x < 4; x++) {
        var result = this.signups.filter(e => {
          return (
            (e.class == "Witch" ||
              e.class == "Wizard" ||
              e.class == "Valkyrie") &&
            e.pa_adhoc == x
          );
        });
        grupy.push(result);
      }
      return grupy;
    },
    countpa: function() {
      var policz = [];
      for (var x = 0; x < 4; x++) {
        var result = this.pa[x].length;
        policz.push(result);
      }
      return policz;
    }
  },
  methods: {
    zaladujSzablon: function() {
      axios.get("http://35.156.131.239:3000/loadSzablon").then(res => {
        this.signups = res.data;
      });
    },
    getImg: function(x) {
      if (x == null) {
        return;
      } else {
        return `../static/img/${x}.png`;
      }
    },
    onDropParty: function(e) {
      e.stopPropagation();
      e.preventDefault();

      var src_grp = this.src_grp;
      var src_fn = this.src_fn;
      var src_id = this.signups.findIndex(e => {
        return e.family_name == src_fn;
      });

      var tgt_grp = e.target.parentNode.parentNode.children[0].innerHTML;
      var tgt_fn = e.target.parentNode.children[1].innerHTML;
      var tgt_id = this.signups.findIndex(e => {
        return e.family_name == tgt_fn;
      });

      var data = {
        tgt_fn: tgt_fn,
        tgt_grp: tgt_grp,
        src_fn: src_fn,
        src_grp: src_grp,
        d_name: this.discordName,
        f_name: this.familyName
      };

      // axios request tu bydzie
      axios.post("http://35.156.131.239:3000/updateGroupAdhoc", data);

      this.signups[src_id].grp_adhoc = tgt_grp;
      if (tgt_id > -1) {
        this.signups[tgt_id].grp_adhoc = src_grp;
      }
    },
    onDropSmietnik: function(e) {
      e.stopPropagation();
      e.preventDefault();

      var src_fn = this.src_fn;
      var src_id = this.signups.findIndex(e => {
        return e.family_name == src_fn;
      });

      var data = {
        tgt_fn: "null",
        tgt_grp: "null",
        src_fn: src_fn,
        src_grp: 0,
        d_name: this.discordName,
        f_name: this.familyName
      };

      // axios request tu bydzie
      axios.post("http://35.156.131.239:3000/updateGroupAdhoc", data);

      this.signups[src_id].grp_adhoc = 0;
    },
    onDrag: function(e) {
      this.src_grp = e.target.children[0].innerHTML;
      this.src_fn = e.target.children[1].children[1].innerHTML;
    },
    onDropPA: function(e) {
      e.stopPropagation();
      e.preventDefault();

      var src_pa = this.src_pa;
      var src_fn = this.src_fn;
      var src_id = this.signups.findIndex(e => {
        return e.family_name == src_fn;
      });

      var tgt_pa = e.target.parentNode.parentNode.children[0].innerHTML;
      var tgt_fn = e.target.parentNode.children[1].innerHTML;
      var tgt_id = this.signups.findIndex(e => {
        return e.family_name == tgt_fn;
      });

      var data = {
        tgt_fn: tgt_fn,
        tgt_pa: tgt_pa,
        src_fn: src_fn,
        src_pa: src_pa,
        d_name: this.discordName,
        f_name: this.familyName
      };
      // axios request tu bydzie
      axios.post("http://35.156.131.239:3000/updatePAadhoc", data);

      this.signups[src_id].pa_adhoc = tgt_pa;
      if (tgt_id > -1) {
        this.signups[tgt_id].pa_adhoc = src_pa;
      }
    },
    onDropSmietnikPA: function(e) {
      e.stopPropagation();
      e.preventDefault();

      var src_fn = this.src_fn;
      var src_id = this.signups.findIndex(e => {
        return e.family_name == src_fn;
      });

      var data = {
        tgt_fn: "null",
        tgt_pa: "null",
        src_fn: src_fn,
        src_pa: 0,
        d_name: this.discordName,
        f_name: this.familyName
      };
      // axios request tu bydzie
      axios.post("http://35.156.131.239:3000/updatePAadhoc", data);

      this.signups[src_id].pa_adhoc = 0;
    },
    onDragPA: function(e) {
      this.src_pa = e.target.children[0].innerHTML;
      this.src_fn = e.target.children[1].children[1].innerHTML;
    }
  },
  mounted() {
    var user = JSON.parse(localStorage.getItem("userData"));
    this.discordName = user.discord_name;
    this.familyName = user.family_name;

    axios.get("http://35.156.131.239:3000/loadNextSignupsParty").then(res => {
      this.signups = res.data;
    });

    axios.get("http://35.156.131.239:3000/loadPartyEdit").then(res => {
      console.log(res.data);
      this.disco = res.data[0].party_d;
      this.fam = res.data[0].party_f;
      this.editTime = res.data[0].party;
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cont {
  display: grid;
  grid-template-columns: 6fr 1fr;
  grid-template-rows: auto;
  margin: 10px;
}

.party {
  display: grid;
  grid-template-columns: 2fr 2fr 2fr 2fr 2fr 2fr;
  grid-template-rows: auto;
  grid-gap: 3px;
}

.szablon {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto;
}

.button {
  font-size: 12px;
  padding: 2px;
}

.pa {
  display: grid;
  grid-template-columns: 1fr 2fr 2fr 2fr 2fr 2fr;
  grid-template-rows: auto;
  grid-gap: 3px;
}

.pa-smietnik {
  display: grid;
  grid-template-columns: 2fr 2fr 2fr 2fr 2fr 2fr;
  grid-template-rows: auto;
  grid-gap: 3px;
}

.party-h1 {
  background-color: darkred;
  font-size: 14px;
  margin: 3px;
  padding: 1px;
}

.party-h2 {
  background-color: darkblue;
  font-size: 14px;
  margin: 3px;
  padding: 1px;
}

.party-h3 {
  background-color: #707070;
  color: #919191;
  font-size: 14px;
  margin: 3px;
  padding: 1px;
}

.party-h3-yes {
  background-color: green;
  font-size: 14px;
  display: grid;
  grid-template-columns: 1fr 6fr 1fr;
  grid-template-rows: auto;
  margin: 3px;
  padding: 1px;
}

.party-h3-no {
  background-color: red;
  font-size: 14px;
  display: grid;
  grid-template-columns: 1fr 6fr 1fr;
  grid-template-rows: auto;
  margin: 3px;
  padding: 1px;
}

.item {
  grid-area: 1 / col2-start / last-line / 2;
}

img {
  width: 20px;
  height: 20px;
  filter: brightness(0) invert(1);
  display: block;
}
</style>
