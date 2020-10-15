<template>
    <view class="container">
        <text class="headline">This is the controller page for {{authUser.email}}</text>
        
        <!-- List of windows -->
        <scroll-view :content-container-style="{contentContainer: {flex:1}}">
            <!-- Buttons for each window -->
            <view v-for="window in retrievedInfo" :key="window.key" >
                <view v-if="window.owner == authUser.email" class="windowContainer z-depth-1">
                    <text class="WindowText">Window with id: 
                      <text class="dynamicText">{{window.key}} </text> is currently :
                      <text class="dynamicText">{{window.status}} </text> 
                      <text v-if="window.status != 'Requiring Configuration. Please open or close the window.'">
                      since :
                      <text class="dynamicText">{{ window.since}} </text> 
                      sec ago
                      </text>
                    </text>
                    <view class="topBtn">
                        <view class="topBtnLeft">
                            <button  title="Open Window" @press="openWindow(window)" color="green" ></button>
                        </view>

                        <view class="topBtnRight">
                            <button  title="Close Window" @press="closeWindow(window)" color=" rgb(203, 182, 26)" ></button>
                        </view>
                        <view class="topBtnRight">
                            <button  title="Delete" @press="deleteWindow(window.key)" color = "rgb(203, 26, 26)"></button>
                        </view>
                    </view>
                </view>
            </view>
        </scroll-view>

        <!-- add a new window -->
        <view class="midBtn">
            <button title="Add new window" @press="spawnWindow" color = "green"></button>
        </view>

        <!-- Sign Out button -->
        <view class="midBtn">
        <button  title="Sign out" @press="logout" color="rgb(203, 26, 26)"></button>
        </view>
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
            authUser : {}
        }
    },

    props: {
        navigation: {
        type: Object
        },
    },    

    methods: {
        goToLoginScreen() {
            this.navigation.navigate("Login");
        },

        spawnWindow(){
            this.information.push({windowStatus: "Requiring Configuration", owner: this.authUser.email, windowsSince: "0"})
        },


        openWindow(window){
            if (window.status != "Opened") {
                this.information.child(window.key).update({windowStatus: "Opened", windowsSince:"0"})
                console.log("Window opened")
            } else {
                console.log("Window alreadey opened")
            }
            // this.information.child(key).update({windowStatus: "Opened"})
            
        },

        closeWindow(window){
            // this.information.push({windowStatus: "Closed"})
            if (window.status != "Closed") {
                this.information.child(window.key).update({windowStatus: "Closed", windowsSince:"0"})
                console.log("Window closed")
            } else {
                // console.log(window)
                console.log("Window already closed")
            }
            

        },

        deleteWindow(key){
            this.information.child(key).remove();
            console.log("Window with id " + key + " removed")
        },

        logout(){
            var out = false
            firebase.auth().signOut().then(function() {
                console.log("Sign out successful")
                out = true
                // Sign-out successful.
            }).catch(function(error) {
                // An error happened.
            });
            if (out = true){ this.goToLoginScreen()}
        }
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

            firebase.auth().onAuthStateChanged(user => {
                if(user){
                    this.authUser = user
                    // console.log( "User is : " + this.authUser.email)
                } else {
                    this.authUser = {}
                }
            })

            // Loads information 
            for (const key in retrValue) {
                const dbWindow = retrValue[key];

                if (dbWindow.windowStatus && dbWindow.owner && dbWindow.windowsSince) {
                    const status = dbWindow.windowStatus;
                    const owner = dbWindow.owner;
                    const since = dbWindow.windowsSince
                    // console.log(dbMessage.text);
                    this.retrievedInfo.push({ key, status, owner, since });
                    // var animate = this.$refs.scrlview
                    // animate.scrollToEnd({animated: true})
                }
            }
        })
    }
}
</script>

<style >
.container {
  background-color:#315963;
  height: 100%;
  align-items: center;
  flex: 1;
}
.headline{
  margin-top: 10;
  margin-bottom: 10;
  font-size: 20;
  color:whitesmoke;
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
  color: rgb(203, 26, 26);
}
.topBtnLeft{
    margin-right:auto;
    margin-left: auto;
}

.midBtn{
    margin-top: 10;
    margin-bottom: 10;
    padding-bottom: 10;
}

.WindowText {
  font-size:16;
  color:black;
  margin-top: 5%;
}

.windowContainer{
  border-color: whitesmoke;
  background-color: white;
  border-width: 2px;
  border-radius: 5;
  margin-top: 3%;
  padding-left: 5px;
  padding-right: 5px;
  margin-left: 3%;
  margin-right: 3%;
}

.dynamicText{
  font-size:16;
  color:black;
  font-weight: bold;
}
</style>