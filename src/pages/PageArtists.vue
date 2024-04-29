<template>
    <div>
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
                artists: []
            }
        },
        methods: {
            fetchArtists() {
                fetch("http://webservies.be/eurosong/Artists")
                    .then((response) => response.json())
                    .then((_artists) => {
                        this.artists = _artists;
                    })
            }
        }
    }
</script>