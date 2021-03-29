<template>
  <div>
    <div class="parent">
       <v-app-bar color="" height="100px"
       app
       clipped-right
        dark>
            <v-toolbar-title>
                <div class="display-1">
                    SHOPPIE...
                </div>
            </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn color="primary " text @click="dialog = !dialog">
              <span class="font-weight-bold">Nominated</span>
            </v-btn>
            <v-btn @click="dialog = !dialog" x-small fab color="primary">
              {{nominated.length}}
            </v-btn>
          </v-app-bar>
          <v-navigation-drawer clipped class="hidden-sm-and-down" right app color="white" v-model="dialog" width="30%"  disable-resize-watcher >
             <v-list v-if="nominated.length" >
               <v-list-item class="marg"  v-for="doc in nominated" :key="doc.imdbID">
                  <v-container class="">
                     <v-row>
                       <v-col> <div class="title">{{doc.Title}}</div> </v-col>
                     </v-row>
                     <v-row>
                       <v-col>
                             <v-btn 
                            @click="handleFil(doc.imdbID)"
                             small elevation="" dark color="">remove</v-btn>
                         
                       </v-col>
                       <v-col><span>{{doc.Year}}</span></v-col>
                     </v-row>
                   </v-container>
                   
               </v-list-item>
                   
             </v-list>
          </v-navigation-drawer>
           <v-navigation-drawer   class="hidden-md-and-up" right  app color="white" v-model="dialog" width="70%"   disable-resize-watcher >
               <v-list v-if="nominated.length" >
               <v-list-item class="marg" v-for="doc in nominated" :key="doc.imdbID">
                  <v-container class="">
                     <v-row>
                       <v-col> <div class="title">{{doc.Title}}</div> </v-col>
                     </v-row>
                     <v-row>
                       <v-col>
                             <v-btn
                             @click="handleFil(doc.imdbID)"
                              small elevation="" dark color="">remove</v-btn>
                         
                       </v-col>
                       <v-col><span>{{doc.Year}}</span></v-col>
                     </v-row>
                   </v-container>
               </v-list-item>
             </v-list>
          </v-navigation-drawer>
    </div>
  </div>
</template>

<script>
import { bus } from '../main'
export default {
  data:()=>({
    info:false,
    dialog: false,
      nominated:[],
      indox:[]
      

  }),
  methods:{
        handleFil(id){
        //  
          const index = this.nominated.find(data =>{
            return data.imdbID == id
          })
          // console.log(index)
        bus.$emit('reversed' , index)
        let res2 = this.indox.filter(doc => doc != id)
        bus.$emit('res2', res2)

    const res =  this.nominated.filter(doc => doc.imdbID != id)
        this.nominated = res
        bus.$emit('reverse' , this.nominated)
         
        
        
    }
  },
  computed:{

  },
  created(){

    bus.$on('chang', ((data) => {
      this.nominated = data
      
    }))
    bus.$on('me', ((data) => {
      this.indox = data
      
    }))
   
   
  },
  
  

}
</script>

<style scoped>

  .parent{
    position: relative;
    z-index: 2000;
  }
  .marg.v-list-item{
    border-bottom-color: black;
  }
</style>