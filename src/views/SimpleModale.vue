<template>
    <ion-modal :is-open="isOpen" @onDidDismiss="handleDidDismiss(true)">
        <ion-page>
            <ion-header>
                <ion-toolbar>
                    <ion-title>Simple Modal</ion-title>
                </ion-toolbar>
            </ion-header>
            <ion-content class="ion-padding">
                <ion-item>
                    <ion-label position="stacked">Title</ion-label>
                    <ion-input v-model="formInfo.title"></ion-input>
                </ion-item>
                <ion-item>
                    <ion-label position="stacked">Description</ion-label>
                    <ion-textarea v-model="formInfo.description" rows='4'></ion-textarea>
                </ion-item>
                <ion-item>
                    <ion-label position="stacked">Due Date</ion-label>
                    <ion-datetime
                    display-format="MM DD YY"
                    v-model="formInfo.dueDate"
                    ></ion-datetime>
                </ion-item>
                <ion-button @click="handleDidDismiss(false)">Save</ion-button>
                <ion-button color="danger" @click="handleDidDismiss(true)">Cancel</ion-button>
            </ion-content>
        </ion-page>
    </ion-modal>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonModal, IonButton,IonLabel, IonInput, IonItem, IonTextarea,IonDatetime } from '@ionic/vue';
import { defineComponent, ref, SetupContext } from 'vue'

export default defineComponent({
    name: 'simple-modal',
    components: {
        IonContent,
        IonHeader,
        IonPage,
        IonTitle,
        IonToolbar,
        IonModal,
        IonButton,
        IonLabel, 
        IonInput, 
        IonItem ,
        IonTextarea,
        IonDatetime 

    },
    props: {
        isOpen: {
            //   type: Boolean,
            default: false,
            required: true
        },

    },
    setup(props: any, ctx: SetupContext) {
        const formInfo = ref<any>({
            title: '',
            description: '',
            dueDate: ''
        });
        const handleDidDismiss = (isClose: boolean) => {
        // this is to ignore the onDidDismiss event when the modal is closed by clicking outside the modal
            if ( !props.isOpen) return;
            console.log('isClose', isClose);
            ctx.emit('modal-close', {
                isClose,
                formInfo: !isClose ? formInfo.value : null,
                });
                formInfo.value = {};
        };
        return {
            handleDidDismiss,
            formInfo
        }
    }

})

</script>