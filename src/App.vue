<template>
  <div id="app" class="container">
    <h1 class="text-center app-title">GAMES LIST</h1>
    <div class="row">
      <game-form
      :systemOptions="systemOptions"
      @add:game="addGame" />
    </div>
    <div class="row">
      <game-table
      :games="games"
      :headers="headers"
      :systemOptions="systemOptions"
      @delete:game="deleteGame"
      @edit:game="editGame"/>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import GameTable from '@/components/GameTable.vue'
import GameForm from '@/components/GameForm.vue'

import { SHEETS_API_URL, SHEETS_API_ID } from '@/config.js'

import './styles/app.scss';

export default {
  name: 'app',
  components: {
    GameTable,
    GameForm,
  },
  data() {
    return {
      games: [],
      systemOptions: [
        {
          id: 1,
          title: 'Xbox'
        },
        {
          id: 2,
          title: 'PS4'
        },
        {
          id: 3,
          title: 'Switch'
        },
      ],
      headers:[
        {
          id: 1,
          name: 'Name'
        },
        {
          id: 2,
          name: 'System'
        },
        {
          id: 3,
          name: 'Progress'
        },
        {
          id: 4,
          name: 'Complete'
        },
        {
          id: 5,
          name: 'Actions'
        }
      ]
    }
  },
  mounted(){
    this.getGames()
  },
  methods: {
    // Get the game list
    async getGames() {
      axios.get(SHEETS_API_URL , {
        params: {
          spreadsheetId: SHEETS_API_ID,
          limit: 40
        }
      })
      .then(response => {
        this.games = response.data.results
      })
      .catch(error => {
        console.log(error);
      })
    },
    // Add Game
    addGame(game) {
      let lastId =
        this.games.length > 0
          ? this.games[this.games.length - 1].id
          : 0;
      let id = lastId + 1;
      let newGame = { ...game, id };

      // Differentiates added games with a type
      game.new = true;

      this.games = [...this.games, newGame];

    },
    // Delete Game
    deleteGame(id) {
      // Creates a new array that removes the game
      this.games = this.games.filter(
        game => game.id !== id
      )
    },
    // Edit Game
    editGame(id, updatedGame) {
      this.games = this.games.map(game =>
        game.id === id ? updatedGame : game
      )
    }
  },
}
</script>

<style>
  .app-title{
    margin: 5rem 0;
  }
</style>
