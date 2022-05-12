<template>
  <div class="q-pa-md q-gutter-sm">
    <q-btn label="Add Data" color="primary" no-caps @click="prompt = true" />

    <q-dialog v-model="prompt" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">KPI1</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input dense v-model="content.kpi1" autofocus @keyup.enter="prompt = false" />
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" v-close-popup />
          <q-btn flat label="Submit" @click.prevent="submitData" />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <q-separator/>
    <div class="q-pa-md" style="max-width: 350px">
    <q-list bordered separator>
      <q-item clickable v-ripple>
        <q-item-section><q-chip class="q-mt-xs q-ml-sm" square 
                  :color="(contents.kpi1 <= '30')?'red':(contents.kpi1 > '30' && contents.kpi1 <= '40')?'orange':(contents.kpi1 > '40' && contents.kpi1 < '50')?'yellow-7':(contents.kpi1 >= '50' && contents.kpi1 < '700')?'green':(contents.kpi1 >= '700'?'blue':'grey-2')">
                    {{contents.kpi1}}
                    <q-tooltip>KPI 1</q-tooltip>
                  </q-chip> </q-item-section>
      </q-item>
    </q-list>
  </div>
  </div>
</template>

<script>
import db from 'src/boot/firebase'

export default {
  name:'firebaseInput',    
  data () {    
    return {
      prompt: false,
      content : {
        kpi1: null,
      },
      contents: []
    }
  },
  methods: {
    readData(){
      db.collection("contents").get().then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
              // doc.data() is never undefined for query doc snapshots
              //console.log(doc.id, " => ", doc.data());
              this.contents = doc.data();
          });
      });
    },

    submitData(){
      // NOW THIS MEANS IS UPDATE DATA 
      var updatenRef = db.collection("contents").doc("osman");

        // Set the "capital" field of the city 'DC'
        return updatenRef.update({
            kpi1: this.content.kpi1
        })
        .then(() => {
            console.log("Document successfully updated!");
        })
        .catch((error) => {
            // The document probably doesn't exist.
            console.error("Error updating document: ", error);
        });


    /*    THIS COMMENT AREA IS ADD DATA TO FIRESTORE
    //  db.collection("contents").add(this.content)
    //    .then((docRef) => {
    //        console.log("Document written with ID: ", docRef.id);
    //        this.readData();
    //    })
    //    .catch((error) => {
    //        console.error("Error adding document: ", error);
    //    });
    */
    },
    reset(){
    //  Object.assign(this.$data, this.$options.data.apply(this));
    }
  },
  created(){
    this.readData()
  }
}
</script>
