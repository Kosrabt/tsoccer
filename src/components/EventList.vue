<template>
  <div class="event-list">
     <div id="events-card" class="ui segment" style="width: 600px; margin: auto; margin-top: 10px;">
            <table class="ui very basic celled table">
                <thead>
                    <tr>
                        <th>Player</th>
                        <th>Event</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-bind:class="{positive: item.isPositive, negative: !item.isPositive}" v-for="item in events" :key="item">
                        <td>
                            <h4 class="ui image header" v-bind:class="{red: item.isRed, blue: !item.isRed}">
                                <img src="https://semantic-ui.com/images/avatar/small/elliot.jpg" class="ui mini rounded image">
                                <div class="content">
                                    {{ item.playerName }}
                                    <div class="sub header">{{ item.dept }}
                                    </div>
                                </div>
                            </h4>
                        </td>
                        <td>
                            {{ item.event }}
                            <br />
                            <small>{{ item.timestamp }}</small>
                        </td>
                        <td>     
                            <div class="ui dropdown action-menu">
                                Select action...
                                <i class="dropdown icon"></i>
                                <div class="menu">
                                    <div class="item">Choice 1</div>
                                    <div class="item">Disabled</div>
                                    <div class="item">Choice 3</div>
                                </div>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
  </div>
</template>

<script>
export default {
  name: "GameBoard",
  props: ["events"],
  data() {}
};
</script>

<style>
</style>
