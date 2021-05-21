<template>
  <section class="score">
    Player <span>1</span> x <span>2</span> Computer
  </section>
  <h1>The communication protocol NFC stands for Near-Field Control.</h1>
  <input type="radio" name="options" value="True" />
  <label>True</label>
  <br />
  <input type="radio" name="options" value="False" />
  <label>False</label>
  <br />
  <button class="send" type="button">Send</button>
</template>

<script>
export default {
  name: "App",
  components: {},
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type="radio"] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
