<template>
  <div id="app">
    <h1>Random User</h1>
    <div class="person" v-for="(person, idx) in persons" :key="idx">
      <div class="left">
        <img :src="person.picture.large" />
      </div>
      <div class="right">
        <p>{{ person.name.first }} {{ person.name.last }}</p>
        <ul>
          <li>
            <strong>Birthday:</strong> {{ person.dob.date }}
          </li>
          <li class="text-capitalize">
            <strong>Location:</strong> {{ person.location.city }},
            {{ person.location.state }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      msg: "Random Persons",
      persons: [],
    };
  },
  methods: {
    getInitialUsers() {
      axios.get(`https://randomuser.me/api/?results=5`).then(response => {
        this.persons = response.data.results;
      });
    },

    scroll(person) {
      window.onscroll = () => {
        let bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight;
        if (bottomOfWindow) {
          axios.get(`https://randomuser.me/api/`).then(res => {
            this.persons.push(res.data.results[0]);
            console.log("vector personas =>>", this.persons);
          });
        }
      };
    }
  },

  mounted() {
    this.scroll(this.persons);
  },

  beforeMount() {
    this.getInitialUsers();
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

/* Optional Styles */
.person {
  background: #ccc;
  border-radius: 2px;
  width: 20%;
  margin: 0 auto 15px auto;
  padding: 15px;
  img {
    width: 100%;
    height: auto;
    border-radius: 2px;
  }
  p:first-child {
    text-transform: capitalize;
    font-size: 2rem;
    font-weight: 900;
  }
  .text-capitalize {
    text-transform: capitalize;
  }
}
</style>
