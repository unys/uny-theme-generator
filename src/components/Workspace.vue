<template>
  <div>
    <div class="main">
      <div class="content">
        <div class="section">
          <div class="inner is-small">
            <div class="controls">
              <div>
                <label for="colorSet" class="label">Color set</label>
                <label for="colorSet" class="input is-select is-col-full">
                  <select v-model="colorSet" @change="changeColorSet" id="colorSet">
                    <option value="custom">Custom</option>
                    <option value="dark">Dark theme</option>
                    <option value="mint">Mint theme</option>
                  </select>
                </label>
              </div>
              <hr>
              <div class="grid is-col-2">
                <div>
                  <label class="label" for="baseColor">Base color</label>
                  <input v-model="baseColor" class="input is-col-full" type="text" placeholder="#000000" id="baseColor">
                </div>
                <div>
                  <label class="label" for="accentColor">Accent color</label>
                  <input v-model="accentColor" class="input is-col-full" type="text" placeholder="#000000" id="accentColor">
                </div>
                <div>
                  <label class="label" for="secondaryColor">Secondary color</label>
                  <input v-model="secondaryColor" class="input is-col-full" type="text" placeholder="#000000" id="secondaryColor">
                </div>
                <div>
                  <label class="label" for="textColor">Text color</label>
                  <input v-model="textColor" class="input is-col-full" type="text" placeholder="#000000" id="textColor">
                </div>
                <div>
                  <label class="label" for="linkColor">Link color</label>
                  <input v-model="linkColor" class="input is-col-full" type="text" placeholder="#000000" id="linkColor">
                </div>
              </div>
              <hr>
              <div class="unit">
                <button @click="previewTheme" class="button is-white"><i class="fa fa-eye"></i> Preview</button>
                <button @click="createFile" class="button"><i class="fa fa-download"></i> Download</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import hexRgb from 'hex-rgb'
  import FileSaver from 'file-saver'

  export default {
    data () {
      return {
        colorSet: 'custom',
        baseColor: '',
        accentColor: '',
        secondaryColor: '',
        textColor: '',
        linkColor: ''
      }
    },
    methods: {
      changeColorSet () {
        switch (this.colorSet) {
          case 'custom':
            this.baseColor = ''
            this.accentColor = ''
            this.secondaryColor = ''
            this.textColor = ''
            this.linkColor = ''
            break
          case 'dark':
            this.baseColor = '#263238'
            this.accentColor = '#607d8b'
            this.secondaryColor = '#37474f'
            this.textColor = '#f2f2f2'
            this.linkColor = '#1976d2'
            break
          case 'mint':
            this.baseColor = '#009688'
            this.accentColor = '#00897b'
            this.secondaryColor = '#00695c'
            this.textColor = '#f2f2f2'
            this.linkColor = '#00e5ff'
            break
        }
      },
      previewTheme () {
        if (this.baseColor !== '' && this.accentColor !== '' && this.secondaryColor !== '' && this.textColor !== '' && this.linkColor !== '') {
          this.removeStyles()
          this.addStyles()
        } else {
          alert('Please enter a value.')
        }
      },
      getColors () {
        return {
          baseColor: this.baseColor,
          accentColor: this.accentColor,
          secondaryColor: this.secondaryColor,
          textColor: this.textColor,
          linkColor: this.linkColor
        }
      },
      addStyles () {
        const $style = document.createElement('style')
        const rules = document.createTextNode(this.styleTemplate(this.getColors()))

        $style.id = 'custom-styles'

        if ($style.styleSheet) {
          $style.styleSheet.cssText = rules.nodeValue
        } else {
          $style.appendChild(rules)
        }
        document.getElementsByTagName('head')[0].appendChild($style)
      },
      removeStyles () {
        const $style = document.getElementById('custom-styles')
        if ($style !== null) {
          $style.parentNode.removeChild($style)
        }
      },
      convertRGBa (hex, alpha) {
        return `rgba(${hexRgb(hex)[0]}, ${hexRgb(hex)[1]}, ${hexRgb(hex)[2]}, ${alpha})`
      },
      createFile () {
        if (this.baseColor !== '' && this.accentColor !== '' && this.secondaryColor !== '' && this.textColor !== '' && this.linkColor !== '') {
          const blob = new Blob([this.styleTemplate(this.getColors())], {type: 'text/plain;charset=utf-8'})
          FileSaver.saveAs(blob, 'uny-custom.css')
        } else {
          alert('Please enter a value.')
        }
      },
      styleTemplate (colors) {
        return (`/*! UNY CSS Framework @1x | unycss.com | Copyright 2017 kokushin | MIT license */

::-webkit-input-placeholder {
  color: ${this.convertRGBa(colors.textColor, 0.3)};
}

::-moz-placeholder {
  color: ${this.convertRGBa(colors.textColor, 0.3)};
}

:-ms-input-placeholder {
  color: ${this.convertRGBa(colors.textColor, 0.3)};
}

:-moz-placeholder {
  color: ${this.convertRGBa(colors.textColor, 0.3)};
}

a { color: ${colors.linkColor}; }

hr { background-color: ${colors.secondaryColor}; }

body {
  background-color: ${colors.baseColor};
  color: ${colors.textColor};
}

pre:before {
  background-color: ${colors.secondaryColor};
  color: ${colors.textColor};
}

pre code {
  border-color: ${colors.secondaryColor};
  background-color: ${this.convertRGBa(colors.textColor, 0.1)};
  color: ${colors.textColor};
}

code {
  background-color: ${this.convertRGBa(colors.textColor, 0.1)};
}

figure figcaption {
  color: ${colors.textColor};
}

.video .video-caption {
  color: ${colors.textColor};
}

.logo {
  color: ${colors.textColor};
}

.logo a {
  outline-color: ${colors.linkColor};
  color: ${colors.textColor};
}

.button {
  background-color: ${colors.accentColor};
  border-color: ${colors.accentColor};
}

.button.is-burger span, .button.is-close span {
  background-color: ${colors.textColor};
}

.button.is-list {
  background-color: transparent;
  color: ${colors.textColor};
}

.button.is-list + .button.is-list {
  border-color: ${colors.secondaryColor};
}

.button.is-icon {
  color: ${colors.textColor};
}

table th {
  border-color: ${colors.secondaryColor};
}

table tr:nth-child(even) td {
  background-color: ${this.convertRGBa(colors.textColor, 0.1)};
}

.input:not(label) {
  background-color: ${this.convertRGBa(colors.textColor, 0.1)};
  border-color: transparent;
  color: ${colors.textColor};
}

.input:not(label):hover {
  border-color: ${this.convertRGBa(colors.textColor, 0.1)};
}

.input:not(label):focus {
  border-color: ${colors.linkColor};
  -webkit-box-shadow: 0 0 4px ${colors.linkColor};
          box-shadow: 0 0 4px ${colors.linkColor};
}

.input.is-disabled, .input[disabled] {
  color: ${this.convertRGBa(colors.textColor, 0.3)};
  background-color: ${this.convertRGBa(colors.textColor, 0.1)};
  border-color: ${colors.secondaryColor};
}

.input.is-file {
  background-color: ${colors.accentColor};
  border-color: ${colors.accentColor};
  color: ${colors.textColor};
}

.input.is-select {
  background-color: ${colors.accentColor};
}

.input.is-select:after {
  color: ${colors.textColor};
}

.input.is-select > select {
  color: ${colors.textColor};
  border-color: ${colors.accentColor};
}

.input.is-select > select:focus {
  border-color: ${colors.linkColor};
  -webkit-box-shadow: 0 0 4px ${colors.linkColor};
          box-shadow: 0 0 4px ${colors.linkColor};
}

.input.is-checkbox:hover > span:before {
  border-color: ${colors.accentColor};
}

.input.is-checkbox > input[type=checkbox]:checked + span:before {
  background-color: ${colors.accentColor};
  border-color: ${colors.accentColor};
}

.input.is-radio:hover > span:before {
  border-color: ${colors.accentColor};
}

.input.is-radio > input[type=radio]:checked + span:before {
  background-color: ${colors.accentColor};
  border-color: ${colors.accentColor};
}

.input-filename {
  color: ${this.convertRGBa(colors.textColor, 0.5)};
}

.is-brand {
  background-color: ${colors.accentColor};
}

.is-gradation {
  background: -webkit-gradient(linear, right top, left top, from(${colors.baseColor}), to(${colors.accentColor}));
  background: linear-gradient(270deg, ${colors.baseColor}, ${colors.accentColor});
  background-size: 200% 200%;
  -webkit-animation: isGradation 5s ease infinite;
          animation: isGradation 5s ease infinite;
  color: ${colors.textColor};
}

.is-gradation a {
  color: ${colors.textColor};
}

@-webkit-keyframes isGradation {
  0%{
    background-position: 0% 50%;
  }
  50%{
    background-position: 100% 50%;
  }
  100%{
    background-position: 0% 50%;
  }
}

@keyframes isGradation {
  0%{
    background-position: 0% 50%;
  }
  50%{
    background-position: 100% 50%;
  }
  100%{
    background-position: 0% 50%;
  }
}

blockquote {
  border-color: ${colors.secondaryColor};
}

.content {
  background-color: $base-color;
}

.section + .section {
  border-top-color: ${colors.secondaryColor};
}

.section.is-gray {
    background-color: ${colors.secondaryColor};
}

.box.is-border {
  border-color: ${colors.secondaryColor};
}

.header,
.footer {
  background-color: ${colors.secondaryColor};
}

.header.is-sticky {
  background-color: ${colors.secondaryColor};
}

.header-menu a:not(.button) {
  color: ${colors.textColor};
}

.card {
  background-color: ${colors.baseColor};
  border-color: ${colors.accentColor};
}

.card.is-clickable > * {
  color: ${colors.textColor};
}

.card.is-skeleton {
  border-color: ${colors.secondaryColor};
}

.card.is-skeleton > * {
  color: ${this.convertRGBa(colors.textColor, 0.3)};
}

.card.is-dialog .card-header,
  .card.is-dialog .card-content,
  .card.is-dialog .card-footer {
    border-color: ${colors.secondaryColor};
}

.offcanvas .offcanvas-content {
  background-color: ${colors.baseColor};
}

.offcanvas .offcanvas-menu a:not(.button) {
  color: ${colors.textColor};
}

.sidebar:first-child {
  border-color: ${colors.secondaryColor};
}

.sidebar:not(:first-child) {
  border-color: ${colors.secondaryColor};
}

.tab .tab-list {
  border-color: ${colors.secondaryColor};
}

.tab .tab-list a {
  background-color: ${colors.baseColor};
  border-color: ${colors.secondaryColor};
  color: ${colors.textColor};
}

.tab .tab-list a.is-current {
  color: ${colors.linkColor};
}

.tab .tab-content {
  border-color: ${colors.secondaryColor};
}

.navbar a:not(.button) {
  color: ${colors.textColor};
}

.navbar a.is-current {
  background-color: ${colors.accentColor};
}

.tree ul ul,
.tree ul ol,
.tree ol ul,
.tree ol ol {
  border-color: ${colors.secondaryColor};
}

.tree ul > li a, .tree ol > li a {
  color: ${colors.textColor};
}

.tree ul > li.is-current > a, .tree ol > li.is-current > a {
  background-color: ${colors.accentColor};
}

.hero {
  background-color: ${colors.secondaryColor};
}

.field .button {
  color: ${colors.textColor};
  background-color: ${colors.accentColor};
  border-color: ${colors.accentColor};
}

.snackbar {
  background-color: ${colors.accentColor};
}

.pulldown .pulldown-content {
  background-color: ${colors.baseColor};
  border: 1px solid ${colors.secondaryColor};
}

.tabbar {
  background-color: ${colors.accentColor};
}

.tabbar a:not(.button) {
  color: ${colors.textColor};
}

.tabbar .button.is-icon {
  color: ${colors.textColor};
}

.pager {
  border-color: ${colors.secondaryColor};
}

.pager > li > a {
  border-color: ${colors.secondaryColor};
  color: ${colors.textColor};
}

.pager > li > span.is-current {
  border-color: ${colors.accentColor};
  background-color: ${colors.accentColor};
}

.modal .modal-content > .inner {
  background-color: ${colors.baseColor};
}

.modal .modal-content > .card.is-dialog .button {
  color: ${colors.linkColor};
}

.modal .modal-content > .card.is-dialog .button + .button {
  border-color: ${colors.secondaryColor};
}

@media (min-width: 769px) {
  .sidebar:first-child {
    border-color: ${colors.secondaryColor};
  }

  .sidebar:not(:first-child) {
    border-color: ${colors.secondaryColor};
  }
}`)
      }
    }
  }
</script>

<style scoped lang="scss">
  .grid {
    margin-top: -2rem;
  }
  .label {
    margin-top: 0;
  }
</style>
