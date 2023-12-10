<template>
  <div id="table" class="table-container">
    <div class="table-content">
     <h2 v-if="isLoading" class="loading">Loading...</h2>
      <input v-if="!isLoading" v-model="searchQuery" type="text" placeholder="Search by name" class="search-input" />
      <table  v-if="!isLoading" class="table">
        <thead>
          <tr>
            <th @click="handleSort('name')">Name</th>
            <th @click="handleSort('height')">Height</th>
            <th @click="handleSort('mass')">Mass</th>
            <th @click="handleSort('created')">Created</th>
            <th @click="handleSort('edited')">Edited</th>
            <th @click="handleSort('homeworld')">Planet Name</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(person, index) in filteredPeople" :key="person.name">
            <td>{{ person.name }}</td>
            <td>{{ person.height }}</td>
            <td>{{ person.mass }}</td>
            <td>{{ formatDate(person.created) }}</td>
            <td>{{ formatDate(person.edited) }}</td>
            <td> 
              <button @click="openPopup(index)" class="table-button-planet">
                {{ planets[index].name }}
                <PlanetIcon/>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <Popup v-if="selectedPlanet" :isVisible="isVisible" :content="selectedPlanet"> 
      <template #button>
        <div @click="closePopup" class="popup-icon-close">
          <CloseIcon/>
        </div>
      </template>
    </Popup>
  </div>


</template>
<script setup>
import { ref, onMounted, computed } from 'vue';
import Popup from './Popup.vue';
import CloseIcon from './icons/Close.vue';
import PlanetIcon from './icons/Planet.vue';

const people = ref([]);
const planets = ref([]);
const sortBy = ref(null);
const selectedPlanet = ref(null);
const isLoading = ref(true);

const fetchData = async () => {
  try {
    isLoading.value = true;
    const response = await fetch('https://swapi.dev/api/people/');
    const data = await response.json();
    people.value = data.results;

    const planetPromises = data.results.map(async (person) => {
      const planetResponse = await fetch(person.homeworld);
      const planetData = await planetResponse.json();
      return planetData;
    });

    planets.value = await Promise.all(planetPromises);
  } catch (error) {
    console.error('Error fetching data:', error);
  } finally {
    isLoading.value = false;
  }
};


const handleSort = (key) => {
  if (key === 'homeworld') {
    // Sorting by "Planet Name"
    if (sortBy.value === key) {
      planets.value.reverse();
    } else {
      planets.value.sort((a, b) => {
        return a.name.localeCompare(b.name);
      });
    }
  } else {
    // Sorting by other columns
    if (sortBy.value === key) {
      people.value.reverse();
    } else {
      people.value.sort((a, b) => {
        if (key === 'height' || key === 'mass') {
          const numA = parseFloat(a[key]);
          const numB = parseFloat(b[key]);
          return numA - numB;
        } else {
          return a[key] > b[key] ? 1 : -1;
        }
      });
    }
  }

  sortBy.value = key;
};

onMounted(() => {
  fetchData();
});

const searchQuery = ref('');

const filteredPeople = computed(() => {
  return people.value.filter((person) => {
    return person.name.toLowerCase().includes(searchQuery.value.toLowerCase());
  });
});

const formatDate = (dateString) => {
  const date = new Date(dateString);
  const options = { year: 'numeric', month: 'long', day: 'numeric'};
  return date.toLocaleDateString('en-UK', options);
};

const isVisible = ref(false);

const openPopup = (index) => {
  selectedPlanet.value = planets.value[index];
  isVisible.value = true;
};

const closePopup = () => {
  isVisible.value = false;
};
</script>

<style scoped>
.table-container {
  background-image: url("https://source.unsplash.com/mountain-with-snow-cap-_lEexUTVsHA");
  width: 100%;
  background-repeat: no-repeat;
  background-size: cover;
  min-height: 100vh;
  padding: 2rem 1rem;
}

.table-content {
  margin: auto;
  max-width: 80rem;
  overflow-x: auto;
}

.search-input {
  background-color: var(--color-white-soft);
  border: none;
  border-radius: 1rem;
  color: var(--color-lynch);
  font-size: 1rem;
  opacity: 0.9;
  padding: 0.5rem;
  width: 100%;
}

.search-input:focus {
  outline: none;
}

.table {
  background-color: var(--color-white-soft);
  border: 1px solid;
  border-radius: 1rem;
  font-size: 0.75rem;
  margin-top: 2rem;
  margin-bottom: 2rem;
  opacity: 0.9;
  overflow-x: scroll;
  width: 100%;
  text-align: left;
}

.table th {
  color: var(--color-lynch);
  font-weight: 600;
  padding: 0.5rem;
}
.table td {
  padding: 0.5rem;
}

.loading{
  color: var(--color-white-soft);
  text-align: center;
}

.table-button-planet {
  align-items: center;
  border: none;
  color: var(--color-lynch);
  display: flex;
  font-size: 0.75rem;
  width: 100%;
}

.table-button-planet:hover {
  color: var(--color-rhino);
}

.table-button-planet svg {
  height: 1.25rem;
  margin-left: 0.5rem;
}

.popup-icon-close {
  position: absolute;
  right: 4rem;
  top: 4rem;
}

@media (min-width: 767px) {
  .table-container {
    padding: 5rem;
  }

  .table{
    font-size: 1rem;
  }
  .search-input {
    width: 19rem;
  }

  .table-button-planet { 
    font-size: 1rem;
  }

  .popup-icon-close {
    right: 6rem;
    top: 6rem;
  }
}
</style>
