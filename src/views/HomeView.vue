<script setup lang="ts">
import HeaderComponent from './HeaderComponent.vue'
import MailingBox from './MailingBox.vue'
import PacmanLoader from 'vue-spinner/src/DotLoader.vue';
</script>

<template>
  <main>
    <div class="page-wrapper">
      <HeaderComponent />
      <form @submit.prevent="submitRequest" method="post">
        <!-- <input type="hidden" name="subject" value="New Movie Request"> -->
        <!-- <input type="hidden" name="from_name" :value="formValues.name"> -->
        <!-- <input type="checkbox" name="botcheck" class="hidden" v-model="botcheck" style="display: none;"> -->
        <!-- <input type="hidden" name="_next" value="localhost:5173/thanks"> -->
        <div class="form-control">
          <input type="text" id="name" placeholder="Movie Name" autofocus="true" autocomplete="false"
            v-model="formValues.name" required>
        </div>
        <div class="form-control">
          <label for="quality">Select Desired Quality: </label>
          <select name="quality" id="quality" v-model="formValues.quality" required>
            <option value="480p">480p</option>
            <option value="720p">720p</option>
            <option value="1080p" selected>1080p</option>
            <option value="2160p">2160p</option>
            <option value="Highest Possible">Highest Possible</option>
          </select>
        </div>
        <div class="form-control">
          <label for="size">Select File Size Constraint: </label>
          <select name="size" id="size" v-model="formValues.size" required>
            <option value="low">Prefer Low Size (1GB-10GB)</option>
            <option value="med" selected>Somewhat Large Size (10GB-20GB)</option>
            <option value="high" selected>High Size (> 20GB)</option>
            <option value="Highest Possible">Highest Size Possible</option>
          </select>
        </div>
        <div class="form-control">
          <input type="text" id="customer-name" placeholder="Your Name" autocomplete="name"
            v-model="formValues.customerName" required>
        </div>
        <div class="form-control">
          <input type="email" id="email" placeholder="Your Email" autocomplete="email" v-model="formValues.email"
            required>
        </div>
        <div class="form-control h-captcha" data-captcha="true" data-theme="dark"></div>
        <div class="form-control">
          <button type="submit" :disabled="submitting">Send</button>
        </div>
        <p>You will receive an email containing the download link within the next 24 hours.
        </p>
      </form>

      <div v-if="submitting">
        <p>Please Wait...</p>
      </div>

      <div class="form-loader">
        <pacman-loader :loading="submitting"></pacman-loader>
      </div>

      <div v-if="submitted" class="thankyou">

        <p class="lead-text">Thank you!</p>

        <p class="sublead-text">You will find the Download Link in your inbox in less than 24 hours.</p>

      </div>
      <div v-if="error" class="error">

        <p class="lead-text">Error!</p>

        <p class="sublead-text">Some kind of error occurred while sending the request, please try again.</p>

      </div>

      <hr class="divider">

      <footer>
        <MailingBox />
      </footer>
    </div>

  </main>
</template>

<script lang="ts">

export default {
  data() {
    return {
      submitting: false,
      submitted: false,
      botcheck: false,
      error: false,
      formValues: {
        name: "",
        quality: "1080p",
        size: "low",
        customerName: "",
        upi: "",
        email: "",
      },
    };
  },
  methods: {
    async submitRequest() {

      this.submitting = true;

      // try {
      //   const response = await fetch("https://formcarry.com/s/xdzTV97Ns6R", {
      //     method: 'POST',
      //     headers: {
      //       "Accept": "application/json",
      //       "Content-Type": "application/json"
      //     },
      //     body: JSON.stringify({ name: this.formValues.name, quality: this.formValues.quality, size: this.formValues.size, email: this.formValues.email })
      //   });

      //   const data = await response.json();

      //   if (data.code === 200) {
      //     this.submitted = true;
      //   } else if (data.code === 422) {
      //     // error.value = data.message;
      //   } else {
      //     // error.value = data.message;
      //   }
      // } catch (err) {
      //   // error.value = err.message ? err.message : err;
      // }

      const hCaptcha = document.querySelector('textarea[name=h-captcha-response]')?.value;

      if (hCaptcha != null) {
        if (!hCaptcha) {
          alert("Please fill out captcha field");
          return;
        }
      }

      const response = await fetch("https://api.web3forms.com/submit", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
        body: JSON.stringify({
          access_key: import.meta.env.VITE_WEB3FORMS_ACCESS_KEY,
          name: this.formValues.name,
          quality: this.formValues.quality,
          size: this.formValues.size,
          customerName: this.formValues.customerName,
          upi: this.formValues.upi,
          email: this.formValues.email,

          subject: "New Movie Request",
          from_name: "Movie Vault App",
          // botcheck: this.botcheck,
        }),
      });
      const result = await response.json();
      if (result.success) {
        // console.log(result);
        setTimeout(() => {
          this.submitted = true;
          this.submitting = false;
        }, 1000);
      } else {
        setTimeout(() => {
          this.error = true;
          this.submitting = false;
        }, 1000);
      }
    }
  },
  components: { HeaderComponent, MailingBox, PacmanLoader }
}
</script>

<style>
.page-wrapper {
  min-height: 100vh;
  margin-top: 4rem;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}

div.lead {
  text-align: center;
}

.lead .lead-text {
  font-size: 3.2rem;
  color: white;
  font-weight: 700;
  line-height: 5rem;
}

@media (max-width: 768px) {
  .lead .lead-text {
    font-size: 2.2rem;
    font-weight: 900;
    line-height: 3.5rem;
  }
}

.lead .sublead-text {
  font-size: 1.2rem;
  line-height: 3rem;
}

form {
  margin-top: 2rem;
}

div.form-control {
  padding: 1.5rem;
  min-width: 50vw;
  text-align: start;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

div.form-control.h-captcha {
  justify-content: center;
  padding-left: 0;
  padding-right: 0;
}

div.form-loader {
  /* margin-top: 2rem; */
  padding: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

label {
  font-size: 1.4rem;
}


input {
  height: 3rem;
  width: 100%;
  padding: 1.5rem;
  background-color: rgba(255, 255, 255, 0.05);
  border: 0;
  border-radius: 0.5rem;
  border-bottom: 2px solid grey;
  color: white;
  font-size: 1.8rem;
  text-align: center;
  font-weight: 200;
}

select {
  height: 2.5rem;
  padding: 0.5rem;
  background-color: rgba(255, 255, 255, 0.05);
  border-radius: 0.25rem;
  border: 0;
  border-bottom: 2px solid grey;
  color: white;
  font-size: 1.2rem;
  text-align: center;
  font-weight: 200;
}

@media (max-width: 768px) {
  label {
    font-size: 1rem;
  }

  input {
    font-size: 1.2rem;
  }

  select {
    font-size: 1rem;
    max-width: 25vw;
  }
}

div.form-control button {
  margin-right: auto;
  margin-left: auto;
  padding: 0.5rem;
  width: 50%;
  background-color: rgba(0, 119, 255, 0.2);
  border-radius: 0.5rem;
  border: 2px solid grey;
  color: white;
  font-size: 1.2rem;
  text-align: center;
  font-weight: 200;
  transition: all 250ms;
}

div.form-control button:hover {
  background-color: rgba(94, 185, 192, 0.01);
}

div.form-control button:disabled {
  background-color: rgba(255, 255, 255, 0);
  color: rgba(128, 128, 128, 0.445);
  border-color: black;
}

div.thankyou {
  text-align: center;
  color: white
}

div.thankyou .lead-text {
  font-size: 2.2rem;
  font-weight: 900;
  line-height: 3.5rem;
}

div.thankyou .sublead-text {
  font-size: 1.2rem;
  line-height: 3rem;
}

div.error {
  text-align: center;
  color: red;
}

div.error .lead-text {
  font-size: 2.2rem;
  font-weight: 900;
  line-height: 3.5rem;
}

div.error .sublead-text {
  color: rgba(196, 0, 0, 0.767);
  font-size: 1.2rem;
  line-height: 3rem;
}

hr.divider {
  margin-top: 5rem;
  margin-bottom: 10rem;
  width: 100%;
}

@media (max-width: 768px) {
  hr.divider {
    margin-top: 1.5rem;
    margin-bottom: 5rem;
    /* width: 100%; */
  }
}

footer {
  min-width: 100%;
}

div.box {
  min-width: 100%;
  min-height: 10rem;
  border: 2.5px dashed white;
  border-radius: 10px;
}

div.box>p {
  font-size: 2rem;
  margin-top: 2rem;
  margin-bottom: 0;
}
</style>