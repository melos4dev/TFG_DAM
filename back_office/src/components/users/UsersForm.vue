<template>
  <v-card>
    <v-form>
      <v-container>
        <v-row>
          <v-col cols="12" md="6">
            <v-text-field v-model="email" label="Email profesor"></v-text-field>
          </v-col>
          <v-col cols="12" md="6">
            <v-text-field v-model="username" label="Nombre profesor"></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" md="6">
            <v-text-field v-model="password" label="Contraseña"></v-text-field>
          </v-col>
          <v-col cols="12" md="6">
            <v-container class="px-0" fluid>
              <v-checkbox v-model="checkbox" :label="`Bloqueado`"></v-checkbox>
            </v-container>
          </v-col>
        </v-row>

        <br />
        <v-btn @click="save()" color="primary">Enviar</v-btn>

      </v-container>
    </v-form>

  </v-card>
</template>

<script>
import UsersRepository from '@/repository/UsersRepository.js';
export default {

  data() {
    return {
      password: '',
      username: '',
      emailUser: '',
      checkbox: false
    };
  },
  methods: {
    async save() {
      const nuevoRegistro = {
        password: this.password,
        username: this.username,
        email: this.email,
        checkbox: this.locked

      };

      try {
        await UsersRepository.save(nuevoRegistro)
        this.$emit("userCreate")
      } catch (error) {
        console.log(error)
      }

      this.resetForm();
    },
    resetForm() {
      this.password = '',
        this.username = '',
        this.checkbox = false,
        this.emailUser = ''
    }
  }
};
</script>