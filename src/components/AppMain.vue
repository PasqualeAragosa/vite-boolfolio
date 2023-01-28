<script>
import axios from 'axios';

export default {
    name: 'AppMain',
    data() {
        return {
            projects: null,
            base_api_url: 'http://127.0.0.1:8000',
            loading: true,
            error: null,
            max: 100,
        }
    },
    methods: {
        getProjects(url) {
            axios
                .get(url)
                .then(response => {
                    console.log(response.data.results);
                    this.projects = response.data.results;
                    this.loading = false;
                })
                .catch(error => {
                    console.error(error.message)
                    this.error = error.message;
                })
        },
        getImagePath(path) {
            if (path) {
                return this.base_api_url + '/storage/' + path;
            } else {
                return 'https://picsum.photos/300/200.jpg';
            }
        },
        /**
         * 
         * @param {string} text 
         */
        trimBody(text) {
            if (text.length > this.max) {
                return text.slice(0, this.max) + '...';
            }
            return text;
        },
        /**
         * 
         * @param {string} url 
         */
        prevPage(url) {
            console.log(url);
            this.getProjects(url);
        },
        /**
         * 
         * @param {string} url 
         */
        nextPage(url) {
            console.log(url);
            this.getProjects(url);
        }
    },
    mounted() {
        this.getProjects(this.base_api_url + '/api/projects');
    }
}
</script>

<template>
    <section class="vue-home pt-5">
        <div class="container">
            <template v-if="projects">
                <div class="row row-cols-1 row-cols-sm-3">
                    <div class="col" v-for="project in projects.data">
                        <div class="card rounded-0 border-0 rounded-top shadow-lg">
                            <img class="card-image rounded-top" :src="getImagePath(project.cover_image)" alt="image">
                            <div class="card-body">
                                <h4>{{ project.title }}</h4>
                                <p class="text-secondary">{{ trimBody(project.body) }}</p>
                            </div>
                            <div class="card-footer text-muted">
                                <div class="category">
                                    <strong>Category:</strong>
                                    <span v-if="project.type">
                                        {{ project.type.name }}
                                    </span>
                                    <span v-else>
                                        No type
                                    </span>
                                </div>
                                <div class="technology">
                                    <strong>Technology: </strong>
                                    <template v-if="project.technologies > 0">
                                        <span v-for="technology in project.technologies">
                                            #{{ technology.name }}
                                        </span>
                                    </template>
                                    <template v-else>
                                        <span>
                                            No Technologies
                                        </span>
                                    </template>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <nav aria-label="Page navigation" class="d-flex justify-content-center pt-5">
                    <ul class="pagination">
                        <!-- Prev Page -->
                        <li class="page-item" v-if="projects.prev_page_url" @click="prevPage(projects.prev_page_url)">
                            <a class="page-link" aria-label="Previous">
                                <span aria-hidden="true">&laquo;</span>
                            </a>
                        </li>
                        <!-- Curr Page -->
                        <li class="page-item active" aria-current="page"><a class="page-link" href="#">{{
                            projects.current_page
                        }}</a></li>
                        <!-- Next Page -->
                        <li class="page-item" v-if="projects.next_page_url" @click="nextPage(projects.next_page_url)">
                            <a class="page-link" aria-label="Next">
                                <span aria-hidden="true">&raquo;</span>
                            </a>
                        </li>

                    </ul>
                </nav>
            </template>
            <div v-else>
                <p>No project here</p>
            </div>
        </div>
    </section>
</template>


<style lang="scss">

</style>