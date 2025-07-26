# Wer-l-gt-Demo

<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Wer lügt Demo</title>
<style>
  body { font-family: Arial, sans-serif; background: #f9f9f9; padding: 20px; }
  .lobby, .game { max-width: 400px; margin: auto; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 0 10px #ccc; }
  button { margin: 5px; padding: 10px 20px; font-size: 16px; }
  .cards { display: flex; gap: 8px; margin: 10px 0; }
  .card { width: 50px; height: 70px; background: #e3e3e3; border-radius: 5px; display: flex; align-items: center; justify-content: center; font-weight: bold; cursor: pointer; }
  .card.selected { background: #f44336; color: white; }
</style>
</head>
<body>

<div class="lobby" id="lobby">
  <h2>Wer lügt - Demo Lobby</h2>
  <input type="text" id="name" placeholder="Dein Name" /><br />
  <button onclick="createLobby()">Lobby erstellen</button>
  <button onclick="joinLobby()">Lobby beitreten</button>
  <input type="text" id="joinCode" placeholder="Lobby-Code" />
</div>

<div class="game" id="game" style="display:none;">
  <h2>Lobby: <span id="lobbyCode"></span></h2>
  <div>Rundenkarte: <span id="roundCardType">Herz</span></div>
  <div>Aktueller Spieler: <span id="currentPlayer">Du</span></div>
  <div>Deine Karten:</div>
  <div class="cards" id="handCards"></div>
  <button onclick="playCards()">Karten legen</button>
  <button onclick="callLie()">Lüge rufen</button>
</div>

<script>
  let lobbyCode = '';
  let handCards = ['Herz 7', 'Herz 8', 'Herz 9', 'Herz 10', 'Herz Bube', 'Herz Dame'];
  let selectedIndices = [];

  function createLobby() {
    lobbyCode = 'ABCDE';
    document.getElementById('lobbyCode').textContent = lobbyCode;
    document.getElementById('lobby').style.display = 'none';
    document.getElementById('game').style.display = 'block';
    renderHand();
    alert('Lobby erstellt! Code: ' + lobbyCode);
  }

  function joinLobby() {
    const code = document.getElementById('joinCode').value.trim().toUpperCase();
    if (!code) return alert('Bitte Lobby-Code eingeben');
    lobbyCode = code;
    document.getElementById('lobbyCode').textContent = lobbyCode;
    document.getElementById('lobby').style.display = 'none';
    document.getElementById('game').style.display = 'block';
    renderHand();
    alert('Lobby betreten! Code: ' + lobbyCode);
  }

  function renderHand() {
    const handDiv = document.getElementById('handCards');
    handDiv.innerHTML = '';
    handCards.forEach((card, i) => {
      const cardEl = document.createElement('div');
      cardEl.className = 'card' + (selectedIndices.includes(i) ? ' selected' : '');
      cardEl.textContent = card;
      cardEl.onclick = () => {
        if (selectedIndices.includes(i)) {
          selectedIndices = selectedIndices.filter(idx => idx !== i);
        } else {
          if (selectedIndices.length < 3) selectedIndices.push(i);
        }
        renderHand();
      };
      handDiv.appendChild(cardEl);
    });
  }

  function playCards() {
    if (selectedIndices.length === 0) return alert('Bitte mindestens eine Karte auswählen');
    const played = selectedIndices.map(i => handCards[i]);
    alert('Karten gelegt: ' + played.join(', '));
    handCards = handCards.filter((_, i) => !selectedIndices.includes(i));
    selectedIndices = [];
    renderHand();
  }

  function callLie() {
    alert('Lüge gerufen!');
  }
</script>

</body>
</html>
