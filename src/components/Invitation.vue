<template>
  <section class="inviteWrap">
    <div class="inviteCard" :style="{ backgroundImage: `url(${bg})` }" ref="card">
      <!-- overlay pour harmoniser la couleur (comme intro) -->
      <div class="tone"></div>

      <!-- décor haut -->
      <img v-if="topDeco" class="topDeco" src="/topDeco.png" alt="" aria-hidden="true" />

      <!-- décor fleur -->
      <img v-if="floralDeco" class="floralDeco" :src="floralDeco" alt="" aria-hidden="true" />

      <!-- contenu -->
      <div class="inner">
        <!-- calligraphie bismillah -->
        <div class="bismi">﷽</div>

        <div class="smallCaps">
          Au nom d'Allah, le Tout Miséricordieux, <br> le Très Miséricordieux. <br>

        "Et parmi ses signes il a créé de vous, pour vous, des épouses pour que vous viviez en tranquillité avec elles et Il a mis entre vous de l'affection et de la bonté. Il y a en cela des preuves pour des gens qui réfléchissent. <br> (Sourate 30, verset 21)
        </div>

        <div class="title">Mariage religieux</div>

        <!-- noms -->
        <div class="names">
          <span class="name">{{ groom }}</span>
          <span class="amp">&</span>
          <span class="name">{{ bride }}</span>
        </div>

        <!-- photo du couple dans cadre -->
        <div class="photoBlock" v-if="couplePhoto">
          <img class="photo" src="/couple.png" alt="Photo du couple" />
        </div>

        <!-- date style luxe -->
        <div class="dateBox">
          <div class="dateRow">
            <span class="dateSide">{{ dayName }}</span>
            <span class="dateCenter">{{ dayNumber }}</span>
            <span class="dateSide">{{ year }}</span>
          </div>
          <div class="dateMonth">{{ month.toUpperCase() }}</div>
        </div>

        <!-- PROGRAMME -->
        <div class="program" v-if="program">
          <div class="programTitle">Programme</div>

          <p class="programText">
            Les grandes familles <span class="fam">{{ program.groomFamily }}</span> et <span class="fam">{{ program.brideFamily }}</span>
            vous prient d’honorer de votre présence ce moment de bénédiction,
            à l’occasion du mariage de leur enfant
            <span class="child">{{ groom }}</span> et <span class="child">Raïcha Yasmina</span>.
          </p>

          <div class="steps">
            <div class="step">
              <img class="stepIcon" :src="program.mosqueIcon" alt="" aria-hidden="true" />
              <div class="stepBody">
                <div class="stepLabel">Mariage religieux</div>
                <div class="stepValue">
                  {{ program.date }} à {{ program.time }} <br> {{ program.mosqueName }}
                </div>
              </div>
        </div>

    <div class="step">
      <img class="stepIcon" :src="program.homeIcon" alt="" aria-hidden="true" />
      <div class="stepBody">
        <div class="stepLabel">Réception</div>
        <div class="stepValue">
          {{ program.date }} à 16h30 <br> {{ program.receptionPlace }}
        </div>
      </div>
    </div>
  </div>

  <div class="programHint">
    Les localisations Google Maps sont juste après.
  </div>
</div>

<!-- MAPS -->
<div class="mapsBlock" id="maps">
  <div class="mapsHeader reveal">
    <div class="mapsTitle">Localisations</div>
    <div class="mapsSub">
      Cliquez sur <span class="chip">Ouvrir</span> pour lancer l’itinéraire sur Google Maps.
    </div>
  </div>

  <div class="mapsGrid" v-if="locations?.length">
    <div class="mapCard reveal" v-for="loc in locations" :key="loc.title">
      <div class="mapTop">
        <div class="mapText">
          <div class="mapName">{{ loc.title }}</div>
          <div class="mapAddr">{{ loc.address }}</div>
        </div>

        <a class="mapBtn" :href="loc.openLink" target="_blank" rel="noreferrer">
          Ouvrir ↗
        </a>
      </div>

      <div class="mapFrameWrap">
        <iframe
          class="mapFrame"
          :src="loc.embed"
          loading="lazy"
          referrerpolicy="no-referrer-when-downgrade"
        ></iframe>
      </div>
    </div>
  </div>

  <div class="mapsEmpty" v-else>
    Les localisations seront bientôt disponibles.
  </div>
</div>


<!-- COUNTDOWN -->
<div class="countdownBlock" id="countdown">
  <div class="countHeader reveal">
    <div class="countTitle">Compte à rebours</div>
    <div class="countSub">Plus que…</div>
  </div>

  <div class="countGrid reveal">
    <div class="countTile">
      <div class="n">{{ cd.days }}</div>
      <div class="t">Jours</div>
    </div>
    <div class="countTile">
      <div class="n">{{ cd.hours }}</div>
      <div class="t">Heures</div>
    </div>
    <div class="countTile">
      <div class="n">{{ cd.minutes }}</div>
      <div class="t">Minutes</div>
    </div>
    <div class="countTile">
      <div class="n">{{ cd.seconds }}</div>
      <div class="t">Secondes</div>
    </div>
  </div>

  <div class="countDone reveal" v-if="cd.isOver">
    C’est le grand jour ! Qu’Allah bénisse cette union. 🤍
  </div>
</div>





        <!-- RSVP -->
        <div class="rsvp" v-if="rsvp">
          <div class="rsvpTitle">Infoline</div>
          <div class="rsvpLine">76 71 49 35</div>
        </div>

        
      </div>

      <!-- décor bas -->
      <img v-if="bottomDeco" class="bottomDeco" src="/bottomDeco.png" alt="" aria-hidden="true" />
    </div>
  </section>
</template>

<script setup>
import { onMounted, ref, onBeforeUnmount, reactive } from "vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const props = defineProps({
  /* couleurs harmonisées avec l’intro */
  bg: { type: String, default: "/invite-bg.png" },
  topDeco: { type: String, default: "/invite-top.png" },
  bottomDeco: { type: String, default: "/invite-bottom.png" },
  floralDeco: { type: String, default: "/floral.png" },

  couplePhoto: { type: String, default: "/couple.jpg" },
  frame: { type: String, default: "/photo-frame.png" },

  groom: { type: String, required: true },
  bride: { type: String, required: true },

  dayName: { type: String, default: "SAMEDI" },
  dayNumber: { type: String, default: "24" },
  time: { type: String, default: "15:30" },
  month: { type: String, default: "Janvier" },
  year: { type: String, default: "2026" },

  venue: { type: String, default: "Mosquée …, Ville …" },
  rsvp: { type: String, default: "+226 XX XX XX XX" },

  program: {
  type: Object,
  default: () => ({
    groomFamily: "KONE",
    brideFamily: "ZIDA",
    date: "24 Janvier 2026",
    time: "15h30",
    mosqueName: "Mosquée de Bassiko",
    receptionPlace: "Domicile de la mariée",
    mosqueIcon: "/mosque.png",
    homeIcon: "/home.png",
  })
},

dateISO: { type: String, required: true },
locations: { type: Array, default: () => [] },


});

const card = ref(null);

onMounted(() => {// 1️⃣ Animation d'entrée de la carte
  gsap.from(card.value, { 
    y: 40, 
    opacity: 0, 
    duration: 1, 
    ease: "power3.out" 
  });

  // 2️⃣ Animation en cascade du header (visible dès le début)
  gsap.from(".bismi, .smallCaps, .title", {
    y: 30,
    opacity: 0,
    duration: 0.8,
    stagger: 0.15,
    ease: "power2.out",
    delay: 0.3
  });

  // 3️⃣ Animation des noms
  gsap.from(".name", {
    opacity: 0,
    y: 20,
    duration: 2,
    stagger: 0.3,
    ease: "power2.out",
    delay: 0.6
  });

  gsap.from(".amp", {
    scale: 0,
    opacity: 0,
    duration: 0.5,
    ease: "back.out(2)",
    delay: 1
  });

  // 4️⃣ Photo avec ScrollTrigger
  gsap.from(".photoBlock", {
    scale: 0.9,
    opacity: 0,
    duration: 2,
    ease: "power2.out",
    scrollTrigger: {
      trigger: ".photoBlock",
      start: "top 85%",
    }
  });

  // 5️⃣ Date box
  gsap.from(".dateBox", {
    y: 40,
    opacity: 0,
    duration: 2,
    ease: "power2.out",
    scrollTrigger: {
      trigger: ".dateBox",
      start: "top 85%",
    }
  });

  // 6️⃣ Section Programme
  gsap.from(".programTitle, .programText", {
    y: 30,
    opacity: 0,
    duration: 2,
    stagger: 0.15,
    ease: "power2.out",
    scrollTrigger: {
      trigger: ".program",
      start: "top 80%",
    }
  });

  // 7️⃣ Steps du programme (effet rebond)
  gsap.from(".step", {
    x: -50,
    opacity: 0,
    duration: 2,
    stagger: 0.2,
    ease: "back.out(1.4)",
    scrollTrigger: {
      trigger: ".steps",
      start: "top 80%",
    }
  });

  // 8️⃣ Section Maps
  gsap.from(".mapsTitle, .mapsSub", {
    y: 30,
    opacity: 0,
    duration: 2,
    stagger: 0.15,
    ease: "power2.out",
    scrollTrigger: {
      trigger: ".mapsBlock",
      start: "top 80%",
    }
  });

  // 9️⃣ Cartes Maps (apparition une par une)
  gsap.from(".mapCard", {
    y: 60,
    opacity: 0,
    duration: 2,
    stagger: 0.25,
    ease: "power2.out",
    scrollTrigger: {
      trigger: ".mapsGrid",
      start: "top 80%",
    }
  });

  // 🔟 Section Countdown
  gsap.from(".countTitle, .countSub", {
    y: 30,
    opacity: 0,
    duration: 2,
    stagger: 0.15,
    ease: "power2.out",
    scrollTrigger: {
      trigger: ".countdownBlock",
      start: "top 85%",
    }
  });

  // 1️⃣1️⃣ Tiles du countdown (effet scale)
  gsap.from(".countTile", {
    scale: 0.5,
    opacity: 0,
    duration: 2,
    stagger: 0.1,
    ease: "back.out(1.7)",
    scrollTrigger: {
      trigger: ".countGrid",
      start: "top 85%",
    }
  });

  // 1️⃣2️⃣ Section RSVP
  gsap.from(".rsvp", {
    y: 40,
    opacity: 0,
    duration: 2,
    ease: "power2.out",
    scrollTrigger: {
      trigger: ".rsvp",
      start: "top 85%",
    }
  });

  // 1️⃣3️⃣ Animation flottante continue (décorations)
  gsap.to(".floralDeco", {
    y: 15,
    rotation: 3,
    duration: 3,
    ease: "sine.inOut",
    yoyo: true,
    repeat: -1
  });

  // 1️⃣4️⃣ Pulse subtil sur Bismillah
  gsap.to(".bismi", {
    scale: 1.03,
    duration: 2.5,
    ease: "sine.inOut",
    yoyo: true,
    repeat: -1
  });

  // ✨ Pulse sur l'enveloppe (effet "clique-moi")
  gsap.to(".photoBlock", {
    scale: 1.04,
    duration: 2.2,
    ease: "sine.inOut",
    yoyo: true,
    repeat: -1,
  });



  // Timer countdown
  tick();
  timer = setInterval(tick, 1000);

});

const cd = reactive({ days: 0, hours: 0, minutes: 0, seconds: 0, isOver:false });
let timer = null;

function tick(){
  const now = new Date().getTime();
  const target = new Date(props.dateISO).getTime();
  const diff = target - now;

  if(diff <= 0){
    cd.isOver = true;
    cd.days = cd.hours = cd.minutes = cd.seconds = 0;
    return;
  }
  cd.isOver = false;

  const s = Math.floor(diff / 1000);
  cd.days = Math.floor(s / (3600*24));
  cd.hours = Math.floor((s % (3600*24)) / 3600);
  cd.minutes = Math.floor((s % 3600) / 60);
  cd.seconds = Math.floor(s % 60);
}

onBeforeUnmount(() => {
  if(timer) clearInterval(timer);
  // Nettoyer les ScrollTriggers
  ScrollTrigger.getAll().forEach(st => st.kill());
});


</script>

<style scoped>
.inviteWrap{
  width: 100%;
  margin: 0;
}

.inviteCard{
  position: relative;
  width: 100%;
  min-height: 100vh;         /* occupe l’écran */
  border-radius: 0;          /* plus d’arrondi */
  overflow: hidden;
  box-shadow: none;          /* plus d’ombre "carte" */
  border: none;              /* plus de contour */
  background-size: cover;
  background-position: center;
}

/* Harmonisation couleur (même ambiance intro) */
.tone{
  position:absolute;
  inset:0;
  background:
    radial-gradient(900px 520px at 50% 10%, rgba(255,255,255,.40), rgba(255,255,255,.10) 55%, rgba(0,0,0,.05) 100%);
  pointer-events:none;
}

/* Décor haut */
.topDeco{
  position:absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 560px;
  height: auto;
  z-index: 2;
  pointer-events:none;
}

/* Décor bas */
.bottomDeco{
  position:absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 560px;
  height: auto;
  z-index: 2;
  pointer-events:none;
}

/* Fleur décorative */
.floralDeco{
  position:absolute;
  right: -16px;
  top: 120px;
  width: 160px;
  height: auto;
  opacity: .85;
  z-index: 2;
  pointer-events:none;
  filter: drop-shadow(0 16px 22px rgba(0,0,0,.10));
}

.inner{
  position: relative;
  z-index: 3;
  padding: 110px 22px 140px; /* laisse place top et bottom */
  text-align:center;
  color: rgba(40,32,20,.85);
}

/* Calligraphie */
.bismi{
  font-size: 34px;
  color: rgba(163,133,76,.88);
  margin-bottom: 10px;
}

/* texte haut */
.smallCaps{
  margin-top: 30px;
  font-size: 8px;
  letter-spacing: .16em;
  text-transform: uppercase;
  color: rgba(80,60,25,.55);
  line-height: 1.5;
}

/* titre */
.title{
  margin-top: 10px;
  font-family: "Cormorant Garamond", serif;
  font-weight: 700;
  font-size: 30px;
  color: rgba(163,133,76,.90);
}

/* noms */
.names{
  margin-top: 10px;
  margin-bottom: 30px;
  font-family: "Great Vibes", cursive;
  font-size: clamp(44px, 10vw, 74px);
  line-height: 1;
  color: rgba(142, 23, 33, .92); /* bordeaux comme l’inspiration */
}
.amp{
  font-family: Inter, system-ui;
  font-size: 18px;
  font-weight: 600;
  opacity: .55;
  margin: 0 10px;
}

/* photo */
.photoBlock{ margin-top: 4px; }
.photoWrap{
  width: min(340px, 78vw);
  margin: 0 auto;
  position: relative;
  aspect-ratio: 4/5;
  border-radius: 18px;
  overflow:hidden;
  box-shadow: 0 18px 48px rgba(0,0,0,.18);
  border: 1px solid rgba(163,133,76,.30);
  background: rgba(255,255,255,.40);
}
.photo{
  width:100%;
  height:100%;
  object-fit: cover;
  display:block;
  transform: scale(1.25);       /* ⬅ zoom (1.05 à 1.35) */
  transform-origin: center; 
}
.frame{
  position:absolute;
  inset: 0;
  width:100%;
  height:100%;
  object-fit: cover;
  pointer-events:none;
}

/* date */
.dateBox{
  width: min(380px, 88vw);
  margin: 64px auto 10px;
  padding: 12px 12px 10px;
  border-top: 3px solid rgba(163,133,76,.35);
  border-bottom: 3px solid rgba(163,133,76,.35);
}

.dateRow{
  display:flex;
  justify-content:space-between;
  align-items:baseline;
  gap: 10px;
  color: rgba(80,60,25,.70);
  font-family: Inter, system-ui;
  letter-spacing: .10em;
  text-transform: uppercase;
  font-size: 20px;
}
.dateCenter{
  font-family: Inter, system-ui;
  font-size: 30px;
  font-weight: 700;
  letter-spacing: .08em;
  color: rgba(142, 23, 33, .85);
  line-height: 1;
}
.dateMonth{
  margin-top: 6px;
  font-family: Inter, system-ui;
  font-size: 24px;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: rgba(80,60,25,.55);
}

/* lieu */
.location{
  margin-top: 10px;
  font-size: 12px;
  letter-spacing: .12em;
  text-transform: uppercase;
  color: rgba(80,60,25,.60);
}
.pin{ color: rgba(163,133,76,.9); margin-right: 6px; }

/* PROGRAMME */
.program{
  margin-top: 40px;
  padding-top: 6px;
}

.programTitle{
  font-family: "Great Vibes", cursive;
  font-size: 38px;
  color: rgba(142, 23, 33, .88); /* même bordeaux */
  margin-bottom: 10px;
}

.programText{
  margin: 0 auto;
  width: min(520px, 92vw);
  font-size: 13px;
  line-height: 1.75;
  color: rgba(80,60,25,.68);
}

.programText .fam{
  font-weight: 800;
  color: rgba(80,60,25,.78);
  letter-spacing: .02em;
}

.programText .child{
  font-weight: 800;
  color: rgba(142, 23, 33, .82);
}

/* steps (mosquée + maison) */
.steps{
  width: min(520px, 92vw);
  margin: 14px auto 0;
  display: grid;
  gap: 10px;
}

.step{
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 12px;
  border-radius: 18px;
  border: 1px solid rgba(163,133,76,.22);     /* doré léger */
  background: rgba(255,255,255,.35);          /* ivoire glass */
  backdrop-filter: blur(8px);
}

.stepIcon{
  width: 42px;
  height: 42px;
  object-fit: contain;
  flex: 0 0 auto;
  filter: drop-shadow(0 10px 14px rgba(0,0,0,.08));
  opacity: .92;
}

.stepBody{ text-align: left; }

.stepLabel{
  font-size: 11px;
  letter-spacing: .16em;
  text-transform: uppercase;
  color: rgba(80,60,25,.55);
  font-weight: 700;
}

.stepValue{
  margin-top: 4px;
  font-size: 13px;
  color: rgba(80,60,25,.72);
  font-weight: 600;
  line-height: 1.35;
}

.programHint{
  margin-top: 10px;
  font-size: 12px;
  color: rgba(80,60,25,.52);
  letter-spacing: .08em;
  text-transform: uppercase;
}

/* ===== COUNTDOWN ===== */
.countdownBlock{
  margin-top: 60px;
  padding-top: 6px;
  text-align: center;
}

.countTitle{
  font-family: "Great Vibes", cursive;
  font-size: 38px;
  color: rgba(142, 23, 33, .88);
  margin-bottom: 8px;
}

.countSub{
  font-size: 12px;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: rgba(80,60,25,.55);
}

.countGrid{
  width: min(520px, 92vw);
  margin: 14px auto 0;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  align-items: stretch; /* mêmes hauteurs */
}

.countTile{
  border-radius: 18px;
  border: 1px solid rgba(163,133,76,.22);
  background: rgba(255,255,255,.35);
  backdrop-filter: blur(8px);
  padding: 12px 10px;
}

.countTile .n{
  font-weight: 900;
  font-size: 22px;
  color: rgba(80,60,25,.82);
}

.countTile .t{
  margin-top: 6px;
  font-size: 11px;
  letter-spacing: .16em;
  text-transform: uppercase;
  color: rgba(80,60,25,.55);
}

.countDone{
  width: min(520px, 92vw);
  margin: 12px auto 0;
  font-size: 12px;
  letter-spacing: .10em;
  text-transform: uppercase;
  color: rgba(80,60,25,.58);
}

@media (max-width: 420px){
  .countGrid{ grid-template-columns: repeat(2, 1fr); }
}

/* ===== MAPS (même style que Programme/RSVP) ===== */
.mapsBlock{
  margin-top: 60px;
  padding-top: 10px;
}

.mapsHeader{
  text-align: center;
}

.mapsTitle{
  font-family: "Great Vibes", cursive;
  font-size: 38px;
  color: rgba(142, 23, 33, .88);
  margin-bottom: 8px;
}

.mapsSub{
  font-size: 12px;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: rgba(80,60,25,.55);
  line-height: 1.6;
}

.chip{
  display: inline-block;
  padding: 2px 8px;
  border-radius: 999px;
  border: 1px solid rgba(163,133,76,.25);
  background: rgba(255,255,255,.35);
  color: rgba(80,60,25,.65);
  letter-spacing: .10em;
}

.mapsGrid{
  width: min(520px, 92vw);
  margin: 14px auto 0;
  display: grid;
  gap: 14px;
}

.mapCard{
  padding: 12px;
  border-radius: 18px;
  border: 1px solid rgba(163,133,76,.22);
  background: rgba(255,255,255,.35);
  backdrop-filter: blur(8px);
}

.mapTop{
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 10px;
}

.mapName{
  font-weight: 900;
  color: rgba(80,60,25,.82);
}

.mapAddr{
  margin-top: 4px;
  font-size: 13px;
  color: rgba(80,60,25,.55);
  line-height: 1.35;
}

.mapBtn{
  text-decoration: none;
  font-size: 12px;
  font-weight: 800;
  letter-spacing: .12em;
  text-transform: uppercase;
  padding: 10px 12px;
  border-radius: 999px;
  border: 1px solid rgba(142, 23, 33, .22);
  background: linear-gradient(180deg, rgba(142, 23, 33, .14), rgba(163,133,76,.10));
  color: rgba(80,60,25,.82);
  transition: transform .15s ease;
}
.mapBtn:hover{ transform: translateY(-1px); }

.mapFrameWrap{
  border-radius: 16px;
  overflow: hidden;
  border: 1px solid rgba(163,133,76,.18);
  box-shadow: 0 18px 48px rgba(0,0,0,.10);
}

.mapFrame{
  width: 100%;
  height: 240px;
  border: 0;
  display: block;
}

.mapsEmpty{
  width: min(520px, 92vw);
  margin: 14px auto 0;
  font-size: 12px;
  letter-spacing: .12em;
  text-transform: uppercase;
  color: rgba(80,60,25,.55);
  text-align: center;
}


/* rsvp */
.rsvp{
  margin-top: 60px;
}
.rsvpTitle{
  font-family: "Great Vibes", cursive;
  font-size: 34px;
  color: rgba(142, 23, 33, .85);
}
.rsvpLine{
  margin-top: 6px;
  font-size: 18px;
  letter-spacing: .16em;
  text-transform: uppercase;
  color: rgba(80,60,25,.55);
}

@media (max-width: 420px){
  .inner{ padding: 96px 18px 130px; }
  .floralDeco{ width: 130px; top: 110px; }
  .bismi{ font-size: 30px; }
}
</style>
