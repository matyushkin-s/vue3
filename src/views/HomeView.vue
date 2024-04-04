<script setup>
  import { useRouter } from 'vue-router'
  import { reactive } from 'vue'
  import { useVuelidate } from '@vuelidate/core'
  import { email, required, minLength } from '@vuelidate/validators'

  const router = useRouter()

  const initialState = {
    pass: '',
    email: '',
  }

  const state = reactive({
    ...initialState,
  })

  const rules = {
    pass: { required, minLength: minLength(6) },
    email: { required, email },
  }

  const v$ = useVuelidate(rules, state)

  const onLogin = async () => {
    await v$.$validate

    if (!v$.value.$invalid) {
      router.push({ path: '/game' })
    }
  }
</script>

<template>
  <v-card
    title="Login"
    variant="outlined"
  >
    <form>
      <v-text-field
        v-model="state.email"
        :error-messages="v$.email.$errors.map(e => e.$message)"
        label="E-mail"
        required
        @blur="v$.email.$touch"
        @input="v$.email.$touch"
      ></v-text-field>

      <v-text-field
        v-model="state.pass"
        type="password"
        :counter="6"
        :error-messages="v$.pass.$errors.map(e => e.$message)"
        label="Password"
        required
        @blur="v$.pass.$touch"
        @input="v$.pass.$touch"
      ></v-text-field>

      <div class="d-flex justify-center">
        <v-btn
          prepend-icon="$vuetify"
          variant="outlined"
          class="mb-2"
          @click="onLogin()"
        >
          Login
        </v-btn>
      </div>
    </form>
  </v-card>
</template>
