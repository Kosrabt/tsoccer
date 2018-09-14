<template>
  <div >
   <div id="score-card" class="ui clearing segment" style="width: 600px; margin: auto; margin-top: 30px;">
            <h3 class="ui right floated blue header">
                Blue Team
                <div class="sub header">{{ blue.striker == null ? '?' : blue.striker.name }} & {{ blue.defense == null
                    ? '?' : blue.defense.name }}</div>
            </h3>
            <h3 class="ui left floated red header">
                Red Team
                <div class="sub header">{{ red.striker == null ? '?' : red.striker.name }} & {{ red.defense == null ?
                    '?' : red.defense.name }}</div>
            </h3>
            <h3 class="ui center aligned header">
                {{ red.score }} : {{ blue.score }}
            </h3>
        </div>

        <div id="players-card" class="ui segment" style="width: 600px; margin: auto; margin-top: 10px;">
            <div class="ui two column grid">
                <div v-if="blue.striker == null || blue.defense == null || red.striker == null || red.defense == null" class="row">
                    <div class="column">
                        <div v-if="red.striker == null || red.defense == null" class="ui compact fluid menu">
                            <div class="ui simple dropdown fluid item">
                                <i class="red plus icon"></i>
                                Enter Game
                                <div class="menu">
                                    <div class="item" v-if="red.striker == null" v-on:click="setPlayer('red', 'striker')">Striker</div>
                                    <div class="item" v-if="red.defense == null" v-on:click="setPlayer('red', 'defense')">Defense</div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="right aligned column">
                        <div v-if="blue.striker == null || blue.defense == null" class="ui compact fluid menu">
                            <div class="ui simple dropdown fluid blue item">
                                <i class="blue plus icon"></i>
                                Enter Game
                                <div class="menu">
                                    <div class="item" v-if="blue.striker == null" v-on:click="setPlayer('blue', 'striker')">Striker</div>
                                    <div class="item" v-if="blue.defense == null" v-on:click="setPlayer('blue', 'defense')">Defense</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="column">
                        <a v-if="red.striker != null" class="ui large red image fluid label" v-on:click="score(red.striker)">
                            <img src="https://semantic-ui.com/images/avatar/small/elliot.jpg">
                            {{ red.striker.name }}
                            <div class="detail" style="float:right">
                                31
                                <i class="arrow down icon"></i>
                            </div>
                        </a>
                    </div>
                    <div class="column">
                        <a v-if="blue.defense != null" class="ui large blue image fluid label" v-on:click="score(blue.defense)">
                            <img src="https://semantic-ui.com/images/avatar/small/elliot.jpg">
                            {{ blue.defense.name }}
                            <div class="detail" style="float:right">
                                22
                                <i class="arrow up icon"></i>
                            </div>
                        </a>
                    </div>
                </div>

                <div class="row">
                    <div class="column">
                        <a v-if="red.defense != null" class="ui large red image fluid label" v-on:click="score(red.defense)">
                            <img src="https://semantic-ui.com/images/avatar/small/elliot.jpg">
                            {{ red.defense.name }}
                            <div class="detail" style="float:right">
                                31
                                <i class="arrow down icon"></i>
                            </div>
                        </a>
                    </div>
                    <div class="column">
                        <a v-if="blue.striker != null" class="ui large blue image fluid label" v-on:click="score(blue.striker)">
                            <img src="https://semantic-ui.com/images/avatar/small/elliot.jpg">
                            {{ blue.striker.name }}
                            <div class="detail" style="float:right">
                                22
                                <i class="arrow up icon"></i>
                            </div>
                        </a>
                    </div>
                </div>
            </div>
        </div>

        <div id="events-card" class="ui segment" style="width: 600px; margin: auto; margin-top: 10px;">
            <div class="ui padded grid">
                <div class="eight wide column">
                    <button class="ui positive fluid button">
                        <i class="futbol outline icon"></i>
                        Goal
                    </button>
                </div>
                <div class="four wide padded column">
                    <button class="ui fluid button">
                        Self Goal
                    </button>
                </div>
                <div class="four wide right floated column">
                    <button class="ui fluid button">
                        End Game
                    </button>
                </div>
            </div>
        </div>
      <event-list :events="history"/>
     
  </div>
</template>

<script>
import moment from 'moment'
import eventList from './EventList'

export default {
  name: "GameBoard",
  component: [eventList],
  data() {
    return {
      red: {
                score: 0,
                striker: null,
                defense: null
            },
            blue: {
                score: 0,
                striker: null,
                defense: null
            },
            history: []
    };
  },
  computed: {
            players: function() {
                return [
                    this.red.striker,
                    this.red.defense,
                    this.blue.striker,
                    this.blue.defense
                ].filter(p => p != null);
            }
        },
        methods: {
            score: function (player) {
                var color = null;
                if (player === this.red.striker || player === this.red.defense) {
                    this.red.score++;
                    color = "red";
                } else if (player === this.blue.striker || player === this.blue.defense) {
                    this.blue.score++;
                    color = "blue";
                } else {
                    console.error("Cannot determine team for " + player);
                }

                this.history.unshift({
                    playerName: player.name,
                    dept: player.dept,
                    event: player.name !== "Sanyi" ? `Goal •  ${this.red.score} : ${this.blue.score}` : `Own Goal •  ${this.red.score} : ${this.blue.score}`,
                    isPositive: player.name !== "Sanyi",
                    isRed: color === "red",
                    timestamp: moment().format("HH:mm:ss")
                });
            },
            setPlayer: function (team, position) {
                if (team === "red" && position === "defense") {
                    this.red.defense = { name: "Sanyi", dept: "Puli", fullName: "Sandor Koncz" };
                } else if (team === "red" && position === "striker") {
                    this.red.striker = { name: "Józsi", dept: "Puli", fullName: "Jozsef Sipos" };
                } else if (team === "blue" && position === "defense") {
                    this.blue.defense = { name: "Marci", dept: "Comms", fullName: "Marcell Miso" };
                } else if (team === "blue" && position === "striker") {
                    this.blue.striker = { name: "Rudi", dept: "Comms", fullName: "Rudolf Serfozo" };
                } else {
                    console.error("Invalid team and/or position: [" + team + "], [" + position + "]");
                }

            },
            removeHistoryItem: function (item) {
                var index = this.history.indexOf(item);
                if (index > -1) {
                    this.history.splice(index, 1);
                }
            }
        }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
