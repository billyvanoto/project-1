<template>
  <div class="container is-fluid">
    <div class="columns is-mobile is-centered">
      <div class="column">
        <b-field position="is-centered" :type="success ? '' : 'is-danger'" >
          <b-input placeholder="Rp 75.000" v-model="money" v-on:keyup.enter.native="validate"></b-input>
          <p class="control">
            <a class="button is-primary" @click="validate">Enter</a>
          </p>
        </b-field>
        <p class="help is-danger" v-show="success === false">{{error}}</p>
        <p>Denominations : {{combination}}</p>
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
      success:true,
      fractions:[100000,50000,20000,10000,5000,2000,1000,500,200,100,50],
      error:'',
      combination:'',
    }
  },
  methods:{
    validate:function(){
      this.reinitialize();

      const only_currency = /^Rp$/gm
      if(only_currency.test(this.money)){
        this.error = "Missing value";
        this.success = false;
        return;
      }

      const start_string = /(?=^Rp[ \d])|\d/gm
      if(!start_string.test(this.money)){
        this.error = "Invalid input";
        this.success = false;
        return;
      }

      const end_string = /(?<!\Rp)$/gm
      if(!end_string.test(this.money)){
        this.error = "Valid character in wrong position";
        this.success = false;
        return;
      }

      const separator = /\d+(\,\d{3})|\d+(\ \d{3})/gm
      if(separator.test(this.money)){
        this.error = "Invalid separator";
        this.success = false;
        return;
      }

      this.count_denominal();
    },
    count_denominal: function(){
      let money_value = this.money.replace(/^Rp|\.|(\,).*/gm,'');

      let after = parseInt(money_value);
      for(var i =0; i<this.fractions.length; i++){
        let temp = after;
        if(after >= this.fractions[i]){
          after = after%this.fractions[i];
          const count_fraction = (temp - after)/this.fractions[i];
          this.combination += ` ${count_fraction}x Rp${this.add_decimal_separator(this.fractions[i])}`;
          if(after !== 0){
            this.combination +=`,`;
          }
        }
      }

      if(after > 0){
        this.combination += ` left Rp${after} (no available fraction)`;
      }
    },
    add_decimal_separator:function(num){
      return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1.');
    },
    reinitialize:function(){
      this.error = [];
      this.combination = '';
      this.success = true;
    }
  }
};
</script>
