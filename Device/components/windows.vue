<template>
  <view class="container">
    <!-- List of windows -->
    <scroll-view :content-container-style="{ contentContainer: { flex: 1 } }">
      <!-- Buttons for each window -->
      <view v-for="window in retrievedInfo" :key="window.key">
        <view class="windowContainer z-depth-1">
          <text class="WindowText"
            >Window with id:
            <text class="dynamicText">{{ window.key }} </text> owned by
            <text class="dynamicText">{{ window.owner }} </text> is currently :
            <text class="dynamicText">{{ window.status }} </text>
          </text>

          <image
            v-if="window.status == 'Opened'"
            :source="require('../assets/opened.jpg')"
            class="imageContainerOpened"
          />
          <image
            v-else
            :source="require('../assets/closed.jpg')"
            class="imageContainerClosed"
          />

          <view class="topBtn">
            <view class="topBtnLeft">
              <button
                title="Open Window"
                @press="openWindow(window)"
                color="#007aff"
              ></button>
            </view>

            <view class="topBtnRight">
              <button
                title="Close Window"
                @press="closeWindow(window)"
                color="#ff954f"
              ></button>
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
  data() {
    return {
      information: "",
      retrievedInfo: [],
      windowOpen: "false",
      timeout: [],
    };
  },

  methods: {
    recOpen() {
      for (var i = 1; i < 99999; i++) {
        clearInterval(i);
      }

      setInterval(() => {
        this.retrievedInfo.forEach((element) => {
          if (element.status == "Opened") {
            console.log("Updated Window with id " + element.key +  " opened in Firebase !");
            this.information.child(element.key).update({ windowStatus: element.status, windowsSince: (parseInt(element.since) + 1).toString() });
          }
      });
      }, 1000); //Do this action every 1s

       setInterval(() => {
        this.retrievedInfo.forEach((element) => {
          if (element.status != "Opened") {
            console.log("Updated Window with id " + element.key +  " closed in Firebase !");
            this.information.child(element.key).update({ windowStatus: element.status, windowsSince: (parseInt(element.since) + 60).toString() });
          }
        });
      }, 60000); //Do this action every 1min
    },

    openWindow(window) {
      if (window.status != "Opened") {
        this.information.child(window.key).update({ windowStatus: "Opened" , windowsSince:"0"});
        console.log("Window opened");

        // this.recOpen()
      } else {
        console.log("Window alreadey opened");
      }
    },

    closeWindow(window) {
      if (window.status != "Closed") {
        this.information.child(window.key).update({ windowStatus: "Closed", windowsSince:"0"});

        // this.recOpen("Closed")
        // clearTimeout(this.OpenTimeout)
        console.log("Window closed");
      } else {
        // console.log(window)
        console.log("Window already closed");
      }
    },
  },

  async mounted() {
    this.recOpen()
    this.information = await firebase.database().ref("window");
    await firebase
      .database()
      .ref("window") // name of the collection
      .on("value", (snap) => {
        // Empty the previously retrieved information to avoid duplicates
        this.retrievedInfo = [];
        const retrValue = snap.val();
        // Loads information
        for (const key in retrValue) {
          const dbWindow = retrValue[key];
          if (dbWindow.windowStatus && dbWindow.owner && dbWindow.windowsSince) {
            const status = dbWindow.windowStatus;
            const owner = dbWindow.owner;
            const since = dbWindow.windowsSince
            this.retrievedInfo.push({ key, status, owner, since });
          }
        }
      });
      
  },
};
</script>

<style>
.container {
  background-color: #8e8e93;
  align-items: center;
  flex: 1;
}
.topBtn {
  display: flex;
  flex-direction: row;
  width: 100%;
  margin-top: 5%;
  margin-bottom: 5%;
}

.topBtnRight {
  margin-left: auto;
  margin-right: auto;
}
.topBtnLeft {
  margin-right: auto;
  margin-left: auto;
}

.midBtn {
  margin-top: 2%;
  margin-bottom: 5%;
}

.WindowText {
  font-size: 16;
  color: black;
  margin-top: 5%;
}

.dynamicText {
  font-size: 16;
  color: black;
  font-weight: bold;
}

.windowContainer {
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

.imageContainerClosed {
  width: 200px;
  height: 180px;
  margin-left: auto;
  margin-right: auto;
  margin: 5px;
}

.imageContainerOpened {
  width: 250px;
  height: 180px;
  margin-left: auto;
  margin-right: auto;
  margin: 5px;
}
</style>
