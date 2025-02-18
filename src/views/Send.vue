<template>
  <div class="relative">
    <SendDefault
      :active="onStep(SendStep.Default)"
      :select="select"
    ></SendDefault>
    <SendInstructions
      :active="onStep(SendStep.Instructions)"
      :back="backFrom(SendStep.Instructions)"
      :file="file"
    ></SendInstructions>
    <SendProgress
      :active="onStep(SendStep.Progress)"
      :back="goToStep(SendStep.Default)"
    ></SendProgress>
    <SendComplete
      :active="onStep(SendStep.Complete)"
      :sendMore="sendMore"
    ></SendComplete>
    <input ref="fileInput" type="file" class="ion-hide" @change="fileChanged" />
  </div>
</template>

<style scoped>
/*
.fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
}

.fade-enter-from, .fade-leave-to {
    opacity: 0;
}
 */
</style>

<style lang="css">
/*
.modal {
//--min-width: 100vw; //--min-height: 100vh; max-height: 500px;
    max-width: 700px;
} */

.size {
  font-size: small;
}

.filename {
  font-weight: bold;
}
</style>

<script lang="ts">
import SendComplete from '@/components/send/SendComplete.vue';
import SendDefault from '@/components/send/SendDefault.vue';
import SendInstructions from '@/components/send/SendInstructions.vue';
import SendProgress from '@/components/send/SendProgress.vue';
import { RESET_PROGRESS, SEND_FILE, SET_FILE_META } from '@/store/actions';
import { SendStep } from '@/types';
import { defineComponent } from 'vue';
import { mapActions, mapMutations } from 'vuex';

declare interface SendData {
  step: SendStep;
  file?: File;
}

export default defineComponent({
  name: 'Send',
  data(): SendData {
    return {
      step: SendStep.Default,
      file: undefined,
    };
  },
  beforeUpdate() {
    if (typeof this.$route.query.select !== 'undefined') {
      this.$router.replace('/s');
      this.select();
    }
  },
  methods: {
    ...mapActions([SEND_FILE]),
    ...mapMutations([SET_FILE_META, RESET_PROGRESS]),
    select(file?: File) {
      if (typeof file === 'undefined') {
        (this.$refs.fileInput as HTMLInputElement).click();
      } else {
        this.file = file;
        this.step = SendStep.Instructions;
        this.sendFile();
      }
    },
    fileChanged() {
      const fileInput = this.$refs.fileInput as HTMLInputElement;
      if (fileInput.files!.length > 0) {
        this.file = fileInput.files![0] as File;
        this.sendFile();
        this.step = SendStep.Instructions;
      }
    },
    // TODO: refactor.
    async sendFile(): Promise<void> {
      const progressNext = this.nextFrom(SendStep.Instructions);
      const opts = {
        progressFunc: progressNext,
        size: this.file?.size,
      };
      const payload = { file: this.file, opts };
      const p = this[SEND_FILE](payload);
      this.step = SendStep.Instructions;
      try {
        const { done } = await p;
        await done;
        this.step = SendStep.Complete;
      } catch (err) {
        // TODO: error handling / message
        console.error(err);
        // NB: error during transfer.
        this.step = SendStep.Default;
      }
    },
    sendMore(): void {
      this[SET_FILE_META]({ name: '', size: 0 });
      this.step = SendStep.Default;
      this[RESET_PROGRESS]();
      this.$router.replace('/s');
    },
    onStep(step: SendStep): boolean {
      return this.step === step;
    },
    stepForward() {
      if (this.step < SendStep.Complete) {
        this.step++;
      } else {
        this[SET_FILE_META]({ name: '', size: 0 });
        this.step = SendStep.Default;
        this[RESET_PROGRESS]();
      }
    },
    stepBack() {
      if (this.step > SendStep.Default) {
        this.step--;
      }
    },
    nextFrom(step: SendStep): () => void {
      return (): void => {
        if (this.step === step) {
          this.stepForward();
        }
      };
    },
    backFrom(step: SendStep): () => void {
      return (): void => {
        if (this.step === step) {
          this.stepBack();
        }
      };
    },
    goToStep(step: SendStep): () => void {
      return (): void => {
        this.step = step;
      };
    },
  },
  components: {
    SendDefault,
    SendInstructions,
    SendProgress,
    SendComplete,
  },
  setup() {
    return {
      SendStep,
    };
  },
});
</script>
