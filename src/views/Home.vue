<template>
  <div class="home">
    <div class="preview">
      <h1>IntersectionObserver with routing</h1>
    </div>
    <section id="a">
      <h1>A</h1>
    </section>
    <section id="b">
      <h1>B</h1>
    </section>
    <section id="c">
      <h1>C</h1>
    </section>
    <section id="d">
      <h1>D</h1>
    </section>
    <section id="e">
      <h1>E</h1>
    </section>
    <section id="f">
      <h1>F</h1>
    </section>
    <section id="g">
      <h1>G</h1>
    </section>
    <section id="h">
      <h1>H</h1>
    </section>
    <section id="i">
      <h1>I</h1>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  data() {
    return {
      sectionObserver: null,
    };
  },
  mounted() {
    this.observeSections();
  },
  methods: {
    observeSections() {
      try {
        this.sectionObserver.disconnect();
      } catch (error) {
        console.error(error);
      }
      const options = {
        rootMargin: "0px 0px",
        threshold: 0,
      };
      this.sectionObserver = new IntersectionObserver(
        this.sectionObserverHandler,
        options
      );

      const sections = document.querySelectorAll("section");
      sections.forEach((section) => {
        this.sectionObserver.observe(section);
      });
    },
    sectionObserverHandler(entries) {
      for (const entry of entries) {
        if (entry.isIntersecting) {
          const sectionId = entry.target.id;
          this.$router.push({ name: this.$route.name, hash: `#${sectionId}` });
        }
      }
    },
  },
};
</script>

<style scoped>
.home {

}

.preview {
  height: 70vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgb(163, 238, 238)  ;
}

section {
  width: 100%;
  height: 50vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

section:nth-child(1n) {
  background-color: rgb(216, 216, 216);
}

section:nth-child(2n) {
  background-color: rgb(160, 160, 160);
}
</style>
