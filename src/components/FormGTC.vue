<template>
    <FormKit
        type="form"
        submit-label="Save"
        @submit="saveMe"
      >
        <FormKit
            type="datetime-local"
            label="Time to execute"
            name="executeAt"
            :step="1"
        />
        <FormKit
            type="text"
            label="username"
            name="username"
            validation="required"
            help="Only approved usernames will work"
        />
        <FormKit
            type="password"
            label="password"
            name="password"
            validation="required"
        />
        <FormKit
            type="text"
            label="userId"
            name="userId"
            validation="required|number"
            help="This is not login username, it is the userId to be used for reservation"
        />
        <FormKit
            type="radio"
            name="intervalInMins"
            label="Interval In Mins"
            :options="[30, 45, 60, 90]"
            validation="required"
        />
        <FormKit
            type="radio"
            label="Location to Reserve"
            name="locationToReserve"
            validation="required"
            :options="{ 
                1: 'Tennis', 
                35: 'Pickleball / Mini Tennis',
            }"
        />
        <FormKit
            type="datetime-local"
            label="Reserve Court Time"
            name="timeToReserveDateTime"
            validation="required"
        />
        <FormKit
            :value="false"
            type="checkbox"
            name="isConfirmed"
            label="Actually Make Reservation (over not reserving/testing)"
        />
        <FormKit
            type="number"
            help="How many times should we retry?"
            label="Retry Count"
            name="retryCount"
            value="0"
            min="0"
            max="10"
            step="1"
        />
    </FormKit>
</template>

<script>


export default {
    name: 'FormGTC',
    methods: {
      //values, {setSubmitting, setErrors, setStatus, resetForm}
        async saveMe(values) {
          console.log(values)
          this.$emit('requestTime', values);
        }
    }
}
</script>


<style>
li {
   list-style-type:none;
}
p {
  font-size: 0.9em;
  color: #646464;
  line-height: 1.5;
}
h1 {
  margin-top: 0;
}
h2 {
  color: green;
}
hr {
  display: block;
  height: 1px;
  margin: 1.5em 0;
  border: 0;
  background-color: #e4e4e4;
}
.formkit-form {
  width: 420px;
  padding: 1.5em;
  border: 1px solid #e4e4e4;
  border-radius: 1em;
  margin: 0 auto 1em auto;
}
.hide {
  display: none;
}
#registration-example pre {
  margin-bottom: 10px;
}
@media (min-width: 400px) {
  .double {
    display: flex;
    justify-content: space-between;
  }

  .double > .formkit-outer {
    width: calc(50% - 0.5em);
  }
}
</style>