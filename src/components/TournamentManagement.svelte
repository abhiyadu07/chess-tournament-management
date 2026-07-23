<script>
import { onMount } from "svelte";
  let tournaments = [];
let availablePlayers = [
  {
    id: 1,
    name: "Abhishek",
    email: "abhishek@gmail.com",
    rating: 1500
  },
  {
    id: 2,
    name: "Rahul",
    email: "rahul@gmail.com",
    rating: 1600
  }
];
onMount(() => {
  const savedTournaments =
    localStorage.getItem("chessTournaments");

  if (savedTournaments) {
    tournaments = JSON.parse(savedTournaments);
  }
});
  function saveTournaments() {
    localStorage.setItem(
      "chessTournaments",
      JSON.stringify(tournaments)
    );
  }
  let tournamentName = "";
  let tournamentDate = "";
  let tournamentLocation = "";

  function addTournament() {
    if (
      tournamentName === "" ||
      tournamentDate === "" ||
      tournamentLocation === ""
    ) {
      alert("Please fill all fields");
      return;
    }

    const newTournament = {
      id: Date.now(),
      name: tournamentName,
      date: tournamentDate,
      location: tournamentLocation,
      players: []
    };

    tournaments = [...tournaments, newTournament];

    tournamentName = "";
    tournamentDate = "";
    tournamentLocation = "";
    saveTournaments();
  }
function addPlayerToTournament(tournamentId, player) {
  tournaments = tournaments.map((tournament) => {
    if (tournament.id === tournamentId) {
      const alreadyAdded = tournament.players.some(
        (existingPlayer) => existingPlayer.id === player.id
      );

      if (alreadyAdded) {
        return tournament;
      }

      return {
        ...tournament,
        players: [...tournament.players, player]
      };
    }

    return tournament;
  });
  saveTournaments();
}
  function deleteTournament(id) {
    tournaments = tournaments.filter(
      (tournament) => tournament.id !== id
    );
  }
</script>

<div class="tournament-page">

  <h2>Tournament Management</h2>

  <div class="tournament-form">

    <input
      type="text"
      placeholder="Tournament name"
      bind:value={tournamentName}
    />

    <input
      type="date"
      bind:value={tournamentDate}
    />

    <input
      type="text"
      placeholder="Location"
      bind:value={tournamentLocation}
    />

    <button on:click={addTournament}>
      Create Tournament
    </button>

  </div>

  <div class="tournament-list">

    {#each tournaments as tournament}

      <div class="tournament-card">

        <h3>{tournament.name}</h3>

        <p>Date: {tournament.date}</p>

        <p>Location: {tournament.location}</p>

       <p>
  Players: {tournament.players.length}
</p>

<div class="add-player-section">

  <select
    on:change={(event) => {
      const selectedPlayerId = Number(event.target.value);

      const selectedPlayer = availablePlayers.find(
        (player) => player.id === selectedPlayerId
      );

      if (selectedPlayer) {
        addPlayerToTournament(
          tournament.id,
          selectedPlayer
        );
      }

      event.target.value = "";
    }}
  >

    <option value="">
      Select Player
    </option>

    {#each availablePlayers as player}

      <option value={player.id}>
        {player.name} - Rating: {player.rating}
      </option>

    {/each}

  </select>

</div>

{#if tournament.players.length > 0}

  <div class="selected-players">

    <h4>Registered Players</h4>

    {#each tournament.players as player}

      <p>
        {player.name} - Rating: {player.rating}
      </p>

    {/each}

  </div>

{/if}

<button
  on:click={() => deleteTournament(tournament.id)}
>
  Delete Tournament
</button>
      </div>

    {/each}

  </div>

</div>