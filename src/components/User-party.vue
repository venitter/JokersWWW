<template>
  <div class="cont">
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
          <div :key="grp.id">
            <div>
              <div hidden>{{ i }}</div>
              <div v-if="grp.status == 'YES'" class="party-h3-yes">
                <div>
                  <img :src="getImg(grp.class)" />
                </div>
                <div @mouseover="eggOn" @mouseleave="eggOff">{{ grp.family_name }}</div>
                <div>{{ grp.pos }}</div>
              </div>
              <div v-else-if="grp.status == 'NO'" class="party-h3-no">
                <div>
                  <img :src="getImg(grp.class)" />
                </div>
                <div @mouseover="eggOn" @mouseleave="eggOff">{{ grp.family_name }}</div>
                <div>{{ grp.pos }}</div>
              </div>
            </div>
          </div>
        </template>
        <div class="party-h3" v-for="index in 17 - countgrp[i]" :key="index.id">
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
          <div :key="grp.id">
            <div>
              <div hidden>{{ i + 6 }}</div>
              <div v-if="grp.status == 'YES'" class="party-h3-yes">
                <div>
                  <img :src="getImg(grp.class)" />
                </div>
                <div @mouseover="eggOn" @mouseleave="eggOff">{{ grp.family_name }}</div>
                <div>{{ grp.pos }}</div>
              </div>
              <div v-else-if="grp.status == 'NO'" class="party-h3-no">
                <div>
                  <img :src="getImg(grp.class)" />
                </div>
                <div @mouseover="eggOn" @mouseleave="eggOff">{{ grp.family_name }}</div>
                <div>{{ grp.pos }}</div>
              </div>
            </div>
          </div>
        </template>
        <div class="party-h3" v-for="index in 3 - countgrp[i + 6]" :key="index.id">
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
    <div>
      <!-- PROTECTIVE AREA -->
      <div style="margin-top: 40px">
        <div class="pa" v-for="i in 3" :key="i.id">
          <div class="party-h1">PA {{ i }}</div>
          <template v-for="pa in pa[i]">
            <div :key="pa.id">
              <div>
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
          <div class="party-h3" v-for="index in 5 - countpa[i]" :key="index.id">
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
    </div>
    <div class="egg" v-html="egg"></div>
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
      egg: null
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
    getImg: function(x) {
      if (x == null) {
        return;
      } else {
        return `../static/img/${x}.png`;
      }
    },
    eggOn: function(e) {
      console.log(e.target.innerHTML);
      this.egg = `<img src='../static/img/${e.target.innerHTML}.PNG' />`;
    },
    eggOff: function() {
      this.egg = null;
    }
  },
  mounted() {
    axios.get("http://35.156.131.239:3000/loadNextSignupsParty").then(res => {
      this.signups = res.data;
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cont {
  display: grid;
  grid-template-columns: 6fr;
  grid-template-rows: auto;
  margin: 10px;
}

.party {
  display: grid;
  grid-template-columns: 2fr 2fr 2fr 2fr 2fr 2fr;
  grid-template-rows: auto;
  grid-gap: 3px;
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

.egg {
  position: absolute;
  bottom: 0px;
  right: 0px;
  z-index: 10;
}
</style>
