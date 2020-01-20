<template>
  <div id="game-table">
    <!-- If there are no games -->
    <div v-if="games.length < 1" class="empty-table text-center">
      <x-circle-icon size="2x" class="no-game-icon"/> No Games :(
    </div>
    <!-- Render table otherwise -->
    <table v-else class="u-full-width">
      <thead>
        <tr>
          <th v-for="header in headers" :key="header.id">{{ header.name }}</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="game in games" :key="game.id" :class="{ 'new': game.new }">

          <td v-if="editing === game.id">
            <input type="text" v-model="game.name" :class="{ 'error' : invalid }"/>
          </td>
          <td v-else>{{ game.name }}</td>

          <td v-if="editing === game.id">
            <select
            v-model="game.system">
              <option v-for="system in systemOptions" :key="system.id" :value="system.title">{{system.title}}</option>
            </select>
          </td>
          <td v-else>{{ game.system }}</td>

          <td v-if="editing === game.id" class="action-btns">
            <button
              @click="editGame(game)">
              <save-icon size="1.1x" class="action-icon"/>
            </button>
            <button
              class="muted-button" @click="cancelEdit(game)">
              <corner-down-left-icon size="1.1x" class="action-icon"/>
            </button>
          </td>

          <td v-else class="action-btns">
            <button
              @click="editMode(game)">
              <edit-2-icon size="1.1x" class="action-icon"/>
            </button>
            <button
              @click="$emit('delete:game', game.id)">
              <x-icon size="1.1x" class="action-icon"/>
            </button>
          </td>

        </tr>
      </tbody>

    </table>
  </div>
</template>

<script>
  import {
    Edit2Icon,
    XIcon,
    XCircleIcon,
    SaveIcon,
    CornerDownLeftIcon } from 'vue-feather-icons'

  export default {
    name: 'game-table',
    components: {
      Edit2Icon,
      XIcon,
      XCircleIcon,
      SaveIcon,
      CornerDownLeftIcon
    },
    props: {
      headers: Array,
      games: Array,
      systemOptions : Array
    },
    data() {
      return {
        editing: null,
        invalid: false,
      }
    },
    methods: {
      // Edit game method
      editGame(game) {
        // If the game name or system is blank
        if (game.name === '' || game.system === '') {
          this.invalid = true;
          return
        }
        this.$emit('edit:game', game.id, game)
        this.editing = null
      },
      editMode(game) {
        this.invalid = false;
        this.cachedGame = Object.assign({}, game)
        this.editing = game.id
      },
      cancelEdit(game) {
        Object.assign(game, this.cachedGame)
        this.editing = null;
      }
    }
  }
</script>

<style lang="scss" scoped>
  .empty-table{
    display: flex;
    justify-content: center;
    align-items: center;
    color: red;
    font-size: 2rem;
    .no-game-icon{
      margin-right: 1rem;
    }
  }
  input, select{
    width: 100%;
    &.error{
      border: 1.5px solid red;
    }
  }
  td{
    &:first-of-type{
      width: 60%;
    }
  }
  .action-btns{
    button{
      padding: 1rem;
      height: auto;
      line-height: 1;
      margin: 0 .3rem;
      display: inline-flex;
      align-items: center;
    }
  }
  .new{
    color: green;
  }
</style>
