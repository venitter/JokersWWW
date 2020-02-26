<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
export default {};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Prompt&display=swap");
#app {
  font-family: "Prompt", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
}

body {
  margin: 0px;
  max-width: 100vw;
  max-height: 100%;
  -moz-user-select: none; /* nie pozwól na zaznaczenia */
  user-select: none; /* nie pozwól na zaznaczenia */
  background-color: #262626;
  color: white;
}

input,
select,
option,
textarea {
  background-color: black;
  border: white 1px dashed;
  color: white;
  padding: 5px;
  font-size: 20px;
  text-align: center;
  margin-bottom: 10px;
}

input:focus,
select:focus,
option:focus,
textarea:focus {
  outline: none;
  border: lightgreen 1px dashed;
}

.button {
  display: inline-block;
  background-color: green;
  padding: 5px;
  cursor: pointer;
  border-radius: 5px;
  border: solid 2px black;
  letter-spacing: 1px;
  color: white;
}

.button:hover {
  filter: brightness(1.3);
}

.button:active {
  filter: brightness(1.3);
  border: solid 2px white;
}
.header-title {
  font-size: 24px;
  margin-bottom: 10px;
}

input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus,
input:-webkit-autofill:active {
  -webkit-transition: "color 9999s ease-out, background-color 9999s ease-out";
  -webkit-transition-delay: 9999s;
}
</style>
