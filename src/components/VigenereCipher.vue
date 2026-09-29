<template>
  <ion-card class="cipher-card">
    <ion-card-header>
      <div class="card-heading">
        <div>
          <p class="card-kicker">KEYWORD METHOD</p>
          <ion-card-title>Vigenère Cipher</ion-card-title>
        </div>
        <span class="method-badge">02</span>
      </div>
      <p class="card-description">Use a repeating keyword to create a more nuanced shift.</p>
    </ion-card-header>

    <ion-card-content>
      <div class="key-row">
        <ion-item class="key-field">
        <ion-label position="stacked">Secret Keyword</ion-label>
        <ion-input 
          type="text" 
          v-model="keyword" 
          @ionInput="onKeywordChange"
          placeholder="e.g. KEY"
        ></ion-input>
        </ion-item>
        <span class="key-hint">Letters only · case insensitive</span>
      </div>

      <!-- Plaintext Input (Auto-Encrypts) -->
      <ion-item class="text-field ion-margin-top">
        <ion-label position="stacked"><span class="field-index">01</span> Plaintext <span class="field-note">encrypts live</span></ion-label>
        <ion-textarea 
          v-model="plaintext" 
          @ionInput="onPlaintextInput"
          placeholder="Type plain text to encrypt..." 
          :rows="3"
        ></ion-textarea>
      </ion-item>

      <!-- Ciphertext Input (Auto-Decrypts) -->
      <ion-item class="text-field ion-margin-top">
        <ion-label position="stacked"><span class="field-index">02</span> Ciphertext <span class="field-note">decrypts live</span></ion-label>
        <ion-textarea 
          v-model="ciphertext" 
          @ionInput="onCiphertextInput"
          placeholder="Type encrypted text to decrypt..." 
          :rows="3"
        ></ion-textarea>
      </ion-item>

      <!-- Clear Button -->
      <div class="card-actions ion-margin-top">
        <ion-button 
          fill="clear" 
          color="danger" 
          size="small" 
          @click="clearFields"
          :disabled="!plaintext && !ciphertext"
        >
          Clear workspace
        </ion-button>
      </div>
    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { ref } from 'vue';
import { 
  IonCard, 
  IonCardHeader, 
  IonCardTitle, 
  IonCardContent, 
  IonItem, 
  IonLabel, 
  IonTextarea, 
  IonInput,
  IonButton 
} from '@ionic/vue';

const keyword = ref('');
const plaintext = ref('');
const ciphertext = ref('');
const lastModified = ref('plain');

const vigenere = (text, key, isEncrypt) => {
  if (!text) return '';
  const cleanedKey = key.replace(/[^a-zA-Z]/g, '');
  if (!cleanedKey) return text;

  let result = '';
  let keyIndex = 0;
  const keyLength = cleanedKey.length;

  for (let i = 0; i < text.length; i++) {
    const char = text[i];
    if (/[a-zA-Z]/.test(char)) {
      const isUpper = char === char.toUpperCase();
      const baseCode = isUpper ? 65 : 97;

      const keyChar = cleanedKey[keyIndex % keyLength].toUpperCase();
      const shift = keyChar.charCodeAt(0) - 65;

      const charCode = char.charCodeAt(0) - baseCode;

      const newCharCode = isEncrypt
        ? (charCode + shift) % 26
        : (charCode - shift + 26) % 26;

      result += String.fromCharCode(newCharCode + baseCode);
      keyIndex++;
    } else {
      result += char;
    }
  }
  return result;
};

const onPlaintextInput = () => {
  lastModified.value = 'plain';
  ciphertext.value = vigenere(plaintext.value, keyword.value, true);
};

const onCiphertextInput = () => {
  lastModified.value = 'cipher';
  plaintext.value = vigenere(ciphertext.value, keyword.value, false);
};

const onKeywordChange = () => {
  if (lastModified.value === 'plain') {
    ciphertext.value = vigenere(plaintext.value, keyword.value, true);
  } else {
    plaintext.value = vigenere(ciphertext.value, keyword.value, false);
  }
};

const clearFields = () => {
  plaintext.value = '';
  ciphertext.value = '';
};
</script>

<style scoped>
.cipher-card { margin: 0; border: 1px solid #dfe6e1; border-radius: 16px; background: #ffffff; box-shadow: 0 14px 35px rgba(23, 35, 38, 0.06); }
ion-card-header { padding: 26px 28px 20px; }
ion-card-content { padding: 0 28px 25px; }
.card-heading, .key-row { display: flex; align-items: center; justify-content: space-between; gap: 16px; }
.card-kicker { margin: 0 0 8px; color: #087f76; font-size: 10px; font-weight: 800; letter-spacing: 1.3px; }
ion-card-title { color: #172326; font-size: 25px; font-weight: 750; }
.card-description { margin: 9px 0 0; color: #768481; font-size: 13px; line-height: 1.45; }
.method-badge { display: grid; width: 34px; height: 34px; place-items: center; border: 1px solid #d5ebe5; border-radius: 10px; color: #087f76; font-size: 11px; font-weight: 800; }
.key-row { margin-bottom: 22px; padding: 13px 15px 13px 0; border-bottom: 1px solid #e7eeea; }
.key-field { width: 220px; --background: transparent; --border-width: 0; --padding-start: 0; }
.key-hint { color: #9aa7a3; font-size: 11px; }
.text-field { --background: #f5f8f6; --border-radius: 10px; --border-width: 1px; --border-color: #e1eae5; --highlight-color-focused: #087f76; --padding-start: 15px; margin-left: 0; margin-right: 0; }
.text-field ion-label { color: #526460; font-size: 12px; font-weight: 750; }
.field-index { margin-right: 7px; color: #087f76; font-size: 10px; }
.field-note { float: right; color: #9aa7a3; font-size: 10px; font-weight: 500; }
ion-input, ion-textarea { --padding-top: 10px; --padding-bottom: 12px; color: #172326; font-size: 14px; }
.card-actions { display: flex; justify-content: flex-end; }
ion-button { --color: #c85c54; font-size: 12px; font-weight: 700; text-transform: none; }
@media (max-width: 520px) { ion-card-header { padding: 22px 18px 18px; } ion-card-content { padding: 0 18px 20px; } .key-hint { display: none; } }
</style>