<template>
  <ion-footer class="ion-no-border">
    <ion-grid>
      <ion-row class="ion-align-items-center">
        <ion-col class="footer flex ion-align-items-center">
          <div class="links flex ion-justify-content-between">
            <div>
              <a href="" target="_blank">
                <ion-text color="black"> FAQ </ion-text>
              </a>
            </div>
            <div>
              <a href="" target="_blank">
                <ion-text color="black"> Privacy </ion-text>
              </a>
            </div>
            <div>
              <a href="" target="_blank">
                <ion-text color="black"> About Us </ion-text>
              </a>
            </div>
            <div>
              <a href="" target="_blank">
                <ion-text color="black"> GitHub </ion-text>
              </a>
            </div>
          </div>
          <div class="mobile-la-logo flex ion-justify-content-center">
            <div class="la-logo"></div>
          </div>
        </ion-col>
        <ion-col
          class="made-with-love-container flex ion-justify-content-end ion-align-items-center"
        >
          <ion-text color="dark-grey" class="made-with-love">
            made with love for privacy by
          </ion-text>
          <div class="la-logo"></div>
        </ion-col>
      </ion-row>
    </ion-grid>
  </ion-footer>
</template>

<script>
import { IonFooter, IonGrid, IonRow, IonCol, IonText } from '@ionic/vue';

export default {
  name: 'footer',
  components: {
    IonFooter,
    IonGrid,
    IonRow,
    IonCol,
    IonText,
  },
};
</script>

<style lang="css" scoped>
ion-footer > ion-grid {
  max-width: var(--max-width);
}

ion-footer {
  margin-bottom: 8px;
}

.la-logo {
  background-image: url('/assets/logos/la-logo.svg');
  width: 143px;
  height: 28px;
  background-repeat: no-repeat;
  background-size: 140px;
}

.made-with-love-container {
  flex: 0 0 360px;
  width: 360px;
  max-width: 360px;
}

.footer {
  text-align: center;
}

.made-with-love-container {
  padding-left: 0;
}

.links {
  width: 100%;
  max-width: 380px;
  margin-left: var(--main-left-margin);
  margin-right: 4vw;
  font-size: 0.8rem;
}

.la-logo {
  margin-right: var(--main-right-margin);
}

.links > * {
  text-align: center;
}

@media screen and (max-width: 716px) {
  .footer {
    width: 100% !important;
    max-width: 100% !important;
    flex-basis: 100% !important;

    justify-content: center;
  }

  .links {
    margin-right: 6vw;
    margin-left: 16px;
  }

  .made-with-love-container {
    display: none;
  }
}

@media screen and (min-width: 717px) {
  .mobile-la-logo {
    display: none;
  }
}

@media screen and (max-width: 474px) {
  .links {
    margin-bottom: var(--md-margin);
    justify-content: space-between !important;
    margin-right: var(--main-right-margin);
  }

  .made-with-love-container {
    display: flex;
    justify-content: center !important;
    flex: 0 0 100%;
    width: 100%;
    max-width: 100%;
  }

  .la-logo {
    margin-right: 0;
  }

  .mobile-la-logo {
    display: none;
  }
}

@media screen and (max-width: 346px) {
  .links {
    margin-left: calc(var(--main-left-margin) / 2);
    margin-right: var(--main-right-margin);
  }
}

@media screen and (max-width: 387px) {
  .made-with-love {
    display: none;
  }
}
</style>
