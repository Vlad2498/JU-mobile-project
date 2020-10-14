<template>
      <view class="container">
        
        <!-- List of windows -->
        <scroll-view :content-container-style="{contentContainer: {flex:1}}">
            <!-- Buttons for each window -->
            <view v-for="window in retrievedInfo" :key="window.key">
                <view class="windowContainer z-depth-1">
                    <text class="WindowText">Window with id: 
                      <text class="dynamicText">{{window.key}} </text> owned by 
                      <text class="dynamicText">{{window.owner}} </text> is currently :
                      <text class="dynamicText">{{window.status}} </text> 
                    </text>

                    <image v-if="window.status == 'Opened'" :source= "require('../assets/opened.jpg')" class="imageContainerOpened"/>
                    <image v-else :source= "require('../assets/closed.jpg')" class="imageContainerClosed"/> 

                    <view class="topBtn">
                        <view class="topBtnLeft">
                            <button  title="Open Window" @press="openWindow(window)" color = "#007aff"></button>
                        </view>

                        <view class="topBtnRight">
                            <button  title="Close Window" @press="closeWindow(window)" color = "#ff954f"></button>
                        </view>
                        <view class="topBtnRight">
                            <button  title="Loop" @press="recOpen(window.status)" color = "#ff954f"></button>
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
      timeout: [],
    } 
  },

  methods: {

    recOpen(status) { 
      // console.log("called loop")
      // if (status == "Opened"){
      // console.log("Window opened in one second loops")
      //   this.timeout = setTimeout(recOpen("Opened"), 1000);
      // }else if(status == "Closed") {
      //   clearTimeout(this.timeuot)
      //   console.log("in else condition")
      // }

      var opnTimer
      var clsdTimer 

        function loop(){
          
          // var clsdTimer = {};
          if(status == "Opened") {
            opnTimer = setTimeout(loop,1000)
            console.log("Window open in loop every second")
            clearTimeout(opnTimer) //If this function is here, or in the same scope, it will work. If it's outside this if statement like in the i(status == closed) then it will not work
            if(opnTimer) {
              // clearTimeout(opnTimer)
            }
            // console.log(opnTimer)
            

            
          } else if(status == "Closed") {
            
            // clearTimeout(opnTimer) 
            clsdTimer = setTimeout(loop, 2000)
            console.log("Windwos is closed in loop every 2 seconds")
            clearTimeout(clsdTimer)
            
          }
        }

        loop();
    },

        openWindow(window){
            if (window.status != "Opened") {
                this.information.child(window.key).update({windowStatus: "Opened"})
                console.log("Window opened")

                // this.recOpen()

            } else {
                console.log("Window alreadey opened")
            }            
        },

        closeWindow(window){
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
        this.information = firebase.database().ref("window");
        firebase
        .database()
        .ref("window") // name of the collection
        .on("value", (snap) => { 
            // Empty the all showing information to avoid duplicates
            this.retrievedInfo = [];
            const retrValue = snap.val();

            // Loads information 
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
  background-color: #8e8e93;
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
  color:black;
  margin-top: 5%;
}

.dynamicText{
  font-size:16;
  color:black;
  font-weight: bold;
}

.windowContainer{
  border-color: whitesmoke;
  background-color: white;
  border-width: 2px;
  border-radius: 15;
  margin-top: 5%;
  padding-left: 5px;
  padding-right: 5px;
  margin-left: 2%;
  margin-right: 2%;
  
}

.imageContainerClosed{
  width: 200px;
  height: 180px;
  margin-left: auto;
  margin-right: auto;
  margin:5px;
}

.imageContainerOpened{
  width: 250px;
  height: 180px;
  margin-left: auto;
  margin-right: auto;
  margin:5px;
}
</style>
