<script setup>
import axios from "axios";
import { ref } from "vue";

const url = "https://restcountries.com/v3.1/all?fields=translations";
const countries = ref([]);
const contactData = ref({
  email: "",
  message: "",
  selectedCountry: "",
  privacyAgreement: false,
});
const errors = ref({
  email: "",
  message: "",
  selectedCountry: "",
  privacyAgreement: "",
});

const handleSubmit = () => {
  if (handleValidation()) {
    // send to backend
    // if response = code 422 => set local errors to response errors
  }
};
const handleValidation = () => {
  let result = true;

  // regular expression found on the internet
  const emailPattern =
    /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

  if (!contactData.value.email.toLowerCase().match(emailPattern)) {
    result = false;
    errors.value.email = "Du musst eine gültige E-Mail-Adresse eingeben.";
  }
  if (contactData.value.email === "") {
    result = false;
    errors.value.email = "Du musst eine E-Mail-Adresse angeben.";
  }

  if (
    contactData.value.message.length < 20 ||
    contactData.value.message.length > 20000
  ) {
    result = false;
    errors.value.message =
      "Die Nachricht muss zwischen 20 und 20000 Zeichen haben.";
  }
  if (contactData.value.message === "") {
    result = false;
    errors.value.message = "Du musst eine Nachricht eingeben.";
  }

  if (!contactData.value.privacyAgreement) {
    result = false;
    errors.value.privacyAgreement =
      "Du musst die Datenschutzerklärung akzeptieren.";
  }

  return result;
};

axios.get(url).then(({ data }) => {
  let temp = [];
  data.forEach((element) => {
    /* Strings are cut-off to prevent overflow on mobile devices */
    if (element.translations.deu.common.length >= 30) {
      temp.push(element.translations.deu.common.substring(0, 29) + " ...");
    } else {
      temp.push(element.translations.deu.common);
    }
  });

  countries.value = temp.sort();
});
</script>

<template>
  <section aria-labelledby="contact--heading" class="contact">
    <div class="contact-wrapper inner grid-flow">
      <h2 id="contact--heading" class="contact--headline">Kontakt</h2>
      <form
        class="contact--form form-group flex-flow"
        method="post"
        @submit.prevent="handleSubmit"
      >
        <div class="email-input flex-flow">
          <label for="email" :class="errors.email ? 'error' : ''"
            >E-Mail-Adresse</label
          >
          <input
            id="email"
            name="email"
            placeholder="Deine E-Mail-Adresse..."
            v-model="contactData.email"
            :class="errors.email ? 'error' : ''"
            @input="errors.email = ''"
          />
          <span class="error">{{ errors.email }}</span>
        </div>
        <div class="region-select flex-flow">
          <label for="region" :class="errors.selectedCountry ? 'error' : ''"
            >Land</label
          >
          <span class="is-optional">optional</span>
          <select
            id="region"
            name="region"
            v-model="contactData.selectedCountry"
            :class="errors.selectedCountry ? 'error' : ''"
            @change="errors.selectedCountry = ''"
          >
            <option value="" disabled selected>Landwählen...</option>
            <option
              :value="country"
              v-for="country in countries"
              :key="country"
            >
              {{ country }}
            </option>
          </select>
          <span class="error">{{ errors.selectedCountry }}</span>
        </div>
        <div class="message-textarea flex-flow">
          <label for="message" :class="errors.message ? 'error' : ''"
            >Nachricht</label
          >
          <textarea
            id="message"
            name="message"
            placeholder="Deine Nachricht..."
            v-model="contactData.message"
            :class="errors.message ? 'error' : ''"
            @input="errors.message = ''"
          ></textarea>
          <span class="error">{{ errors.message }}</span>
        </div>
        <div class="form--verification">
          <label
            >Datenschutz
            <div class="checkbox-wrapper">
              <input
                type="checkbox"
                v-model="contactData.privacyAgreement"
                @input="errors.privacyAgreement = ''"
              />
              <mark
                class="checkmark"
                :class="errors.privacyAgreement ? 'error' : ''"
              ></mark>
              <p>Ich bin mit der Verarbeitung meiner Daten einverstanden.</p>
            </div></label
          ><span class="error">{{ errors.privacyAgreement }}</span>
        </div>
        <button class="button is-primary" type="submit">Senden</button>
      </form>
      <div class="contact--image">
        <img
          src="../assets/images/image-contact.jpg"
          alt="Bild eines alten Schnurtelefons"
        />
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
span.error,
label.error {
  margin-block-start: 5px;
  color: crimson;
}

input.error,
textarea.error,
mark.error {
  border-color: crimson;
}
</style>
