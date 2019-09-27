<template>
  <div id="app">
    <div class="container">


      <div class="row">

        
           <div class="col-6 card" style="width: 18rem;" v-for="board in boards" :key="board.id">
              <div class="card-body">
                <h5 class="card-title" v-text="board.name"></h5>
                <button class="btn btn-primary"   @click="getListFromBoard(board.id)"> ver list</button>
                
                <template v-if="listFromBoard.board_id == board.id">
                    
                    <li class="card-text mt-3"  v-for="list in listFromBoard.lists"  :key="list.id"  > {{list.name}} 
                      
                      <button  class="btn btn-success"  @click="getCardsFromList(list.id)"> Ver cards </button>

                        <template v-if="cardsFromList.list_id == list.id">
                    
                           <ul class="mt-3">
                              <li class="card-text mt-2"   v-for="card in cardsFromList.cards"  :key="card.id"  > {{card.name}} 
                              
                                <a :href="card.shortUrl" target="_blank" >  ver en trello</a>
                            </li>
                           </ul>
                        </template> 

                    </li>
                </template>

             
              
              </div>
         </div>

      



    </div>
 
  </div>






    
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'app',
  data () {

    return {

      token: '0ec0194b227b52871b7fb9004877a5a6dbd78d9d00e7c3f9d604c9c742168831',
      key: '5f39fd32ce375d936870391efedad435',
      boards: {},
      listFromBoard:{},
      cardsFromList: [],
      cardsFilter:{},
      parentUri: 'https://api.trello.com/1/members/me/'

    }

  },
  components: {
    
  }, 

  computed: {



  },

  methods: {

    getBoards() {

      let uri = `${this.parentUri}boards?key=${this.key}&token=${this.token}`

      axios.get( uri )
      .then(res => {

        this.boards = res.data
       
      
      })


    }, 

    getCards() {


    }, 

    getCardsFromList( list_id ) {


      let uri = `https://api.trello.com/1/lists/${list_id}/cards?token=${this.token}&key=${this.key}`
      
      let me = this
      
      axios.get(uri)
      .then (res =>  {
        
          me.cardsFromList = {
            list_id,
            cards: res.data
          }

         
      })

    
    

    },

    getListFromBoard(board_id) {

      let me = this
      me.cardsFromList = {}
      let uri = `https://api.trello.com/1/boards/${board_id}/lists?cards=none&card_fields=all&filter=open&fields=all&key=${this.key}&token=${this.token}`


      axios.get(uri)
        .then( res => {

            me.listFromBoard = {
              board_id,
              lists: res.data
            }


        })
    }

    



  },
  mounted(){

    this.getBoards()


  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
