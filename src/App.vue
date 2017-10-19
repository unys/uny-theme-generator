<template>
  <div id="app">
    <Workspace />
    <Preview />
  </div>
</template>

<script>
import Workspace from './components/Workspace'
import Preview from './components/Preview'

export default {
  name: 'app',
  components: {
    Workspace,
    Preview
  }
}
</script>

<style scoped lang="scss">

</style>
