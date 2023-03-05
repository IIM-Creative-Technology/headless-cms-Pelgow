<template>
    <div v-if="project">
        <a href="javascript:history.back()">Retour</a>
        <h3>Nom du projet : <u>{{  project.Name }} ({{ project.Type }})</u></h3>
        <div v-if="project.Images.length">
            <div class="ImgContent">
                <div v-for="image in project.Images" :key="image.id">
                    <img :src="image.url" alt="Image du projet" style="height: auto;width: 20rem;">
                </div>
            </div>
        </div>
        <div v-if="project.Description.length">
            <h4>Description :</h4>
            <p>{{ project.Description }}</p>
        </div>
        <div v-if="project.technologies.length">
            <div>
                <h4>Technologies utilisées :</h4>   
                <ul v-for="technologie in project.technologies" :key="technologie.id">
                    <li>{{ technologie.Name }}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script setup>
    const {findOne} = useStrapi();
    const route = useRoute();

    const project = ref();

    onMounted(async () => {
        project.value = await findOne(`projects?filters[slug]=${route.params.slug}&populate=*`);
        project.value = project.value.data[0];
        console.log(project.value);
    });
</script>