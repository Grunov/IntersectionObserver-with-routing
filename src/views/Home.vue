<template>
  <div class="home">
    <div class="preview">
      <h1>IntersectionObserver with routing</h1>
    </div>
    <div class="section-buttons">
      <button 
        v-for="section in sections"
        :key="section"
        @click="navigationToSection(section)"
      >
        {{ section }}
      </button>
    </div>
    <section 
       v-for="section in sections"
      :key="section"
      :id="section"
    >
      <h1>
        {{ section }}
      </h1>
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
      sections: ['a', 'b', 'c', 'd', 'f', 'g', 'i']
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
        rootMargin: "0px",
        threshold: 1,
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
    navigationToSection(sectionId) {
      document.getElementById(sectionId).scrollIntoView();
      this.$router.push({ name: this.$route.name, hash: `#${sectionId}` });
    }
  },
};
</script>

<style scoped>
.home {
  padding-bottom: 30px;
  background-color: rgb(22, 75, 75);
}

.preview {
  height: 70vh;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
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
