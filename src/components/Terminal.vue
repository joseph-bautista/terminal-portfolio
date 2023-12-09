/* eslint-disable */
<template>
  <div class="terminal" ref="terminal">
    <div class="terminal"></div>
    <ul>
      <CommandLine v-for="(command, index) in commands" :user="command[0]" :domain="domain" :dir="dir" :cmd="command[1]"
        :key="command[1] + index" />
    </ul>
    <Prompt :user="user" :domain="domain" :dir="dir" ref="prompt" />

  </div>
</template>

<script>
import CommandLine from './CommandLine.vue'
import Prompt from './Prompt.vue'
import axios from 'axios'

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Terminal',
  components: {
    CommandLine,
    Prompt
  },
  data: function () {
    return {
      user: 'guest',
      domain: 'josephbautista.com',
      dir: '~',
      advice: '',
      commands: [
        ['root', 'welcome']
      ]
    }
  },
  mounted: function () {
    this.getAdvice()
    this.$refs.prompt.focus()
  },
  updated: function () {
  },
  methods: {
    addCommand: function (user, cmd) {
      this.commands.push([user, cmd])
    },
    run: function (user, cmd) {
      if (cmd === 'welcome') {
        return this.welcome(user)
      } else if (cmd === 'whoami' || cmd === 'Whoami') {
        return this.whoami()
      } else if (cmd === 'ls' || cmd === 'ls -a') {
        return this.ls(cmd)
      } else if (cmd === 'clear' || cmd === 'Clear') {
        return this.clear()
      } else if (cmd === 'cat' || cmd.startsWith('cat ')) {
        return this.cat(cmd)
      } else if (cmd === 'su' || cmd.startsWith('su ')) {
        return this.su(cmd)
      } else if (cmd === 'help' || cmd === 'Help') {
        return this.help()
      } else if (cmd !== '') {
        return this.nope(cmd)
      }
    },
    getAdvice: function () {
      let vm = this
      axios.get('https://api.adviceslip.com/advice')
        .then(function (response) {
          vm.$nextTick(function () {
            vm.advice = response.data.slip.advice
          })
        })
    },
    welcome: function () {
      // if (user === 'root') {
        const currentDate = new Date().toLocaleString('en-US', { timeZoneName: 'short' })
        return [
          '<span class="ascii">···················································</span>',
          '<span class="ascii">:      #                                          :</span>',
          '<span class="ascii">:      #  ####   ####  ###### #####  #    #       :</span>',
          '<span class="ascii">:      # #    # #      #      #    # #    #       :</span>',
          '<span class="ascii">:      # #    #  ####  #####  #    # ######       :</span>',
          '<span class="ascii">:#     # #    #      # #      #####  #    #       :</span>',
          '<span class="ascii">:#     # #    # #    # #      #      #    #       :</span>',
          '<span class="ascii">: #####   ####   ####  ###### #      #    #       :</span>',
          '<span class="ascii">:                                                 :</span>',
          '<span class="ascii">:######                                           :</span>',
          '<span class="ascii">:#     #   ##   #    # ##### #  ####  #####   ##  :</span>',
          '<span class="ascii">:#     #  #  #  #    #   #   # #        #    #  # :</span>',
          '<span class="ascii">:######  #    # #    #   #   #  ####    #   #    #:</span>',
          '<span class="ascii">:#     # ###### #    #   #   #      #   #   ######:</span>',
          '<span class="ascii">:#     # #    # #    #   #   # #    #   #   #    #:</span>',
          '<span class="ascii">:######  #    #  ####    #   #  ####    #   #    #:</span>',
          '<span class="ascii">···················································</span>',
          `<i class="grey">Last login: ${currentDate}</i>`,
          ' ',
          `Hi <i class="user">${this.user}</i>, I'm <i class="pink" style="font-size:19px">Joseph Bautista.👋</i>`,
          ' ',
          `I am a senior software engineer based in the Philippines. I am a passionate professional, willing to have a go-to for everything related to software development technologies, hence, I am a lifelong learner. When I'm not coding, I do short rides up in the mountains.`,
          ' ',
          'Type <i class="teal">`help`</i> to get a list of commands'

        ]
      // }
    },
    ls: function (cmd) {
      if (!cmd.includes(' -a')) {
        return ['portfolio.md \t skills.json \t contact.md'] // change these options
      } else {
        return ['portfolio.md \t skills.json \t contact.md \t <i class="yellow">.secret</i>']
      }
    },
    cat: function (cmd) {
      if (cmd.includes('portfolio.md')) {
        return [
          ' work in progress'
        ]
      } else if (cmd.includes('skills.json')) {
        return [
          '{',
          `   <i class="purple">frontEnd</i>: [ '<i class="blue">javascript</i>', '<i class="blue">vuejs</i>', '<i class="blue">typescript</i>', '<i class="blue">react</i>', '<i class="blue">scss</i>', '<i class="blue">sass</i>' ],`,
          `   <i class="purple">backEnd</i>: [ '<i class="blue">python</i>', '<i class="blue">java</i>', '<i class="blue">node.js</i>', '<i class="blue">oracle sql</i>', '<i class="blue">vite</i>', '<i class="blue">c++</i>', '<i class="blue">c#</i>', '<i class="blue">pandas</i>', '<i class="blue">matplotlib</i>' ],`,
          `   <i class="purple">tools</i>: [ '<i class="blue">jira</i>', '<i class="blue">gitlab</i>', '<i class="blue">git</i>' ],`,
          `   <i class="purple">design</i>: [ '<i class="blue">figma</i>' ],`,
          `   <i class="purple">misc</i>: [ '<i class="blue">jsx</i>', '<i class="blue">tsx</i>' ]`,
          '}'
        ]
      } else if (cmd.includes('contact.md')) {
        ' '
        return [
        ]
      } else if (cmd.includes('.secret')) {
        if (this.user !== 'guest') {
          return [
            '<img src="https://statuscage.com/404" alt="Not Found Image" style="max-width:4200px; max-height: 400px;" />'
          ] // secwettttt
        } else {
          return [
            'cat: .secret: Permission denied for user <i class="user">guest</i>',
            'Type `<i class="yellow">su</i> <i class="light">USERNAME</i>` to login...'
          ]
        }
      } else {
        return [`${cmd}: No such file or directory`]
      }
    },
    su: function (cmd) {
      if (cmd === 'su' || cmd.includes('root')) {
        return [
          'su: Authentication failed. root is now disabled on this machine',
          'Log in with your personal username.'
        ]
      } else if (cmd.startsWith('su ') && cmd.length > 3) {
        this.user = cmd.replace('su ', '')
        const currentDate = new Date().toLocaleString('en-US', { timeZoneName: 'short' })
        return [
          `logged in as <i class="user">${this.user}</i>.`,
          '<span class="ascii">···················································</span>',
          '<span class="ascii">:      #                                          :</span>',
          '<span class="ascii">:      #  ####   ####  ###### #####  #    #       :</span>',
          '<span class="ascii">:      # #    # #      #      #    # #    #       :</span>',
          '<span class="ascii">:      # #    #  ####  #####  #    # ######       :</span>',
          '<span class="ascii">:#     # #    #      # #      #####  #    #       :</span>',
          '<span class="ascii">:#     # #    # #    # #      #      #    #       :</span>',
          '<span class="ascii">: #####   ####   ####  ###### #      #    #       :</span>',
          '<span class="ascii">:                                                 :</span>',
          '<span class="ascii">:######                                           :</span>',
          '<span class="ascii">:#     #   ##   #    # ##### #  ####  #####   ##  :</span>',
          '<span class="ascii">:#     #  #  #  #    #   #   # #        #    #  # :</span>',
          '<span class="ascii">:######  #    # #    #   #   #  ####    #   #    #:</span>',
          '<span class="ascii">:#     # ###### #    #   #   #      #   #   ######:</span>',
          '<span class="ascii">:#     # #    # #    #   #   # #    #   #   #    #:</span>',
          '<span class="ascii">:######  #    #  ####    #   #  ####    #   #    #:</span>',
          '<span class="ascii">···················································</span>',
          `<i class="grey">Last login: ${currentDate}</i>`,
          ' ',
          `Hi <i class="user">${this.user}</i>, I'm <i class="pink" style="font-size:19px">Joseph Bautista.👋</i>`,
          ' ',
          `I am a senior software engineer based in the Philippines. I am a passionate professional, willing to have a go-to for everything related to software development technologies, hence, I am a lifelong learner. When I'm not coding, I do short rides up in the mountains.`,
          ' ',
          'Type <i class="teal">`help`</i> to get a list of commands'
        ]        
      } else {
        return ['su...who goes there? 🧐']
      }
    },
    clear: function () {
      this.commands = []
    },
    help: function () {
      return [
        'Use the commands below to get around:',
        '<i class="yellow">su</i><i class="light"> USERNAME</i> \t <i class="grey">log in</i>',
        '<i class="yellow">ls</i> \t\t <i class="grey">show files in directory</i>',
        '<i class="yellow">whoami</i> \t\t <i class="grey">learn more about me</i>',
        '<i class="yellow">cat</i> <i class="light">FILENAME</i> \t <i class="grey">show filename content</i>',
        '<i class="yellow">clear</i> \t\t <i class="grey">clear current screen</i>',
      ]
    }
  }
}
</script>
<style lang="scss">
@import '../assets/colors';
</style>
