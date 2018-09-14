<template>

</template>

<script>
  import fs from 'fs'
  import path from 'path'

  export default {
    name: 'test',
    data: () => ({
    }),
    mounted () {
      const p = path.join(this.$electron.remote.app.getPath('appData'), 'elecapp', 'test2.txt')
      fs.writeFile(p, 'qewrt', (err) => {
        if (err) console.log(err)
      })
    },
    methods: {}
  }
</script>

<style scoped>
</style>
