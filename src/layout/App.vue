<template name="default-layout">
  <div class="bg-dark app">
    <section class="row overlay">
      <div class="container-fluid">
        <div class="header text-center">
          <h2 class="text-uppercase"> My Books Store </h2>
          <h4> Record of all the books i've read</h4>
        </div>

        <div >
          <Home />
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Home from '../pages/Home.vue';
export default {
  components: {
    Home
  }
}
</script>

<style scoped>
   @import url('https://fonts.googleapis.com/css2?family=Nunito+Sans:ital,wght@0,300;1,400&display=swap');


    .app{
      font-family: 'Nunito Sans', sans-serif;
      
      background: #121212;
      
      overflow-x: hidden;
      margin: 0 auto;
      padding: 0 auto;
    }

    .overlay{
      margin: 10px;
      background: rgb(20, 20, 20);
      
      padding: 1em;
      border-radius: 10px;
      color: rgba(248, 248, 248, 0.8);
      text-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    }

</style>