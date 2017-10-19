<template>
  <div id="app">
    <header class="header is-gradation">
      <h1 class="logo is-small is-center">UNY Theme Generator ✨</h1>
    </header>
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

<style>
  .fa {
    margin-right: .25em;
  }
  .button.is-icon .fa,
  .button.is-round .fa {
    margin-right: 0;
  }
  .header .menu {
    margin-left: auto;
    margin-right: 2.5rem;
  }
  .uny-logo:before {
    content: "Uny";
    font-size: 8rem;
    line-height: 1;
    color: #fff;
    font-weight: 300;
  }
  @media screen and (min-width: 768px) {
    .uny-logo:before {
      font-size: 12.5rem;
    }
  }
</style>
