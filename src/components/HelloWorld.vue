
<template>
    <h1>Photo List</h1>
    <div>
        <button class="button-62" @click="prev">
            Go back
        </button>
        Page
        {{page}}
        <button class="button-62" @click="next">
            Go forward
        </button>
        {{limit}}
    </div>

    <div class="grid-container">

        <div v-for="item in lists" v-bind:key="item.id">
            <div @click="showModal = true">
                <img id="myImg" class="grid-container" :src="item.download_url" />
            </div>
            {{item.id}}
            {{item.author}}
             <!-- overlay -->
        <div class="overlay" v-if="showModal" @click="showModal = false"></div>
        <!-- modal -->
         <div class="modal" v-if="showModal">
            <button class="close" @click="showModal = false">x</button>
            <img :src="item.download_url" alt="lists.url" />
        </div>
            
        </div>

       
    </div>

</template>

<script>

import axios from 'axios';

export default {
  name: "PhotoList",
  
    methods: {
      next() {
      this.page = this.page === 30 ? 30 : this.page + 1;
      axios
        .get(
          `https://picsum.photos/v2/list?page=${this.page}&limit=${this.limit}`
        )
        .then((response) => {
          this.lists = response.data;
        });
    },
    prev() {
      this.page = this.page === 1 ? 1 : this.page - 1;
      axios
        .get(
          `https://picsum.photos/v2/list?page=${this.page}&limit=${this.limit}`
        )
        .then((response) => {
          this.lists = response.data;
        });
    },
    },

    openModal(item) {
      this.offer = item;
      this.$bvModal.show(this.id);
    },
  
  data() {
        return {
            lists: [],
            limit: 30,
            page: 1,
            showModal: false, 
        };
    },
    mounted() {
        const fetchData = async () => {
            try { const response = await axios.get(`https://picsum.photos/v2/list?page=${this.page}&limit=${this.limit}`) 

                this.lists =  response.data;
                console.log(this.lists)

              } catch (error) {
                console.log(error.message);
              } 
        }
        fetchData(); 
    }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.grid-container {
 display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr;
  grid-gap: 5%;
  width: 367px;
  height: 267px;
  margin: 15px 10px 10px 45px;
  
}
.button-62 {
  background: linear-gradient(to bottom right, #EF4765, #FF9A5A);
  border: 0;
  border-radius: 12px;
  color: #FFFFFF;
  cursor: pointer;
  display: inline-block;
  font-size: 10px;
  line-height: 2.5;
  outline: transparent;
  padding: 0 1rem;
  text-align: center;
  text-decoration: none;
  transition: box-shadow .2s ease-in-out;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  white-space: nowrap;
}

.button-62:not([disabled]):focus {
  box-shadow: 0 0 .25rem rgba(0, 0, 0, 0.5), -.125rem -.125rem 1rem rgba(239, 71, 101, 0.5), .125rem .125rem 1rem rgba(255, 154, 90, 0.5);
}

.button-62:not([disabled]):hover {
  box-shadow: 0 0 .25rem rgba(0, 0, 0, 0.5), -.125rem -.125rem 1rem rgba(239, 71, 101, 0.5), .125rem .125rem 1rem rgba(255, 154, 90, 0.5);
}

.overlay {
  position: relative;
  z-index: 9990;
  top: 0;
  left: 0;
  width: 100px;
  height: 100px;
  background-color: rgba(0, 0, 0, .5);
 
}

.modal {
  position: relative;
  width: 37px;
  height: 27px;
  z-index: 9999;
  margin: 0 auto;
  padding: 5px 5px;
  background-color: rgb(124, 86, 86);
  
}

.modal-vue .close{
  position: absolute;
  width: 367px;
  height: 267px;
  top: 10px;
  left: 10px;
}
@media only screen and (max-width: 768px) {
  /* For mobile phones: */
   .grid-container {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr;
  grid-gap: 5px;
  width: 80%;
  height: 80%;
  margin: 25px 10px 10px 25px;
}
}
</style>
