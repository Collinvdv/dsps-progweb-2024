<template>
    <div>

        <table border="1">
            <tr>
                <th>Name of the song</th>
                <th>Artist name </th>
                <th># points</th>
            </tr>

            <!-- Loop over the songs -->
            <tr
                v-for="(song, index) in songs"
                :key="'song'+ index"
            >

                <td>
                    {{  song.title }}
                </td>
                <td>
                    {{  song.artistObject.name }}
                </td>
                <td>
                    {{  song.totalPoints }}
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
    export default {
        name: "PageRanking",
        data() {
            return {
                songs: []
            }
        },
        mounted() {
            this.fetchSongs();
        },
        methods: {
            fetchSongs() {
                fetch("http://webservies.be/eurosong/Songs")
                    .then((response) => {
                        return response.json()
                    })
                    .then((songs) => {
                        this.fetchArtists(songs);
                    });
            },
            fetchArtists(songs) {
                fetch("http://webservies.be/eurosong/Artists")
                    .then((response) => {
                        return response.json()
                    })
                    .then((artists) => {
                        songs = songs.map((song) => {
                            song.artistObject = artists.find((artist) => artist.id == song.artist);
                            return song;
                        })
                        this.fetchVotes(songs);
                    });
            },
            fetchVotes(songs) {
                fetch("http://webservies.be/eurosong/Votes")
                    .then((response) => {
                        return response.json()
                    })
                    .then((votes) => {
                        songs = songs.map((song) => {
                            let votesForSong = votes.filter((vote) => vote.songID == song.id);

                            let amountOfPoints = 0;

                            votesForSong.forEach((song) => {
                                amountOfPoints += song.points;
                            });

                            song.totalPoints = amountOfPoints;
                            return song;
                        })

                        songs.sort((a, b) => b.totalPoints - a.totalPoints);
                        this.songs = songs;
                    });
            }
        }
    }
</script>