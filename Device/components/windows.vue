<template>
      <view class="container">
        
        <!-- List of windows -->
        <scroll-view :content-container-style="{contentContainer: {flex:1}}">
            <!-- Buttons for each window -->
            <view v-for="window in retrievedInfo" :key="window.key">
                <view class="windowContainer">
                    <text class="WindowText">Window with id: 
                      <text class="dynamicText">{{window.key}} </text> owned by 
                      <text class="dynamicText">{{window.owner}} </text> is currently :
                      <text class="dynamicText">{{window.status}} </text> 
                    </text>

                    <view class="topBtn">
                        <view class="topBtnLeft">
                            <button  title="Open Window" @press="openWindow(window)"></button>
                        </view>

                        <view class="topBtnRight">
                            <button  title="Close Window" @press="closeWindow(window)"></button>
                        </view>
                        
                    </view>
                </view>
            </view>
        </scroll-view>
  </view>
</template>

<script>
var firebase = require("firebase/app");

export default {
  
  data(){
    return {
      information : "",
      retrievedInfo : [],
      windowOpen: "false",
    } 
  },

  methods: {

    recOpen() {
      console.log("Window opened")
      
        var timeout = setTimeout(this.recOpen("Opened"), 1000);
        // console.log("in opened loop")
      // if(status == "Closed") {
      //   clearTimeout(timeout)
      //   console.log("in else condition")
      // }
    },

        openWindow(window){
            if (window.status != "Opened") {
                this.information.child(window.key).update({windowStatus: "Opened"})
                console.log("Window opened")

                // this.recOpen()

            } else {
                console.log("Window alreadey opened")
            }
            // this.information.child(key).update({windowStatus: "Opened"})
            
        },

        closeWindow(window){
            // this.information.push({windowStatus: "Closed"})
            if (window.status != "Closed") {
                this.information.child(window.key).update({windowStatus: "Closed"})

                // this.recOpen("Closed")
                // clearTimeout(this.OpenTimeout)
                console.log("Window closed")
            } else {
                // console.log(window)
                console.log("Window already closed")
            }
            

        },

  },

  mounted(){
      // try {
      //   firebase.initializeApp(firebaseConfigDevice);
      // } catch {
      //   var device = firebase.initializeApp(firebaseConfigDevice, "secondary");
      // }
      

        this.information = firebase.database().ref("window");
        firebase
        .database()
        .ref("window") // name of the collection
        .on("value", (snap) => { 
            // Empty the all showing messages to avoid duplicates
            this.retrievedInfo = [];
            const retrValue = snap.val();

            // Loads messages 
            for (const key in retrValue) {
                const dbWindow = retrValue[key];

                if (dbWindow.windowStatus && dbWindow.owner) {
                    const status = dbWindow.windowStatus;
                    const owner = dbWindow.owner;
                    // console.log(dbMessage.text);
                    this.retrievedInfo.push({ key, status, owner });
                    // var animate = this.$refs.scrlview
                    // animate.scrollToEnd({animated: true})
                }
            }
        })
    }

}
</script>

<style>
.container {
  background-color: #0a84ff;
  align-items: center;
  flex: 1;
  
}
.topBtn{
  display: flex;
  flex-direction: row;
  width: 100%;
  margin-top: 5%;
  margin-bottom: 5%;
}

.topBtnRight{
  margin-left: auto;
  margin-right: auto;
}
.topBtnLeft{
    margin-right:auto;
    margin-left: auto;
}

.midBtn{
    margin-top: 2%;
    margin-bottom: 5%;
}

.WindowText {
  font-size:16;
  color:white;
  margin-top: 5%;
}

.dynamicText{
  font-size:16;
  color:white;
  font-weight: bold;
}

.windowContainer{
  border-color: whitesmoke;
  border-width: 2px;
  border-radius: 5;
  margin-top: 3%;
  padding-left: 5px;
  padding-right: 5px;
  margin-left: 2%;
  margin-right: 2%;
}
</style>
