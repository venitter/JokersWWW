<template>
  <div class="note">
    <div class="item-a">
      <div class="button" @click="zapisz">Zapisz [ Ilość znaków: {{ dlugosc }} ]</div>
    </div>
    <div>
      <textarea v-model="noteIn" rows="30"></textarea>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  name: "Admin-notes",
  data() {
    return {
      noteIn: ""
    };
  },
  computed: {
    noteOut() {
      return this.noteIn.replace(/\n/g, "<br>");
    },
    dlugosc() {
      return this.noteOut.length;
    }
  },
  methods: {
    zapisz: function() {
      var data = {
        note: this.noteOut
      };
      // axios request tu bydzie
      axios.post("http://35.156.131.239:3000/saveNote", data);
    }
  },
  mounted() {
    axios.get("http://35.156.131.239:3000/loadPartyEdit").then(res => {
      this.noteIn = res.data[0].note.replace(/<br\s*\/?>/gm, "\n");
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.note {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto;
  grid-template-areas: "header header";
  padding: 10px;
}

.item-a {
  grid-area: header;
  padding: 20px;
}

textarea {
  width: 80vw;
  text-align: left;
}
</style>
