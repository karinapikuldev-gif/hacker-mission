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

<q-step
  :name="1"
  title="1. Sortieralgorithmen"
  icon="swap_horiz"
>
  <div class="text-center text-h6 q-mb-md">
    👧🧒👦 Kinder sortieren sich selbst
  </div>

  <div class="text-center q-mb-lg">
    Stellt euch in die richtige Reihenfolge nach Größe!
  </div>

  <div class="row justify-center q-gutter-md q-my-lg">

    <q-card
      v-for="child in children"
      :key="child.name"
      class="q-pa-md text-center"
      style="width: 100px"
    >
      <div class="text-h5">{{ child.emoji }}</div>
      <div>{{ child.name }}</div>
      <div class="text-caption">Größe: {{ child.value }}</div>
    </q-card>

  </div>

  <q-banner class="bg-blue-2 text-blue-10" rounded>
    💡 Aufgabe: Bringt die Kinder in die richtige Reihenfolge (klein → groß).
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
          Passwort: <b>ROBOT</b>
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

            <q-step
              :name="3"
              title="3. Kryptographie"
              icon="lock"
            >
              <div class="text-center">
                Geheime Nachrichten werden entschlüsselt
                und selbst verschlüsselt.
              </div>
            </q-step>

            <q-step
              :name="4"
              title="4. Next Token Prediction"
              icon="smart_toy"
            >
              <div class="text-center">
                Kinder erleben,
                wie Sprachmodelle Wörter vorhersagen.
              </div>
            </q-step>

            <q-step
              :name="5"
              title="5. Binärcode"
              icon="lightbulb"
            >
              <div class="text-center">
                "Lampe an, Lampe aus"
                macht digitale Informationen sichtbar.
              </div>
            </q-step>

            <q-step
              :name="6"
              title="6. Pseudocode"
              icon="route"
            >
              <div class="text-center">
                Befehle, Bedingungen und Schleifen
                leiten zum finalen Raum.
              </div>
            </q-step>

          </q-stepper>

        </q-card>

      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref, computed } from 'vue'

const step = ref(1)

const switchA = ref(false)
const switchB = ref(false)

const lampOn = computed(() => switchA.value && switchB.value)


</script>

<style scoped>
.rounded-borders {
  border-radius: 18px;
}

.q-stepper {
  box-shadow: none;
}
</style>
