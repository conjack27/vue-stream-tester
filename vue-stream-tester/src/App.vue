<script setup></script>

<template>
  <div class="st">
    <header>
      <h1 class="st__heading">Stream Tester</h1>
    </header>

    <main>
      <form class="st__form" @submit.prevent="handleSubmit">
        <input
          class="st__input"
          type="text"
          v-model="vidURL"
          placeholder="www.video-url.com"
        />
        <button class="st__button">Submit</button>
      </form>
      <p v-if="latestVidUrl">{{ latestVidUrl }}</p>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      url: "",
      errorMessage: "",
      latestVidUrl: "",
    };
  },
  mounted() {
    console.log("The component has been mounted!");
    this.getLatestURL();
  },
  methods: {
    async handleSubmit() {
      try {
        await fetch(`http://localhost:3000/`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({ videoURL: this.vidURL }),
        });
      } catch (error) {
        console.error("Error saving new URL ", error);
      }
    },
  },
  async getLatestURL() {
    fetch("http://localhost:3000/latest")
      .then((response) => response.json())
      .then((vidURL) => {
        this.latestVidUrl = vidURL;
      });
  },
};
</script>

<style lang="scss" scoped>
.st {
  width: 100%;
  height: 100vh;
  background: #a39ba8;
  display: flex;
  flex-wrap: wrap;
}

header {
  top: 0;
  text-align: center;
  width: 100%;
}
main {
  width: 100%;
  padding: 50px;
}

.st__heading {
  font-size: 42px;
}

.st__form {
  width: 100%;
  display: flex;
}

.st__input {
  border: none;
  width: 80%;
  min-height: 50px;
  padding: 10px;
  font-size: 32px;
}

:focus-visible {
  outline: 0;
}

.st__button {
  margin-left: 8px;
  background: snow;
  width: auto;
  min-height: 50px;
  font-size: 32px;
  cursor: pointer;
  padding: 10px 20px;
  border: none;

  &:hover {
    opacity: 0.8;
  }
}

@media (min-width: 1024px) {
}
</style>
