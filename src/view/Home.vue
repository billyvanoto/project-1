<template>
  <div class="container is-fluid">
    <div class="columns is-mobile is-centered">
      <div class="column">
        <b-field position="is-centered">
          <b-input placeholder="Rp 75.000" v-model="money"></b-input>
          <p class="control">
            <a class="button is-primary" @click="test">Enter</a>
          </p>
        </b-field>
        <p>{{money}}</p>
        <p>{{res}}</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name : 'home',
  data(){
    return {
      money:'',
      res:''
    }
  },
  methods:{
    test:function(){
      const regex = /^Rp\s\d$|Rp\d|\d+[^\Rp]$/gm
      this.res = regex.test(this.money);
      return this.res;
    }
  }
};
</script>
