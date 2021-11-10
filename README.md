# vue-nhl-power-rankings
Small vue project to show NHL rankings based on teams played and not by points.
Power Rankings are calculated by wins and loses against teams played and their overall standing. First place is 32 and last place is 1.
Beating a team in first place is +32, second place is +31, last place is +1. Losing to a team in first place is -1, second place is -2, last place is -32.
Overtime loses reduces the value by half. 

https://nhl-power-rankings.herokuapp.com/

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
