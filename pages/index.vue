<template>
<div>
<div class="container">
  <h1 style="color:#FD379E;text-shadow: 2px 2px #000;font-size:80px;">Retro News</h1>
  <small  style="color:#FD379E;text-shadow: 1px 1px #000;font-size:20px;margin-bottom:1rem;">News tailored to you.</small>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css" integrity="sha512-+4zCK9k+qNFUR5X+cKL9EIR+ZOhtIloNl9GIKS57V1MyNsYpYcUrUeQc9vNfzsWfV28IaLL3i96P9sdNyeRssA==" crossorigin="anonymous" />
    <div v-if="!Loading" class="card" v-for="item in items">
        <h3 @click="GoToUrl(item.url)" style="margin-left:1%;">{{truncate(item.title)}}</h3>
        <div class="bottom">
         <p style="margin-left:1%;margin-bottom:1%;" >{{item.source}}<i style="margin-left:5px;" class="fas fa-bolt"></i></p>
        </div>
    </div>
    <button @click="LoadMore" v-if="!Loading">Load more</button>
    <Loader v-if="Loading" />
</div>
</div>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from 'axios'
export default  {
    data() {
        return {
            items: [],
            clicked: 0,
            Loading: true
        }
    },
    methods: {
      async LoadMore() {
        this.items = []
        this.clicked = this.clicked + 1;
        var interests: string[]
        interests = this.$cookies.get('interests')

        for (let i = 0; i < interests.length; i++) {
            const inter = interests[i]
            const limit = 50 * this.clicked / interests.length;
            const res = await axios({
                "method": "POST",
                "url": "https://tidalwaves-news-analytics2.p.rapidapi.com/articles?limit=" + limit,
                "headers": {
                    "x-rapidapi-host": "tidalwaves-news-analytics2.p.rapidapi.com",
                    "x-rapidapi-key": "1087dcd8f0msh57e4fe6234f1578p1f3b03jsnf17cdb409969",
                    "useQueryString": true
                },
                "data": {
                    "keywords": {
                        "ids": [
                            inter
                        ]
                    },
                }
            })
            res.data.data.forEach((item: object) => {
              console.log(item)
              this.items.push(item)
            })
        }
      },
      GoToUrl(url) {
        window.location.href = url;
      },
      truncate(item) {

            const length = 65;
            const ending = '...';

            if (item.length > length) {
                return item.substring(0, length - ending.length) + ending;
            } else {
                return item;
            }
        },
    },
    async mounted() {
      const interval = setInterval(() => {
        console.log(this.items.length)
        if (this.items.length > 40) {
          this.Loading = false;
          clearInterval(interval)
        }
      },500)

      if (this.$cookies.get('interests') == null) {
        this.$router.push('/select')
      }
        var interests: string[]
        interests = this.$cookies.get('interests')
        console.log(interests)

        for (let i = 0; i < interests.length; i++) {
            const inter = interests[i]
            const limit = 50 / interests.length;
            const res = await axios({
                "method": "POST",
                "url": "https://tidalwaves-news-analytics2.p.rapidapi.com/articles?limit=" + limit,
                "headers": {
                    "x-rapidapi-host": "tidalwaves-news-analytics2.p.rapidapi.com",
                    "x-rapidapi-key": "1087dcd8f0msh57e4fe6234f1578p1f3b03jsnf17cdb409969",
                    "useQueryString": true
                },
                "data": {
                    "keywords": {
                        "ids": [
                            inter
                        ]
                    },
                }
            })
            res.data.data.forEach((item: object) => {
                this.items.push(item)
            })
        }
    }
}
</script>

<style scoped>
body {
    background:#9BFEB5;
}

.container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap:wrap;
    background:#9BFEB5;
    flex-direction: column;
}
.card{
  background:#E5E5E5;
  margin-bottom:1rem;
  min-height:5rem;
  width:80%;
  margin-left:1rem;
  border-radius:4px;
}

.bottom {
 margin-top:4rem;
}

p {
  color:#636363;
}

h3 {
  transition: 0.2s ease;
  cursor: pointer;
  text-shadow: 1.5px 1.5px #FD379E;
}

h3:hover {
  color:#FD379E;
  margin-left:1.5rem !important;
  text-shadow: 2px 2px #000;
}

.modal {
  position: fixed;
  width:50vw;
  height:30vh;
  background:#9BFEB5;
  z-index:999999999999;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 8px;
}

.modal-holder {
    position:relative;
}

button {
    margin-top: 2rem;
    background: #E5E5E5;
    width: 10rem;
    height: 2rem;
    border-radius:4px;
    border:1px solid #222222;
    transition:0.2s ease;
    margin-bottom:5rem;
}
button:hover {
        -webkit-box-shadow: 10px 10px 0px -2px rgba(0, 0, 0, 1);
    -moz-box-shadow: 10px 10px 0px -2px rgba(0, 0, 0, 1);
    box-shadow: 10px 10px 0px -2px rgba(0, 0, 0, 1);
}

</style>
