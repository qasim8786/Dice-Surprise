# Exploring Probability Dynamics in a Dice Race Game: A Simulation and Analytical Study

**Author:** Qasim Mansoori

*First-Year BCA Student, India*

**Contact:** qasimmansoori853@gmail.com

---

### **Abstract**

This paper investigates a simple yet intriguing probability problem: *"In a dice race game where two players aim to reach different target distances, how does their win probability change?"* Using Python simulations and mathematical analysis, I explore scenarios where players race to complete their targets (e.g., 6 vs. 6, 6 vs. 7, up to 6 vs. 49). The results reveal surprising patterns:

- When both targets are **6 or less**, the win probabilities are approximately **50-50**, regardless of the difference in targets.
- When one player's target is fixed at **6** and the other player's target increases beyond **6**, the win probability initially shifts significantly (around **10%** difference at 6 vs. 7) and then continues to decrease by about **2%** for each additional increment.
- These observations extend to larger targets, with the player needing fewer points consistently having a higher chance of winning.

The inspiration for this exploration came while playing Ludo, a game that also involves dice rolls and movement towards a goal. This study serves as a beginner-friendly introduction to probability simulations and their applications in game theory.

---

### **1. Introduction**

Probability plays a significant role in understanding real-world phenomena, from predicting weather patterns to determining outcomes in games. As a student newly immersed in computer science and mathematics, I was curious about how probability influences seemingly simple games. Inspired by this curiosity—and while playing **Ludo**, a game where players race their tokens to the finish based on dice rolls—I explored a dice race game with varying target distances for two players.

**The central question:** *"If two players have different distances to cover using dice rolls, how does this affect their chances of winning?"*

To investigate, I designed a simulation where two players race to reach their respective targets by rolling a standard six-sided die. This paper presents the findings from both simulations and mathematical analysis, including comprehensive results and code.

---

### **2. The Dice Race Game**

#### **2.1 Game Rules**

1. **Players and Targets:**

   - **Player 1** has a target distance \( T_1 \).
   - **Player 2** has a target distance \( T_2 \).

2. **Gameplay Mechanics:**

   - Players take turns rolling a standard six-sided die (\( 1 \) to \( 6 \)).
   - On their turn, a player rolls the die and, if the roll is less than or equal to their remaining distance, they subtract the roll value from their remaining distance.
   - If the roll exceeds their remaining distance, the player does not move forward that turn.
   - The game continues until both players have reached or surpassed their targets.

3. **Winning Condition:**

   - The player who reaches or surpasses their target in fewer rolls wins.
   - **Tie Condition:** If both players reach their targets in the same number of rolls, the game is declared a tie.

---

### 3.1 Equal Targets Within Die Range (Targets ≤ 6)

When both players have targets within the die’s range (1 to 6), the probabilities are surprisingly **50–50**, regardless of the difference in targets.

**Proof:**

1. **Expected Number of Rolls for Target $N$ (where $N \leq 6$):**

   - Let $E(N)$ be the expected number of rolls for a player to reach a target of $N$.
   - Since a player can potentially win in a single roll, we can model this using geometric probability.

2. **For Target $N$:**

   - The probability of rolling any specific number between 1 and 6 is $\frac{1}{6}$.
   - The probability of rolling a number that exactly matches the remaining distance $N$ is $\frac{1}{6}$.
   - If the player doesn’t roll $N$, they effectively start over, as their position doesn’t change.

3. **Expected Number of Rolls $E(N)$:**

$$
E(N) = \frac{1}{6} \times 1 + \left(1 - \frac{1}{6}\right) \times (1 + E(N))
$$

   - **Explanation:**
     - $\frac{1}{6} \times 1$: Chance to win on the first roll.
     - $\left(1 - \frac{1}{6}\right) \times (1 + E(N))$: One roll used, then the expected rolls starting over.

4. **Solving for $E(N)$:**

$$
E(N) = \frac{1}{6} + \frac{5}{6}(1 + E(N))
$$

Simplify:

$$
E(N) = \frac{1}{6} + \frac{5}{6} + \frac{5}{6}E(N)
$$

$$
E(N) = 1 + \frac{5}{6}E(N)
$$

Subtract $\frac{5}{6}E(N)$ from both sides:

$$
E(N) - \frac{5}{6}E(N) = 1
$$

$$
\left(1 - \frac{5}{6}\right)E(N) = 1
$$

$$
\frac{1}{6}E(N) = 1
$$

$$
E(N) = 6
$$

5. **Conclusion:**

   - The expected number of rolls $E(N)$ is **6** for any target $N$ from 1 to 6.
   - Therefore, both players have an equal chance of winning when their targets are within the die’s range, leading to approximately **50–50** win probabilities.

---

### 3.2 Targets Beyond Die Range (Targets > 6)

When one or both targets exceed 6, the probabilities begin to shift.

**Understanding the Shift:**

1. **Expected Number of Rolls for Targets > 6:**

   - The expected number of rolls $E(N)$ can be approximated as:

$$
E(N) \approx \frac{N}{3.5}
$$

   - Since the average roll of a die is $3.5$.

2. **Comparing Expected Rolls for Players:**

   - **Player 1 (Target $T_1 = 6$):**

$$
E(T_1) = \frac{6}{3.5} \approx 1.714 \text{ rolls}
$$

   - **Player 2 (Target $T_2 \geq 7$):**

$$
E(T_2) = \frac{T_2}{3.5}
$$

3. **Probability Difference:**

   - The player with the lower expected number of rolls has a higher probability of winning.
   - As $T_2$ increases, $E(T_2)$ increases, making Player 2 less likely to win.

4. **Significant Probability Shift at 6 vs. 7:**

   - Moving from targets of 6 vs. 6 to 6 vs. 7 shows an immediate shift in win probability (around **10%** difference).
   - This is due to the discrete nature of small numbers and the added roll required for Player 2.

5. **Gradual Decrease in Probability for Player 2:**

   - For each additional increment in Player 2’s target beyond 7, the expected number of rolls increases linearly.
   - This results in a consistent decrease in Player 2’s win probability by about **2%** per increment.

---

### **4. Simulation Methodology**

#### **4.1 Python Code**

The entire code used for the simulations is presented here:

```python
import random
import matplotlib.pyplot as plt

def dice_race(target1, target2):
    """Simulates a single game where two players race to their respective targets."""
    # Initialize players' remaining distances and roll counts
    p1_distance = target1
    p2_distance = target2
    p1_rolls = 0
    p2_rolls = 0

    # Continue the game until both players reach or surpass their targets
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
    """Simulates multiple games and returns win counts for each player."""
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

def simulate_multiple_targets(num_simulations, target_pairs):
    """Simulates games for multiple target pairs and collects win probabilities."""
    results = []
    for target1, target2 in target_pairs:
        p1_wins, p2_wins, ties = simulate_games(num_simulations, target1, target2)
        total_wins = p1_wins + p2_wins
        p1_percentage = round((p1_wins / total_wins) * 100, 2)
        p2_percentage = round((p2_wins / total_wins) * 100, 2)
        results.append(((target1, target2), (p1_percentage, p2_percentage)))
    return results

def plot_results(results, title):
    """Generates a bar chart comparing win probabilities for different target pairs."""
    labels = [f'{t1}-{t2}' for t1, t2 in [targets for targets, _ in results]]
    p1_wins = [prob[0] for _, prob in results]
    p2_wins = [prob[1] for _, prob in results]

    x = range(len(labels))

    fig, ax = plt.subplots()
    bar_width = 0.35

    ax.bar(x, p1_wins, bar_width, label='Player 1', color='blue')
    ax.bar([i + bar_width for i in x], p2_wins, bar_width, label='Player 2', color='orange')

    ax.set_xlabel('Target Pairs')
    ax.set_ylabel('Win Probability (%)')
    ax.set_title(title)
    ax.set_xticks([i + bar_width / 2 for i in x])
    ax.set_xticklabels(labels, rotation=45)
    ax.legend()

    plt.tight_layout()
    plt.show()

# Define multiple lists of target pairs
target_pairs_list_1 = [
    (6, 6),
    (6, 7),
    (6, 8),
    (6, 9),
    (6, 10),
    (6, 11),
    (6, 12)
]

target_pairs_list_2 = [
    (7, 8),
    (8, 9),
    (9, 10),
    (10, 11),
    (11, 12)
]

target_pairs_list_3 = [
    (6, 13),
    (6, 19),
    (6, 25),
    (6, 31),
    (6, 37),
    (6, 43),
    (6, 49)
]

target_pairs_list_4 = [
    (7, 12),
    (13, 18),
    (18, 24),
    (24, 30)
]

target_pairs_list_5 = [
    (6, 12),
    (12, 18),
    (18, 24),
    (24, 30),
    (30, 36)
]

# Run simulations for each list
num_simulations = 100000

results_list_1 = simulate_multiple_targets(num_simulations, target_pairs_list_1)
results_list_2 = simulate_multiple_targets(num_simulations, target_pairs_list_2)
results_list_3 = simulate_multiple_targets(num_simulations, target_pairs_list_3)
results_list_4 = simulate_multiple_targets(num_simulations, target_pairs_list_4)
results_list_5 = simulate_multiple_targets(num_simulations, target_pairs_list_5)

# Store results in a list
all_results = [results_list_1, results_list_2, results_list_3, results_list_4, results_list_5]

# Print results for each list
for i, results in enumerate(all_results, 1):
    print(f"Results for Target Pairs List {i}:")
    for targets, probabilities in results:
        print(f"Targets: {targets}, Probabilities: {probabilities}")
    print()

# Plot results for each list
plot_results(results_list_1, 'Dice Races: Win Probabilities for Target Pairs List 1')
plot_results(results_list_2, 'Dice Races: Win Probabilities for Target Pairs List 2')
plot_results(results_list_3, 'Dice Races: Win Probabilities for Target Pairs List 3')
plot_results(results_list_4, 'Dice Races: Win Probabilities for Target Pairs List 4')
plot_results(results_list_5, 'Dice Races: Win Probabilities for Target Pairs List 5')
```

#### **4.2 Target Pairs and Results**

The simulations were run for various target pairs, and the results are presented in tables.

**Results for Target Pairs List 1 (Player 1 Target = 6; Player 2 Targets from 6 to 12):**

| Targets (P1 vs. P2) | Player 1 Win % | Player 2 Win % |
|---------------------|----------------|----------------|
| (6, 6)              | 49.90%         | 50.10%         |
| (6, 7)              | 59.06%         | 40.94%         |
| (6, 8)              | 60.17%         | 39.83%         |
| (6, 9)              | 61.74%         | 38.26%         |
| (6, 10)             | 63.28%         | 36.72%         |
| (6, 11)             | 65.07%         | 34.93%         |
| (6, 12)             | 66.93%         | 33.07%         |

**Results for Target Pairs List 2 (Consecutive Increasing Targets):**

| Targets (P1 vs. P2) | Player 1 Win % | Player 2 Win % |
|---------------------|----------------|----------------|
| (7, 8)              | 51.11%         | 48.89%         |
| (8, 9)              | 51.83%         | 48.17%         |
| (9, 10)             | 52.00%         | 48.00%         |
| (10, 11)            | 52.20%         | 47.80%         |
| (11, 12)            | 52.93%         | 47.07%         |

**Results for Target Pairs List 3 (Player 1 Target = 6; Player 2 with Larger Targets):**

| Targets (P1 vs. P2) | Player 1 Win % | Player 2 Win % |
|---------------------|----------------|----------------|
| (6, 13)             | 69.32%         | 30.68%         |
| (6, 19)             | 77.38%         | 22.62%         |
| (6, 25)             | 83.55%         | 16.45%         |
| (6, 31)             | 88.00%         | 12.00%         |
| (6, 37)             | 91.11%         | 8.89%          |
| (6, 43)             | 93.50%         | 6.50%          |
| (6, 49)             | 95.35%         | 4.65%          |

**Results for Target Pairs List 4 (Moderate Increases):**

| Targets (P1 vs. P2) | Player 1 Win % | Player 2 Win % |
|---------------------|----------------|----------------|
| (7, 12)             | 60.20%         | 39.80%         |
| (13, 18)            | 61.02%         | 38.98%         |
| (18, 24)            | 62.86%         | 37.14%         |
| (24, 30)            | 62.81%         | 37.19%         |

**Results for Target Pairs List 5 (Larger Targets for Both Players):**

| Targets (P1 vs. P2) | Player 1 Win % | Player 2 Win % |
|---------------------|----------------|----------------|
| (6, 12)             | 66.95%         | 33.05%         |
| (12, 18)            | 63.92%         | 36.08%         |
| (18, 24)            | 63.47%         | 36.53%         |
| (24, 30)            | 62.73%         | 37.27%         |
| (30, 36)            | 62.26%         | 37.74%         |

---

### **5. Analysis and Discussion**

#### **5.1 Observations from the Results**

- **Equal Targets (6 vs. 6):** The win probabilities are approximately equal, confirming the mathematical proof that when targets are within the die's range, both players have an equal chance of winning.

- **Initial Shift Beyond Die Range (6 vs. 7):** A noticeable shift in win probability occurs when Player 2's target increases from 6 to 7, with Player 1's win probability increasing by about **9%**.

- **Gradual Decrease in Player 2's Win Probability:** As Player 2's target increases further, their win probability decreases by approximately **1-2%** per increment, consistent across different target ranges.

- **Large Target Differences:** When Player 2's target is significantly higher (e.g., 6 vs. 49), Player 1's win probability becomes overwhelmingly high (**95.35%**), illustrating the cumulative disadvantage for Player 2.

#### **5.2 Implications**

- **Impact of Target Difference:** The greater the difference in targets, the more advantageous it is for the player with the lower target, due to the fewer expected rolls needed to win.

- **Practical Understanding:** This has practical implications in games like **Ludo**, where players race to reach a goal based on dice rolls. Understanding these probabilities can influence game strategies.

- **Probability in Game Design:** Game designers can use these insights to balance games, ensuring fairness by adjusting rules or mechanics to account for probabilistic advantages.

---

### **6. Conclusion**

This study explores the dynamics of win probabilities in a simple dice race game. Through mathematical proofs and simulations, we observe that:

1. **Equal Targets Within Die Range:**

   - Both players have an equal chance of winning when their targets are within the die's range (1-6), leading to approximately **50-50** win probabilities.

2. **Targets Beyond Die Range:**

   - An initial significant shift in win probability occurs from 6 vs. 6 to 6 vs. 7.
   - Subsequent increases in Player 2's target result in a consistent decrease in their win probability by about **1-2%** per increment.

3. **Larger Target Differences:**

   - As the target difference widens, the player with the lower target gains a substantial probabilistic advantage.

**Final Thought:**

This exploration demonstrates how even simple games can exhibit complex probability dynamics. By combining mathematical reasoning with practical experimentation, we gain deeper insights into game mechanics—a journey that began with a simple thought inspired by playing **Ludo**.

---

**Contact Information**

Feel free to reach out with questions or comments:

- **Email:** qasimmansoori853@gmail.com

---

*This study showcases how simple games can reveal complex probability behaviors. By combining mathematical proofs with simulations, we gain a deeper understanding of the nuances in game outcomes. I hope this inspires others to explore and question the underlying mathematics in everyday scenarios, just as I did while playing Ludo.*
