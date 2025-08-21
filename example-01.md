# Example 01 Ollama Gemma3 Image Recognition

In this example we attempt to use the Gemma3 models image recognition capabilities. 
Using images of a game of go board we hope to use the model to score the game. 

## Test image used

![gogame6](https://github.com/user-attachments/assets/0a72a4ab-b68a-484c-b60e-6b1cce8317bb)

## Comparison

<img width="530" height="605" alt="image" src="https://github.com/user-attachments/assets/30baa799-f720-4bec-9a6e-6f7d24448673" />

[Online scoring site](https://www.crazy-sensei.com/?lang=en&location=kifu_snap&board_size=19&position=....%23..................%23...............%23%23%23%23......................................................................................................................................................................................................................................................................OOOO..................O.................................&color=b&rules=c&komi=7.5&handicap=0#analysis)

## Ollama ModelFile Customisations

We want the model to respond deterministically so we turn the temperture down to zero.

```
FROM gemma3:4b
PARAMETER num_ctx 131072
PARAMETER temperature 0.0
PARAMETER repeat_penalty 1.5
```

## Prompts

### Prompt 1

```
ollama run gemma3:4b_ex "What are the rules for the ancient board game of Go?"
```

```
Okay, let's break down the rules of Go, a game of incredible depth and strategy. It’s a deceptively simple game with a vast amount of complexity. Here's a comprehensive overview:

**1. The Basics**

* **The Board:** Go is played on a grid of lines. Standard boards are 19x19, but smaller boards (9x9 and 13x13) are common for beginners.
* **The Stones:** Two players, Black and White, use stones of their respective colors to play.
* **Objective:** The goal is to surround more territory than your opponent. Territory is empty space completely enclosed by your stones.

**2. Gameplay**

* **Turns:** Black always goes first. Players alternate placing one stone of their color on an empty intersection (called a "point") on the board.
* **Placement:** Stones are placed permanently once they are placed. You cannot move them.
* **No Capturing (Initially):**  Unlike chess, capturing isn't the primary mechanic at the start. Stones are placed to surround and control territory.

**3. Capturing Stones**

* **Liberties:** Every stone (or group of connected stones) has "liberties." A liberty is an empty point directly adjacent (horizontally or vertically, *not* diagonally) to the stone or group.
* **Surrounding:** If you completely surround an opponent's stone or group, so that it has *no* remaining liberties, you "capture" it. The captured stones are removed from the board and kept as prisoners.
* **Self-Capture:** You cannot play a stone that would immediately result in the self-capture of your own group. (This is a crucial rule!)

**4. Ending the Game**

The game ends when both players agree that no further moves will significantly change the territory control. This usually happens when the board is mostly filled, and further plays would just be about
securing small areas.

* **Passing:** A player can "pass" their turn. Passing indicates that they don't want to play a stone.
* **Mutual Agreement:** The game ends when both players pass consecutively.

**5. Scoring**

This is where it gets a little more involved:

* **Territory:** Each player counts the empty points they have completely surrounded.
* **Prisoners:** Each player adds the number of stones they captured (prisoners) to their territory count.
* **Komi:** Because White always goes second, Black has a slight advantage. To compensate, White receives a "komi" – a fixed number of points added to their score at the end of the game. The standard komi is
6.5 points (the .5 prevents ties).
* **Black's Score:** Black's score is the total territory + prisoners.
* **White's Score:** White's score is the total territory + prisoners + komi.

**The player with the higher score wins.**

**6. Key Concepts & Strategic Elements**

* **Influence:** Stones don't just control territory; they also exert "influence" over surrounding areas, making it easier to secure territory later.
* **Shape:** The arrangement of your stones is critical. Certain shapes are stronger and more difficult to attack.
* **Joseki:** These are established sequences of moves played in the corners of the board. Learning joseki can give you a good starting position.
* **Tesuji:** Clever tactical moves that can create opportunities or save your stones.


**Resources for Learning More:**

* **Online Go Servers:** OGS (Online Go Server) - [https://online-go.com/](https://online-go.com/) - Great for playing against others and learning.
* **Sensei's Library:** [http://senseis.xoom.com/](http://www.senseis.xoom.com/) - A fantastic online encyclopedia of Go knowledge.
* **YouTube Tutorials:** Search for "Go tutorial for beginners" on YouTube – there are many excellent videos.

---

**Important Note:** Go is a game of immense depth. These rules provide a basic understanding. Mastering Go takes years of practice and study.

Do you want me to delve deeper into a specific aspect of the game, such as:

*   Joseki?
*   Capturing techniques?
*   The scoring system in more detail?
```

### Prompt 2

```
ollama run gemma3:4b_ex "Describe in detail how to score the game of Go?"
```

```
Okay, let's break down how to score a game of Go. It’s a deceptively simple system with surprisingly complex implications. Here's a detailed explanation, covering the key concepts and the scoring process:

**1. The Basics: Territory and Captures**

* **Territory:** The core of Go is claiming territory. Territory is empty space completely surrounded by your stones.  A single empty point surrounded by your stones is one point of territory.
* **Captures:** When a group of stones is completely surrounded by the opponent’s stones, it’s captured and removed from the board. Captured stones are *not* counted as territory.


**2. The Scoring Process – At the End of the Game**

The scoring process happens *after* both players have passed, indicating they believe no more advantageous moves can be made. Here's the breakdown:

**Step 1: Dead Stones**

* **Identify Dead Stones:** The players must agree on which stones are "dead." A stone or group of stones is considered dead if it has no *liberties* (empty adjacent points) and cannot be saved by any further
play.  This is often the most contentious part of the game, and players may dispute dead stones.
* **Removal:** Once dead stones are agreed upon, they are removed from the board.

**Step 2: Counting Territory**

This is where the actual scoring happens. There are two main methods for counting territory:

* **Area Scoring (Most Common):** This is the most widely used method today.
    * **Each player counts the empty points completely surrounded by their stones.** This includes the points within their territory.
    * **Each player also counts the number of stones they have on the board.** These stones contribute to their territory.
* **Territory Scoring (Traditional):** This method is less common now but still used in some circles.
    * **Each player counts the empty points completely surrounded by their stones.**
    * **Each player then adds the number of stones they have on the board to the territory they’ve claimed.**

**Step 3: Komi**

* **What is Komi?** Komi is a compensation given to White (who plays first) to balance the advantage of going first.  Because White has the initiative, they are statistically more likely to gain territory.
* **How is it calculated?** The komi is a fractional number (usually 6.5 or 7.5) that is added to White’s score. The 0.5 is crucial – it prevents ties.
* **Why fractional?** If the komi were a whole number, a player could theoretically force a tie by playing perfectly. The fractional komi ensures that a win is always possible.

**Step 4: Final Calculation**

1. **White’s Score:** White’s score is the number of points they claim in territory *plus* the number of stones they have on the board.
2. **Black’s Score:** Black’s score is the number of points they claim in territory *plus* the number of stones they have on the board.
3. **The player with the higher score wins.**  If the scores are tied after all the adjustments, the game is a draw.


**Example (Simplified)**

Let’s say:

* White has 100 points of territory.
* White has 20 stones on the board.
* Black has 80 points of territory.
* Black has 30 stones on the board.
* Komi is 6.5

White’s Score: 100 + 20 = 120
Black’s Score: 80 + 30 = 110

White wins by 10 points.

**Important Notes & Nuances**

* **Suicide:** You cannot play a move that results in your own stones being immediately captured (suicide).
* **Ko Rule:** This rule prevents infinite loops. If a capture creates a situation where the opponent can immediately recapture, the player who made the initial capture cannot recapture immediately. They must
play elsewhere first.
* **Handicap Games:** In handicap games (where a weaker player gets a head start), the scoring process is adjusted to account for the handicap stones.

**Resources for Further Learning:**

* **Sensei's Library:** [https://senseis.com/go/scoring](https://www.senseis.com/go/scoring) – A fantastic, detailed explanation.
* **Online Go Servers:** Playing online (OGS, KGS) is the best way to learn by doing.

Do you want me to delve deeper into a specific aspect of Go scoring, such as:

*   The Ko rule in more detail?
*   How dead stones are determined?
*   How handicap games are scored?
```
