<template>
    <div class="tableContainer">
        <table id="rankings">
            <thead>
                <tr>
                    <th>Rank</th>
                    <th>Team</th>
                    <th>Power Rank</th>
                    <th>Overall Staning</th>
                </tr>
            </thead>
            <tbody>
                <tr :key="team.id" v-for="(team, index) in teams">
                    <td>{{index + 1}}</td>
                    <td>{{team.name}}</td>
                    <td>{{team.powerRank}}</td>
                    <td>{{team.standing}}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import Team from "./Team.vue"

export default {
    name: 'Teams',
    props: {
        teams: Array
    },
    components: {
        Team
    }
}
</script>

<style scoped>
tableContainer {
    align-items: center;
    justify-content: center;
}
table {
    font-family: 'Open Sans', sans-serif;
    width: 750px;
    border-collapse: collapse;
    border: 3px solid #44475C;
    margin: 10px 100px 0px 100px;
}

table th {
    text-transform: uppercase;
    text-align: center;
    background: #44475C;
    color: #FFF;
    padding: 8px;
    min-width: 30px;
}

table td {
    text-align: center;
    padding: 8px;
    border-right: 2px solid #7d82a8;
}
table td:last-child {
    border-right: none;
}
table tbody tr:nth-child(2n) td {
    background: #d4d8f9;
}
</style>