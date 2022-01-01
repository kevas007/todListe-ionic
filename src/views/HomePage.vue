<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Home</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <SimpleModale :isOpen="modalInfo.show" @modal-close="handleModalClose" />
      <ion-button @click="showModal">Add Item</ion-button>
      <!-- <pre>{{ JSON.stringify(displayList, null, 2) }}</pre> -->
      <div v-for="item in displayList" :key="item.id">
        <ion-item>
          <ion-label>
            <div>{{ item.title }}</div>
            <div class="ion-text-wrap">{{ item.description }}</div>
            <div class="ion-text-wrap">{{ item.dueDate }}</div>
            <p> id:{{ item.id }}</p>
          </ion-label>
          <ion-button @click.self="handleDelete(item)" slot="end" fill="clear">
            <ion-icon :icon="trashSharp" slot="icon-only" color="danger"></ion-icon>
          </ion-button>
        </ion-item>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { trashSharp } from 'ionicons/icons';
import { IonContent, IonHeader, IonPage, IonTitle, IonButton, IonToolbar, IonItem, IonIcon } from '@ionic/vue';
import { defineComponent, reactive, computed } from 'vue';
import SimpleModale from './SimpleModale.vue';
export default defineComponent({
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButton,
    SimpleModale,
    IonItem,
    IonIcon
  },
  setup() {
    const listData = reactive<Map<string, any>>(new Map<string, any>());
    const modalInfo = reactive<{ show: boolean, data: any }>({
      show: false,
      data: null
    });
    const displayList = computed(() => Array.from(listData.values()));

    //delete function
    const handleDelete = (item: any) => {
      listData.delete(item.id);
    };
    //show modal
    const showModal = () => {
      modalInfo.show = true;
    };
    const handleModalClose = (eventData: any) => {
      modalInfo.show = false;
      if (eventData.isClose) {
        alert('Modal Cancelled');
      } else {
        const id = new Date().getTime().toString();
        listData.set(id, {
          id,
          ...eventData.formInfo
        })
      }
      modalInfo.data = eventData;
    };
    return {
      //function 
      showModal,
      handleModalClose,
      handleDelete,
      //proporties
      modalInfo,
      displayList,
      trashSharp
    }
  }
});
</script>

<style scoped>
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>