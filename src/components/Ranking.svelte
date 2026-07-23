<script>
  let rankings = [
    {
      position: 1,
      player: "Abhishek",
      rating: 1500,
      points: 3
    },
    {
      position: 2,
      player: "Rahul",
      rating: 1600,
      points: 1
    }
  ];
</script>

<div class="ranking-page">

  <h2>Final Rankings</h2>

  <div class="ranking-list">

    {#each rankings as player}

      <div class="ranking-card">

        <div class="position">
          {#if player.position === 1}
            🥇
          {:else if player.position === 2}
            🥈
          {:else if player.position === 3}
            🥉
          {:else}
            {player.position}
          {/if}
        </div>

        <div class="player-info">
          <h3>{player.player}</h3>
          <p>Rating: {player.rating}</p>
          <p>Points: {player.points}</p>
        </div>

      </div>

    {/each}

  </div>

</div>