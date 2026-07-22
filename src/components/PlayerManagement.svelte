<script>
  import { onMount } from "svelte";
  let players = [
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

  let name = "";
  let email = "";
  let rating = "";
  let editingPlayerId = null;
onMount(() => {
  const savedPlayers = localStorage.getItem("chessPlayers");

  if (savedPlayers) {
    players = JSON.parse(savedPlayers);
  }
});
function savePlayers() {
  localStorage.setItem("chessPlayers", JSON.stringify(players));
}
 function addPlayer() {
  if (name === "" || email === "" || rating === "") {
    alert("Please fill all fields");
    return;
  }

  if (editingPlayerId) {
    players = players.map((player) => {
      if (player.id === editingPlayerId) {
        return {
          ...player,
          name: name,
          email: email,
          rating: rating
        };
      }

      return player;
    });

    editingPlayerId = null;
  } else {
    const newPlayer = {
      id: Date.now(),
      name: name,
      email: email,
      rating: rating
    };

    players = [...players, newPlayer];
  }

  name = "";
  email = "";
  rating = "";
}


  function deletePlayer(id) {
    players = players.filter((player) => player.id !== id);
  }
  function editPlayer(player) {
  editingPlayerId = player.id;

  name = player.name;
  email = player.email;
  rating = player.rating;
}
</script>

<div class="player-page">

  <h1>Chess Tournament Manager</h1>

  <div class="form">

    <input
      type="text"
      placeholder="Player name"
      bind:value={name}
    />

    <input
      type="email"
      placeholder="Email"
      bind:value={email}
    />

    <input
      type="number"
      placeholder="Rating"
      bind:value={rating}
    />

   <button on:click={addPlayer}>
  {editingPlayerId ? "Update Player" : "Add Player"}
</button>

  </div>

  <h2>Players</h2>

  {#each players as player}

    <div class="player">

      <div>
        <h3>{player.name}</h3>
        <p>{player.email}</p>
        <p>Rating: {player.rating}</p>
      </div>
    <div class="actions">
<button
  class="edit"
  on:click={() => editPlayer(player)}
>
  Edit
</button>
      <button
        class="delete"
        on:click={() => deletePlayer(player.id)}
      >
        Delete
      </button>
      </div>

    </div>

  {/each}

</div>