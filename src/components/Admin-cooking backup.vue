<template>
  <div>
    <div>
      <div>Jak jakas recepta nie laduje sie, PM na discordzie do Wietnama</div>
      <select v-model="selected" @change="recipeActive">
        <option
          v-for="recipe in recipes"
          :key="recipe.id"
          :value="{id: recipe.id, idb: recipe.idb }"
        >{{ recipe.name }}</option>
      </select>
    </div>
    <div>{{ loading }}</div>
    <div v-for="sOne in recipe" :key="sOne.id" class="sOne">
      <div class="item-a">
        <div
          class="item-b"
          v-if="sOne.stock == 0"
          style="color: red"
        >{{ sOne.name }} x {{ sOne.quant }}</div>
        <div class="item-b" v-else>{{ sOne.name }} x {{ sOne.quant }}</div>
        <div class="item-c">MP: {{ sOne.stock }}</div>
        <div>Buy</div>
        <div v-if="sOne.cost > 0">{{ sOne.cost }}</div>
        <div v-else>{{ sOne.buy }}</div>
        <div>Make</div>
        <div>{{ sOne.make }}</div>
        <div>Sell</div>
        <div>{{ sOne.sell }}</div>
        <div>Blue</div>
        <div>{{ sOne.idb }}</div>
      </div>
      <div v-for="sTwo in sOne.recipe" :key="sTwo.id" class="sTwo">
        <div class="item-a">
          <div
            class="item-b"
            v-if="sTwo.stock == 0"
            style="color: red"
          >{{ sTwo.name }} x {{ sTwo.quant }}</div>
          <div class="item-b" v-else>{{ sTwo.name }} x {{ sTwo.quant }}</div>
          <div class="item-c">MP: {{ sTwo.stock }}</div>
          <div>Buy</div>
          <div>{{ sTwo.cost }}</div>
          <div>Make</div>
          <div>{{ sTwo.make }}</div>
          <div>Sell</div>
          <div>{{ sTwo.sell }}</div>
          <div>Blue</div>
          <div>{{ sTwo.idb }}</div>
        </div>
        <div v-for="sThree in sTwo.recipe" :key="sThree.id" class="sThree">
          <div class="item-a">
            <div
              class="item-b"
              v-if="sThree.stock == 0"
              style="color: red"
            >{{ sThree.name }} x {{ sThree.quant }}</div>
            <div class="item-b" v-else>{{ sThree.name }} x {{ sThree.quant }}</div>
            <div class="item-c">MP: {{ sThree.stock }}</div>
            <div>Buy</div>
            <div>{{ sThree.cost }}</div>
            <div>Make</div>
            <div>{{ sThree.make }}</div>
            <div>Sell</div>
            <div>{{ sThree.sell }}</div>
            <div>Blue</div>
            <div>{{ sThree.idb }}</div>
          </div>
          <div v-for="sFour in sThree.recipe" :key="sFour.id" class="sFour">
            <div class="item-a">
              <div
                class="item-b"
                v-if="sFour.stock == 0"
                style="color: red"
              >{{ sFour.name }} x {{ sFour.quant }}</div>
              <div class="item-b" v-else>{{ sFour.name }} x {{ sFour.quant }}</div>
              <div class="item-c">MP: {{ sFour.stock }}</div>
              <div>Buy</div>
              <div>{{ sFour.cost }}</div>
              <div>Make</div>
              <div>{{ sFour.make }}</div>
              <div>Sell</div>
              <div>{{ sFour.sell }}</div>
              <div>Blue</div>
              <div>{{ sFour.idb }}</div>
            </div>
            <div></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import recipes from "./recipes.json";

export default {
  name: "Cooking",
  data() {
    return {
      loading: "",
      selected: "",
      mastery: 2.63,
      masteryS: 0.3,
      recipes: recipes,
      shop: [
        {
          name: "Base Sauce",
          id: 9018,
          pricePerOne: 40,
          count: "infinite"
        },
        {
          name: "Cooking Oil",
          id: 9007,
          pricePerOne: 20,
          count: "infinite"
        },
        {
          name: "Cooking Wine",
          id: 9017,
          pricePerOne: 40,
          count: "infinite"
        },
        {
          name: "Deep Frying Oil",
          id: 9016,
          pricePerOne: 40,
          count: "infinite"
        },
        {
          name: "Leavening Agent",
          id: 9005,
          pricePerOne: 20,
          count: "infinite"
        },
        {
          name: "Mineral Water",
          id: 9059,
          pricePerOne: 30,
          count: "infinite"
        },
        {
          name: "Olive Oil",
          id: 9015,
          pricePerOne: 40,
          count: "infinite"
        },
        {
          name: "Raw Sugar",
          id: 9009,
          pricePerOne: 200,
          count: "infinite"
        },
        {
          name: "Salt",
          id: 9001,
          pricePerOne: 20,
          count: "infinite"
        },
        {
          name: "Sugar",
          id: 9002,
          pricePerOne: 20,
          count: "infinite"
        },
        {
          name: "Sun-Dried Salt",
          id: 9008,
          pricePerOne: 200,
          count: "infinite"
        },
        {
          name: "Strawberry",
          id: 7304,
          pricePerOne: 700,
          count: "infinite"
        },
        {
          name: "Paprika",
          id: 7312,
          pricePerOne: 850,
          count: "infinite"
        }
      ],
      recipe: []
    };
  },
  methods: {
    recipeActive: async function() {
      this.loading = "Ładuje się...";
      // czyszczenie recepty
      this.recipe = [];
      //glowna recepta

      //funkcja do znajdowania po ID
      var data = null;
      var blue = 0;
      data = await this.findShop(this.selected.id);
      if (data == undefined) {
        data = await axios
          .get(`https://omegapepega.com/eu/${this.selected.id}/0`)
          .then(response => {
            return response.data;
          })
          .catch(error => {
            console.log(error);
          });
      }
      var av = data.count;
      if (data.count > 1000) {
        av = "1000+";
      }
      //funkcja do znajdowania po ID koniec
      var obj = {
        id: this.selected.id,
        idb: this.selected.idb,
        name: data.name,
        buy: data.pricePerOne,
        buyBlue: 0,
        sell: Math.round(data.pricePerOne * 0.845),
        cost: data.pricePerOne,
        make: 0,
        profit: 0,
        stock: av,
        quant: 1,
        recipe: []
      };
      //nowy stage 1
      var f1 = this.recipes.find(x => {
        return x.id == this.selected.id;
      });
      if (f1 != undefined) {
        var ings1 = [f1.ing1, f1.ing2, f1.ing3, f1.ing4, f1.ing5];
        var quants1 = [f1.quant1, f1.quant2, f1.quant3, f1.quant4, f1.quant5];

        var lenf1 = (Object.keys(f1).length - 3) / 2;
        for (var i = 0; i < lenf1; i++) {
          //funkcja do znajdowania po ID s2
          data = await this.findShop(ings1[i]);
          if (data == undefined) {
            data = await axios
              .get(`https://omegapepega.com/eu/${ings1[i]}/0`)
              .then(response => {
                return response.data;
              })
              .catch(error => {
                console.log(error);
              });
          }
          av = data.count;
          if (data.count > 1000) {
            av = "1000+";
          }
          //funkcja do znajdowania po ID koniec

          var obj1 = {
            id: ings1[i],
            idb: 0,
            name: data.name,
            buy: data.pricePerOne,
            buyBlue: 0,
            sell: Math.round(data.pricePerOne * quants1[i] * 0.845),
            cost: data.pricePerOne * quants1[i],
            make: 0,
            profit: 0,
            stock: av,
            quant: quants1[i],
            recipe: []
          };
          //nowy stage 2
          var f2 = this.recipes.find(x => {
            return x.id == obj1.id;
          });
          if (f2 != undefined) {
            var ings2 = [f2.ing1, f2.ing2, f2.ing3, f2.ing4, f2.ing5];
            var quants2 = [
              f2.quant1,
              f2.quant2,
              f2.quant3,
              f2.quant4,
              f2.quant5
            ];
            var lenf2 = (Object.keys(f2).length - 3) / 2;
            for (var j = 0; j < lenf2; j++) {
              //funkcja do znajdowania po ID s2
              data = await this.findShop(ings2[j]);
              if (data == undefined) {
                data = await axios
                  .get(`https://omegapepega.com/eu/${ings2[j]}/0`)
                  .then(response => {
                    return response.data;
                  })
                  .catch(error => {
                    console.log(error);
                  });
              }
              av = data.count;
              if (data.count > 1000) {
                av = "1000+";
              }
              //funkcja do znajdowania po ID koniec

              var obj2 = {
                id: ings2[j],
                idb: 0,
                quant: quants2[j],
                name: data.name,
                buy: data.pricePerOne,
                buyBlue: 0,
                sell: Math.round(data.pricePerOne * quants2[j] * 0.845),
                cost: data.pricePerOne * quants2[j],
                make: 0,
                profit: 0,
                stock: av,
                recipe: []
              };
              //nowy stage 3
              var f3 = this.recipes.find(x => {
                return x.id == obj2.id;
              });
              if (f3 != undefined) {
                var ings3 = [f3.ing1, f3.ing2, f3.ing3, f3.ing4, f3.ing5];
                var quants3 = [
                  f3.quant1,
                  f3.quant2,
                  f3.quant3,
                  f3.quant4,
                  f3.quant5
                ];
                var lenf3 = (Object.keys(f3).length - 3) / 2;
                for (var k = 0; k < lenf3; k++) {
                  //funkcja do znajdowania po ID s2
                  data = await this.findShop(ings3[k]);
                  if (data == undefined) {
                    data = await axios
                      .get(`https://omegapepega.com/eu/${ings3[k]}/0`)
                      .then(response => {
                        return response.data;
                      })
                      .catch(error => {
                        console.log(error);
                      });
                  }
                  av = data.count;
                  if (data.count > 1000) {
                    av = "1000+";
                  }
                  //funkcja do znajdowania po ID koniec

                  var obj3 = {
                    id: ings3[k],
                    idb: 0,
                    name: data.name,
                    buy: data.pricePerOne,
                    buyBlue: 0,
                    sell: Math.round(data.pricePerOne * quants3[k] * 0.845),
                    cost: data.pricePerOne * quants3[k],
                    make: 0,
                    profit: 0,
                    stock: av,
                    quant: quants3[k],
                    recipe: []
                  };
                  // stage 3 sumup
                  obj2.make += Math.round(
                    (obj3.cost * obj2.quant) / this.mastery
                  );
                  if (obj3.make == 0) {
                    obj3.make = obj3.cost;
                  }
                  obj2.recipe.push(obj3);
                }
              }
              // stage 2 sumup
              obj1.make += Math.round((obj2.cost * obj1.quant) / this.mastery);
              if (obj2.make == 0) {
                obj2.make = obj2.cost;
              }
              if (obj1.idb != undefined && obj1.idb != "" && obj1.idb != 0) {
                blue = await axios
                  .get(`https://omegapepega.com/eu/${obj1.idb}/0`)
                  .then(response => {
                    return response.data.pricePerOne;
                  })
                  .catch(error => {
                    console.log(error);
                  });
              }
              if (f3 != undefined) {
                obj2.idb = f3.idb;
              }
              obj1.buyBlue = blue;
              obj1.recipe.push(obj2);
            }
          }
          // stage 1 sumup
          obj.make += Math.round((obj1.cost * obj.quant) / this.mastery);
          if (obj1.make == 0) {
            obj1.make = obj1.cost;
          }
          if (obj.idb != undefined && obj.idb != "" && obj.idb != 0) {
            blue = await axios
              .get(`https://omegapepega.com/eu/${obj.idb}/0`)
              .then(response => {
                return response.data.pricePerOne;
              })
              .catch(error => {
                console.log(error);
              });
          }
          if (f2 != undefined) {
            obj1.idb = f2.idb;
          }
          console.log(blue);
          obj.buyBlue = blue;
          obj.recipe.push(obj1);
        }
      }
      this.recipe.push(obj);
      this.loading = null;
      // nowy stage 1 koniec
    },
    findShop: function(x) {
      var wynik = this.shop.find(f => {
        return f.id == x;
      });
      return wynik;
    }
  },
  computed: {},
  mounted() {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cont {
  display: grid;
  color: #d9d9d9;
}

.sOne {
  display: grid;
  grid-column: 1 / 2;
  grid-template-columns: repeat(1fr);
  place-self: stretch start;
}
.sTwo {
  display: grid;
  grid-column: 2 / 3;
  grid-template-columns: repeat(1fr);
  place-self: stretch start;
}
.sThree {
  display: grid;
  grid-column: 3 / 4;
  grid-template-columns: repeat(1fr);
  place-self: stretch start;
}
.sFour {
  display: grid;
  grid-column: 4 / 5;
  grid-template-columns: repeat(1fr);
  place-self: stretch start;
}

.item-a {
  display: grid;
  grid-template-columns: 50px 50px 50px 50px 50px;
  grid-template-rows: auto;
  text-align: center;
  font-family: "Courier New", Courier, monospace;
  font-size: 12px;
  grid-row: 1 / 6;
  grid-column: 1 / col2-start;
  place-self: stretch stretch;
  border: solid 1px grey;
  margin-right: -1px;
  margin-top: -1px;
}

.item-b {
  grid-column: 1 / 5;
  text-align: center;
  place-self: center center;
}
.item-c {
  grid-column: 5 / end;
  text-align: center;
  place-self: center center;
}
</style>
