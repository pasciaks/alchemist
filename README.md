# alchemist


## Here's simple JavaScript code that retrieves the stored scores from the backend.

```Javascript

    /* -------------------------------------------------------
       API FETCH
    ------------------------------------------------------- */

    async function getItchScores() {
      const response = await fetch(
        "https://smartsign.smartsigntechnology.com/itch-scores-get",
        {
          method: "GET",
          headers: { "Content-Type": "application/json" }
        }
      );

      if (!response.ok) {
        throw new Error(`HTTP error ${response.status}`);
      }

      return response.json();
    }


```

## Here's simple JavaScript code that sends the scores to the backend.

```Javascript

    async function submitScoresForItchGame(
      playerName,
      playerTime,
      playerLevel,
      playerStars,
      playerGame
    ) {
      try {
        const response = await fetch(
          "https://smartsign.smartsigntechnology.com/itch-scores",
          {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
              // Add auth headers if needed
            },
            body: JSON.stringify({
              playerName: playerName.toString(),
              playerTime: playerTime.toString(),
              playerLevel: playerLevel.toString(),
              playerStars: playerStars.toString(),
              playerGame: playerGame.toString()
            }),
          }
        );

        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }

        const result = await response.json();
        console.log("Score submitted:", result);

      } catch (err) {
        console.error("Error submitting score:", err);
      }
    }

    // Example usage
    // submitScoresForItchGame("Player Name", 15.25, "1", "***", "alchemist");

```

## Note: For now, the game name 'alchemist' is the only supported game.  This will change soon, but for now I wanted to limit use until I fully test and evaluate if anyone else really would considering using this backend for global scores.  If you are interested in using it, just submit a GitHub issue with your proposed game name as well as your itch.io game page link.

## Additionally, if you create the appropriate code for Godot, Unity or other platforms which can serve as a simple example of how to use this, please submit that as well and I will include it here.

