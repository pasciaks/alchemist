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

---

# Chat GPT yielded the following conversions for possible use in Godot

---

## Godot Script (GDScript 4.x)

## You can drop this on any Node.

```gdscript

extends Node

const ITCH_SCORES_URL := "https://smartsign.smartsigntechnology.com/itch-scores-get"

var http_request: HTTPRequest

func _ready():
    http_request = HTTPRequest.new()
    add_child(http_request)
    http_request.request_completed.connect(_on_request_completed)

    # Call it whenever you want
    get_itch_scores()


func get_itch_scores() -> void:
    var headers = [
        "Content-Type: application/json"
    ]

    var err = http_request.request(
        ITCH_SCORES_URL,
        headers,
        HTTPClient.METHOD_GET
    )

    if err != OK:
        push_error("Failed to start HTTP request: %s" % err)


func _on_request_completed(
    result: int,
    response_code: int,
    headers: PackedStringArray,
    body: PackedByteArray
) -> void:

    if result != HTTPRequest.RESULT_SUCCESS:
        push_error("Request failed with result: %s" % result)
        return

    if response_code < 200 or response_code >= 300:
        push_error("HTTP error %s" % response_code)
        return

    var json_text := body.get_string_from_utf8()
    var data = JSON.parse_string(json_text)

    if data == null:
        push_error("Failed to parse JSON")
        return

    # Equivalent to: return response.json()
    print("Itch scores:", data)



```

## Godot 4.x — POST Score Submission Script

## Attach this to any Node.

```gdscript

extends Node

const SUBMIT_SCORE_URL := "https://smartsign.smartsigntechnology.com/itch-scores"

var http_request: HTTPRequest


func _ready():
    http_request = HTTPRequest.new()
    add_child(http_request)
    http_request.request_completed.connect(_on_submit_completed)

    # Example usage
    submit_scores_for_itch_game(
        "Player Name",
        15.25,
        "1",
        "***",
        "alchemist"
    )


func submit_scores_for_itch_game(
    player_name,
    player_time,
    player_level,
    player_stars,
    player_game
) -> void:

    var headers = [
        "Content-Type: application/json"
        # Add auth headers here if needed
        # "Authorization: Bearer TOKEN"
    ]

    var payload = {
        "playerName": str(player_name),
        "playerTime": str(player_time),
        "playerLevel": str(player_level),
        "playerStars": str(player_stars),
        "playerGame": str(player_game)
    }

    var json_body := JSON.stringify(payload)

    var err = http_request.request(
        SUBMIT_SCORE_URL,
        headers,
        HTTPClient.METHOD_POST,
        json_body
    )

    if err != OK:
        push_error("Failed to start POST request: %s" % err)


func _on_submit_completed(
    result: int,
    response_code: int,
    headers: PackedStringArray,
    body: PackedByteArray
) -> void:

    if result != HTTPRequest.RESULT_SUCCESS:
        push_error("Request failed: %s" % result)
        return

    if response_code < 200 or response_code >= 300:
        push_error("HTTP error! status: %s" % response_code)
        return

    var json_text := body.get_string_from_utf8()
    var data = JSON.parse_string(json_text)

    if data == null:
        push_error("Failed to parse response JSON")
        return

    print("Score submitted:", data)


```

