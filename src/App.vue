<template>
  <IntroEnvelope
    v-if="!opened"
    :bride="data.bride.name"
    :groom="data.groom.name"
    :date="dateObj"
    bg="#f9f4ee"
    flower="/flower.png"
    envelope="/enveloppe.png"
    @opened="handleOpened"
  />

  <template v-else>
    <Invitation
      :groom="data.groom.name"
      :bride="data.bride.name"
      :date="dateObj"
      :dateISO="data.dateISO"
      :locations="data.locations"
      :program="{
      groomFamily: data.groom.family,
      brideFamily: data.bride.family,
      date: '24 Janvier 2026',
      time: '15h30',
      mosqueName: 'Mosquée de Bassiko',
      receptionPlace: 'Domicile de la mariée',
      mosqueIcon: '/mosque.png',
      homeIcon: '/home.png',
      }"
      venue="Mosquée …, Ouagadougou"
      rsvp="+226 XX XX XX XX"
      bg="/white-bg.png"
      topDeco="/invite-top.png"
      bottomDeco="/invite-bottom.png"
      frame="/photo-frame.png"
      couplePhoto="/couple.jpg"
      floralDeco="/floral.png"
    />

    <WeddingContent :data="data" />

    <AudioBar src="/music.mp3" title="J’te donnerai - Lynda" />
  </template>
</template>

<script setup>
import { reactive, ref, computed } from "vue";
import IntroEnvelope from "./components/IntroEnvelope.vue";
import Invitation from "./components/Invitation.vue";
import AudioBar from "./components/AudioBar.vue";

const opened = ref(false);

const data = reactive({
  bride: { name: "Yasmina", family: "ZIDA" },
  groom: { name: "Oumar", family: "KONÉ" },
  dateISO: "2026-01-24T15:30:00+00:00",
  time: "15h30",
  contactName: "Comité d’organisation",
  contactPhone: "+226 XX XX XX XX",
  programNote:
    "Nous vous prions d’honorer de votre présence ce moment de bénédictions et de partage. Qu’Allah سبحانه وتعالى accorde la baraka à cette union.",
  whatsappNumber: "226XXXXXXXX",
  locations: [
    { title: "Mosquée de Bassinko", address: "Adresse mosquée…", embed: "https://www.google.com/maps/embed?pb=!1m17!1m12!1m3!1d3896.899361381719!2d-1.6370538249333169!3d12.389669387874843!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m2!1m1!2zMTLCsDIzJzIyLjgiTiAxwrAzOCcwNC4xIlc!5e0!3m2!1sfr!2sbf!4v1768726477941!5m2!1sfr!2sbf", openLink: "https://maps.app.goo.gl/pZgrWsym3VupycPx8" },
    { title: "Chez la Mariée", address: "Adresse…", embed: "https://www.google.com/maps/embed?pb=!1m17!1m12!1m3!1d3896.8670847055855!2d-1.640363424933269!3d12.391829487872805!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m2!1m1!2zMTLCsDIzJzMwLjYiTiAxwrAzOCcxNi4wIlc!5e0!3m2!1sfr!2sbf!4v1768726607241!5m2!1sfr!2sbf", openLink: "https://maps.app.goo.gl/4GEXaBCLnzfzCoWU9" },
  ],
});

const dateObj = computed(() => ({
  dayName: "SAMEDI",
  dayNumber: "24",
  time: "15:30",
  month: "Janvier",
  year: "2026",
}));

function handleOpened(){
  opened.value = true;
  window.scrollTo({ top: 0, behavior: "instant" });
}
</script>
