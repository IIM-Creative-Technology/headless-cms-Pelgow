<template>
  <div v-if="projects">
    <h3>Liste des projets</h3>
    <div v-if="types">
      <div>
        <button class="test" @click="filterProjects('all')">All</button>
        <button :class="{ 'filter-button': true, active: activeFilter.value === type, }" v-for="type in types" @click="filterProjects(type)" :key="type" >
          {{ type }}
        </button>
      </div>
    </div>
    <div class="ProjetContent">
      <div v-for="project in filteredProjects" :key="project.id" class="ImgEtBtn">
        <button @click="() => $router.push(`/projects/${project.slug}`)">
        <img :src="project.Images[0].url" alt="Image du projet" style="height: auto;width: 10rem;">
        <p>Projet : <b>{{ project.Name }}</b></p>
        </button>
       </div>
    </div>
  </div>
</template>

<script setup>

const { find } = useStrapi();
const projects = ref();

const types = ref([]);
const activeFilter = ref('all');

const filterProjects = (type) => {
  activeFilter.value = type;
};

const filteredProjects = computed(() => {
  if (activeFilter.value === "all") return projects.value.data;
  return projects.value.data.filter(
    (project) => project.Type === activeFilter.value
  );
});

onMounted(async () => {
  projects.value = await find("projects", { populate: "*" });
  types.value = new Set(
    projects.value.data.map((project) => {
      console.log(project)
      return project.Type;
    })
  );
});
</script>
