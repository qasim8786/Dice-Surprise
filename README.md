# The 50-50 Dice Surprise: How I Accidentally Rediscovered a Math Truth

**Qasim Mansoori**

*BCA Student, India*

**Contact**: qasimmansoori853@gmail.com

---

## The Day My Intuition Failed

As a 20-year-old student who switched from PCB (Biology) to computer science, while coding a simple dice game, I stumbled onto something that made me rethink everything I knew about probability.

Here’s what happened: I created a game where two players race to reach their targets by rolling dice. Player A needed just **1 point** to win, while Player B needed **6 points**. "Obviously," I thought, "Player A will dominate." But when I ran the code, the results shocked me. After 1 million simulations, the win rate was split almost **50-50**.

I spent some time tweaking the targets, testing other pairs (5 vs. 2, 4 vs. 3) – still the same result.

---

## The Code That Broke My Brain

Here’s the Python code that started it all. Feel free to copy-paste and try it yourself:

```python
import random
import plotly.graph_objects as go

def dice_race(target1, target2):
    p1_distance = target1
    p2_distance = target2
    p1_rolls = 0
    p2_rolls = 0

    while p1_distance > 0 or p2_distance > 0:
        # Player 1's turn
        if p1_distance > 0:
            roll = random.randint(1, 6)
            p1_rolls += 1
            if roll <= p1_distance:
                p1_distance -= roll

        # Player 2's turn
        if p2_distance > 0:
            roll = random.randint(1, 6)
            p2_rolls += 1
            if roll <= p2_distance:
                p2_distance -= roll

    return p1_rolls, p2_rolls

def simulate_games(num_simulations, target1, target2):
    p1_wins = 0
    p2_wins = 0
    ties = 0
    for _ in range(num_simulations):
        p1_rolls, p2_rolls = dice_race(target1, target2)
        if p1_rolls < p2_rolls:
            p1_wins += 1
        elif p2_rolls < p1_rolls:
            p2_wins += 1
        else:
            ties += 1

    return p1_wins, p2_wins, ties

# Let's run 1 million games!
num_simulations = 1000000
target1 = 1  # player 1
target2 = 6  # player 2

p1_wins, p2_wins, ties = simulate_games(num_simulations, target1, target2)

# Visualize the madness
labels = ['Player 1 (Needs 1)', 'Player 2 (Needs 6)', 'Ties']
wins = [p1_wins, p2_wins, ties]

fig = go.Figure(data=[go.Bar(x=labels, y=wins, marker_color=['blue', 'orange', 'green'])])
fig.update_layout(
    title='1 Million Dice Races: Who Wins?',
    xaxis_title='Players',
    yaxis_title='Number of Wins',
    template='plotly_dark'
)
fig.show()
```

---

## My Results

After running this code repeatedly, here’s what I found:

| Players       | Wins (1 Million Trials) | Win Percentage |
|---------------|-------------------------|----------------|
| Needs 1 (A)   | 454,468                 | 45.45%         |
| Needs 6 (B)   | 454,273                 | 45.45%         |
| Ties          | 91,259                  | 9.13%          |

The 50-50 split held in every case.

---

## The Math Behind the Madness

Later, I discovered why this happens – there’s an equation that explains it:

For any target distance $N$, the **average number of rolls needed** is always 6. Here’s the proof that made me gasp:

1. Let $E(N)$ be the expected number of rolls to reach 0 from $N$.

2. For $N = 1$:

   - You have a $\frac{1}{6}$ chance to win in 1 roll.
   - A $\frac{5}{6}$ chance to stay at 1 and try again.

   So,

   $$E(1) = \frac{1}{6} \times 1 + \frac{5}{6} \times (1 + E(1))$$

   Simplify:

   $$E(1) = \frac{1}{6} + \frac{5}{6} (1 + E(1))$$

   $$E(1) = \frac{1}{6} + \frac{5}{6} + \frac{5}{6} E(1)$$

   $$E(1) = 1 + \frac{5}{6} E(1)$$

   Subtract $\frac{5}{6} E(1)$ from both sides:

   $$E(1) - \frac{5}{6} E(1) = 1$$
   
   $$\left(1 - \frac{5}{6}\right) E(1) = 1$$

   $$\frac{1}{6} E(1) = 1$$

   Therefore,

   $$E(1) = 6$$

4. This logic extends to any $N$! Whether you need 1 roll or 6, the average is always **6 rolls**.

---

## Final Thoughts

To anyone saying "This is already known": You’re right. But rediscovering it myself taught me more than any lecture ever could. To every student out there – your curiosity matters, even if you’re "just" verifying old truths.
