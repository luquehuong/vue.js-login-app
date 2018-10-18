<template>
  <div>
    <FinalForm
      :initialValues="{userAcc: user, password: password}"
      :submit="handleSubmit"
      @change="updateState">
      <form slot-scope="props" @submit="props.handleSubmit">
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
      initialValues: {
        email: 'example@gmail.com'
      },
      user: 'admin',
      password: 'adminPassword'
    }
  },
  methods: {
    async handleSubmit (state) {
      await sleep(2000)
      console.log(state)
      this.login(state.userAcc, state.password)
    },
    updateState (state) {
      this.formState = state
    },
    required (v) {
      return v ? null : 'This field is required!'
    },
    matchedPassword (value, values) {
      return value === values.password ? null : 'Mismatched password!'
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
      } else {
        window.alert('The username or password is incorrect')
      }
    }
  }
}
</script>
<style scoped>
</style>
