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
      } else if (cmd === 'exit' || cmd === 'Exit') {
        return this.exit()
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
    exit: function () {
      return [
        'work in progress'
      ]
    },
    whoami: function () {
      return [
        `Hey there, friend!`,
        ' ',
        `I'm Joseph – a software engineer on a mission to make tech waves! 🌊 With about 5 years under my belt, I've been diving into Full-stack development, focusing on the backend using cool tools like PHP, Laravel, JavaScript, and MySQL. But hey, I'm no one-trick pony! I'm always hungry to learn and have been tinkering with Python, Django, NodeJS, Express.js, ReactJS, VueJS, jQuery, Docker, and AWS. Phew, that's quite a list, huh?`,
        ' ',
        `Lately, I've been captivated by Blockchain and Artificial Intelligence – they're like the superheroes of the tech world, aren't they?`,
        ' ',
        `My strength? I'd say it's my thirst for learning and how I adapt to any coding habitat. But, like everyone, I've got quirks. Distractions can throw me off, so I've mastered the art of taking short breaks. Oh, and when I'm not deep in code, you'll catch me cruising up mountain roads for a quick recharge.`,
        ' ',
        `In a nutshell, I'm a tech enthusiast with a toolkit filled with diverse skills, a passion for learning that never fizzles out, and a drive to use tech to make life better. Let's team up and craft something that'll have a positive impact worldwide! 🚀`
      ]
    },
    cat: function (cmd) {
      if (cmd.includes('portfolio.md')) {
        return [
          'work in progress'
        ]
      } else if (cmd.includes('skills.json')) {
        return [
          '{',
          `   <i class="yellow">Programming Languages</i>: [ '<i class="purple">PHP</i>', '<i class="purple">JavaScript</i>', '<i class="purple">Python</i>', '<i class="purple">C</i>' ],`,

          `   <i class="yellow">Frameworks and Libraries</i>: [ '<i class="purple">Laravel</i>', '<i class="purple">NodeJS</i>', '<i class="purple">ExpressJS</i>', '<i class="purple">Django</i>', '<i class="purple">Django Rest Framework</i>', '<i class="purple">ReactJS</i>', '<i class="purple">VueJS</i>' ],`,

          `   <i class="yellow">Tools</i>: [ '<i class="purple">Jira</i>', '<i class="purple">Gitlab</i>', '<i class="purple">Git</i>', '<i class="purple">GitHub</i>', '<i class="purple">Bitbucket</i>', '<i class="purple">Postman</i>', '<i class="purple">Docker</i>', '<i class="purple">AWS</i>' ],`,
          

          `   <i class="yellow">Operating Systems</i>: [ '<i class="purple">MacOs</i>', '<i class="purple">Linux</i>', '<i class="purple">Windows</i>' ]`,
          '}'
        ]
      } else if (cmd.includes('contact.md')) {
        return [
          '<a href="mailto:josephreuben.bautista@gmail.com">josephreuben.bautista@gmail.com</a>',
          '<a href="https://github.com/joseph-bautista" target="blank">github</a>',
          '<a href="https://twitter.com/jayrbautista26" target="_blank">twitter</a>',
          '<a href="https://www.linkedin.com/in/joseph-bautista" target="_blank">linkedin</a>',
          '<a href="https://joseph-bautista.hashnode.dev" target="_blank">blog</a>',
          '<a href="https://www.youtube.com/@joseph-bautista" target="_blank">youtube</a>',
          '<a href="https://www.instagram.com/jayrbautista26" target="_blank">instagram</a>',
        ]
      } else if (cmd.includes('.secret')) {
        if (this.user !== 'guest') {
          return [
            '<img src="https://statuscage.com/404" alt="Not Found Image" style="max-width:4200px; max-height: 400px;" />'
          ]
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
        return [
          `logged in as <i class="user">${this.user}</i>.`,
          
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
        '<i class="yellow">exit</i> \t\t <i class="grey">to exit command-line interface</i>',
      ]
    },
  }
}
</script>
<style lang="scss">
@import '../assets/colors';
</style>
