<template>
    <div>
        <div class="container">
            <div class="row align-items-start">
                <div class="col">
                    <select id="idJeu" class="form-control" @click="setJeu()" required>
                        <option disabled selected hidden>Choisir jeu</option>
                        <option v-for="jeu in listJeu" :key="jeu" :value="jeu.id"> {{jeu.nom}}
                        </option>
                    </select>
                    <br>
                    <input type="text" class="form-control" v-model="link" placeholder="Lien">
                </div>
                <div class="col">
                    <input type="text" id="link" class="form-control" @click="copy()"
                        :value="`<iframe src='${this.url}?idJeu=${this.idJeu}&link=${this.link}' width='600' height='700'></iframe>`"
                        readonly>
                    <br>
                    <div v-if="erreur" class="alert alert-danger animate__animated animate__bounceIn" role="alert">
                        Erreur!
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            listJeu: null,
            idJeu: null,
            url: "http://localhost:8080/Capchat/",
            link: null,
            erreur: false,
            copie: false
        }
    },
    created() {
    this.getListJeu();
    },
    methods: {
        getListJeu() {
            axios.get('getListJeu')
                .then(
                    data => {
                        this.listJeu = data.data
                    }
                )
                .catch(
                    err => {
                        console.error(err);
                    }
                )
        },
        setJeu() {
            this.idJeu = document.getElementById('idJeu').value;
        },
        copy() {
            if (!this.link && !this.value) {
                this.copie = false;
                this.erreur = true;
            }
            else {
                navigator.clipboard.writeText(document.getElementById('link').value);
                this.erreur = false;
                this.copie = true;
                setTimeout(() => {
                    this.copie = false;
                }, 8000);
            }
        }
    }
}
</script>