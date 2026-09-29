<template>
  <ion-page>
    <ion-header class="app-header" :translucent="true">
      <ion-toolbar>
        <div class="brand-lockup">
          <div class="brand-mark" aria-hidden="true">CS</div>
          <div>
            <p class="eyebrow">Private workspace</p>
            <ion-title>Cipher Studio</ion-title>
          </div>
        </div>
      </ion-toolbar>

      <ion-toolbar class="navigation-toolbar">
        <ion-segment v-model="selectedCipher" :scrollable="true">
          <ion-segment-button value="caesar">
            <ion-label>Caesar</ion-label>
          </ion-segment-button>
          <ion-segment-button value="vigenere">
            <ion-label>Vigenère</ion-label>
          </ion-segment-button>
        </ion-segment>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <main class="workspace">
        <section class="intro" aria-labelledby="workspace-title">
          <div>
            <p class="eyebrow">Text transformation</p>
            <h1 id="workspace-title">Make every message<br /><em>meaningfully unreadable.</em></h1>
          </div>
          <p class="intro-copy">Choose a method, set your key, and watch the result update as you type.</p>
        </section>

        <section class="tool-area" aria-label="Cipher tool">
          <CaesarCipher v-if="selectedCipher === 'caesar'" />
          <VigenereCipher v-if="selectedCipher === 'vigenere'" />
        </section>

        <footer class="workspace-footer">
          <span class="status-dot"></span>
          <span>Local only · Nothing leaves this device</span>
        </footer>
      </main>
    </ion-content>
  </ion-page>
</template>

<script setup>
import { ref } from 'vue';
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonSegment,
  IonSegmentButton,
  IonLabel
} from '@ionic/vue';

import CaesarCipher from '@/components/CaesarCipher.vue';
import VigenereCipher from '@/components/VigenereCipher.vue';

const selectedCipher = ref('caesar');
</script>

<style scoped>
.app-header {
  background: rgba(245, 247, 244, 0.9);
  box-shadow: none;
}

ion-toolbar {
  --background: transparent;
  --border-width: 0;
  padding: 0 max(20px, calc((100vw - 1040px) / 2));
}

.brand-lockup {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 18px 0 12px;
}

.brand-mark {
  display: grid;
  width: 38px;
  height: 38px;
  place-items: center;
  border-radius: 11px;
  background: #172326;
  color: #d8f2e9;
  font-size: 11px;
  font-weight: 800;
  letter-spacing: 1px;
}

.eyebrow {
  margin: 0 0 3px;
  color: #087f76;
  font-size: 10px;
  font-weight: 800;
  letter-spacing: 1.5px;
  line-height: 1.2;
  text-transform: uppercase;
}

ion-title {
  padding: 0;
  color: #172326;
  font-size: 18px;
  font-weight: 750;
  text-align: left;
}

.navigation-toolbar {
  border-bottom: 1px solid #dfe6e1;
  padding-bottom: 10px;
}

ion-segment {
  max-width: 1040px;
  margin: 0 auto;
  --background: #e8eeea;
  border-radius: 10px;
}

ion-segment-button {
  min-height: 38px;
  --border-radius: 8px;
  --color: #6d7b79;
  --color-checked: #172326;
  --indicator-color: #ffffff;
  font-size: 13px;
  font-weight: 700;
}

.workspace {
  width: min(100% - 40px, 1040px);
  margin: 0 auto;
  padding: 42px 0 30px;
}

.intro {
  display: flex;
  align-items: end;
  justify-content: space-between;
  gap: 32px;
  margin-bottom: 30px;
}

h1 {
  margin: 8px 0 0;
  color: #172326;
  font-size: clamp(30px, 4vw, 48px);
  font-weight: 700;
  letter-spacing: -1.5px;
  line-height: 1.04;
}

h1 em {
  color: #087f76;
  font-style: normal;
}

.intro-copy {
  max-width: 270px;
  margin: 0 0 3px;
  color: #6d7b79;
  font-size: 14px;
  line-height: 1.55;
}

.tool-area {
  animation: rise-in 420ms ease-out both;
}

.workspace-footer {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 18px;
  color: #81908c;
  font-size: 12px;
}

.status-dot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #4fae82;
  box-shadow: 0 0 0 4px #dcefe3;
}

@keyframes rise-in {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 620px) {
  .workspace {
    width: min(100% - 28px, 1040px);
    padding-top: 30px;
  }

  .intro {
    display: block;
    margin-bottom: 24px;
  }

  .intro-copy {
    margin-top: 14px;
  }
}
</style>