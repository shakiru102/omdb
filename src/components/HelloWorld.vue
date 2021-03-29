<template>
<div>
  <v-dialog
    v-model="dialog"
     
    persistent :overlay="false"
    max-width="500px"
    transition="dialog-transition"
  >
    <v-card>
      <v-container>
        <v-row>
          <v-col><div class="subtitle grey--text">You have reached the maximum nominations that you can make </div></v-col>
        </v-row>
        <v-row>
          <v-card-actions>
            <v-btn text color="error" @click="dialog = false">close</v-btn>
          </v-card-actions>
        </v-row>
      </v-container>
    </v-card>
  </v-dialog>
  <div class="parent">
  <div class="test">
    <input type="text" 
    v-model="search"
    @keyup.prevent.enter="handleKey"
     placeholder="Search movie" class="text">
      </div>
    
<v-carousel
    cycle
    height="80vh"
    hide-delimiter-background
   hide-delimiters
    show-arrows-on-hover
  >
    <v-carousel-item class="black" 
    v-for="doc in documents"
    :key="doc.Title"
   
     xs12
     >
     <v-img class="mx-auto" contain width="70%"  gradient="to left, rgba(0,0,0,.5), rgba(0,0,0,.5)"
     :src="doc.Poster"></v-img>
     
   
    </v-carousel-item>
  </v-carousel>
  </div>
  <div >
     <v-container class="py-16">
       <v-layout row wrap justify-center>
         <v-flex xs12 sm6 md3
         v-for="doc in documents"
    :key="doc.Title"
         >
           <v-card
           class="ma-2"
           tile
           elevation="5"
           color="grey lighten-4"
           
           >
                <v-img height="200px" :src="doc.Poster"></v-img>
                <v-card-subtitle><div class="subtitle black--text">{{doc.Title}}</div></v-card-subtitle>
                  <v-container class="">
                     <v-row>
                       <v-col><span>    <v-btn
                       @click="handleClick(doc.imdbID)"
                       tile
                       :disabled="doc.nominate"
                         color="primary">nominate</v-btn>
                       </span></v-col>
                       <v-col><div class="text-right grey--text">{{doc.Year}}</div></v-col>
                     </v-row>
                   </v-container>
           </v-card>
         </v-flex>
       </v-layout>
      </v-container>
  </div>
</div>
</template>

<script>
import { bus } from '../main'

  export default {
    name: 'HelloWorld',
    data:()=>({
      documents:[],
      apiKey: 'f40fb17',
      search:'',
      nominated:[],
      dialog: false,
      me:[],
      scape:[]
      
    }),
    methods:{
      
       handleKey(){
         if(this.search != ''){
         fetch(`https://www.omdbapi.com/?s=${this.search}&apikey=${this.apiKey}`)
         .then(res=>{
           return res.json()
         })
         .then(data =>{
          //  let gets
          //  let get
          //  let me = []
              this.documents = data.Search.map(doc =>{
            return  {...doc, nominate:false}})
            
          


         }).then(()=>{
          //  const ray =[]
            //  let index =  this.me.findIndex((docs, ind) =>{
            // return docs == ind
          // })
          this.documents.forEach(doc =>{
              if( this.me.includes(doc.imdbID)){
                doc.nominate = true
              }
                 

            // let mes = this.documents.findIndex(docs => {
            //    return docs.imdbID == doc.imdbID})
            //   //  if(doc){
            //    this.documents[mes].nominate = true

              //  }
              //  ray.push(mes)
            //  console.log(mes)
            //  console.log(this.nominated)
          // if(mes.match(doc)){
          //    doc.nominate = true
          //    console.log(doc.Title)
          // }
          
          })


         })
         }
       },
       handleClick(id){
         const max = this.nominated.length <= 4
         if(max){
           const singleDoc = this.documents.findIndex(doc =>{
           return doc.imdbID == id
         })
           this.documents[singleDoc].nominate = true
           this.nominated = [...this.nominated, this.documents[singleDoc]]
           bus.$emit('chang', this.nominated)
           this.me = [...this.me , id]
           bus.$emit('me', this.me)
          this.dialog =false

         }else{
           this.dialog = true
         }
    
    

       }
    },
    mounted(){
      fetch(`https://www.omdbapi.com/?s=movie&apikey=${this.apiKey}`)
      .then(data => {
       return data.json()
      })
      .then(res =>{
        this.documents = res.Search.map(doc =>{
            return  {...doc, nominate:false}
        })
      })

    },
    created(){
      bus.$on('reverse', (data => this.nominated = data))
      bus.$on('reversed', (data => {
        this.dialog = false
        this.scape = data
        
         let res2 = this.me.filter(doc => doc != this.scape.imdbID)
         this.me = res2

         this.documents.forEach(docs => {
                 let res = this.documents.indexOf(docs)
                 if(this.documents[res].imdbID.includes(this.scape.imdbID)){
        this.documents[res].nominate = false
                   
                 }

           
         });
                 console.log(this.documents)

      
      }))
    },
    updated(){
      if(this.nominated < 4){
        this.dialog = false
      }
    }
  
   

  }
</script>
<style scoped>
.text{
  width: 20em;
  background: rgba(236, 232, 232, 0.8);
  padding: 0.9em 1em;
  border-radius: 30px;
  z-index: 1;
 
  outline: none;

  
 
}

.test{
  
   position: absolute;
  /* left: 50%;
  transform: translateX(-50%);
  top: 50%;  */
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
div.parent{
  position: relative;
}

</style>
