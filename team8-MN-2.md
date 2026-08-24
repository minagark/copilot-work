---
layout: default
title: "Rock Paper Scissors: Expert Mode"
heading: "Rock Paper Roshambo in JavaScript"
subheading: "Game History"
description: "Roshambo on Expert Mode"
user-story: "As a player, I want to play Roshambo against the computer and view my game history so that I can see the results of my previous games."
---

Which one will it be?
<a href="#" onclick="playRoshambo('rock')">rock</a>
<a href="#" onclick="playRoshambo('paper')">paper</a>
<a href="#" onclick="playRoshambo('scissors')">scissors</a>
<div id="results"></div>
<br/>
<div id="history"></div>

<script>
games = JSON.parse(localStorage.getItem('games')) || [];
playRoshambo = function(clientGesture){
    if (clientGesture=='rock') {
        result = "win";
    }
    if (clientGesture=='paper') {
        result = "lose";
    }
    if (clientGesture=='scissors') {
        result = "tie";
    }
    document.getElementById('results').innerHTML = result;
    serverGesture = 'scissors';
    saveGame(clientGesture, serverGesture, result);
    showHistory();
}

saveGame = function(clientGesture, serverGesture, result) {
    game = {
        result: result,
        serverGesture: serverGesture,
        clientGesture: clientGesture,
        time: new Date()
    };
    games.push(game);
    localStorage.setItem('games', JSON.stringify(games));
    showHistory();
}

showHistory = function() {
    historyText = "";
    for (game of games) {
        historyText += "<a href='#' onclick=\"deleteGame('" + game.time + "')\">delete</a>";
        historyText += "<div>";
        historyText += game.clientGesture + " | ";
        historyText += game.serverGesture + " | ";
        historyText += game.result + " | ";
        historyText += game.time + " | ";
        historyText += "</div>";
    }
    document.getElementById('history').innerHTML = historyText;
}

deleteGame = function(time) {
    games = games.filter(game => game.time != time);
    localStorage.setItem('games', JSON.stringify(games));
    showHistory();
}

</script>
