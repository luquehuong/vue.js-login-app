<template>
  <div>
    <FinalForm
      :submit="handleSubmit"
      @change="updateState">
      <form slot-scope="props" @submit="props.handleSubmit">
        <div class="note note--down"><p>{{ note }}</p></div>
        <FinalField name="userAcc" :validate="required">
          <div slot-scope="props">
            <input
              v-on="props.events"
              :name="props.name"
              :value="props.value"
              type="text"
              placeholder="User Account"
            />
            <span
              class="error"
              v-if="props.meta.touched && props.meta.error">
              {{ props.meta.error }}
            </span>
          </div>
        </FinalField>
        <FinalField name="password" :validate="[range(6, 20), noSpecialChars]">
          <div slot-scope="props">
            <input
              v-on="props.events"
              :name="props.name"
              :value="props.value"
              type="password"
              placeholder="Password"
            />
            <span
              class="error"
              v-if="props.meta.touched && props.meta.error">
              {{ props.meta.error }}
            </span>
          </div>
        </FinalField>
        <button type="submit" :disabled="props.submitting">
          {{ props.submitting ? 'Submitting' : 'Log In' }}
        </button>
      </form>
    </FinalForm>
  </div>
</template>

<script>
import { FinalForm, FinalField } from 'vue-final-form'

function sleep (timeout) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve()
    }, timeout)
  })
}
export default {
  components: {
    FinalForm,
    FinalField
  },
  data () {
    return {
      formState: null,
      user: 'admin',
      password: 'adminPassword',
      note: ''
    }
  },
  watch: {
    note () {
      const note = document.querySelector('.note')
      if (this.note.length) {
        note.classList.add('note--up')
      } else {
        note.classList.remove('note--up')
        note.classList.add('note--down')
      }
    }
  },
  methods: {
    async handleSubmit (state) {
      await sleep(2000)
      console.log(state)
      await this.login(state.userAcc, state.password)
    },
    updateState (state) {
      this.formState = state
    },
    required (v) {
      return v ? null : 'This field is required!'
    },
    range (min, max) {
      return value => {
        return value && value.length >= min && value.length <= max ? null : `Password should be between length ${min} and ${max}`
      }
    },
    noSpecialChars (v) {
      return /[!@#$%^&*()]/.test(v) ? 'Please do not use specfial chars' : null
    },
    login (userAcc, password) {
      if (userAcc === this.user && password === this.password) {
        this.$router.replace({ name: 'home' })
        this.note = ''
      }
      this.note = 'Login failed'
    }
  }
}
</script>
<style scoped>
</style>
