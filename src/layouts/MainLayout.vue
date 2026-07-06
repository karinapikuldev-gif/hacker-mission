<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <q-page class="q-pa-xl">

        <q-card flat bordered class="rounded-borders">

          <div class="row items-center justify-between q-pa-md">

            <q-chip
              color="amber-8"
              text-color="white"
              class="text-h6"
            >
              Die sechs Missionen
            </q-chip>

 <q-chip
  color="deep-orange"
  text-color="white"
  class="text-h6"
>
  Informatik entdecken
</q-chip>

          </div>

          <q-stepper
            v-model="step"
            flat
            animated
            header-nav
            alternative-labels
            color="primary"
          >

<q-step :name="1" title="1. Sortieralgorithmen" icon="swap_horiz">

  <div class="text-center text-h6 q-mb-md">
    🔢 Sortiere nach Größe
  </div>

  <div class="text-center q-mb-md">
    Sortiere die Personen nach Größe (cm), klein → groß
  </div>

  <!-- SORTABLE CONTAINER -->
<div
  ref="listRef"
  class="no-wrap row justify-center q-gutter-sm q-mb-lg"
  style="overflow-x: auto;"
>
    <q-card
      v-for="child in dragList"
      :key="child.name"
      class="q-pa-sm text-center cursor-move mini-card"
    >

      <!-- simple visual bar -->

      <div class="text-subtitle2 q-mt-sm">
        {{ child.name }}
      </div>

      <div class="text-caption text-grey">
        {{ child.value }} cm
      </div>

    </q-card>
  </div>

<div class="row justify-between q-mt-md">

  <q-btn
    color="primary"
    label="Prüfen"
    @click="checkDragOrder"
  />

  <q-btn
    color="positive"
    label="Weiter"
    :disable="!sortedCorrect"
    @click="step = 2"
  />

</div>

  <q-banner v-if="sortedCorrect" class="bg-green-2 text-green-10 q-mt-md">
    ✅ Richtig sortiert!
  </q-banner>

  <q-banner v-if="error" class="bg-red-2 text-red-10 q-mt-md">
    ❌ Falsche Reihenfolge
  </q-banner>


</q-step>

      <q-step
        :name="2"
        title="2. Logikgatter"
        icon="account_tree"
      >
        <div class="text-center text-h6 q-mb-lg">
          Der Computer startet nur,
          wenn beide Schalter eingeschaltet sind.
        </div>

        <div class="row justify-around q-my-xl">

          <q-toggle
            v-model="switchA"
            label="Schalter A"
            size="xl"
          />

          <q-toggle
            v-model="switchB"
            label="Schalter B"
            size="xl"
          />

        </div>

        <div class="text-center q-my-lg">

          <q-icon
            :name="lampOn ? 'lightbulb' : 'lightbulb_outline'"
            :color="lampOn ? 'amber' : 'grey'"
            size="90px"
          />

          <div class="text-h6">
            {{ lampOn ? '💡 Lampe AN' : 'Lampe AUS' }}
          </div>

        </div>

        <q-banner
          v-if="lampOn"
          rounded
          class="bg-green-2 text-green-10"
        >
          ✅ Super! Das ist ein <b>UND-Gatter</b>.
          <br>
        </q-banner>

        <q-stepper-navigation>
          <q-btn
            color="primary"
            label="Weiter"
            :disable="!lampOn"
            @click="step = 3"
          />
        </q-stepper-navigation>

            </q-step>

            <!-- STEP 3 -->
            <q-step :name="3" title="3. Kryptographie" icon="lock">

              <div class="text-center text-h6 q-mb-md">
                🕵️ Entschlüssle die geheime Nachricht
              </div>

              <q-banner class="bg-black text-green-4 q-mb-md">
                Geheimcode: <b>URERW</b>
              </q-banner>

              <!-- ALPHABET HINT -->
              <q-card flat bordered class="q-pa-md q-mb-md bg-blue-1">

                <div class="text-center text-subtitle2 q-mb-sm">
                  🔤 Caesar-Chiffre Hilfe
                </div>

                <div class="text-center text-body2"
                    style="letter-spacing:4px; display:flex; flex-direction:column; align-items:center;">
                  <div>ABCDEFGHIJKLM</div>
                  <div>NOPQRSTUVWXYZ</div>
                </div>

                <q-separator class="q-my-sm" />

                <div class="text-center text-caption">
                  Jeder Buchstabe wurde um <b>3 Stellen nach rechts</b> verschoben.
                  <br>
                  Zum Entschlüsseln gehst du <b>3 zurück</b>.
                </div>

              </q-card>

              <q-banner class="bg-blue-2 text-blue-10 q-mb-md">
                💡 Tipp: A → D, B → E, C → F ...
              </q-banner>

              <q-input v-model="answer" label="Deine Lösung" />
<div class="row q-mt-md q-gutter-sm">
  <q-btn
    color="primary"
    label="Prüfen"
    @click="check"
  />

  <q-btn
    color="primary"
    label="Weiter"
    :disable="!correct"
    @click="step = 4"
  />
</div>

<q-banner v-if="checked && correct" class="bg-green-2 text-green-10 q-mt-md">
  ✅ Richtig! Passwort: <b>ROBOT</b>
</q-banner>

<q-banner v-if="checked && !correct" class="bg-red-2 text-red-10 q-mt-md">
  ❌ Leider falsch, versuche es nochmal.
</q-banner>


            </q-step>
            <!-- STEP 4 FIXED -->
<q-step :name="4" title="4. Next Token Prediction" icon="smart_toy">

  <div class="text-center text-h6 q-mb-md">
    🤖 Welche Wahl ist am wahrscheinlichsten?
  </div>

  <q-banner class="bg-blue-2 text-blue-10 q-mb-lg" rounded>
    Satzanfang: <b>Der Hacker öffnet das ...</b>
  </q-banner>

  <div class="column items-center q-gutter-sm">

    <q-btn
      v-for="o in optionsWithProb"
      :key="o.word"
      :label="`${o.word} (${o.prob}%)`"
      color="primary"
      outline
      class="full-width"
      style="max-width: 300px"
      @click="selectWord(o.word)"
    />

  </div>

  <q-banner
    v-if="selected"
    class="q-mt-md"
    :class="selected === correctNext
      ? 'bg-green-2 text-green-10'
      : 'bg-orange-2 text-orange-10'"
  >
    <div v-if="selected === correctNext">
      ✅ Richtig! Das ist das wahrscheinlichste Token.
    </div>
    <div v-else>
      ❌ Nicht optimal. KI wählt statistisch das wahrscheinlichste Wort.
    </div>
  </q-banner>

  <q-stepper-navigation>
    <q-btn
      color="primary"
      label="Weiter"
      :disable="!selected"
      @click="step = 5"
    />
  </q-stepper-navigation>

</q-step>

<q-step :name="5" title="5. Binärcode" icon="lightbulb">

  <div class="text-center text-h6 q-mb-md">
    🤖 Entschlüssle den Binärcode
  </div>

  <!-- PUZZLE CODE -->
  <q-banner class="bg-black text-green-4 q-mb-md">
    <b>Binärcode:</b>
    <div style="font-family: monospace; font-size: 18px; margin-top: 8px;">
      {{ binaryCode }}
    </div>
  </q-banner>



  <!-- 🧠 HELPER (DEIN WUNSCH) -->
  <div class="text-center text-h5 q-mb-md" style="font-family: monospace;">
    {{ binary }} → {{ preview }}
  </div>

  <div class="row justify-center q-gutter-md q-mb-lg">
    <q-toggle v-model="b1" label="1" />
    <q-toggle v-model="b2" label="2" />
    <q-toggle v-model="b3" label="3" />
    <q-toggle v-model="b4" label="4" />
  </div>


  <q-input
    v-model="userAnswer"
    label="Dein entschlüsseltes Wort"
    filled
  />
  
      <q-banner class="bg-blue-2 text-blue-10 q-mt-md">
    💡 Tipp: Jeder 4-Bit Block = 1 Buchstabe
  </q-banner>


  <q-banner
    v-if="decodedCorrect"
    class="bg-green-2 text-green-10 q-mt-md"
  >
    ✅ Richtig entschlüsselt!
  </q-banner>

  <div class="row justify-between q-mt-md">

  <q-btn
    color="primary"
    label="Prüfen"
    @click="checkBinary"
  />

  <q-btn
    color="positive"
    label="Weiter"
    :disable="!decodedCorrect"
    @click="step = 6"
  />

</div>

</q-step>
<!-- STEP 6 -->
 <q-step :name="6" title="Pseudocode" icon="route">

              <div class="text-center text-h6 q-mb-md">
                🕵️ Finde den Hacker im System
              </div>

              <q-banner class="bg-blue-1 text-blue-10 q-mb-md">
                Du bist ein Geheimagent im System. Der Hacker hat Spuren im Netzwerk hinterlassen. 
                Schreibe Befehle, um seinen Weg zu verfolgen.
              </q-banner>

              <!-- Controls -->
              <div class="q-mb-md">
   <div
  v-for="(cmd, i) in commandList"
  :key="i"
  class="row items-center q-gutter-sm q-mb-sm"
>
  <q-select
    v-model="cmd.dir"
    :options="['right','left','up','down']"
    label="Richtung"
    dense
    style="width: 120px"
  />

  <q-input
    v-model.number="cmd.num"
    type="number"
    label="Schritte"
    dense
    style="width: 100px"
  />

  <q-space />

  <q-btn
    icon="delete"
    flat
    color="negative"
    @click="removeCmd(i)"
  />
</div>

                <q-btn color="primary" icon="add" label="Schritt" @click="addCmd" />
              </div>

              <!-- ACTIONS -->
              <div class="row q-gutter-sm q-mb-md">
                <q-btn color="primary" label="Start" @click="runProgram" />
                <q-btn color="negative" label="Reset" @click="reset" />
              </div>

              <!-- STATUS -->
              <q-banner v-if="success" class="bg-green-2 text-green-10">
                🎉 Ziel erreicht!
              </q-banner>

              <q-banner v-if="failed" class="bg-red-2 text-red-10 explosion-banner">
                💥 BOOM! Fehler im Code
              </q-banner>

              <!-- GRID -->
              <div class="labyrinth">
                <div v-for="(row, y) in grid" :key="y" class="row">

                  <div
                    v-for="(cell, x) in row"
                    :key="x"
                    class="cell"
                    :class="getCellClass(x, y)"
                  >
                    <span v-if="player.x === x && player.y === y">🕵️‍♂️</span>
                    <span v-else-if="cell === 3">👤</span>
                  </div>

                </div>
              </div>

            </q-step>


          </q-stepper>

        </q-card>

      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import { onMounted, nextTick } from 'vue'
import Sortable from 'sortablejs'

const step = ref(1)

const listRef = ref(null)

onMounted(() => {
  Sortable.create(listRef.value, {
    animation: 150,

    onEnd(evt) {
      const movedItem = dragList.value.splice(evt.oldIndex, 1)[0]
      dragList.value.splice(evt.newIndex, 0, movedItem)
    }
  })
})

const sortedCorrect = ref(false)
const error = ref(false)

const dragList = ref([
  { name: "Anna",  value: 120 },
  { name: "Ben",  value: 110 },
  { name: "Chris",  value: 130 },
  { name: "Lena",  value: 125 },
])

function checkDragOrder () {
  const correct = [...dragList.value].sort((a, b) => a.value - b.value)

  sortedCorrect.value = dragList.value.every(
    (c, i) => c.value === correct[i].value
  )

  error.value = !sortedCorrect.value
}

const switchA = ref(false)
const switchB = ref(false)

const lampOn = computed(() => switchA.value && switchB.value)

const answer = ref('')

// richtige Lösung (für "URERW" mit Shift -3)
const solution = 'ROBOT'

// Prüfung
const correct = ref(false)
const checked = ref(false)

function check () {
  checked.value = true
  correct.value = answer.value.trim().toUpperCase() === solution
}

const optionsWithProb = [
  { word: 'Terminal', prob: 17 },
  { word: 'Programm', prob: 13 },
  { word: 'System', prob: 66 },
  { word: 'Fenster', prob: 4 }
]

const selected = ref('')

function selectWord(word) {
  selected.value = word
}

const correctNext = 'System'

// --------------------
// STEP 5: BINARY RIDDLE
// --------------------

const b1 = ref(false)
const b2 = ref(false)
const b3 = ref(false)
const b4 = ref(false)

const binary = computed(() =>
  [b1, b2, b3, b4].map(b => b.value ? "1" : "0").join("")
)

const preview = computed(() =>
  symbolMap[binary.value] || "?"
)

// Binär → Buchstaben Mapping
const symbolMap = {
  "0001": "A",
  "0010": "B",
  "0011": "C",
  "0100": "D",
  "0101": "E",
  "0110": "F",
  "0111": "G",
  "1000": "H",
  "1001": "I",
  "1010": "J",
  "1011": "K",
  "1100": "L",
  "1101": "M",
  "1110": "N",
  "1111": "O"
}

// 👉 DAS ist dein Rätsel (CODE = Beispiel)
const binaryCode = "0011 1111 0100 0101"

// User Input
const userAnswer = ref('')
const decodedCorrect = ref(false)

// Check Function
function checkBinary () {
  const chunks = binaryCode.replace(/\s+/g, '').match(/.{1,4}/g)

  const decoded = chunks
    .map(b => symbolMap[b] || '?')
    .join('')

  decodedCorrect.value =
    userAnswer.value.trim().toUpperCase() === decoded
}
const choice = ref('')


/* STEP 6 STATE */

/* ================= STATE ================= */
const grid = [
  [1,0,0,0,0,0,0],
  [1,1,1,1,0,0,0],
  [0,0,0,1,1,1,0],
  [0,0,0,0,0,1,0],
  [0,0,0,0,0,1,0],
  [0,0,0,0,1,1,0],
  [0,0,3,1,1,0,0]
]

const player = ref({ x: 0, y: 0 })

const commandList = ref([
  { dir: "down", num: 1 }
])

const visited = ref(new Set())
const explosionCell = ref(null)

const success = ref(false)
const failed = ref(false)
const running = ref(false)
let runId = 0

/* ================= HELPERS ================= */
const sleep = (ms) => new Promise(r => setTimeout(r, ms))

function addCmd() {
  commandList.value.push({ dir: "right", num: 1 })
}

function removeCmd(i) {
  commandList.value.splice(i, 1)
}

/* ================= RESET ================= */
async function reset() {
  player.value = { x: 0, y: 0 }
  visited.value = new Set()

  success.value = false
  failed.value = false
  explosionCell.value = null
  await nextTick()
}

/* ================= EXPLOSION ================= */
function explode(x, y) {
  explosionCell.value = `${x},${y}`
  failed.value = true
  running.value = false

  setTimeout(() => {
    explosionCell.value = null
  }, 600)
}

/* ================= RUN ================= */
async function runProgram() {
  const id = ++runId
  if (running.value) return

  running.value = true
  await reset()
  await sleep(150)

  for (const cmd of commandList.value) {
    for (let i = 0; i < cmd.num; i++) {

      if (id !== runId) return

      let nx = player.value.x
      let ny = player.value.y

      if (cmd.dir === "right") nx++
      if (cmd.dir === "left") nx--
      if (cmd.dir === "down") ny++
      if (cmd.dir === "up") ny--

      if (
        nx < 0 || ny < 0 ||
        ny >= grid.length ||
        nx >= grid[0].length
      ) {
        explode(nx, ny)
        return
      }

      if (grid[ny][nx] === 0) {
        explode(nx, ny)
        return
      }

      player.value = { x: nx, y: ny }
      visited.value.add(`${nx},${ny}`)

      if (grid[ny][nx] === 3) {
        success.value = true
        running.value = false
        return
      }

      await sleep(120)
    }
  }

  running.value = false
}

/* ================= CELL CLASS ================= */
function getCellClass(x, y) {
  const key = `${x},${y}`
  const cell = grid[y][x]

  return {
    path: cell === 1,
    wall: cell === 0,
    key: cell === 3,

    visited: visited.value.has(key),

    // ⭐ FORCE START CELL ALWAYS GREEN
    start: x === 0 && y === 0,

    explosion: explosionCell.value === key,
    player: player.value.x === x && player.value.y === y
  }
}
</script>

<style scoped>
.rounded-borders {
  border-radius: 18px;
}

.q-stepper {
  box-shadow: none;
}

/* =========================
   LABYRINTH CONTAINER
========================= */

.start {
  background: #b9f6ca !important; /* strong green */
  box-shadow: inset 0 0 8px rgba(0,0,0,0.2);
}

.labyrinth {
  background: #f8fafc;
  border: 2px solid #e5e7eb;
  padding: 8px;
  border-radius: 14px;

  display: inline-block;
  overflow: auto;

  /* center on page */
  margin: 0 auto;
}

/* =========================
   GRID RESPONSIVE WRAPPER
   (IMPORTANT FOR MOBILE)
========================= */
.labyrinth .row {
  flex-wrap: nowrap;
}

/* =========================
   CELLS (DEFAULT DESKTOP)
========================= */
.cell {
  width: 42px;
  height: 42px;

  border: 1px solid #eaeaea;
  display: flex;
  align-items: center;
  justify-content: center;

  transition: all 0.2s ease;
  font-size: 16px;
}

/* =========================
   VISITED PATH (GREEN)
========================= */
.visited {
  background: #b9f6ca;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.08);
}

/* =========================
   WALL (DARK BUT SOFT)
========================= */
.wall {
  background: #263238;
}

/* =========================
   KEY
========================= */
.key {
  background: #ffd54f;
  animation: glowKey 1.5s infinite ease-in-out;
}

@keyframes glowKey {
  0%, 100% { filter: brightness(1); }
  50% { filter: brightness(1.2); }
}

/* =========================
   PLAYER (ROBOT)
========================= */
.player {
  background: #40c4ff;
  border-radius: 8px;
  box-shadow: 0 0 12px rgba(64, 196, 255, 0.7);
  transform: scale(1.1);
}

/* =========================
   EXPLOSION CELL (RED BUT MODERN)
========================= */
.explosion {
  background: #ff3b30 !important;
  animation: boom 0.4s ease;
  border-radius: 6px;
}

@keyframes boom {
  0% { transform: scale(1); filter: brightness(1); }
  50% { transform: scale(1.5); filter: brightness(2); }
  100% { transform: scale(1); filter: brightness(1); }
}

/* =========================
   SHAKE BANNER
========================= */
.explosion-banner {
  animation: shake 0.35s ease;
}

@keyframes shake {
  0% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  50% { transform: translateX(5px); }
  75% { transform: translateX(-3px); }
  100% { transform: translateX(0); }
}

.q-banner {
  border-radius: 10px;
}

.mini-card {
  width: 85px;
  padding: 6px;
  border-radius: 10px;
  font-size: 12px;
}

.no-wrap {
  flex-wrap: nowrap !important;
  overflow-x: auto;
  gap: 8px;
  padding-bottom: 6px;
}

.no-wrap::-webkit-scrollbar {
  height: 6px;
}
</style>
