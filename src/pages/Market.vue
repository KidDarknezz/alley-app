<template>
  <q-page class="q-pa-md">
    <div class="row q-mb-md">
      <q-input filled v-model="search" label="Buscar" clearable class="full-width" />
    </div>
    <div>
      <q-list class="full-width" bordered separator>
        <q-item v-for="(item, i) in searchFilter" :key="i">
          <q-item-section>{{ item.name }}</q-item-section>
          <q-item-section side>Pasillo {{ item.alley }}</q-item-section>
        </q-item>
      </q-list>
    </div>
    <q-dialog v-model="prompt" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Add item</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input dense v-model="newItem.name" label="Item" autofocus />
          <br />
          <q-input dense v-model="newItem.alley" label="Alley" />
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" v-close-popup />
          <q-btn flat label="Save" @click="postItem" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-page-sticky position="bottom-right" :offset="[18, 18]" v-if="param == 'p455w0rd'">
      <q-btn fab icon="add" color="accent" @click="prompt = true" />
    </q-page-sticky>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      itemsList: {},
      search: "",
      prompt: false,
      newItem: {
        name: "",
        alley: ""
      },
      param: ""
    };
  },
  methods: {
    postItem() {
      this.$http
        .post("https://alley-app-3d7b5.firebaseio.com/items.json", this.newItem)
        .then(response => {
          this.getItems();
          this.newItem.name = "";
          this.newItem.alley = "";
        });
    },
    getItems() {
      this.$http.get("https://alley-app-3d7b5.firebaseio.com/items.json").then(
        response => {
          this.itemsList = response.body;
        },
        response => {
          console.log(response);
        }
      );
    }
  },
  computed: {
    searchFilter() {
      let result = [];
      for (let item in this.itemsList) {
        if (
          this.itemsList[item].name
            .toLowerCase()
            .includes(this.search.toLowerCase())
        )
          result.push(this.itemsList[item]);
      }
      return result;
    }
  },
  beforeMount() {
    this.getItems();
    const queryString = window.location.search;
    const urlParams = new URLSearchParams(queryString);
    this.param = urlParams.get("pass");
    console.log(this.param);
  }
};
</script>
