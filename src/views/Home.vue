<template>
  <div class="home">
    <div class="feature-card">
    
<!--tt0409591 (OMDb API Key) give us a movie trailer from omdb-->
      <router-link to="/movie/tt0409591">
        <img src="../assets/home.png" alt="Movie Searcher" class="featured-img" />
        <div class="detail">
          <h3>Movie Search Engine</h3>
          <p>Enter Your TV / Game / TV Series</p>
        </div>
      </router-link>
    </div>

<!--Form to Search Other Movies-->
    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for?" v-model="search" />
      <input type="submit" value="Search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
      <!--The Exact Same Logic Above but in this time we let the user Add a Name (Using Search Bar) so we can get the ID-->
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
        <!--Get the Image Also from the API-->
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster" />
            <!--Get the Type of ur search (Game or TV Series)-->
            <div class="type">{{ movie.Type }}</div>
          </div>

          <!--Movie / TV Serie Preview-->
          <div class="detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
// Composition API
// ref allows u to reference to an file (in our case is search) 
import { ref } from 'vue';
// import environement.js file to get the API key
import env from '@/env.js'

export default {
  setup () {
    const search = ref("");
    const movies = ref([]);

    // Search Method (using Arrow Func)
    const SearchMovies = () => {
      if (search.value != "") {
        //console.log(search.value);
        // run the API Key and fetch Data
        fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
          .then(response => response.json())
          .then(data => {
            // console.log(data);
            movies.value = data.Search; //Here where the Aray of movies are stored
            search.value = "";
            // console.log(data); //IT WORKS !!!
          });
      }
    }

    return {
      search,
      movies,
      SearchMovies
    }
  }
 
}
</script>

<!--Styling the Home Page-->
<style lang="scss">
.home {
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      object-fit: cover;
      height: 700px;
      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;
      margin-left: auto;
      margin-right: auto;
      text-align: center;

      h3 {
        color:#FFF;
        margin-bottom: 16px;
      }

      p {
        color: #FFF;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #FFF;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }

        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42B883;
        padding: 16px;
        border-radius: 8px;
        color: #FFF;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;
        cursor: pointer;

        &:active {
          background-color: #3B8070;
        }
      }
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42B883;
            color: #FFF;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }

        .detail {
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year {
            color: #AAA;
            font-size: 14px;
          }

          h3 {
            color: #FFF;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}
</style>
