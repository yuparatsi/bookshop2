<template>
    <v-row  justify="center">
    <v-dialog v-model="shower" scrollable persistent max-width="800px" >
          <v-card  >
               
                  <v-card-actions>
          <v-spacer></v-spacer>
         
          <v-btn fab text class="pa-0" small  @click="close()" >x</v-btn>
        </v-card-actions>
                <!-- <v-card-text > -->
                    <br/>
                  
                      
                   <v-img class="mx-auto" :src="data_item.file_img" max-width="300px"></v-img>
                     
              <v-card-title>{{data_item.file_name}}</v-card-title>
              <v-card-text>
               <v-rating
          :value="data_item.file_rate.rate"
          color="amber"
          dense
          half-increments
          readonly
          size="14"
        ></v-rating>

        <div class="grey--text ms-4">
         {{data_item.file_rate.rate}} ({{data_item.file_rate.amount}})
        </div>
               <div class="my-4 text-subtitle-1">
        ราคา 100 บาท <v-btn dark tile @click="addToCart()" >add to cart</v-btn>
      </div>
               <div>{{data_item.file_detail}}</div>  
              </v-card-text>  
                <!-- </v-card-text> -->
                
          </v-card>
    </v-dialog>
    </v-row>
</template>
<script>
export default {   
     props: ["show","data_item"],
data: function() {
    return {
    
    };
  },
        
     computed: {   
    shower: {
      get() {
        if (this.show === true) {
          console.log(this.data_item);
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
     methods:{
         close(){
            this.$emit("close");
         },
         addToCart(){
             this.$emit("select_book",this.data_item);
             this.$emit("close");
         }
     }
     
}
</script>
<style>
.magic { 
 cursor: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAABkCAMAAABHPGVmAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDcuMS1jMDAwIDc5LmVkYTJiM2ZhYywgMjAyMS8xMS8xNy0xNzoyMzoxOSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDIzLjEgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkY3ODNGQTM0RDY5NjExRUNBMTA1QkEwNEQ4NkM2NkM0IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkY3ODNGQTM1RDY5NjExRUNBMTA1QkEwNEQ4NkM2NkM0Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Rjc4M0ZBMzJENjk2MTFFQ0ExMDVCQTA0RDg2QzY2QzQiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Rjc4M0ZBMzNENjk2MTFFQ0ExMDVCQTA0RDg2QzY2QzQiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz54tZeaAAAAHlBMVEX9/PfyogwVFTb6xRD82mDy4byamqf1ukpXVGb///8pWeYkAAAACnRSTlP///////////8AsswszwAAAh9JREFUeNrs2duWqyAMBuAfSaq+/wtvTmpFEZDQi9kwhzWz6urXJIAIWH/QMJCBDGQgfxdh7o9gVvgBMg/kP0DAyCEAWhF1fc+WPp2IRD2nxxjciuSUSiNV+Eel1kj2rnvFlabaSHdh8PWd4Kp994rYVG+CezlNPnei754LUn0QpfbUmEp0QuyHn8nEY1PVCVlZa/vWM5Pqh8Ajvtm/uyCk9eboToV3+dJkGfu7UyQ2X5rIQeanF0JB2dIGAQS+RflyzUWijmnrcmkpgtCROEYYvMUS5k176VwYGuLbLJmvY/61NbGI+cgUah+CcZeab0iME58tBtHW0ZQiiE4rWoWK+KJsY2aGHMJKHwbtAlctVTII9N52xCQOkuPkyzgQyA5GkD4pYTyyLHK00LF83Vl8WjkljvBSKcuxG+6a15dK4areJQs+JrV8BGsSJcyNQHtDnqaPEILztAQdsgRjGEVmmRqvRUmHfz9TtYLkYihai2KfFblaQSqOy0x71KhaQVkc51absdLHOW5RkF1o3z4iemWB6GCMn35Rpbx9jq9SXm8W1CjvdyQqlIZtj3KlZVMNS6FS2gnvu3qh0rbRGZQJPZFCpXXLtkhp3hcuUdo3nwsUgR3uvCKxjQ6PTNwTycYicyCQiUXo1OFZkTraeFTEzk+eFLlDGp6S1Rc8CeJfIF5ZOiMrL/f3FtmDs8Sz9zhnHMhABjKQgQzku/0TYACISEe65TduDwAAAABJRU5ErkJggg=='), auto;
}
</style>