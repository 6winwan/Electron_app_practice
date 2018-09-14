<template>
  <v-layout row wrap >
    <v-flex xs12 sm6>
      <v-card>
        <v-card-title>New Card</v-card-title>
        <v-card-text>
          <v-form ref="form" v-model="valid">
            <v-text-field
              v-model="form.name"
              :rules="rule.name"
              :counter="10"
              label="Name"
              required
            ></v-text-field>
            <v-text-field
              v-model="form.email"
              :rules="rule.email"
              :counter="100"
              label="E-mail"
              required
            ></v-text-field>
            <v-btn 
              color="priamry"
              :disabled="!valid"
              @click="formSubmit"
            >
              <v-icon left>save</v-icon>
              Resister
            </v-btn>
            <v-btn @click="formClear">
              <v-icon left>undo</v-icon>
              Renew
            </v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-flex>
    <v-flex xs12 sm6>
      <v-card>
        <v-card-title>Check DB Card</v-card-title>
        <v-card-text>
          {{content}}
        </v-card-text>
        <v-card-actions>
          <v-btn color="success" @click="read">
          <v-icon left>attachment</v-icon>
          Load File
          </v-btn>
          <v-btn color="error" @click=remove>
            <v-icon left>clear</v-icon>
              Remove All
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
    <v-snackbar
      :timeout="snackbar.timeOut"
      top
      v-model="snackbar.act"
      :color="snackbar.color"
    >
      {{ snackbar.text }}
      <v-spacer></v-spacer>
      <v-btn flat color="white" @click.native="snackbar.act = false">Close</v-btn>
    </v-snackbar>
  </v-layout>
</template>

<script>
  export default {
    name: 'testFile',
    data: () => ({
      snackbar: {
        act: false,
        text: '',
        color: 'success',
        timeOut: 5000
      },
      valid: true,
      form: {
        name: '',
        email: '',
        rmk: ''
      },
      rule: {
        name: [
          v => !!v || 'Please fill in your name.',
          v => (v && v.length <= 10) || 'Maximum 10 letters.'
        ],
        email: [
          v => !!v || 'Please fill in your E-mail',
          v => (v && v.length <= 100) || 'Maximum 100 letters',
          v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'It is not a vaild email'
        ]
      },
      content: ''
    }),
    mounted () {
    },
    methods: {
      pop (msg, cl, t) {
        this.snackbar.act = true
        this.snackbar.text = msg
        this.snackbar.color = cl
        this.snackbar.timeOut = t
      },
      formSubmit (e) {
        if (!this.$refs.form.validate()) return this.pop('Not valid', 'error', 60000)
        this.$db.insert(this.form, (err) => {
          if (err) return this.pop(err.message, 'error', 60000)
          this.pop('Save Complete', 'success', 5000)
          this.formClear()
        })
      },
      formClear () {
        this.$refs.form.reset()
      },
      read () {
        this.$db.find({}, (err, r) => {
          if (err) return this.pop(err.message, 'error', 60000)
          this.content = r
        })
      },
      remove () {
        this.$db.remove({}, { multi: true }, (err) => {
          if (err) return this.pop(err.message, 'error', 60000)
          this.pop('Delete all complete', 'suceess', 5000)
        })
      }
    }
  }
</script>

<style scoped>
</style>