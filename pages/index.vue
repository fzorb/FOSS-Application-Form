<template>
 <div class="container-sm pt-4" style="max-width: 48rem;">
   <b-card header="Apply for a whitelist" footer="Troll requests will be ignored." bg-variant="light">
     <b-card-body>
       Due to FZMC being an invite-only server, we ask you to apply in order to be granted access.
       Your access will be voided if you are caught violating our <NuxtLink to="/rules">rules</NuxtLink>
       <p>Even if you can connect right now to FzMC with the IP <code>{{ ip }} ({{ altip }})</code>, you are not whitelisted.</p>
       <hr>
       <b-form @submit="onSubmit" @reset="onReset" v-if="show">
         <b-form-group
           id="input-group-1"
           label="Email address:"
           label-for="input-1"
           description="We'll never share your email with anyone else. This is mostly used to send you information on moderation"
         >
           <b-form-input
             id="input-1"
             v-model="form.email"
             type="email"
             placeholder="coolboy889@gmail.com"
             required
           ></b-form-input>
         </b-form-group>

         <b-form-group id="input-group-2" label="Your IGN:" label-for="input-2">
           <b-form-input
             id="input-2"
             v-model="form.name"
             placeholder="CoolBoy889"
             required
           ></b-form-input>
         </b-form-group>

         <b-form-group id="input-group-2" label="A social media account you own:" label-for="input-2" description="Please input the code below this form in the bio/description">
           <b-form-input
             id="input-2"
             v-model="form.social"
             placeholder="twitter.com/CoolBoy889"
             required
           ></b-form-input>
         </b-form-group>

         <b-form-group id="input-group-3" label="Minecraft Edition:" label-for="input-3">
           <b-form-select
             id="input-3"
             v-model="form.edition"
             :options="editions"
             required
           ></b-form-select>
         </b-form-group>

         <b-form-group id="input-group-4" v-slot="{ ariaDescribedby }">
           <b-form-checkbox-group
             v-model="form.checked"
             id="checkboxes-4"
             :aria-describedby="ariaDescribedby" required
           >
             <b-form-checkbox value="age" required>I am at least 13 years of age</b-form-checkbox>
             <b-form-checkbox value="rule" required>I have red the <NuxtLink to="/rules">rules</NuxtLink>.</b-form-checkbox>
           </b-form-checkbox-group>
         </b-form-group>

         <b-button type="submit" variant="primary">Submit</b-button>
         <b-button type="reset" variant="danger">Reset</b-button>
       </b-form>
       <div v-if="show == false">
         <div class="d-flex container justify-content-center pb-3">
         <svg xmlns="http://www.w3.org/2000/svg" width="96" height="96" fill="currentColor" class="bi bi-emoji-smile" viewBox="0 0 16 16">
           <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
           <path d="M4.285 9.567a.5.5 0 0 1 .683.183A3.498 3.498 0 0 0 8 11.5a3.498 3.498 0 0 0 3.032-1.75.5.5 0 1 1 .866.5A4.498 4.498 0 0 1 8 12.5a4.498 4.498 0 0 1-3.898-2.25.5.5 0 0 1 .183-.683zM7 6.5C7 7.328 6.552 8 6 8s-1-.672-1-1.5S5.448 5 6 5s1 .672 1 1.5zm4 0c0 .828-.448 1.5-1 1.5s-1-.672-1-1.5S9.448 5 10 5s1 .672 1 1.5z"/>
         </svg></div>
         Thanks for applying! If you do not get whitelisted in the next 24-96 hours, you most likely have been denied. You can now leave this page.
       </div>
       <hr>
       <p>
         Your code: <code>{{ code }}</code>
       </p>
     </b-card-body>
   </b-card>
 </div>
</template>

<script>
export default {
  data() {
    return {
      ip: 'mc.fzorb.xyz',
      altip: '140.238.152.203:8123',
      code: Math.random() * (9999 - 1) + 1,
      form: {
        email: '',
        name: '',
        social: '',
        edition: null,
        checked: []
      },
      editions: ['Minecraft Pocket Edition', 'Minecraft Console Edition', 'Minecraft Windows 10 Edition', 'Minecraft Java Edition'],
      show: true
    };

  },
  methods: {
    onSubmit(event) {
      event.preventDefault()
      let hook = "hidden_for_obvious_reasons"
      const request = new XMLHttpRequest();
      request.open("POST", hook);

      request.setRequestHeader('Content-type', 'application/json');

      var parsedname;

      if (this.form.edition != 'Minecraft Java Edition') {
        parsedname = '*' + this.form.name;
      } else {
        parsedname = this.form.name;
      }

      const params = {
        username: "New application!",
        avatar_url: "",
        content: "> **New Applicant!**\n" +
          "IGN: " + this.form.name + "\n" +
          "Edition " + this.form.edition + "\n" +
          "Social Account " + this.form.social + this.code + "\n" +
          "Quick Command ```/whitelist add " + parsedname + "```"
      }

      request.send(JSON.stringify(params));
      //alert(JSON.stringify(this.form))
      this.show = false

    },
    onReset(event) {
      event.preventDefault()
      // Reset our form values
      this.form.email = ''
      this.form.name = ''
      this.form.social = ''
      this.form.edition = null
      this.form.checked = []
      // Trick to reset/clear native browser form validation state
      this.show = false
      this.$nextTick(() => {
        this.show = true
      })
    }
  },
  name: "index"
}
</script>

<style scoped>

</style>
