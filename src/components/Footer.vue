<template>
    <footer>
        <div>
            Power Rankings are calculated by wins and loses against teams played and their overall standing. First place is 32 and last place is 1.
            Beating a team in first place is +32, second place is +31, last place is +1. Losing to a team in first place is -1, second place is -2, last place is -32.
            Overtime loses reduces the value by half. 
        </div>
        <p>Copyright &copy; 2021</p>
        <router-link to="/about">About</router-link>
    </footer>
</template>

<style scoped>
a {
    color: #333;
}

footer {
    margin-top: 30px;
    text-align: center;
}
</style>