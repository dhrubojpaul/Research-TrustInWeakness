<template>
<div>
  <v-row>
    <v-col cols=1 v-for="photo in images" :key=photo.photoID>
      <v-card @click="openPhoto(photo)" tile>
        <v-img contain :src="photo.src"></v-img>
      </v-card>
      <v-sheet class=mt-1 width=100% height=5px :color="getColor(photo)" tile></v-sheet>
    </v-col>
  </v-row>


   <v-dialog v-model="isPhotoShowing" fullscreen transition="dialog-bottom-transition" persistent>
        <v-row class=pa-12>
          <v-col cols=9>
            <v-card justify>
              <v-img contain :src="current.src"></v-img>
            </v-card>
          </v-col>
          <v-col cols=3>
            <v-card height=100% class=pa-6>
              <v-btn class=mb-6 block @click=analyze(current.photoID)>Analyze and Classify</v-btn>
              <p :style="'display:'+display.stage1">Analyzing...</p>
              <p :style="'display:'+display.stage2">Features: 1,2,3</p>
              <p :style="'display:'+display.stage3">Classifying...</p>
              <p :style="'display:'+display.stage4">Classified as Group2</p>
              <v-btn block class=mb-3 :style="'display:'+display.stage4" @click=accept>Accept</v-btn>
              <v-btn block :style="'display:'+display.stage4" @click=reject>Reject</v-btn>
            </v-card>
          </v-col>
        </v-row>
   </v-dialog>

   <v-snackbar v-model="isAlertShowing">This photo has been analyzed.</v-snackbar>
</div>
</template>

<script>
import images from "../data/images"
export default {
  data(){
    return {
      images,
      isPhotoShowing: false,
      current: {},
      display: {stage1:"none",stage2:"none",stage3:"none",stage4:"none"},
      isAlertShowing: false
    }
  },
  methods: {
    getColor(photo){
      if(photo.accepted){
        return "green";
      } else if (photo.rejected){
        return "red";
      } else {
        return "white";
      }
    },
    openPhoto(photo){
      if(photo.accepted || photo.rejected){
        this.isAlertShowing = true;
      } else {
        this.isPhotoShowing = true;
        this.current = photo;
      }
    },
    accept(){
      this.current.accepted = true;
      this.isPhotoShowing = false;
      this.display= {stage1:"none",stage2:"none",stage3:"none",stage4:"none"};
      this.current= {};
    },
    reject(){
      this.current.rejected = true;
      this.isPhotoShowing = false;
      this.display= {stage1:"none",stage2:"none",stage3:"none",stage4:"none"};
      this.current= {};
    },
    getPhotoByPhotoID(photoID){
      return this.images.find(function(item){return item.photoID == photoID});
    },
    analyze(photoID){
      var component = this;
      component.display.stage1 = "block";
      setTimeout(function(){
        component.display.stage2 = "block";
        setTimeout(function(){
          component.display.stage3 = "block";
            setTimeout(function(){
              component.display.stage4 = "block";
            },2000);
        },3000);
      },4000);
    }
  },
}
</script>
