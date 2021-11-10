<template>
    <div>
        <p>{{team.name}} {{team.powerRank}}  {{team.standing}}</p>
    </div>
</template>

<script>
export default {
    name: 'Team',
    props: {
        team: Object
    }
}
</script>

<style scope>
.fas {
  color: red;
}
.team {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.team h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>