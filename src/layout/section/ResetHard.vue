<template>
  <div class="card bg-dark" style="min-height: 400px">
    <h5 class="card-header text-white">Git | Hard reset </h5>
    <div class="card-body bash-console text-white">
      <p class="card-text">Points HEAD to the specified commit</p>
      <p class="card-text">Discard changes that have been made since the new commit that <strong>HEAD</strong>
        points to, and deletes changes in working copy</p>
      <div class="d-flex git-command-body">
        <div class="git-state-screen">
          <div class="git-pointer-screen">
            <empty-space :count="3"></empty-space>
            <git-transition :offset="180">
              <git-head ref="head" v-show="!complete">Head</git-head>
            </git-transition>
          </div>
          <div class="git-commit-screen">
            <git-commit-box :hasArrow="false">035cc</git-commit-box>
            <git-commit-box>9e78i</git-commit-box>
            <slide-y-down-transition :duration="500">
              <div v-show="!complete">
                <git-commit-box>ec5be</git-commit-box>
                <git-commit-box>i8fd3</git-commit-box>
              </div>
            </slide-y-down-transition>
          </div>
        </div>
        <git-bash style="min-height: 180px">
          <bash-command v-if="step[0]" :command="command[0]" @onComplete="next" :hasNext="true"/>
          <bash-command v-if="step[1]" :command="command[1]" @onComplete="next" :hasNext="true">
            <p class="mb-0">Nothing to commit</p>
          </bash-command>
          <bash-command v-if="step[2]" @onComplete="restartReset"/>
        </git-bash>
      </div>
    </div>
  </div>
</template>

<script>
import { SlideYDownTransition } from 'vue2-transitions'

export default {
  name: 'ResetHard',
  components: {
    SlideYDownTransition
  },
  data () {
    return {
      command: ['git reset --hard HEAD~2', 'git status'],
      step: [],
      next: () => this.step.push(true)
    }
  },
  methods: {
    startReset () {
      this._.delay(() => {
        this.step.push(true)
      }, 1000)
    },
    restartReset (delay = 1000) {
      this._.delay(() => {
        this.step.splice(0, this.step.length)
        this.startReset()
      }, delay)
    }
  },
  computed: {
    complete () {
      return this.step[1] === true
    }
  },
  mounted () {
    this.startReset()
  }
}
</script>

<style scoped>

</style>
