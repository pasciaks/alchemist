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
