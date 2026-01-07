Rock-Paper-Scissors 

Minimal AI Game Referee using Google ADK with clean tool separation.

State Model
Single dictionary holds all game state:
```python
state = {
    "round": 0,              # Current round (0-2)
    "user_score": 0,         # User wins
    "bot_score": 0,          # Bot wins
    "user_bomb_used": False, # Bomb tracking
    "bot_bomb_used": False
}
```

State lives in Python memory and not in AI prompts.

Tool Design

Have use 3 ADK tools :

1. validate_move(move) 
- Purpose: Check's if a move is legal
- Logic: 
  - Normalize input
  - Check valid moves list
  - Check bomb availability
- Returns: `{valid: bool, move/error: str}`

2. resolve_round(user_move, bot_move)
- Purpose: Determine's the round winner
- Logic:
  - Bomb rules (priority)
  - Same move = draw
  - RPS rules
- Returns: {winner: str, msg: str}

3. update_game_state(**updates)
- Purpose: Updates the game state
- Logic: Updates state dictionary
- Returns: Current state as JSON

Architecture 

```
User Input → validate_move() → resolve_round() → update_game_state() → Display
```
Each layer performs one single task

-**For Setup**

```bash
# Install
pip install google-generativeai

# Set API key
export GOOGLE_API_KEY="your-key"

# Run
python game.py
```

## Example Play

```
--- ROUND 1 ---
Score: You 0 - Bot 0
Bomb: AVAILABLE

Your move: rock

You: ROCK
Bot: SCISSORS
→ rock beats scissors!

--- ROUND 2 ---
Score: You 1 - Bot 0
...
```


- **Clear flow**: Linear progression through tools matches game logic

Total: **~100 lines** of clean, testable code that meets all requirements.
