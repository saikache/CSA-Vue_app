<template>
  <div class="ml-8 mr-8 py-6">
        <template>
  <v-toolbar class="loginmain1">
    <!-- <v-toolbar-side-icon></v-toolbar-side-icon> -->
    <v-spacer></v-spacer>
    <v-toolbar-items class="hidden-sm-and-down">
      <v-card style="padding:0.5rem;" class="mt-1 mb-1 loginmain3">
        <span class="mt-5 "  style="padding:0px">
          CHANNELS SELECTED    
        </span>
        <span class="float-right"> VIEW CART </span> <br>
        <span style="margin-right: 40px;">
          INDIGITAL PACKS : {{ userSelection.inDigital.length }} 
        </span>
        <span  style="padding:0px">
          ALACARTE : {{ userSelection.alaCard.length }} 
        </span>
        <span class="ml-12">
          BROADCASTER PACKS : {{ userSelection.broadCast.length }}
        </span>
        <span class="ml-12">
          TOTAL PRICE : 00
        </span>
      </v-card>
     <!-- <v-icon color="white lighten-1" href="/optimization">mdi-cart</v-icon> -->
     <!-- <v-btn rounded class="ml-4 main-button" color="primary white--text text--lighten-1" >Optimize </v-btn>  
     -->
     <!-- <v-icon color="black lighten-1" class="ml-4" style="font-size:2rem;" @click="logout">mdi-logout</v-icon> -->
    <!-- <v-icon color="black lighten-1" class="ml-4" style="font-size:2rem;">mdi-logout</v-icon> -->
    </v-toolbar-items>
  </v-toolbar>
        </template>
    <v-row>
      <v-col md="8">
        <v-tabs>
          <v-tab>INDIGITAL PACKS</v-tab>
          <v-tab-item v-slot:default :search="search"
            :items-per-page.sync="itemsPerPage"
            :sort-by="sortBy" :sort-desc="sortDesc" >
            <v-card flat >
              <v-card-text>
                <v-row >
                  <v-col md="4" v-for="(item,idx) in indigitalpacks" :key="idx">
                    <v-card>
                      <v-card-title>
                        <div align="center" class="body-2 font-weight-medium text-wrap-dots" style="margin-left:50px;">
                          {{ item.name }}
                        </div>
                      </v-card-title>
                      <v-row>
                        <v-col cols="12" sm="^6" md="6" lg="5" style="margin-left:150px;">
                          &#x20b9; {{ item.price }}
                        </v-col>
                      </v-row>
                      <v-card-actions>
                        <v-row align="center" justify="center" class="mb-2">
                          <v-btn color="primary" small @click="addCart(item, 'inDigital');" v-if="!item.isCart">
                            Select
                          </v-btn>
                          <v-btn color="error" small @click="removeCart(item, 'inDigital')" v-else>
                            Remove
                          </v-btn>
                        </v-row>
                      </v-card-actions>
                    </v-card>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-card>
          </v-tab-item>
          <v-tab>BROADCASTER PACKS</v-tab>
          <v-tab-item v-slot:default
            :search="search" :items-per-page.sync="itemsPerPage"
            :sort-by="sortBy" :sort-desc="sortDesc" >
            <v-card flat>
              <v-card-text >
                <v-row >
                  <v-col md="4" v-for="(item,idx) in broadcasterpacks" :key="idx">
                    <v-card>
                      <v-card-title>
                        <div align="center" class="body-2 font-weight-medium text-wrap-dots" style="margin-left:50px;">
                          {{ item.name }}
                        </div>
                      </v-card-title>
                      <v-row>
                        <v-col cols="12" sm="^6" md="6" lg="5" style="margin-left:150px;">
                          &#x20b9; {{ item.price }}
                        </v-col>
                      </v-row>
                      <v-card-actions>
                        <v-row align="center" justify="center" class="mb-2">
                          <v-btn color="primary" small @click="addCart(item, 'broadCast');" v-if="!item.isCart">
                            Select
                          </v-btn>
                          <v-btn color="error" small @click="removeCart(item, 'broadCast')" v-else>
                            Remove
                          </v-btn>
                        </v-row>
                      </v-card-actions>
                    </v-card>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-card>
          </v-tab-item>
          <v-tab>A-LA-CARTE</v-tab>
          <v-tab-item v-slot:default
           >
          <div class="eb-pending">
      <v-card>
        <v-card-title>
          <v-row>
            <v-col sm="12" md="2">
              <v-text-field
                v-model="search"
                label="Search"
                single-line
                hide-details
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="2">
              <v-select
                v-model="priceValue"
                :items="priceList"
                attach
                label="Price"
                item-text="name"
                item-value="id"
              ></v-select>
            </v-col>
            <v-col cols="12" sm="2">
              <v-select
                v-model="genreValue"
                :items="genreList"
                attach
                label="Genre of Channels"
                item-text="name"
                item-value="id"
                multiple
              >
                <template v-slot:selection="{ item, index }">
                  <v-chip v-if="index === 0">
                    <span>{{ item.name }}</span>
                  </v-chip>
                  <span v-if="index === 1" class="grey--text caption" >
                    (+{{ genreValue.length - 1 }})
                  </span>
                </template>
              </v-select>
            </v-col>
            <v-col cols="12" sm="2">
              <v-select
                v-model="broadCastValue"
                :items="broadCasterList"
                label="Broadcaster"
                item-text="name"
                item-value="id"
                hide-details
                multiple
              >
                <template v-slot:selection="{ item, index }">
                  <v-chip v-if="index === 0">
                    <span>{{ item.name }}</span>
                  </v-chip>
                  <span v-if="index === 1" class="grey--text caption">
                    (+{{ broadCastValue.length - 1 }})
                  </span>
                </template>
              </v-select>
            </v-col>
            <v-col cols="12" sm="2">
              <v-select
                v-model="languageValue"
                :items="languageList"
                item-text="name"
                label="Language"
                item-value="id"
                multiple
                hide-details
              >
                <template v-slot:selection="{ item, index }">
                  <v-chip v-if="index === 0">
                    <span>{{ item.name }}</span>
                  </v-chip>
                  <span v-if="index === 1" class="grey--text caption" >
                    (+{{ languageValue.length - 1 }})
                  </span>
                </template>
              </v-select>
            </v-col>
            <v-col sm="12" md="1">
               <v-select
                  item-text="text"
                  item-value="id"
                  v-model="sortBy"
                  hide-details
                  :items="sortItem"
                  label="Sort by"
                ></v-select>
            </v-col>
            <v-col sm="12" md="1">
              <v-btn-toggle v-model="sortDesc" mandatory>
                <v-btn class="mr-0 ml-0 mt-4" tile  outlined fab x-small depressed color="primary"> 
                  <v-icon>mdi-arrow-up</v-icon>
                </v-btn>
                <v-btn class="mr-0 ml-0 mt-4" tile  outlined fab x-small depressed color="primary">
                  <v-icon>mdi-arrow-down</v-icon>
                </v-btn>
              </v-btn-toggle>
            </v-col>
          </v-row>
        </v-card-title>
      </v-card>
          </div>
          <v-data-iterator class="mt-3"
        :search="search"
        :items="filterChannelList"
        :items-per-page.sync="itemsPerPage"
        :footer-props="{ itemsPerPageOptions }"
        :sort-by="sortBy"
        :sort-desc="sortDesc"
      >
        <template v-slot:default="props">
          <v-row>
            <v-col
              v-for="(item, i) in props.items"
              :key="i"
              cols="5"
              sm="4"
              md="4"
              lg="4"
            >
              <v-card class="mx-auto">
                <div class="img-fix">
                  <div class="img-con" style="width:100px;">
                    <img :src="item.imageUrl" class="img-w-100"/>
                  </div>
                </div>
              <v-card-text class="text--primary eb-p0 ctm-pd">
                <div align="center" class="body-2 font-weight-medium text-wrap-dots">
                    {{ item.name }}
                  </div>
                  <div>
                    <v-row>
                      <v-col cols="12" sm="^6" md="6" lg="7">
                        <span class="overline text-wrap-dots">
                           {{ item.genre.name | get5Char}}
                        </span>| 
                        <span class="overline text-wrap-dots">
                          {{ item.language.name | get3Char }}
                        </span>
                      </v-col>
                      <v-col cols="12" sm="^6" md="6" lg="5">
                        &#x20b9; {{ item.price }}
                      </v-col>
                    </v-row>
                  </div>
                </v-card-text>
                <v-card-actions>
                  <v-row align="center" justify="center" class="mb-2">
                      <v-btn color="primary" small @click="addCart(item, 'alaCard');" v-if="!item.isCart">
                        Select
                      </v-btn>
                      <v-btn color="error" small @click="removeCart(item, 'alaCard')" v-else>
                        Remove
                      </v-btn>
                    </v-row>
                </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
        </template>
      </v-data-iterator>
          </v-tab-item>
        </v-tabs>
      </v-col>
      <v-col md="4">
        <v-card flat>
          <v-card-title class="User-Selection-labels">User Selection</v-card-title>
          <v-card-subtitle class="User-Selection-labels" style="font-size:12px;">
            <div>CHANNELS SELECTED</div>
            <div>INDIGITAL PACKS : {{ userSelection.inDigital.length }}</div>
            <div>BROADCASTER PACKS : {{ userSelection.broadCast.length }}</div>
            <div>ALACARTE : {{ userSelection.alaCard.length }}</div>
            <!-- <div>Totalprice</div> -->
          </v-card-subtitle>
          <hr class="ml-2 mr-2" />
          <v-card-text>
            <v-expansion-panels>
              <v-expansion-panel>

                <v-expansion-panel-header>INDIGITAL PACKS ({{ userSelection.inDigital.length }})</v-expansion-panel-header>
                <v-expansion-panel-content>
                  <v-row v-for="(item,idx) in userSelection.inDigital" :key="idx">
                    <v-col md="7">
                      <div style="font-size:16px; color:rgba(133, 50, 142, 1);"> <b>{{item.name}}</b></div>
                    </v-col>
                    <v-col>
                      <b>₹ {{item.price}}</b>
                    </v-col>
                    <v-col>
                      <v-btn color="error" small @click="removeCart(item, 'inDigital')"> X </v-btn>
                    </v-col>
                  </v-row>
                </v-expansion-panel-content>
              </v-expansion-panel>
            </v-expansion-panels>

            <v-expansion-panels>
              <v-expansion-panel>
                <v-expansion-panel-header>
                  BROADCASTER PACKS ({{ userSelection.broadCast.length }})
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                  <v-row v-for="(item,idx) in userSelection.broadCast" :key="idx">
                    <v-col md="7">
                      <div style="font-size:16px; color:rgba(133, 50, 142, 1);"> <b>{{item.name}}</b></div>
                    </v-col>
                    <v-col>
                      <b>₹ {{item.price}}</b>
                    </v-col>
                    <v-col>
                      <!-- ddddddddddddd -->
                      <v-btn color="error" small @click="removeCart(item, 'broadCast')"> X </v-btn>
                    </v-col>
                  </v-row>
                </v-expansion-panel-content>
              </v-expansion-panel>
            </v-expansion-panels>

            <v-expansion-panels>
              <v-expansion-panel>
                <v-expansion-panel-header>
                  A-LA-CARTE ({{ userSelection.alaCard.length }})
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                  <v-row v-for="(item,idx) in userSelection.alaCard" :key="idx">
                    <v-col md="7">
                      <div style="font-size:16px; color:rgba(133, 50, 142, 1);"> <b>{{item.name}}</b></div>
                    </v-col>
                    <v-col>
                      <b>₹ {{item.price}}</b>
                    </v-col>
                    <v-col>
                      <v-btn color="error" small @click="removeCart(item, 'alaCard')"> X </v-btn>
                    </v-col>
                  </v-row>
                </v-expansion-panel-content>
              </v-expansion-panel>
            </v-expansion-panels>

<!--             <v-row v-for="(tem,idx) in userSelection.alaCard" :key="idx">
              <v-col md="7">
                <div style="font-size:16px; color:rgba(133, 50, 142, 1);"> <b>{{tem.name}}</b></div>
              </v-col>
              <v-col>
                <b>₹ {{tem.price}}</b>
              </v-col>
            </v-row> -->
          </v-card-text>
        </v-card>
        <v-col md="4" style="margin-left:200px;">
          <v-btn class="ml-auto d-block main-button" color="primary white--text text--lighten-1" @click="goToOptimize">Optimize </v-btn>
          <!-- <v-btn class="ml-auto d-block main-button" color="primary white--text text--lighten-1" href="/optimization">Optimize </v-btn> -->
        </v-col>
      </v-col>
    </v-row>
  </div>
</template>
<script>
import { mapActions } from "vuex";
import _ from "lodash";
import { getGenreChannelsList, getBroadCasterList, getLanguagelsList, getChannelListByRegion , getPacks} from "../services/channel";
export default {
   name: "UserList",
  data() {
    return {
      previewCard: [],
      itemsPerPage: 20,
      itemsPerPageOptions: [20, 40, 60, 80, 100],
      text: "",
      broadcasterpacks: [],
      userList: [],
      cartList: [],
      headers: [
        {
          text: "Channel",
          value: "channel"
        },
        {
          text: "Name",
          value: "name"
        },
        {
          text: "Description",
          value: "description"
        },
        {
          text: "Price",
          value: "price"
        },
        {
          text: "Broadcaster Name",
          value: "broadcasterName"
        },
        {
          text: "Quality",
          value: "quality"
        }
      ],
      search: "",
      genreList: [],
      broadCasterList: [],
      languageList: [],
      priceList:[
        {
          id:[1,2,3,4,5],
          name: 'Below ₹ 5'
        },
        {
          id: [1,2,3,4,5,6,7,8,9,10],
          name: 'Below ₹ 10'
        },
        {
          id: [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20],
          name: 'Below ₹ 20'
        },
        {
          id: 'ALL',
          name: 'All Channels'
        }
      ],
      priceValue: 'ALL',
      genreValue: [],
      broadCastValue: [],
      indigitalpacks: [],
      userSelection: { broadCast: [], alaCard: [], inDigital: [] },
      languageValue: [],
      channelsList:[],
      sortItem: [
        {
          id:'name',
          text:'Name'
        },
        {
          id:'broadcasterName',
          text:'Broadcaster Name'
        },
        {
          id:'genreName',
          text:'Genre Name'
        },
        {
          id:'quality',
          text:'Quality'
        },
        {
          id:'price',
          text:'Price'
        }
      ],
      sortBy:'price',
      sortDesc: false
    };
  },
  computed: {
    filterChannelList(){
      const gen = this.genreValue;
      const broadcarst = this.broadCastValue;
      const price = this.priceValue;
      const language = this.languageValue;
      const
       result = this.$store.getters["channel/getChannelList"];
      const genreSet = new Set(gen)
      const broadcastSet = new Set(broadcarst)
      const priceChannelSet = new Set(price)
      const languageSet = new Set(language)
      const response = result.filter(post => {
        const validGenre = gen.length == 0 ? true : genreSet.has(post.genre.id)
        const validBroadcast = broadcarst.length == 0 ? true : broadcastSet.has(post.broadcaster.id)
        const validPriceChannel = price == 'ALL' ? true : priceChannelSet.has(post.price);
        const validLanguage = language.length == 0 ? true : languageSet.has(post.language.id) 
        return validGenre && validBroadcast && validPriceChannel && validLanguage
      });
      return response;
    },
    cartUpdate: {
      get: function() {
        return this.$store.getters["channel/getCartList"];
      },
      set: function(newal) {
        this.setCart(newal);
      }
    },
  },
   methods: {
    ...mapActions("channel", ["setCart", "setChannel"]),
    optimization() {
      this.$router.push({ path: "/optimization" });
    },
    setChannelList(){
      const result = this.$store.getters["channel/getChannelList"];
      this.channelsList = result;
    },
    async getPacks1() {
      var userData = JSON.parse(localStorage.getItem("in:user"))        
      const payload = {
        customerType:userData.customerType,
        quality: userData.quality,
        region: userData.region,
        category:"Dpo"
      };
      const result = await getPacks(payload);
      const  data  = result.message;
      let newData = data.map(e => { e.isCart = false; return e})
      this.indigitalpacks = newData;
    },
    async getPacks() {
      var userData = JSON.parse(localStorage.getItem("in:user"))        
      const payload = {
        customerType: userData.customerType,
        quality: userData.quality,
        region: userData.region,
        category:"broadcaster"
      };
      const result = await getPacks(payload);
      const  data  = result.message;
      this.broadcasterpacks = data;
      console.log(this.broadcasterpacks)
    },
    async getChannelItem() {
      var userData = JSON.parse(localStorage.getItem("in:user"))        
      const payload = {
        customerType:userData.customerType,
        quality: userData.quality,
        region: userData.region
      };
      const result = await getChannelListByRegion(payload);
      const  data  = result.message;
      if (data){
        const newData = data.map(e => { e.isCart = false; return e})
        this.setChannel(newData);
      }
    },
    goToOptimize() {
      const inDigi_id_Ayyray = this.userSelection.inDigital.map(bucket => {
        return _.map(bucket.channels, "id")
      })
      const b_Channles_arrays = this.userSelection.broadCast.map(bucket => {
        return _.map(bucket.channels, "id")
      })
      this.storeSelectedBouquetsInLocasStorage()
      const inDigi_ChneIds      = _.uniq(_.flatten(inDigi_id_Ayyray))
      const broadCast_ChanneIds = _.uniq(_.flatten(b_Channles_arrays))
      const ala_Channl_Array    = _.map(this.userSelection.alaCard, 'id')
      const uniqSelectedChannels = _.uniq(_.flatten([inDigi_ChneIds, broadCast_ChanneIds, ala_Channl_Array]) )
      this.$router.push({
        name: "optimization",
        params: { uniqSelectedChannels }
      });
    },
    storeSelectedBouquetsInLocasStorage (){
      console.log(this.userSelection)
      let smllInDigis = this.userSelection.inDigital.map(bucket => {
        return {
          id: bucket.id,
          name: bucket.name,
          description: bucket.description,
          price: bucket.price,
          channelsCount: bucket.channels.length,
          type: 'inDigital'
        }
      })
      let selectedBrodCasts = this.userSelection.broadCast.map(bucket => {
        return {
          id: bucket.id,
          name: bucket.name,
          description: bucket.description,
          price: bucket.price,
          channelsCount: bucket.channels.length,
          type: 'broadCast'
        }
      })
      let SelectedBoucquets = _.uniq(_.flatten([smllInDigis, selectedBrodCasts]))
      console.log(JSON.stringify(SelectedBoucquets))
      localStorage.setItem('SelectedBoucquets', JSON.stringify(SelectedBoucquets))
    },
    async getGenreChannel() {
      const result = await getGenreChannelsList();
      const  data  = result.message;
      if (data){
        this.genreList = data
      }
    },
    async getBroadCaster() {
      const result = await getBroadCasterList();
      const  data  = result.message;
      if (data){
        this.broadCasterList = data;
      }
    },
    async getLanguage() {
      const result = await getLanguagelsList();
      const data  = result.message;
      if (data){
        this.languageList = data;
        // this.languageList.push({
        //   id: 'ALL',
        //   name: 'All'
        // })
      }
    },
    addCart(payload, addingType){
      payload.isCart = true;
      if (addingType == 'alaCard') {
        this.userSelection.alaCard.push(payload)
      } else if (addingType == 'broadCast') {
        this.userSelection.broadCast.push(payload)
      } else if (addingType == 'inDigital') {
        this.userSelection.inDigital.push(payload)
      }
    },
    removeCart(payload, removingType='alaCard'){
      payload.isCart = false;
      if (removingType == 'alaCard') {
        let ala = this.userSelection.alaCard.filter(x => x.id != payload.id)
        this.userSelection.alaCard = ala
      } else if (removingType == 'broadCast') {
        let brd = this.userSelection.broadCast.filter(x => x.id != payload.id)
        this.userSelection.broadCast = brd;
      } else if (removingType == 'inDigital') {
        let inDigi = this.userSelection.inDigital.filter(x => x.id != payload.id)
        this.userSelection.inDigital = inDigi
      }
    }
  },
  mounted(){
    this.getPacks();
    this.getPacks1();
     this.setChannelList();
    // this.getChannelItem();
    //  this.getPacks();
  },
  created(){
    this.setChannelList();
    this.getChannelItem();
    this.getGenreChannel();
    this.getBroadCaster();
    this.getLanguage();
  },
  filters: {
    get3Char: function (value) {
      return value.substring(0,3);
    },
    get5Char: function (value) {
      return value.substring(0,5);
    }
  }
};
</script>
<style scoped>
.User-Selection-labels {
  color: rgb(37, 60, 145) !important;
}
.Channel-card:hover {
  box-shadow: 0px 0px 4px rgb(37, 60, 145) !important;
}
</style>
