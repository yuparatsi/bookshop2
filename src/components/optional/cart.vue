<template>
  <v-row class="magic" justify="center">
    <v-dialog v-model="shower" fullscreen hide-overlay transition="dialog-bottom-transition" >
     <v-card elevation="0">
       <v-app-bar elevation="0" color="white">
      <v-app-bar-nav-icon @click="drawer = true"></v-app-bar-nav-icon>

      <v-toolbar-title class="mx-auto" justify-center>บ้านนายดิน</v-toolbar-title>
    
      <v-icon > shopping_cart </v-icon>
      
    </v-app-bar>
        <!-- <v-navigation-drawer v-model="drawer" absolute temporary>
          <v-list nav dense>
            <v-list-item-group v-model="group" active-class="deep-purple--text text--accent-4">
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-home</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Home</v-list-item-title>
              </v-list-item>

              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-account</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Account</v-list-item-title>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-navigation-drawer> -->
     <v-card elevation="0" class="mx-auto" justify-center>
         <br/>
       
        <v-card-text class="mx-auto pl-16 pr-16 " >
          <v-container elevation-0 class="pl-16 pr-16 ">
            <v-row  class="pl-16 pr-16 " align="center" v-for="(value, key, index) in group_data" :key="index">
                <v-col></v-col>
              <v-col ><v-img class="ml-16" :src="value[0]['file_img']" max-width="200px"></v-img></v-col>
              <v-col> {{ key }}</v-col>
              <v-col
                ><v-text-field
                  
                  type="number"
                  append-outer-icon="add"
                  @click:append-outer="increment(value[0])"
                  prepend-icon="remove"
                  @click:prepend="decrement(value[0])"
                  v-model="value.length"
                ></v-text-field>
              </v-col>
              <v-col></v-col>
            </v-row>
          </v-container>
          <v-divider></v-divider>
          <v-container align="center"  elevation-0 class="mx-auto pl-16 pr-16" justify="center">
            <!-- <v-row class="mx-auto" justify="center"> -->
                <v-card   class="mx-auto ml-16 mr-16"  outlined   >
                    <v-card-title    class="text-center">
                        Payment Summary
                    </v-card-title>
                
                <v-list>
                <v-list-item>
                    <v-list-item-content>
                    <span>รวม </span> 
                    </v-list-item-content>
                    <v-list-item-action>
                        {{ number_book }} เล่ม
                    </v-list-item-action>
                    </v-list-item>
                <v-list-item>
                    <v-list-item-content>
                    <span>ราคารวม </span> 
                    </v-list-item-content>
                    <v-list-item-action>
                        {{ total_price }} บาท
                    </v-list-item-action>
                </v-list-item>
                <v-list-item>
                    <v-list-item-content>
                    <span>ส่วนลด </span> 
                    </v-list-item-content>
                    <v-list-item-action>
                        {{ total_discount }} บาท
                    </v-list-item-action>
                </v-list-item>
                <v-list-item>
                    <v-list-item-content>
                    <span>รวมสุทธิ </span> 
                    </v-list-item-content>
                    <v-list-item-action>
                       {{ net_price }} บาท
                    </v-list-item-action>
                </v-list-item>
              </v-list>
              </v-card>
            <!-- </v-row> -->
          </v-container>
          <br/>
        </v-card-text>
        </v-card>
     
     </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
export default {
  props: ["show", "data_select"],
  data: function () {
    return {
      drawer: false,
      group: null,
      data_in_cart: [],
      group_data: {},
      number_book: 0,
      total_price: 0,
      total_discount: 0,
      net_price: 0,
      data_discount_result: [],
      data_discount_number: [],
    };
  },
  computed: {
    shower: {
      get() {
        if (this.show === true) {
          console.log("cart");
        }
        return this.show;
      },
      set(value) {
        if (!value) {
          //   this.$emit("close");
        }
      },
    },
  },
  watch: {
    show: function () {
      this.calculateBook();
    },
  },
  methods: {
    increment(value) {
      for (let i in this.group_data) {
        if (value.file_name === i) {
          this.group_data[value.file_name].push(value);
          //this.data_discount = console.log(this.group_data);
        }
      }
      this.calPriceBook();
    },
    decrement(value) {
      for (let i in this.group_data) {
        if (value.file_name === i) {
          this.group_data[value.file_name].pop(value);
          //console.log(this.group_data[value.file_name].length);
          if (this.group_data[value.file_name].length === 0) {
            delete this.group_data[value.file_name];
          }
        }
      }
      this.calPriceBook();
    },

    calculateBook() {
      this.group_data = this.data_select.reduce((r, a) => {
        r[a.file_name] = [...(r[a.file_name] || []), a];
        return r;
      }, {});
      //console.log("group", this.group_data);
      this.calPriceBook();
    },
    calPriceBook() {
      let count_different = 0;
      this.number_book = 0;
      this.total_price = 0;
      var min=0;
      let discount_tmp=0;
      this.net_price = 0;
      this.data_discount_result = [];
      this.data_discount_number = [];
      for (let i in this.group_data) { 
        this.number_book += this.group_data[i].length;

        this.data_discount_number[count_different] = this.group_data[i].length;
        count_different += 1;
        //console.log(this.group_data[i]);
      }
      //console.log("gh", this.data_discount_number,this.data_discount_number.length);
    let tmp_data = this.data_discount_number.length
      for (let i = 0; i < tmp_data; i++) {
          this.data_discount_number = this.data_discount_number.filter(function(val) {
       return val !== 0;
    });
      if(this.data_discount_number[0]){
         min = Math.min(...this.data_discount_number); 
     
        
      }
      else{
          min = 0
      }
      this.data_discount_number = this.data_discount_number.map(x => x - min);
        //console.log("mm",i,this.data_discount_number,"min",min);
        discount_tmp = min*(tmp_data-i)*100
        //console.log("m-m",tmp_data-i);
        if(tmp_data-i === 7)
        {
            discount_tmp=discount_tmp*60/100
        }else if(tmp_data-i === 6){
            discount_tmp=discount_tmp*50/100
        }else if(tmp_data-i === 5){
            discount_tmp=discount_tmp*40/100
        }else if(tmp_data-i === 4){
            discount_tmp=discount_tmp*30/100
        }else if(tmp_data-i === 3){
           
            discount_tmp=discount_tmp*20/100
        }else if(tmp_data-i === 2){
            discount_tmp=discount_tmp*10/100
        }else if(tmp_data-i === 1){
            discount_tmp=discount_tmp*0
        }
        this.data_discount_result[i] =  discount_tmp
        //console.log(discount_tmp);
        
      }
      

      //console.log("nvb",this.data_discount_result);
      //console.log(this.data_discount_number);
      this.total_price = this.number_book * 100;
      this.total_discount = this.data_discount_result.reduce((partialSum, a) => partialSum + a, 0);
      this.net_price = this.total_price - this.total_discount;
    },
  },
};
</script>
<style>
.magic { 
 cursor: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAABkCAMAAABHPGVmAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDcuMS1jMDAwIDc5LmVkYTJiM2ZhYywgMjAyMS8xMS8xNy0xNzoyMzoxOSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDIzLjEgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkY3ODNGQTM0RDY5NjExRUNBMTA1QkEwNEQ4NkM2NkM0IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkY3ODNGQTM1RDY5NjExRUNBMTA1QkEwNEQ4NkM2NkM0Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Rjc4M0ZBMzJENjk2MTFFQ0ExMDVCQTA0RDg2QzY2QzQiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Rjc4M0ZBMzNENjk2MTFFQ0ExMDVCQTA0RDg2QzY2QzQiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz54tZeaAAAAHlBMVEX9/PfyogwVFTb6xRD82mDy4byamqf1ukpXVGb///8pWeYkAAAACnRSTlP///////////8AsswszwAAAh9JREFUeNrs2duWqyAMBuAfSaq+/wtvTmpFEZDQi9kwhzWz6urXJIAIWH/QMJCBDGQgfxdh7o9gVvgBMg/kP0DAyCEAWhF1fc+WPp2IRD2nxxjciuSUSiNV+Eel1kj2rnvFlabaSHdh8PWd4Kp994rYVG+CezlNPnei754LUn0QpfbUmEp0QuyHn8nEY1PVCVlZa/vWM5Pqh8Ajvtm/uyCk9eboToV3+dJkGfu7UyQ2X5rIQeanF0JB2dIGAQS+RflyzUWijmnrcmkpgtCROEYYvMUS5k176VwYGuLbLJmvY/61NbGI+cgUah+CcZeab0iME58tBtHW0ZQiiE4rWoWK+KJsY2aGHMJKHwbtAlctVTII9N52xCQOkuPkyzgQyA5GkD4pYTyyLHK00LF83Vl8WjkljvBSKcuxG+6a15dK4areJQs+JrV8BGsSJcyNQHtDnqaPEILztAQdsgRjGEVmmRqvRUmHfz9TtYLkYihai2KfFblaQSqOy0x71KhaQVkc51absdLHOW5RkF1o3z4iemWB6GCMn35Rpbx9jq9SXm8W1CjvdyQqlIZtj3KlZVMNS6FS2gnvu3qh0rbRGZQJPZFCpXXLtkhp3hcuUdo3nwsUgR3uvCKxjQ6PTNwTycYicyCQiUXo1OFZkTraeFTEzk+eFLlDGp6S1Rc8CeJfIF5ZOiMrL/f3FtmDs8Sz9zhnHMhABjKQgQzku/0TYACISEe65TduDwAAAABJRU5ErkJggg=='), auto;
}
</style>
