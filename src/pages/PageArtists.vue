<template>
    <div>
        <!-- Artists -->
        <table border="1">
            <tr>
                <th>
                    id
                </th>
                <th>
                    name of artists
                </th>
            </tr>

            <tr v-for="artist in artists" :key="artist.id">
                <td>
                    {{  artist.id }}
                </td>
                <td>
                    {{  artist.name }}
                </td>
            </tr>
        </table>

        <!-- Add a new artists -->
        <input type="text" v-model="newArtist">
        <button @click="addArtist()">
            Add Artist
        </button>
    </div>
</template>

<script>
    export default {
        name: "PageArtists",
        mounted() {
            this.fetchArtists();
        },
        data() {
            return {
                artists: [],
                newArtist: ""
            }
        },
        methods: {
            fetchArtists() {
                fetch("http://webservies.be/eurosong/Artists")
                    .then((response) => response.json())
                    .then((_artists) => {
                        this.artists = _artists;
                    })
            },
            addArtist() {
                fetch("http://webservies.be/eurosong/Artists", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        name: this.newArtist
                    })
                }).then(response => response.json())
                .then((newArtist) => {
                    console.log(newArtist);
                    this.fetchArtists();
                })
            }
        }
    }
</script>