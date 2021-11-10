<template>
    <div>
        <Teams :teams="sortedTeams"/>
    </div>
</template>

<script>
import Teams from '../components/Teams.vue'

export default {
    name: 'Home',
    props: {
        
    },
    components: {
        Teams
    },
    data() {
      return {
          teams: [],
          standings: {},
          sortedTeams:[]
      }
    },
     methods: {
    async setup() {
        this.standings = await this.createStandings()
        this.teams = await this.createTeams()
        this.calculatePowerRankings()
        console.log(this.standings)
        console.log(this.teams)
    },
    async fetchStandings() {
        const res = await fetch(' https://statsapi.web.nhl.com/api/v1/standings/byLeague')
        const data = res.json()
        return data
    },
    async createStandings() {
        const standingData = await this.fetchStandings()
        const records = standingData.records[0].teamRecords//['teamRecords']
        //console.log(records)
        let rank = 1
        let standings = {}
        records.forEach(record => {
            const name = record.team.name
            standings[name] = rank
            rank += 1
        }); 
        return standings
    },
    async fetchSeason() {
        const res = await fetch('https://statsapi.web.nhl.com/api/v1/seasons/current')
        const data = res.json()
        return data
    },
    async fetchTeams() {
        const seasonData = await this.fetchSeason()
        // console.log(seasonData)
        const start = seasonData.seasons[0].regularSeasonStartDate
        const today = new Date();
        const dd = String(today.getDate()).padStart(2, '0');
        const mm = String(today.getMonth() + 1).padStart(2, '0'); //January is 0!
        const yyyy = today.getFullYear();
        let url = 'https://statsapi.web.nhl.com/api/v1/schedule?startDate=' + start + '&endDate=' + yyyy + '-' + mm + '-' + dd + '&expand=schedule.linescore'
        // console.log(url)
        const res = await fetch(url)
        const data = res.json()
        return data
    },
    async createTeams() {
        const teamData = await this.fetchTeams()
        const dates = teamData.dates
        let teams = {}
        dates.forEach(date => {
            const games = date.games
            // for (const [key, value] of Object.entries(games)) {
            //     console.log(key, value)
            // }

        for (const [key, game] of Object.entries(games)) {
                const away = game.teams.away.team//['teams']['away']['team']['name']
                const home = game.teams.home.team//['teams']['home']['team']['name']
                const home_win = game.teams.home.score > game.teams.away.score
                //console.log(home_win)
                // //console.log("away is " + away)
                if (!(away.name in teams)) {
                    teams[away.name] = {
                        name: away.name,
                        wins: 0,
                        loses: 0,
                        overTimeLoses: 0,
                        standing: 0,
                        powerRank: 0,
                        games: {}
                    }
                }
                //console.log("home is " + home)
                if (!(home.name in teams)) {
                    teams[home.name] = {
                        name: home.name,
                        wins: 0,
                        loses: 0,
                        overTimeLoses: 0,
                        standing: 0,
                        powerRank: 0,
                        games: {}
                    }
                }
                if (!(home.name in teams[away.name].games)) {
                    teams[away.name].games[home.name] = []
                }
                if (!(away.name in teams[home.name].games)) {
                    teams[home.name].games[away.name] = []
                }
                const ending = game.linescore.currentPeriodOrdinal//['linescore']['currentPeriodOrdinal']
                if (home_win === true) {
                    if  (ending === 'OT' || ending === 'SO') {
                        teams[away.name].overTimeLoses += 1
                        teams[away.name].games[home.name].push('ot')
                    }
                    else {
                        teams[away.name].loses += 1
                        teams[away.name].games[home.name].push('lost')
                    }
                    teams[home.name].games[away.name].push('won')
                    teams[home.name].wins += 1
                }
                else {
                    teams[away.name].games[home.name].push('won')
                    teams[away.name].wins += 1
                    if  (ending === 'OT' || ending === 'SO') {
                        teams[home.name].overTimeLoses += 1
                        teams[home.name].games[away.name].push('ot')
                    }
                    else {
                        teams[home.name].loses += 1
                        teams[home.name].games[away.name].push('lost')
                    }
                teams[home.name].standing = this.standings[home.name]
                teams[away.name].standing = this.standings[away.name]
                }
            }
        })
        return teams
    },
    calculatePowerRankings() {
        for (const [name, team] of Object.entries(this.teams)) {        
        //console.log(team)
        let powerRank = 0
        for (const [key, value] of Object.entries(team.games)) {
            //console.log(key, value)
            const other_standing = this.standings[key]
            value.forEach(result => {
                if (result === 'won')
                    powerRank += 33 - other_standing
                else if (result === 'ot')
                    powerRank -= other_standing / 2
                else
                    powerRank -= other_standing
                })
            }
            //console.log(name, powerRank)
            this.teams[name].powerRank = powerRank
        }
        this.sortedTeams = Object.values(this.teams).sort(function(a,b) { 
            if (a.powerRank === b.powerRank)
                return 0
            return a.powerRank > b.powerRank ? -1 : 1
        });
        console.log('sorted', this.sortedTeams)
    }
  },
  async created() {
    await this.setup()
  }
}
</script>
