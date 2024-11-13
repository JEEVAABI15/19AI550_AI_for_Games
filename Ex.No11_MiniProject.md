# Ex.No: 11  Mini Project 
### DATE: 25.10.2024                                                                          
### REGISTER NUMBER : 212221240018
### AIM: 
To write a Python program to simulate an Adaptive Combat Game where the AI learns and adapts to the player’s moves using Reinforcement Learning techniques.
### Algorithm:
1. Initialize the Pygame library and create a window for the game display.
2. Define constants such as screen width, height, and colors.
3. Define game elements like player and AI actions (Attack, Block, Counter).
4. Use Q-learning to train the AI to adapt and learn optimal strategies.
5. Define the rules of the game and create functions to check the player's and AI's moves.
6. Continuously update the game screen to show the current score and display moves.
7. Allow the player to interact by choosing their move, and the AI will adapt.
8. Display the final score and end the game when the player reaches a winning condition.
### Program:
```py
```python
import pygame
import random
import sys
import numpy as np

# Initialize Pygame
pygame.init()
width = 700
height = 600
display = pygame.display.set_mode((width, height))
pygame.display.set_caption("Adaptive Combat Game: AI vs Player")
clock = pygame.time.Clock()

# Define colors
white = (230, 230, 230)
blue = (50, 50, 255)
green = (0, 255, 0)
red = (255, 0, 0)
yellow = (255, 255, 0)

# Initialize fonts
font = pygame.font.SysFont("Arial", 30)

# Define game actions
actions = ["Attack", "Block", "Counter"]
action_color = {"Attack": red, "Block": green, "Counter": blue}

# Initialize scores
player_score = 0
ai_score = 0

# Q-learning class for AI
class AdaptiveAI:
    def __init__(self):
        self.q_table = {}
        self.learning_rate = 0.1
        self.discount_factor = 0.9
        self.exploration_rate = 1.0
        self.exploration_decay = 0.995
        self.min_exploration_rate = 0.1

    def get_state(self, player_move, ai_move):
        return (player_move, ai_move)

    def get_q_value(self, state, action):
        if state not in self.q_table:
            self.q_table[state] = {action: 0 for action in actions}
        return self.q_table[state].get(action, 0)

    def choose_action(self, state):
        if random.uniform(0, 1) < self.exploration_rate:
            return random.choice(actions)  # Explore: random action
        else:
            q_values = [self.get_q_value(state, action) for action in actions]
            max_q_value = max(q_values)
            return actions[q_values.index(max_q_value)]

    def learn(self, state, action, reward, next_state):
        old_q_value = self.get_q_value(state, action)
        future_q_value = max([self.get_q_value(next_state, next_action) for next_action in actions])
        new_q_value = old_q_value + self.learning_rate * (reward + self.discount_factor * future_q_value - old_q_value)
        self.q_table[state][action] = new_q_value

    def decay_exploration_rate(self):
        self.exploration_rate = max(self.min_exploration_rate, self.exploration_rate * self.exploration_decay)

# Function to handle the battle rules
def battle(player_move, ai_move):
    if player_move == ai_move:
        return 0  # Draw
    elif (player_move == "Attack" and ai_move == "Block") or (player_move == "Block" and ai_move == "Counter") or (player_move == "Counter" and ai_move == "Attack"):
        return -1  # AI loses
    else:
        return 1  # AI wins

# Function to display score
def show_score():
    score_text = font.render(f"Player: {player_score} | AI: {ai_score}", True, white)
    display.blit(score_text, (250, 20))

# Function to display the game’s main interface
def game():
    global player_score, ai_score
    ai = AdaptiveAI()
    loop = True

    while loop:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_q:
                    pygame.quit()
                    sys.exit()

            # Handling player input
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_a:
                    player_move = "Attack"
                elif event.key == pygame.K_b:
                    player_move = "Block"
                elif event.key == pygame.K_c:
                    player_move = "Counter"

                # AI makes a move
                ai_move = ai.choose_action(("Player", player_move))
                result = battle(player_move, ai_move)

                if result == 1:
                    ai_score += 1
                elif result == -1:
                    player_score += 1

                ai.learn(("Player", player_move), ai_move, result, ("AI", ai_move))
                ai.decay_exploration_rate()

                # Display game state
                display.fill(blue)
                show_score()

                # Show moves
                player_text = font.render(f"Player chose: {player_move}", True, white)
                ai_text = font.render(f"AI chose: {ai_move}", True, white)
                display.blit(player_text, (150, 100))
                display.blit(ai_text, (150, 150))

                pygame.display.update()
                clock.tick(60)

# Running the game
game()
```










### Output:


![image](https://github.com/user-attachments/assets/767db48d-91cd-4ab7-8bda-c850cdef5f2b)

## Explanation of the Program:
### AI Agent:
###### The AdaptiveAI class uses Q-learning to train the AI. It adjusts its strategy based on the actions taken by both the player and the AI itself.
### Game Rules:
###### The game follows a simplified version of Rock, Paper, Scissors, where each move has an associated advantage over another. The game evaluates each round and updates the score.
### UI and Controls:
###### The user interacts using the keyboard (Press A for Attack, B for Block, and C for Counter).
##### The AI makes its move based on the trained model, and the current state of the game is displayed on the screen with the scores.

### Result:
Thus, the Adaptive Combat Game using AI and Reinforcement Learning was successfully implemented. The AI adapts to the player's moves, providing a progressively harder challenge.


