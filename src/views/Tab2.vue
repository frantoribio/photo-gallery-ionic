<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Galería de Fotos</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col size="6" :key="photo" v-for="photo in photos">
            <ion-img :src="photo.webviewPath" @click="showActionSheet(photo)"></ion-img>
          </ion-col>
        </ion-row>
      </ion-grid>
      <ion-fab vertical="bottom" horizontal="center" slot="fixed">
        <ion-fab-button @click="takePhoto()">
          <ion-icon :icon="camera"></ion-icon>
        </ion-fab-button>
      </ion-fab>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
// Iconos
import { camera, trash, close } from 'ionicons/icons';
// Elementos a usar
import {
  actionSheetController,
  IonPage,
  IonHeader,
  IonFab,
  IonFabButton,
  IonIcon,
  IonToolbar,
  IonTitle,
  IonContent,
  IonGrid,
  IonRow,
  IonCol,
  IonImg,
} from '@ionic/vue';

import { usePhotoGallery, Photo } from '@/composables/usePhotoGallery';

export default {
  name: 'Tab2',
  // Mis componentes
  components: {
    IonPage,
    IonHeader,
    IonFab,
    IonFabButton,
    IonIcon,
    IonToolbar,
    IonTitle,
    IonContent,
    IonGrid,
    IonRow,
    IonCol,
    IonImg,
  },

  // Mis setup
  setup() {
    // Impirto de otros lados
    const { photos, takePhoto, deletePhoto } = usePhotoGallery();

    // Cuadro de diálogo
    const showActionSheet = async (photo: Photo) => {
      const actionSheet = await actionSheetController.create({
        header: '¿Eliminar Foto?',
        buttons: [{
          text: 'Eliminar',
          role: 'destructive',
          icon: trash,
          handler: () => {
            deletePhoto(photo);
          },
        }, {
          text: 'Cancelar',
          icon: close,
          role: 'cancel',
          handler: () => {
            // Nothing to do, action sheet is automatically closed
          },
        }],
      });
      await actionSheet.present();
    };

    // Lo que le devulevo
    return {
      photos,
      takePhoto,
      showActionSheet,
      camera,
      trash,
      close,
    };
  },
};
</script>
