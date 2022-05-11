<template>
  <div class="bg-home">

    <!-- For Component View -->
    <div class="head-top shadow">
      <div class="d-flex">
        <!-- <div class="d-flex">
          <label class="mr-3" for="guru">Nama Guru :</label>
          <p class="pl-3">Guru Solay</p>
          <button @click="test">Test</button>
        </div> -->
      </div>
    </div>
    <div class="flex-zoom">
      <div id="meetingSDKElement">
        <!-- Zoom Meeting SDK Component View Rendered Here -->
      </div>
      <div class="participant-view shadow">
        <div class="row mx-0 py-3">
          <div class="col-12" v-for="item in 3" :key="item.id">
            <div class="w-100 camera-view">

            </div>
          </div>
        </div>
      </div>
      <div class="chat-view">
        <div id="meetingSDKChatElement"></div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ZoomMtgEmbedded from '@zoomus/websdk/embedded';

export default {
  name: 'HelloWorld',
  created () {
    setTimeout(() => {
      this.getSignature()
    }, 200);
  },
  data () {
    return {
      client: ZoomMtgEmbedded.createClient(),
      // This Sample App has been updated to use SDK App type credentials https://marketplace.zoom.us/docs/guides/build/sdk-app
      // sdkKey: "cwJtHDrb3heoCThTlBaKmaroh9zVrqxBTgLY",//bagus
      sdkKey:"Jwdp8A1p7pur6gPlgugOPbkJhFqM4roRGOxl", //kp
      // sdkKey:"7kn90dOqRDuLxujbNFM7Ew", //API KEY
      meetingNumber: this.$route.query.meetingId,
      passWord: this.$route.query.passwordMeeting,
      role: this.$route.query.role,
      signatureEndpoint: "http://zoom.kelaspintar.co.id:4000/",
      userEmail: "",
      userName: this.$route.query.name,
      // pass in the registrant's token if your meeting or webinar requires registration. More info here:
      // Meetings: https://marketplace.zoom.us/docs/sdk/native-sdks/web/component-view/meetings#join-registered
      // Webinars: https://marketplace.zoom.us/docs/sdk/native-sdks/web/component-view/webinars#join-registered
      registrantToken: ''
    }
  },
  methods: {
    test() {
      console.log(this.client.getAttendeeslist())
    },
    getSignature() {
      axios.post(this.signatureEndpoint, {
        meetingNumber: this.meetingNumber,
        role: this.role
      })
      .then(res => {
        console.log(res.data.signature);
        this.startMeeting(res.data.signature);
      })
      .catch(error => {
        console.log(error);
      });
    },
    startMeeting(signature) {
      let meetingSDKElement = document.getElementById('meetingSDKElement');
      let meetingSDKChatElement = document.getElementById('meetingSDKChatElement')

      this.client.init({
        debug: true,
        zoomAppRoot: meetingSDKElement,
        language: 'en-US',
        customize: {
          meetingInfo: ['topic', 'host', 'mn', 'pwd', 'telPwd', 'invite', 'participant', 'dc', 'enctype'],
          toolbar: {
            buttons: [
              {
                text: 'Custom Button',
                className: 'CustomButton',
                onClick: () => {
                  console.log('custom button');
                }
              }
            ]
          },
          video: {
            isResizable: false,
            viewSizes: {
              default: {
                width: document.getElementById('meetingSDKElement').offsetWidth,
                height: document.getElementById('meetingSDKElement').offsetHeight
              },
              ribbon: {
                width: 300,
                height: 700
              }
            },
            popper: {
              disableDraggable: true
            }
          },
          chat: {
            popper: {
              disableDraggable: true,
              anchorElement: meetingSDKChatElement,
              placement: 'top'
            }
          }
        }
      });

      this.client.join({
        sdkKey: this.sdkKey,
        signature: signature,
        meetingNumber: this.meetingNumber,
        password: this.passWord,
        userName: this.userName,
        userEmail: this.userEmail,
        tk: this.registrantToken
      })
    }
  },
  mounted() {
    console.log(this.$route)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#meetingSDKElement{
  width: 50%;
}
.bg-home{
  /* background: #7a796f; */
  padding: 50px;
}
.head-top{
  border-radius: 12px;
  padding: 30px;
  width: 75%;
  min-height: 200px;
  background: white;
}
.flex-zoom{
  display: flex;
  justify-content: start;
  margin-top: 30px;
}
.participant-view{
  margin-left: 20px;
  width: 20%;
  min-height: 300px;
  background: white;
  border-radius: 12px;
}
.chat-view{
  width: 30%;
}
.camera-view{
  height: 100px;
  background: #333;
  border-radius: 4px;
  margin-bottom: 20px;
}
.participant-view .col-12{
  padding: 0 20px;
}
</style>
<style>
main {
  width: 70%;
  margin: auto;
  text-align: center;
}

main button {
  margin-top: 20px;
  background-color: #2D8CFF;
  color: #ffffff;
  text-decoration: none;
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 40px;
  padding-right: 40px;
  display: inline-block;
  border-radius: 10px;
  cursor: pointer;
  border: none;
  outline: none;
}

main button:hover {
  background-color: #2681F2;
}

</style>
