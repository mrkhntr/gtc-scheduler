<template>
  <div>
    How to use: This app only works for pre-approved usernames. It is for 
    auto booking of a court at SF GTC.
    <br />
    To use it you need to know your login and userId for the person you 
    are booking to court on to find your userId login to GTC portal,
    open <a href="https://medium.com/@ConnorFinnegan/how-to-effectively-inspect-network-activity-in-chrome-dev-tools-fd18592a735a#:~:text=feature%20in%20Chrome.-,Getting%20Started,-To%20access%20this">network tools</a>
    and select your name again from the dropdown. You should see "switch-linked-user?", id= is the UserId you selected.
    <br />
    Its important to keep tab open and computer not asleep for it to trigger at
    time specified. You can test by not toggling "Actually Make Reservation".
  </div>
  <VueCountdown v-if="countdown !== null" :time="countdown"  :interval="100" v-slot="{ totalSeconds }"  @end="onCountdownEnd">
    Time Remaining： {{ totalSeconds }} seconds.
  </VueCountdown>
  <iframe v-if="response" :srcdoc="response" />
  <FormGTC @requestTime="onSubmit"/>
</template>

<script>
import FormGTC from './components/FormGTC.vue'
import VueCountdown from '@chenfengyuan/vue-countdown'
import axios from 'axios';

export default {
  name: 'App',
  components: {
    FormGTC,
    VueCountdown,
  },
  data() {
    return {
      error: null,
      countdown: null,
      toSend: null,
      response: null,
    }
  },

  methods: {
    onSubmit(values) {
      this.toSend = null;
      this.response = 'Attempting to make reservation'
      console.log('herer', values);
      this.countdown = values.executeAt ? (new Date(values.executeAt) - new Date()) : 0;
      console.log({ countdown: this.countdown, date: Math.floor(new Date(values.timeToReserveDateTime).getTime() / 1000)});
      const data = {
          username: values.username,
          password: values.password,
          userId: values.userId,
          intervalInMins: values.intervalInMins,
          timeToReserveSeconds: Math.floor(new Date(values.timeToReserveDateTime).getTime() / 1000),
          locationToReserve: values.locationToReserve,
          isConfirmed: values.isConfirmed,
        };
      console.log('Data to be sent', data);
      this.toSend = () => axios.post(
        'https://uglte8qxlf.execute-api.us-east-1.amazonaws.com/dev/gtc/reserve', 
        data
      )
    },
    async onCountdownEnd() {
      try {
        console.log('attempting to send');
        const result = await this.toSend();
        alert('sent sucessfully, see result below');
        console.log(result);
        this.response = result.data;
      } catch (error) {
        alert(`Sending the request resulted in systamic error ${error.message}`)
      }

    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
