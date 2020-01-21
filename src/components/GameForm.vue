<template>
  <div id="game-form">
    <form @submit.prevent="handleSubmit">
      <div class="row notification-bar" :class="{ 'hide': !error && !success }">
        <p v-if="error && submitting" class="error-message">
          ❗All Fields Must Be Filled Out
        </p>
        <p v-if="success" class="success-message">
          ✅ Item Successfully Added
        </p>
      </div>
      <div class="row form-container text-center">
        <div class="form-input">
          <label :class="{ 'error': submitting && invalidName }">Game</label>
          <input
            type="text"
            ref="first"
            :class="{ 'error': submitting && invalidName }"
            v-model="game.name"
            @focus="clearStatus"
            @keypress="clearStatus"
          />
        </div>
        <div class="form-input">
          <label :class="{ 'error': submitting && invalidSystem }">System</label>
          <select
          :class="{ 'error': submitting && invalidSystem }"
          v-model="game.system"
          @focus="clearStatus">
            <option v-for="system in systemOptions" :key="system.id" :value="system.title">{{system.title}}</option>
          </select>
        </div>
        <div class="form-button">
          <button>Add Game</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>

  export default {
    name: 'game-form',
    props: {
      systemOptions: Array
    },
    data() {
      return {
        submitting: false,
        error: false,
        success: false,
        game: {
          name: '',
          system: '',
          new: true,
          progress: 0,
          complete_status: "none"
        },
      }
    },
    computed: {
      invalidName() {
        return this.game.name === ''
      },

      invalidSystem() {
        return this.game.system === ''
      },
    },
    methods: {
      handleSubmit() {
        this.submitting = true
        this.clearStatus()

        if (this.invalidName || this.invalidSystem) {
          this.error = true
          return
        }

        this.$emit('add:game', this.game)
        this.$refs.first.focus()

        // Sets game back to default value after adding
        this.game = {
          name: '',
          system: '',
          new: true
        }
        this.error = false
        this.success = true
        this.submitting = false

      },

      clearStatus() {
        this.success = false
        this.error = false
      }
    },
  }
</script>

<style lang="scss" scoped>
  select{
    min-width: 17rem;
    option{
      font-size: 1.5rem;
      font-family: 'Montserrat';
    }
  }
  .notification-bar{
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #edf1f7;
    border: .15rem solid #d6d8db;
    padding: 1.5rem;
    border-radius: 1rem;
    margin-bottom: 2rem;
    &.hide{
      display: none;
    }
    p{
      margin-bottom: 0;
    }
  }

  .form-container{
    display: flex;
    justify-content: space-around;
    align-items: flex-end;
    @media (max-width: 900px) {
      flex-direction: column;
      align-items: center;
    }
    .form-input{
      display: flex;
      align-items: baseline;
      label{
        font-family: 'Montserrat';
        font-weight: 600;
        margin-right: 1rem;
        &.error{
          color:red;
        }
        &+*.error{
          border: 1.5px solid red;
        }
      }
    }
    .form-button{
      margin-bottom: 1.5rem;
      button{
        margin-bottom: 0;
      }
    }
  }
  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }
</style>
