<script setup lang="ts">
</script>

<template>
  <main>
    <div class="page-wrapper">
      <div class="lead">
        <p class="sublead-text">Movie Vault</p>
        <p class="lead-text">Get High-Speed Download Link</p>
        <p class="lead-text">For you Favourite Movie</p>
      </div>
      <form @submit="submitRequest">
        <input type="hidden" name="subject" value="**New Movie Request**">
        <input type="hidden" name="from_name" value="Movie Request">
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
          <input type="email" id="email" placeholder="Your Email" autocomplete="email" v-model="formValues.email"
            required>
        </div>
        <div class="form-control">
          <button type="submit">Send</button>
        </div>
        <p>You will receive the download link within next 24 hours.</p>
      </form>

      <div v-if="submitted">

        <p style="font-size: 2rem; margin-top: 3rem; color: white;">Thank you!</p>

        <div>You will get the Download Link in less than 24 hours.</div>

      </div>

      <hr>

      <footer>

        <div class="box">
          <p>Join the Mailing List</p>
          <form>
            <div class="form-control">
              <input type="email" id="email" placeholder="Your Email" autocomplete="email">
            </div>
            <p>We won't Spam</p>
            <div class="form-control">
              <button type="submit">Join Up!</button>
            </div>
          </form>
        </div>

      </footer>
    </div>

  </main>
</template>

<script lang="ts">
export default {
  data() {
    return {
      submitted: false,
      formValues: {
        name: '',
        quality: '1080p',
        size: 'low',
        email: '',
      },
    }
  },
  methods: {
    async submitRequest(event) {
      event.preventDefault();
      console.log(this.formValues);
      const response = await fetch("https://api.web3forms.com/submit", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
        body: JSON.stringify({
          access_key: WEB3FORMS_ACCESS_KEY,
          name: this.formValues.name,
          quality: this.formValues.quality,
          size: this.formValues.size,
          email: this.formValues.email,
        }),
      });
      const result = await response.json();
      if (result.success) {
        console.log(result);
      }
      setTimeout(() => {
        this.submitted = true;
      }, 100);
    }
  }
}
</script>

<style scoped>
.page-wrapper {
  min-height: 80vh;
  margin-top: 5rem;
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

.lead .sublead-text {
  font-size: 1.2rem;
  line-height: 3rem;
}

form {
  margin-top: 2rem;
}

div.form-control {
  padding: 2rem;
  min-width: 50vw;
  text-align: center;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

label {
  font-size: 1.4rem;
}


input {
  height: 3rem;
  width: 100%;
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
  background-color: rgba(255, 255, 255, 0.05);
  border-radius: 0.5rem;
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
  background-color: rgba(255, 255, 255, 0.05);
  border-radius: 0.5rem;
  border: 2px solid grey;
  color: white;
  font-size: 1.2rem;
  text-align: center;
  font-weight: 200;
  transition: 250ms all;
}

div.form-control button:hover {
  width: 50%;
  background-color: rgba(255, 255, 255, 0.01);
  border: 2px solid grey;
  border-radius: 0.5rem;
}

hr {
  margin-top: 10rem;
  margin-bottom: 10rem;
  width: 100%;
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