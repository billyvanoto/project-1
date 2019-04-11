<template>
  <div class="container is-fluid">
    <div class="columns is-mobile is-centered">
      <div class="column">
        <b-field position="is-centered" :type="res ? '' : 'is-danger'" >
          <b-input placeholder="Rp 75.000" v-model="money" v-on:keyup.enter.native="validate"></b-input>
          <p class="control">
            <a class="button is-primary" @click="validate">Enter</a>
          </p>
        </b-field>
        <p class="help is-danger" v-show="res === false">{{error}}</p>
        <p>{{money}}</p>
        <p>{{res}}</p>
        <p>{{processed_string}}</p>
        <p>{{combination}}</p>
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
      res:true,
      processed_string:'',
      fractions:[100000,50000,20000,10000,5000,2000,1000,500,200,100,50],
      error:'',
      combination:'',
    }
  },
  methods:{
    validate:function(){
      this.error = [];
      this.res = true;

      const only_currency = /^Rp$/gm
      if(only_currency.test(this.money)){
        this.error = "Missing value";
        this.res = false;
        return;
      }

      const start_string = /(?=^Rp[ \d])|\d/gm
      if(!start_string.test(this.money)){
        this.error = "Invalid input";
        this.res = false;
        return;
      }

      const end_string = /(?<!\Rp)$/gm
      if(!end_string.test(this.money)){
        this.error = "Valid character in wrong position";
        this.res = false;
        return;
      }

      this.processed_string = this.money.replace(/^Rp/gm, '');
      const separator = /\d+(\,\d{3})|\d+(\ \d{3})/gm
      if(separator.test(this.processed_string)){
        this.error = "Invalid separator";
        this.res = false;
        return;
      }

      this.count_denominal();
    },
    count_denominal: function(){
      this.combination = '';
      this.processed_string = this.processed_string.replace(/\.|(\,).*/gm,'');

      let after = parseInt(this.processed_string);
      for(var i =0; i<this.fractions.length; i++){
        let temp = after;
        if(after >= this.fractions[i]){
          after = after%this.fractions[i];
          const count_fraction = (temp - after)/this.fractions[i];
          this.combination += ` ${count_fraction}xRp ${this.fractions[i].toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1.')}`;
          if(after !== 0){
            this.combination +=`,`;
          }
        }
      }
    }
  }
};
</script>
