<template>
  <v-app>
    <v-main>
      <Nav/>
    <div class="black">
      <HelloWorld/>
    </div>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';
import Nav from './components/nav';

export default {
  name: 'App',

  components: {
    HelloWorld,
    Nav
  },

  data: () => ({
    //
  }),
};
</script>
