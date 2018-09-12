<template>
  <v-layout row wrap>
    <v-flex xs12 sm4>
      <v-chip color="red" text-color="white">
        {{msg1}}
      </v-chip>
    </v-flex>
    <v-flex xs12 sm4>
      <v-chip color="orange" text-color="white">
        {{msg2}}
      </v-chip>
    </v-flex>
    <v-flex xs12 sm4>
      <v-chip color="orange" text-color="white">
        {{msg3}}
      </v-chip>
    </v-flex>
    <v-flex xs12>
    	<v-card>
    	  <v-card-title>
    	    {{fileContent}}
    	  </v-card-title>
    	  <v-card-actions>
    	    <v-btn color="primary" @click="dialogOpen">
    	      <v-icon>Save</v-icon>
    	      File Select
    	    </v-btn>
    	  </v-card-actions>
    	</v-card>
    </v-flex>
  </v-layout>
</template>

<script>
  export default {
    name: 'test',
    data: () => ({
      msg1: 'vuetify test',
      msg2: 'vue test',
      msg3: 'electron tset',
      fileContent: 'See the file'
    }),
    methods: {
      dialogOpen () {
        this.fileContent = 'Did you see the file?'
      }
    }
  }
</script>

<style scoped>
</style>
