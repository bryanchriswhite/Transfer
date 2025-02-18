<template>
  <ion-page>
    <div class="bg">
      <div class="bg-circle"></div>
    </div>
    <ion-content>
      <ion-grid>
        <ion-row class="ion-margin-top ion-align-items-center">
          <ion-col size="6">
            <ion-text
              color="black"
              class="transfer-text"
              @click="goToSendReload"
            >
              Transfer
            </ion-text>
          </ion-col>
          <ion-col size="6" class="flex ion-justify-content-end">
            <ion-button
              color="medium-grey"
              class="ion-margin-end nav-button"
              @click="toggleNav()"
            >
              <ion-icon slot="start" :icon="navIcon"></ion-icon>
              <ion-label class="ion-text-uppercase" slot="end">
                {{ navButtonText }}
              </ion-label>
            </ion-button>
          </ion-col>
        </ion-row>
        <ion-row>
          <ion-col class="router-view-col">
            <router-view v-slot="{ Component, route }">
              <transition :name="route.meta.transition">
                <component :is="Component"></component>
              </transition>
            </router-view>
          </ion-col>
        </ion-row>
      </ion-grid>
      <Footer class="non-sticky-footer"></Footer>
    </ion-content>
    <Footer class="sticky-footer"></Footer>
  </ion-page>
</template>

<style lang="css">
.relative {
  position: relative;
}

.router-view-col {
  padding-top: 0;
  position: initial;
}

.transfer-text {
  position: relative;
}

.w-logo {
  position: absolute;
  width: 100px;
  height: 100px;
  background-image: url('/assets/logos/logo.svg');
}

.transfer-text {
  font-size: 2rem;
  margin: 0 0 0 var(--main-left-margin);
}

:root {
  --main-left-margin: 2.5rem;
  --main-right-margin: 1rem;
  --max-width: 1100px;
  --sm-margin: 8px;
  --md-margin: calc(2 * var(--sm-margin));
  --ion-padding: 8px;
  --card-margin: var(--sm-margin);
  --card-padding: var(--ion-padding);

  --header-margin-left: calc(var(--card-margin) + var(--card-padding) + 16px);
  --header-margin-right: calc(var(--card-margin) + var(--card-padding) + 4px);
  /* NB: this is the difference between the left content border
     *  of the root grid (i.e. css selector `#app>ion-page>ion-grid`)
     *  and the left content border of the current view's root `ion-card`.
     */
  --left-offset-diff: 16px;
}

h1.party-popper {
  font-size: 60px;
}

.alert-wrapper {
  background-color: var(--ion-color-light-grey) !important;
}

.alert-message {
  color: var(--ion-color-black) !important;
}
</style>

<style lang="css" scoped>
.ion-page {
  align-items: center;
}

ion-footer {
  background: transparent;
  font-size: 13px;
  font-weight: 600;
}

ion-content > ion-grid {
  max-width: var(--max-width);
}

/*
ion-icon.nav {
    transition: transform .3s ease;
} */

.nav-button {
  margin-right: var(--main-right-margin);
}

.nav-button ion-icon[rotate='true'] {
  transform: rotate(-90deg);
}

.pointer {
  cursor: pointer;
}

.bg {
  z-index: -100;
  overflow: hidden;
  position: absolute;
  height: 100vh;
  width: 100vw;
}

.bg-circle {
  position: relative;
  clip-path: circle(45vw);
  top: 0vw;
  left: -4vw;
  height: 134vh;
  width: 100vw;
  background: #9ee6f5;
}

@media screen and (max-width: 992px) {
  .bg-circle {
    clip-path: circle(55vw);
    top: -4vw;
    left: -48vw;
    width: 170vw;
    height: 120vh;
  }
}

.non-sticky-footer {
  display: none;
}

@media screen and (max-height: 640px) {
  .sticky-footer {
    display: none;
  }

  .non-sticky-footer {
    display: initial;
  }
}

@media screen and (max-width: 399px) {
  .transfer-text {
    font-size: 1.7rem;
  }
}
</style>

<script lang="ts">
import { Component, defineComponent, Transition } from 'vue';
import {
  IonButton,
  IonCol,
  IonContent,
  IonGrid,
  IonIcon,
  IonLabel,
  IonPage,
  IonRow,
  IonText,
  IonFooter,
} from '@ionic/vue';
import { sendOutline, sendSharp } from 'ionicons/icons';
import Send from '@/views/Send.vue';
import Receive from '@/views/Receive.vue';
import Footer from '@/components/Footer.vue';
import { RouterView } from 'vue-router';
import { SendStep } from '@/types';

export default defineComponent({
  computed: {
    navComponent(): Component {
      return this.$route.path === '/s' ? Send : Receive;
    },
    navIcon(): string {
      return this.$route.path === '/s'
        ? '/assets/icon/download.svg'
        : sendSharp;
    },
    navButtonText(): string {
      return this.$route.path === '/s' ? 'Receive' : 'Send';
    },
    navHref(): string {
      return this.$route.path === '/s' ? '/r' : '/s';
    },
    rotated(): boolean {
      return this.$route.path !== '/s';
    },
  },
  methods: {
    toggleNav() {
      // TODO: move into store?
      this.$route.path === '/s'
        ? this.$router.replace('/r')
        : this.$router.replace('/s');
    },
    goToSendReload() {
      this.$router.replace('/s');
      setTimeout(() => {
        window.location.reload();
      }, 100);
    },
  },
  components: {
    Transition,
    IonPage,
    IonContent,
    IonFooter,
    IonGrid,
    IonRow,
    IonCol,
    IonText,
    IonLabel,
    IonButton,
    IonIcon,
    // IonRouterOutlet,
    // IonNavLink,
    RouterView,
    Footer,
  },
  setup() {
    return {
      SendStep,
    };
  },
});
</script>
