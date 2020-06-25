<template>
  <q-page>
    <q-list>
      <q-item v-for="(market, i) in 5" :key="i" class="q-my-sm" clickable v-ripple>
        <q-item-section>
          <q-item-label>99 Villa Lucre</q-item-label>
        </q-item-section>

        <q-item-section side>
          <q-icon name="keyboard_arrow_right" color="green" />
        </q-item-section>
      </q-item>
    </q-list>
    <q-dialog v-model="prompt" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Add supermarket</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input dense v-model="marketName" autofocus @keyup.enter="prompt = false" />
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" v-close-popup />
          <q-btn flat label="Save" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <q-btn fab icon="add" color="accent" @click="prompt = true" />
    </q-page-sticky>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      prompt: false,
      marketName: ""
    };
  }
};
</script>



