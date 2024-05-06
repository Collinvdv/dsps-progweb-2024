<template>
    <div>
        Content of the voting page

        <div>
            <div 
                v-for="(song, index) in songs" 
                :key="song.id"
                >
                <div v-if="index == activeSongIndex">
                    {{song.artistObject.name }} - {{  song.title }}
                </div>
            </div>
        </div>

        <button @click="changeActiveSong(-1)" :disabled="activeSongIndex == 0">
            prev song
        </button>

        <button @click="changeActiveSong(1)" :disabled="activeSongIndex == (songs.length - 1)">
            next song
        </button>

        <br/><br/>

        <div v-for="(voteObject, voteIndex) in votes" :key="voteIndex">
            <button @click="vote(voteObject.points, voteIndex)" v-if="voteObject.isVoted == false">
                Vote {{ voteObject.points }} points
            </button>
        </div>
    </div>
</template>

<script>
    export default {
        name: "PageVoting",
        data() {
            return {
                songs: [],
                activeSongIndex: 0,
                votes: [
                    {
                        points: 2,
                        isVoted: false
                    },
                    {
                        points: 4,
                        isVoted: false
                    },
                    {
                        points: 8,
                        isVoted: false
                    },
                ]
            }
        },
        mounted() {
            fetch("http://webservies.be/eurosong/Songs", {
                method: "GET"
            })
                .then((response) => response.json())
                .then((songs) => {
                    fetch("http://webservies.be/eurosong/Artists", {
                        method: "GET"
                    })
                        .then((response) => response.json())
                        .then((artists) => {
                            this.mixSongsAndArtists(artists, songs);
                        })
                })
        },
        methods: {
            vote(points, voteIndex) {
                let vote = {
                    "songID": this.songs[this.activeSongIndex].id,
                    "ip": "string",
                    "points": points
                }

                fetch("http://webservies.be/eurosong/Votes", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify(vote)
                }).then(response => response.json())
                .then(() => {
                    this.votes[voteIndex].isVoted = true;

                    let unusedVotes = this.votes.filter((vote) => vote.isVoted == false);

                    if (unusedVotes.length == 0) {
                        this.$emit("setactivepage", "ranking");
                    }
                })
            },
            changeActiveSong(value) {
                this.activeSongIndex += value;
            },
            mixSongsAndArtists(artists, songs) {
                let data = [];

                // Loop over songs
                for(let songIndex = 0; songIndex < songs.length; songIndex++) {
                    let song = songs[songIndex];

                    // Find the artists 
                    for(let artistIndex = 0; artistIndex < artists.length; artistIndex++) {
                        if (song.artist == artists[artistIndex].id) {
                            song.artistObject = artists[artistIndex];

                            data.push(song);
                            break;
                        }
                    }
                }

                this.songs = data;
            }
        }
    }
</script>