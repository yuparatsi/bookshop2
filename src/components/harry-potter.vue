<template>
  <!-- <v-card
    class="mx-auto overflow-hidden"
    height="400"
  > -->
  
  <v-div >

    <v-app-bar color="white" class="magic">
      <v-app-bar-nav-icon @click="drawer = true"></v-app-bar-nav-icon>

      <v-toolbar-title class="mx-auto" justify-center>บ้านนายดิน</v-toolbar-title>
      <v-badge v-if="data_select.length>0" color="red" overlap bordered :content="data_select.length">
        <v-icon> shopping_cart </v-icon>
      </v-badge>
      <v-icon v-else> shopping_cart </v-icon>
      
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" absolute temporary>
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
    </v-navigation-drawer>
    <!-- </v-card> -->
    <v-content>
      <v-card class="elevation-0">
        <!-- header -->
        <v-layout class="pl-5">Harry Potter</v-layout>
        <br />
        <!-- body -->
        <v-card class="elevation-0 pa-3" style="cursor: default">
          <v-flex>
            <v-row class="magic">
              <v-col v-for="(item, index) in ItemBook" :key="index" cols="6" md="3">
                <v-card class="pa-0 d-flex align-center" :title="item.file_name" @dblclick="gotoDetail(item)">
                  <v-layout justify-center column align-center>
                    <v-col class="pa-0">
                      <div class="text-center pa-0 pb-5">
                        <v-img :src="item.file_img" max-height="500px" height="500px">
                          <v-app-bar dense class="pa-0" flat color="rgba(0, 0, 0, 0)" height="45px"
                            ><v-app-bar-nav-icon dense class="pa-0" color="white">
                              <!-- <v-icon class="pa-0" color="amber">star</v-icon> -->
                            </v-app-bar-nav-icon>

                            <v-spacer></v-spacer>

                            <v-btn color="white" icon>
                              <v-icon color="#D9D9D9">mdi-dots-vertical</v-icon>
                            </v-btn>
                          </v-app-bar>
                        </v-img>
                        <v-col class="pt-3">
                          <v-layout>
                            <v-flex lg9>
                              <div
                                style="
                                  whitespace: nowrap;
                                  overflow: hidden;
                                  textoverflow: ellipsis;
                                  color: #000;
                                  fontsize: 20px;
                                  padding-left: 8px;
                                "
                                class="text-center"
                              >
                                <span style="font-weight: 450; font-size: 20px">
                                  {{ item.file_name }}
                                </span>
                              </div>
                            </v-flex>
                            <v-flex lg3>
                              <!-- <v-avatar
                                                  title="แชร์ลิงก์"
                                                 
                                                  size="20"
                                                  color="#F37958"
                                                  >
                                                  <v-icon dark small>link</v-icon></v-avatar
                                                > -->
                            </v-flex>
                          </v-layout>
                        </v-col>
                      </div>
                    </v-col>
                  </v-layout>
                </v-card>
              </v-col>
            </v-row>
          </v-flex>
        </v-card>
      </v-card>
      <v-snackbar v-model="snackbar" :vertical="vertical" timeout="-1" right>
        <v-flex class="text-center">
          {{ text }}
          <v-badge color="red" overlap bordered :content="data_select.length">
            <v-icon> shopping_cart </v-icon>
          </v-badge>
        </v-flex>

        <template v-slot:action="{ attrs }">
          <v-btn color="red" block v-bind="attrs" @click="gotoCart()"> ดูตระกร้าสินค้าของฉัน </v-btn>
        </template>
      </v-snackbar>
    </v-content>
    <detail
      :show="opendialog_detail"
      :data_item="item_book"
      @close="opendialog_detail = false"
      @select_book="selectBook"
    ></detail>
    <cart :show="opendialog_cart" :data_select="data_select_temp"></cart>
  </v-div>
</template>

<script>
import detail from "../components/optional/detail";
import cart from "../components/optional/cart";


// const detail = () => import("@/components/optional/detail");
export default {
  components: {
    detail,
    cart,
   
   
  },
  data: () => ({
    snackbar: false,
    text: "เพิ่มสินค้าลงตะกร้าแล้ว",
    vertical: true,
    drawer: false,
    group: null,
    ItemBook: [
      {
        file_id: 1,
        file_name: "แฮร์รี่ พอตเตอร์ กับศิลาอาถรรพ์",
        file_img: require("../assets/book1.jpeg"),
        file_detail:
          "แฮร์รี่ พอตเตอร์ไม่เคยได้ยินชื่อ 'ฮอกวอตส์' มาก่อนด้วยซ้ำ ในวันที่จดหมายฉบับแรกเริ่มร่อนลงที่หน้าประตูบ้านเลขที่สี่ ซอยพรีเว็ต แฮร์รี่ได้เห็นเพียงกระดาษแข็งสีออกเหลือง หมึกสีเขียวมรกต พร้อมตราประทับ",
        file_rate: {
          rate: 5,
          amount: "1234",
        },
        file_price: 100,
      },
      {
        file_id: 2,
        file_name: "แฮร์รี่ พอตเตอร์ กับห้องแห่งความลับ",
        file_img: require("../assets/book2.jpeg"),
        file_detail:
          "ปิดเทอมหน้าร้อนปีนี้ แฮร์รี่ พอตเตอร์ได้พบเจอวันเกิดที่แย่ที่สุดในชีวิต มาพร้อมคำเตือนที่เป็นลางร้ายจากต๊อบบี้ เอลฟ์ประจำวัน ก่อนที่รอน วีสลีย์เพื่อนรักจะขับรถเหาะมาพาเขาหนีจาก บ้านเดอร์สลีย์ได้",
        file_rate: {
          rate: 4.5,
          amount: "332",
        },
        file_price: 100,
      },
      {
        file_id: 3,
        file_name: "แฮร์รี่ พอตเตอร์ กับนักโทษแห่งอัซคาบัน",
        file_img: require("../assets/book3.jpeg"),
        file_detail:
          "ในเล่ม 3 นี้ ความลึกลับหรือข้อสงสัยต่างๆ จะเริ่มมาคลี่คลาย อัสคาบัน คุกแห่งพ่อมดที่ขึ้นชื่อว่าแข็งแกร่งที่สุดกลับมีนักโทษพ่อมดคนหนึ่งหลบหนีออกมาได้ นักโทษคนนี้เป็นใคร! ''แฮร์รี่'' กับเพื่อนเดินทางสู่ป่าแห่งเวทมนต์ค้นพบศาสตร์มืดอันเร้นลับ ในที่สุดแฮร์รี่ก็พบกับนักโทษแห่งอัศคาบัน ปริศนาดำมืดของความชั่วร้ายกำลังจะเปิดเผยออก",
        file_rate: {
          rate: 4,
          amount: "412",
        },
        file_price: 100,
      },
      {
        file_id: 4,
        file_name: "แฮร์รี่ พอตเตอร์ กับถ้วยอัคนี",
        file_img: require("../assets/book4.jpeg"),
        file_detail:
          "เขาต้องเผชิญภารกิจอันตรายหลายอย่าง รวมถึงมังกรและพ่อมดศาสตร์มืด แต่ด้วยความช่วยเหลือจากรอนและเฮอร์ไมโอนี่เพื่อนรัก แฮร์รี่อาจผ่านทุกอย่างได้",
        file_rate: {
          rate: 4.5,
          amount: "340",
        },
        file_price: 100,
      },
      {
        file_id: 5,
        file_name: "แฮร์รี่ พอตเตอร์ กับภาคีนกฟีนิกซ์",
        file_img: require("../assets/book5.jpeg"),
        file_detail:
          "อำนาจมืดคืบคลานมาถึงฮอกวอตส์แล้ว หลังผู้คุ้มวิญญาณทำร้ายดัลลีย์ ลูกพี่ลูกน้อง 'แฮรรี่ พอตเตอร์' รู้ว่าโวลเดอร์มอร์จะไม่หยุดตามล่าเขาแน่ หลายคนไม่เชื่อแฮรรี่ว่า จอมมารกลับมาแล้ว!",
        file_rate: {
          rate: 5,
          amount: "34",
        },
        file_price: 100,
      },
      {
        file_id: 6,
        file_name: "แฮร์รี่ พอตเตอร์ กับเจ้าชายเลือดผสม",
        file_img: require("../assets/book6.jpeg"),
        file_detail:
          "กลางดึกช่วงปิดเทอมฤดูร้อน ดัมเบิลดอร์มารับแฮรี่ พอตเตอร์ที่ซอยพรีเว็ต มือข้างหนึ่งของอาจารย์ดำเกรียมและแห้งเหี่ยว แต่เขาไม่ยอมบอกเหตุผล ความลับและความหวาดระแวงแพร่กระจายไปทั่วโลกเวทมนตร์",
        file_rate: {
          rate: 4,
          amount: "234",
        },
        file_price: 100,
      },
      {
        file_id: 7,
        file_name: "แฮร์รี่ พอตเตอร์ กับเครื่องรางยมทูต",
        file_img: require("../assets/book7.jpeg"),
        file_detail:
          "ขณะทะยานขึ้นฟ้าในมอเตอร์ไซค์พ่วงข้างของแฮกริด ทิ้งซอยพรีเว็ตไว้เบื้องหลังเป็นครั้งสุดท้าย แฮรี่ พอตเตอร์รู้ดีว่า ลอร์ดโวลเดอมอร์กับผู้เสพความตายอยู่ไม่ไกลแล้ว!",
        file_rate: {
          rate: 4.5,
          amount: "123",
        },
        file_price: 100,
      },
    ],
    opendialog_detail: false,
    opendialog_cart: false,
    item_book: {},
    data_select: [],
    data_select_temp: [],
  }),

  methods: {
    gotoDetail(item) {
      this.item_book = item;
      this.opendialog_detail = true;
    },
    selectBook(item) {
      this.snackbar = true;
      this.data_select.push(item);
     // console.log(this.data_select);
    },
    gotoCart() {
      this.opendialog_cart = true;
      this.data_select_temp = this.data_select;
      this.snackbar = false;
    },
  },
};
</script>
<style>
#borderradius {
  border-radius: 15px;
}
#borderradius-img {
  border-radius: 15px 15px 0px 0px;
}
#borderradius-word {
  border-radius: 0px 0px 15px 15px;
}
#borderradius_btn {
  border-radius: 5px;
}
.magic { 
 cursor: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAABkCAMAAABHPGVmAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDcuMS1jMDAwIDc5LmVkYTJiM2ZhYywgMjAyMS8xMS8xNy0xNzoyMzoxOSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDIzLjEgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkY3ODNGQTM0RDY5NjExRUNBMTA1QkEwNEQ4NkM2NkM0IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkY3ODNGQTM1RDY5NjExRUNBMTA1QkEwNEQ4NkM2NkM0Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Rjc4M0ZBMzJENjk2MTFFQ0ExMDVCQTA0RDg2QzY2QzQiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Rjc4M0ZBMzNENjk2MTFFQ0ExMDVCQTA0RDg2QzY2QzQiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz54tZeaAAAAHlBMVEX9/PfyogwVFTb6xRD82mDy4byamqf1ukpXVGb///8pWeYkAAAACnRSTlP///////////8AsswszwAAAh9JREFUeNrs2duWqyAMBuAfSaq+/wtvTmpFEZDQi9kwhzWz6urXJIAIWH/QMJCBDGQgfxdh7o9gVvgBMg/kP0DAyCEAWhF1fc+WPp2IRD2nxxjciuSUSiNV+Eel1kj2rnvFlabaSHdh8PWd4Kp994rYVG+CezlNPnei754LUn0QpfbUmEp0QuyHn8nEY1PVCVlZa/vWM5Pqh8Ajvtm/uyCk9eboToV3+dJkGfu7UyQ2X5rIQeanF0JB2dIGAQS+RflyzUWijmnrcmkpgtCROEYYvMUS5k176VwYGuLbLJmvY/61NbGI+cgUah+CcZeab0iME58tBtHW0ZQiiE4rWoWK+KJsY2aGHMJKHwbtAlctVTII9N52xCQOkuPkyzgQyA5GkD4pYTyyLHK00LF83Vl8WjkljvBSKcuxG+6a15dK4areJQs+JrV8BGsSJcyNQHtDnqaPEILztAQdsgRjGEVmmRqvRUmHfz9TtYLkYihai2KfFblaQSqOy0x71KhaQVkc51absdLHOW5RkF1o3z4iemWB6GCMn35Rpbx9jq9SXm8W1CjvdyQqlIZtj3KlZVMNS6FS2gnvu3qh0rbRGZQJPZFCpXXLtkhp3hcuUdo3nwsUgR3uvCKxjQ6PTNwTycYicyCQiUXo1OFZkTraeFTEzk+eFLlDGp6S1Rc8CeJfIF5ZOiMrL/f3FtmDs8Sz9zhnHMhABjKQgQzku/0TYACISEe65TduDwAAAABJRU5ErkJggg=='), auto;
}
</style>
