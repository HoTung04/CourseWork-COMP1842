<template>
  <div id="app">
    <div class="ui inverted segment navbar">
      <div class="ui center aligned container">
        <div class="ui large secondary inverted pointing menu compact" >
          <router-link to="/words" exact class="item">
            <i class="comment outline icon"></i> Words
          </router-link>
          <router-link to="/words/new" class="item">
            <i class="plus circle icon"></i>New
          </router-link>
          <router-link to="/test" class="item">
            <i class="graduation cap icon"></i> Test
          </router-link>
          <router-link to="/languages" class="item">
            <i class="globe icon"></i> Languages
          </router-link>
        </div>
      </div>
    </div>

    <div class="ui text container">
      <flash-message class="myFlash"></flash-message>
      <div class="ui one column grid">
        <div class="column">
          <router-view />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app'
};
</script>

<style>
div.ui.secondary.inverted.menu .active.item{
  color: #FF9800 !important;  
}
div.ui.secondary.inverted.pointing.menu .active.item {
    border-color: #FF9800;
}
div.ui.secondary.pointing.menu a.item:hover {
  background-color: #FF9800;
}
div.ui.secondary.inverted.menu {
    background-color: #4CAF50;
}
#app > div.navbar {
  margin-bottom: 1.5em;
  background-color: #2196F3;
}
.myFlash {
  width: 250px;
  margin: 10px;
  position: absolute;
  top: 50;
  right: 0;
}
input {
  width: 300px;
}
div.label {
  width: 120px;
}
div.input {
  margin-bottom: 10px;
}
button.ui.button {
  margin-top: 15px;
  display: block;
}
</style>