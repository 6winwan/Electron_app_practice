<template>
  <v-layout row wrap>
    <v-flex xs12 sm6>
      <v-card>
        <v-card-title>title</v-card-title>
        <v-card-text>
          <v-text-field
            id="testing"
            name="input-1"
            label="Content of file"
            v-model="test1.text"
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary"  @click="test1save">
            <v-icon left>save</v-icon>
            File save
         </v-btn>
          <v-btn color="warning"  @click="test1remove">
            <v-icon left>remove</v-icon>
            File remove
         </v-btn>
        </v-card-actions>
      </v-card>     
    </v-flex>
    <v-flex xs12 sm6>
      <v-card>
        <v-card-title>test1 Read</v-card-title>
        <v-card-text>
          {{test1.read}}
        </v-card-text>
        <v-card-actions>
          <v-btn color="success" @click="test1read">
            <v-icon left>attachment</v-icon>
            Read File 
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
  import fs from 'fs'
  import path from 'path'

  export default {
    name: 'testFile',
    data: () => ({
      snackbar: {
        act: false,
        text: '',
        color: 'success',
        timeOut: 5000
      },
      test1: {
        text: '1234asdf',
        read: ''
      },
      appPath: ''
    }),
    mounted () {
      this.appPath = path.join(this.$electron.remote.app.getPath('appData'), 'elecapp', 'test1.txt')
    },
    methods: {
      pop (msg, cl, t) {
        this.snackbar.act = true
        this.snackbar.text = msg
        this.snackbar.color = cl
        this.snackbar.timeOut = t
      },
      test1save () {
        fs.writeFile(this.appPath, this.test1.text, (err) => {
          if (err) return this.pop(err.message, 'error', 60000)
          this.pop('Write Complete', 'success', 3000)
        })
      },
      test1remove () {
        fs.unlink(this.appPath, (err) => {
          if (err) return this.pop(err.message, 'error', 60000)
          this.pop('Remove Complete', 'success', 3000)
        })
      },
      test1read () {
        fs.readFile(this.appPath, (err, r) => {
          if (err) return this.pop(err.message, 'error', 60000)
          this.test1.read = r
          this.pop('Read Complete', 'success', 3000)
        })
      }
    }
  }
</script>

<style scoped>
</style>

