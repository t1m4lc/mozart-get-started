<script setup>
import { ref } from 'vue'
import Step from './components/Step.vue'
import CopyBlock from './components/CopyBlock.vue'

const REPO = 'https://github.com/t1m4lc/mozart-get-started'
const DISCORD = 'https://mozart.build/discord'

// Share targets for the community step
const SHARE_URL = REPO
const SHARE_TEXT =
  'Mozart — a local-first cockpit for running coding agents in parallel. Each agent in its own Git worktree.'
const shareX = `https://twitter.com/intent/tweet?text=${encodeURIComponent(
  SHARE_TEXT,
)}&url=${encodeURIComponent(SHARE_URL)}`
const shareLinkedIn = `https://www.linkedin.com/sharing/share-offsite/?url=${encodeURIComponent(
  SHARE_URL,
)}`

const promptRead =
  'What does the <h2> heading on this page say? Answer in plain text — change nothing.'

const prompt1 =
  'Switch this page to dark mode. Use the violet brand color (#7C3AED) in place of black for accents and highlights. Keep it bold and minimal with strong visual hierarchy. Output only the modified files.'

const prompt2 =
  'Rewrite the README in a fun, chaotic startup voice. Dramatic headers, inside jokes, one totally unnecessary feature list. Keep it markdown.'

// Stepper navigation
const total = 5
const current = ref(1)
const next = () => {
  if (current.value < total) current.value++
}
const prev = () => {
  if (current.value > 1) current.value--
}
const goTo = (n) => {
  current.value = n
}
</script>

<template>
  <div class="page">
    <header class="hero">
      <h1 class="brand">
        <img src="/public/mozart.svg" height="40px"/>
      </h1>
      <h2 class="tagline">Explore the basics in 5 steps.</h2>
    </header>

    <main class="stepper">
      <div class="step-progress">
        <span class="step-count">step {{ current }} / {{ total }}</span>
        <div class="dots" aria-label="Steps">
          <button
            v-for="n in total"
            :key="n"
            class="dot"
            type="button"
            :class="{ active: n === current }"
            :aria-label="`Go to step ${n}`"
            @click="goTo(n)"
          ></button>
        </div>
      </div>

      <Transition name="step-fade" mode="out-in">
        <div class="step-pane" :key="current">
          <Step
            v-if="current === 1"
            number="01"
            label="Ask a read-only question"
            subtitle="Switch to ask mode, then paste and send."
            :has-more="true"
          >
            <CopyBlock :code="promptRead" />
            <template #more>
              Mozart opens your first workspace automatically — an isolated
              <em class="term">sandbox</em> copy of your code. Chats default to
              <em class="term">agent mode</em> (which edits files); switch to
              <em class="term">ask mode</em> to stay read-only — the agent reads
              and answers, but never writes.
            </template>
          </Step>

          <Step
            v-else-if="current === 2"
            number="02"
            label="Open a second chat"
            subtitle="Open a new chat — agent mode — and paste this."
            :has-more="true"
          >
            <CopyBlock :code="prompt1" />
            <template #more>
              A second chat in the same workspace stays in
              <em class="term">agent mode</em> by default, so it makes real edits
              — but only on this workspace's branch, so your main code stays
              untouched until you merge.
            </template>
          </Step>

          <Step
            v-else-if="current === 3"
            number="03"
            label="Open a second workspace"
            subtitle="Spin up a fresh workspace and paste this."
            :has-more="true"
          >
            <CopyBlock :code="prompt2" />
            <template #more>
              While the first agent works, spin up a second workspace for the
              README — separate branch, zero conflicts, all in parallel. Mozart
              is <em class="term">LLM-agnostic</em>, so each workspace can run a
              different model (Claude, GPT, local…).
            </template>
          </Step>

          <Step
            v-else-if="current === 4"
            number="04"
            label="Open a pull request"
            subtitle="Hit the top-right button — Mozart commits and opens the PR."
            :has-more="true"
          >
            <div class="in-app-hint">
              <span class="chip">[ Open PR ]</span>
              <span class="hint-text">→ top-right button in Mozart</span>
            </div>
            <template #more>
              One click auto-<em class="term">commits</em> your changes and opens
              a <em class="term">pull request</em> on GitHub for review — your
              edit proposed, not forced in. Mozart also updates the workspace
              <em class="term">status</em> automatically, so you always see
              what's in review, merged, or still in progress.
            </template>
          </Step>

          <Step
            v-else-if="current === 5"
            number="05"
            label="Join the community"
            subtitle="Star the repo, join Discord, spread the word."
          >
            <div class="btn-row">
              <a class="btn" :href="REPO" target="_blank" rel="noopener">
                [ ★ Star on GitHub ]
              </a>
              <a class="btn" :href="DISCORD" target="_blank" rel="noopener">
                [ Join Discord ]
              </a>
            </div>

            <div class="share-label">Share Mozart</div>
            <div class="btn-row">
              <a class="btn" :href="shareX" target="_blank" rel="noopener">
                [ Share on X ]
              </a>
              <a class="btn" :href="shareLinkedIn" target="_blank" rel="noopener">
                [ Share on LinkedIn ]
              </a>
            </div>
          </Step>
        </div>
      </Transition>

      <div class="step-nav">
        <button
          class="btn nav-btn"
          type="button"
          :disabled="current === 1"
          @click="prev"
        >
          [ ← previous ]
        </button>
        <button
          class="btn btn-primary nav-btn"
          type="button"
          :disabled="current === total"
          @click="next"
        >
          [ next → ]
        </button>
      </div>
    </main>

    <footer class="foot">
      <span>local-first</span><span>·</span><span>llm-agnostic</span><span>·</span><span>parallel agents</span>
      <span>·</span>
      <a class="foot-link" href="https://mozart.build" target="_blank" rel="noopener">mozart.build</a>
      <span>·</span>
      <a class="foot-link" :href="REPO" target="_blank" rel="noopener">GitHub</a>
    </footer>
  </div>
</template>
