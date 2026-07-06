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
              class="text-subtitle1"
            >
              FINALE: HACKER GEFASST
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
            <q-step :name="6" title="6. Pseudocode Labyrinth" icon="route">

  <div class="text-center text-h6 q-mb-md">
    🧭 Entdecke das geheime Labyrinth
  </div>

  <!-- INSTRUCTIONS -->
  <q-card flat bordered class="q-pa-md q-mb-md bg-blue-1">

    <div class="text-subtitle2 q-mb-sm">
      📜 Pseudocode (Schrittfolge)
    </div>

    <pre style="font-family: monospace; font-size: 14px;">
START → (0,0)

1. MOVE RIGHT 2x
2. MOVE DOWN 1x
3. MOVE RIGHT 1x
4. MOVE DOWN 2x
5. MOVE RIGHT 2x → KEY 🔑
6. MOVE RIGHT 1x
7. MOVE UP 1x
8. MOVE RIGHT 2x → EXIT 🚪
    </pre>

  </q-card>

  <!-- STATUS -->
  <q-banner class="bg-blue-2 text-blue-10 q-mb-md">
    Schlüssel:
    <b>{{ hasKey ? "gefunden 🔑" : "noch nicht gefunden" }}</b>
  </q-banner>

  <!-- GRID -->
  <div class="labyrinth q-mb-md column items-center">

    <div
      v-for="(row, r) in grid"
      :key="r"
      class="row no-wrap"
    >
      <div
        v-for="(cell, c) in row"
        :key="c"
        class="cell"
        :class="cellClass(cell, c, r)"
      />
    </div>

  </div>

  <!-- CONTROLS -->
  <div class="row justify-center q-gutter-sm">

    <q-btn icon="arrow_upward" @click="move(0, -1)" />
    <q-btn icon="arrow_back" @click="move(-1, 0)" />
    <q-btn icon="arrow_forward" @click="move(1, 0)" />
    <q-btn icon="arrow_downward" @click="move(0, 1)" />

  </div>

  <!-- WIN -->
  <q-banner v-if="won" class="bg-green-2 text-green-10 q-mt-md">
    🎉 Ziel erreicht! Schlüssel genutzt!
  </q-banner>

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


const player = ref({ x: 0, y: 0 })
const hasKey = ref(false)
const won = ref(false)

// visited tiles
const visited = ref(new Set())

const grid = [
  [0,0,0,1,0],
  [1,1,0,1,0],
  [0,0,0,0,0],
  [0,1,1,1,1],
  [0,0,3,0,2]
]

function key(x, y) {
  return `${x},${y}`
}

// start visible
visited.value.add(key(0, 0))

function move(dx, dy) {

  const newX = player.value.x + dx
  const newY = player.value.y + dy

  if (
    newX < 0 || newY < 0 ||
    newY >= grid.length ||
    newX >= grid[0].length
  ) return

  if (grid[newY][newX] === 1) return

  player.value.x = newX
  player.value.y = newY

  // reveal tile
  visited.value.add(key(newX, newY))

  // key
  if (grid[newY][newX] === 3) {
    hasKey.value = true
    grid[newY][newX] = 0
  }

  // goal
  if (grid[newY][newX] === 2 && hasKey.value) {
    won.value = true
  }
}

function cellClass(cell, x, y) {

  const isVisible = visited.value.has(`${x},${y}`)

  if (!isVisible) {
    return "fog"
  }

  return {
    wall: cell === 1,
    goal: cell === 2,
    key: cell === 3,
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

.rounded-borders {
  border-radius: 18px;
}

.labyrinth {
  display: inline-block;
  background: #111;
  padding: 10px;
  border-radius: 12px;
}

.cell {
  width: 40px;
  height: 40px;
  border: 1px solid #444;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* hidden tiles */
.fog {
  background: #000 !important;
}

/* visible elements */
.wall {
  background: #333;
}

.goal {
  background: #ffd54f;
}

.key {
  background: #ff9800;
}

.player {
  background: #00bcd4;
  transition: all 0.15s ease;
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
