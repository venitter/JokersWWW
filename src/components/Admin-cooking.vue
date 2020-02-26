<template>
  <div>
    <div>
      <div>Jak jakas recepta nie laduje sie, PM na discordzie do Wietnama</div>
      <div
        v-if="this.selectedMasteryCook != '' && this.selectedMasteryBox != ''"
        class="select-box"
      >
        <div>
          <select v-model="selected" @change="recipeActive">
            <option
              v-for="recipe in recipes"
              :key="recipe.id"
              :value="{id: recipe.id, idb: recipe.idb }"
            >{{ recipe.name }}</option>
          </select>
        </div>
        <div>
          <div>Ratio Green / Blue: {{ mastery }} / {{ masteryS }}</div>
          <div class="box-prices">
            <div v-for="(box, index) in box" :key="box.id">
              <div>{{ boxName[index] }}</div>
              <div>{{ box }}</div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="this.selectedMasteryCook == '' || this.selectedMasteryBox == ''">
        <div>Mastery Cook</div>
        <select v-model="selectedMasteryCook" @change="masteryCook">
          <option v-for="m in masteryLvl" :key="m.id" :value="m">{{ m.Mastery }}</option>
        </select>
      </div>
      <div v-if="this.selectedMasteryCook == '' || this.selectedMasteryBox == ''">
        <div>Mastery Box</div>
        <select v-model="selectedMasteryBox" @change="masteryBox">
          <option v-for="m in masteryLvl" :key="m.id" :value="m">{{ m.Mastery }}</option>
        </select>
      </div>
    </div>
    <div>{{ loading }}</div>
    <div v-for="sOne in recipe" :key="sOne.id" class="sOne">
      <div class="item-a">
        <div class="line-a">
          <div
            class="item-b"
            v-if="sOne.stock == 0"
            style="color: red"
          >{{ sOne.name }} x {{ sOne.quant }}</div>
          <div class="item-b" v-else>{{ sOne.name }}</div>
          <div class="item-c">MP: {{ sOne.stock }}</div>
        </div>
        <div class="line-b">
          <div>Buy</div>
          <div>{{ sOne.cost }}</div>
          <div>Make</div>
          <div>{{ sOne.make }}</div>
          <div>Eco</div>
          <div>{{ sOne.eco }}</div>
        </div>
        <div class="line-c">
          <div>BoxBuy</div>
          <div>{{ sOne.boxBuy }}</div>
          <div>BoxMake</div>
          <div>{{ sOne.boxMake }}</div>
          <div>BoxEco</div>
          <div>{{ sOne.boxEco }}</div>
        </div>
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
          <div class="line-b">
            <div>Buy</div>
            <div>{{ sTwo.cost }}</div>
            <div>Make</div>
            <div>{{ sTwo.make }}</div>
            <div>Eco</div>
            <div>{{ sTwo.eco }}</div>
          </div>
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
            <div class="line-b">
              <div>Buy</div>
              <div>{{ sThree.cost }}</div>
              <div>Make</div>
              <div>{{ sThree.make }}</div>
              <div>Eco</div>
              <div>{{ sThree.eco }}</div>
            </div>
          </div>
          <div v-for="sFour in sThree.recipe" :key="sFour.id" class="sFour">
            <div class="item-a">
              <div
                class="item-b"
                v-if="sFour.stock == 0"
                style="color: red"
              >{{ sFour.name }} x {{ sFour.quant }}</div>
              <div class="item-b" v-else>{{ sFour.name }} x {{ sFour.quant }}</div>
              <div class="item-s4a">B: {{ sFour.cost }}</div>
              <div class="item-s4b">MP: {{ sFour.stock }}</div>
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
import masteryList from "./mastery.json";

export default {
  name: "Cooking",
  data() {
    return {
      loading: "",
      selected: "",
      selectedMasteryCook: "",
      selectedMasteryBox: "",
      box: [],
      boxName: [
        "Apprentice",
        "Skilled",
        "Professional",
        "Artisan",
        "Master",
        "Guru"
      ],
      masteryLvl: masteryList,
      mastery: 2.5,
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
    masteryCook: function() {
      this.mastery = this.selectedMasteryCook.Green;
      this.masteryS = this.selectedMasteryCook.Blue;
    },
    masteryBox: function() {
      this.box.push(this.selectedMasteryBox.Apr);
      this.box.push(this.selectedMasteryBox.Skill);
      this.box.push(this.selectedMasteryBox.Pro);
      this.box.push(this.selectedMasteryBox.Art);
      this.box.push(this.selectedMasteryBox.Mas);
      this.box.push(this.selectedMasteryBox.Guru);
    },
    recipeActive: async function() {
      // notyfikacja ladowania
      this.loading = "Ładuje się...";
      // czyszczenie recepty
      this.recipe = [];
      //funkcja do znajdowania po ID
      var data;
      data = await this.findShop(this.selected.id);
      if (data == undefined) {
        data = await this.omegaPepega(this.selected.id);
      }
      //funkcja do znajdowania po ID koniec
      var obj = {
        id: this.selected.id,
        idb: this.selected.idb,
        name: data.name,
        buy: data.pricePerOne,
        buyBlue: 0,
        value: 0,
        sell: 0,
        cost: data.pricePerOne,
        make: 0,
        eco: 0,
        stock: data.count,
        dec: "BUY",
        quant: 1,
        boxBuy: 0,
        boxMake: 0,
        boxEco: 0,
        recipe: ""
      };
      var obj1 = await this.stageTwo(obj.id);
      // wyszukuje cene niebieskiego
      if (obj.idb != 0) {
        var bluePrice = await this.omegaPepega(obj.idb);
        obj.buyBlue = bluePrice.pricePerOne;
      }
      // wylicza koszt zrobienia z kupionych skladnikow
      obj.make = Math.round(
        obj1.reduce((a, { cost }) => a + cost, 0) / this.mastery
      );
      // wylicza koszt zrobienia z optymalnych skladnikow
      obj.eco = Math.round(this.liczEko(obj1) / this.mastery);
      // wylicza wartosc zrobionych potraw jakby dac na sell na MP z VP
      obj.value = Math.round(
        obj.buy + (obj.buyBlue * this.masteryS) / this.mastery
      );
      obj.sell = Math.round(obj.value * 0.845);
      //podejmuje decyzje czy robic czy kupowac
      if (obj.make < obj.sell) {
        obj.dec = "MAKE";
      }
      // oblicza profit z boxow
      var box = await this.findBox(this.selected.id);
      var boxType = box.boxType - 1;
      var boxQuant = box.boxQuant;
      var boxBlueMake = 0;
      if (obj.idb != 0) {
        boxBlueMake = this.box[boxType] * (this.masteryS / (boxQuant / 3));
      }

      console.log(boxBlueMake);

      obj.boxBuy = Math.round(this.box[boxType] - obj.buy * boxQuant);
      obj.boxMake = Math.round(
        this.box[boxType] + boxBlueMake - obj.make * boxQuant
      );
      obj.boxEco = Math.round(
        this.box[boxType] + boxBlueMake - obj.eco * boxQuant
      );
      // dodaje obiekt StageTwo do recept
      obj.recipe = obj1;
      // tworzy aktywna recepte
      this.recipe.push(obj);
      // koniec ladowania
      this.loading = null;
    },
    findShop: function(x) {
      var wynik = this.shop.find(f => {
        return f.id == x;
      });
      return wynik;
    },
    findIdb: function(x) {
      var wynik = this.recipes.find(f => {
        return f.id == x;
      });
      if (wynik != undefined) {
        return wynik.idb;
      } else {
        return 0;
      }
    },
    findBox: function(x) {
      var wynik = this.recipes.find(f => {
        return f.id == x;
      });
      if (wynik != undefined) {
        return wynik;
      } else {
        return 0;
      }
    },
    omegaPepega: function(x) {
      var wynik;
      wynik = axios
        .get(`https://omegapepega.com/eu/${x}/0`)
        .then(response => {
          return response.data;
        })
        .catch(error => {
          console.log(error);
          this.loading =
            "OmegaPepega sie wyjebalo albo maintnance jest. Wybierz ponownie recepte.";
        });
      return wynik;
    },
    stageTwo: async function(x) {
      var wynik = [];
      // recepta
      var f = this.recipes.find(y => {
        return y.id == x;
      });
      if (f != undefined) {
        // tworzy arraya ze znalezionej recepty
        var ings = [f.ing1, f.ing2, f.ing3, f.ing4, f.ing5];
        var quants = [f.quant1, f.quant2, f.quant3, f.quant4, f.quant5];
        // pobiera dane do skladnikow recepty
        for (var i = 0; i < ings.length; i++) {
          // wyszukuje ceny, stock itd
          if (ings[i] != undefined) {
            var data;
            data = await this.findShop(ings[i]);
            if (data == undefined) {
              data = await this.omegaPepega(ings[i]);
            }
            // tworzy obiekt do dodania
            var obj = {
              id: ings[i],
              idb: 0,
              name: data.name,
              buy: data.pricePerOne,
              buyBlue: 0,
              value: 0,
              sell: 0,
              cost: data.pricePerOne * quants[i],
              make: 0,
              eco: 0,
              stock: data.count,
              quant: quants[i],
              dec: "BUY",
              recipe: ""
            };
            // wyznacza najtansza droge do zrobienia recepty
            if (obj.make == 0) {
              obj.eco = obj.cost;
            } else if (obj.make < obj.cost) {
              obj.eco = obj.make;
            } else {
              obj.eco = obj.cost;
            }
            // stageThree start
            var obj1 = await this.stageThree(ings[i]);
            // wyszukuje cene niebieskiego
            obj.idb = await this.findIdb(ings[i]);
            if (obj.idb != 0) {
              var bluePrice = await this.omegaPepega(obj.idb);
              obj.buyBlue = bluePrice.pricePerOne;
            }
            // wylicza koszt zrobienia z kupionych skladnikow
            obj.make = Math.round(
              (obj1.reduce((a, { cost }) => a + cost, 0) * obj.quant) /
                this.mastery
            );
            // wylicza koszt zrobienia z optymalnych skladnikow
            obj.eco = Math.round(
              (this.liczEko(obj1) * obj.quant) / this.mastery
            );
            if (obj.eco > obj.cost) {
              obj.eco = obj.cost;
            }
            // wylicza wartosc zrobionych potraw jakby dac na sell na MP z VP
            obj.value = Math.round(
              obj.buy + (obj.buyBlue * this.masteryS) / this.mastery
            );
            obj.sell = Math.round(obj.value * 0.845) * obj.quant;
            //podejmuje decyzje czy robic czy kupowac
            if (obj.make < obj.sell) {
              obj.dec = "MAKE";
            }
            // wylicza make/eco jesli nie ma recepty
            if (obj.make == 0) {
              obj.make = obj.cost;
              obj.eco = obj.cost;
            }
            // dodaje obiekt StageTwo do recept
            obj.recipe = obj1;
            // tworzy aktywna recepte
            wynik.push(obj);
          }
        }
      }
      return wynik;
    },
    stageThree: async function(x) {
      var wynik = [];
      // recepta
      var f = this.recipes.find(y => {
        return y.id == x;
      });
      if (f != undefined) {
        // tworzy arraya ze znalezionej recepty
        var ings = [f.ing1, f.ing2, f.ing3, f.ing4, f.ing5];
        var quants = [f.quant1, f.quant2, f.quant3, f.quant4, f.quant5];
        // pobiera dane do skladnikow recepty
        for (var i = 0; i < ings.length; i++) {
          // wyszukuje ceny, stock itd
          if (ings[i] != undefined) {
            var data;
            data = await this.findShop(ings[i]);
            if (data == undefined) {
              data = await this.omegaPepega(ings[i]);
            }
            // tworzy obiekt do dodania
            var obj = {
              id: ings[i],
              idb: 0,
              name: data.name,
              buy: data.pricePerOne,
              buyBlue: 0,
              value: 0,
              sell: 0,
              cost: data.pricePerOne * quants[i],
              make: 0,
              eco: 0,
              stock: data.count,
              quant: quants[i],
              dec: "BUY",
              recipe: ""
            };
            // wyszukuje cene niebieskiego
            obj.idb = await this.findIdb(ings[i]);
            if (obj.idb != 0) {
              var bluePrice = await this.omegaPepega(obj.idb);
              obj.buyBlue = bluePrice.pricePerOne;
            }
            // stageFour start
            var obj1 = await this.stageFour(ings[i]);
            // wylicza koszt zrobienia z kupionych skladnikow
            obj.make = Math.round(
              (obj1.reduce((a, { cost }) => a + cost, 0) * obj.quant) /
                this.mastery
            );

            // wylicza koszt zrobienia z optymalnych skladnikow
            obj.eco = Math.round(
              (this.liczEko(obj1) * obj.quant) / this.mastery
            );
            if (obj.eco > obj.cost) {
              obj.eco = obj.cost;
            }
            // wylicza make/eco jesli nie ma recepty
            if (obj.make == 0) {
              obj.make = obj.cost;
              obj.eco = obj.cost;
            }
            // wylicza wartosc zrobionych potraw jakby dac na sell na MP z VP
            obj.value = Math.round(
              obj.buy + (obj.buyBlue * this.masteryS) / this.mastery
            );
            obj.sell = Math.round(obj.value * 0.845) * obj.quant;
            //podejmuje decyzje czy robic czy kupowac
            if (obj.make < obj.sell) {
              obj.dec = "MAKE";
            }
            // dodaje obiekt StageTwo do recept
            obj.recipe = obj1;
            wynik.push(obj);
          }
        }
      }
      return wynik;
    },
    stageFour: async function(x) {
      var wynik = [];
      // recepta
      var f = this.recipes.find(y => {
        return y.id == x;
      });
      if (f != undefined) {
        // tworzy arraya ze znalezionej recepty
        var ings = [f.ing1, f.ing2, f.ing3, f.ing4, f.ing5];
        var quants = [f.quant1, f.quant2, f.quant3, f.quant4, f.quant5];
        // pobiera dane do skladnikow recepty
        for (var i = 0; i < ings.length; i++) {
          // wyszukuje ceny, stock itd
          if (ings[i] != undefined) {
            var data;
            data = await this.findShop(ings[i]);
            if (data == undefined) {
              data = await this.omegaPepega(ings[i]);
            }
            // tworzy obiekt do dodania
            var obj = {
              id: ings[i],
              idb: 0,
              name: data.name,
              buy: data.pricePerOne,
              buyBlue: 0,
              value: 0,
              sell: 0,
              cost: data.pricePerOne * quants[i],
              make: data.pricePerOne * quants[i],
              eco: data.pricePerOne * quants[i],
              stock: data.count,
              dec: "BUY",
              quant: quants[i],
              recipe: ""
            };

            wynik.push(obj);
          }
        }
      }
      return wynik;
    },
    liczEko: function(x) {
      var wynik = 0;
      for (var i = 0; i < x.length; i++) {
        wynik += Math.min(x[i].make, x[i].buy, x[i].eco) * x[i].quant;
      }

      return wynik;
    }
  },
  computed: {},
  mounted() {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.sOne {
  display: grid;
  grid-column: 1 / 2;
  grid-template-columns: repeat(1fr);
  place-self: start start;
}
.sTwo {
  display: grid;
  grid-column: 2 / 3;
  grid-template-columns: repeat(1fr);
  place-self: start start;
}
.sThree {
  display: grid;
  grid-column: 3 / 4;
  grid-template-columns: repeat(1fr);
  place-self: start start;
}
.sFour {
  display: grid;
  grid-column: 4 / 5;
  grid-template-columns: repeat(1fr);
  place-self: start start;
}

.item-a {
  display: grid;
  grid-template-columns: 60px 60px 60px 60px 60px 60px;
  grid-template-rows: auto;
  text-align: center;
  font-family: "Courier New", Courier, monospace;
  font-size: 12px;
  grid-row: 1 / 6;
  grid-column: 1 / col1-end;
  border: solid 1px grey;
  margin-right: -1px;
  margin-top: -1px;
}

.item-b {
  grid-column: 1 / 4;
  text-align: center;
}
.item-c {
  grid-column: 4 / col6-end;
  text-align: center;
}

.item-s4b {
  grid-column: 4 / 5;
  text-align: center;
}

.item-s4b {
  grid-column: 5 / col6-end;
  text-align: center;
}

.line-a {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: auto;
  grid-column: 1 / end;
  align-self: end;
}
.line-b {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: auto;
  grid-column: 1 / end;
  align-self: center;
}

.line-c {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: auto;
  grid-column: 1 / end;
  align-self: start;
}

.select-box {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto;
}

.box-prices {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: auto;
}
</style>
