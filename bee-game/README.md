# Project Roadmap: Endless Runner Game

## Game Overview

* Endless runner game with 3 lanes
* Player moves left and right to avoid obstacles
* Collectible items (x) appear randomly, offering points
* Game ends upon collision with obstacles
* Score based on time survived and items collected
* High score tracking

## Key Features

* 3 lanes with downward movement
* Randomly spawned obstacles (single or double)
* Randomly spawned collectible items (x)
* Time-based scoring with item collection bonuses
* High score display

## Game Architecture Design

### Game Loop

* Responsible for updating and rendering the game state
* Runs continuously until the game is exited
* Typically implemented as a loop that repeats at a fixed rate (e.g., 60 times per second)
* Handles:
	+ User input
	+ Game state updates
	+ Collision detection
	+ Scoring
	+ Rendering

**Game Loop**
```c
while (gameRunning) {
    // Handle input
    handleUserInput();

    // Update game state
    updatePlayer();
    updateObstacles();
    updateCollectibles();

    // Check collisions
    checkCollisions();

    // Update score
    updateScore();

    // Render game state
    renderGame();

    // Cap frame rate
    delay(16); // 60 FPS
}
```
### Game State

* Manages the current status of the game
* Includes:
	+ Player data (position, velocity, score, etc.)
	+ Obstacle data (positions, velocities, types)
	+ Collectible data (positions, types)
	+ Game mode (playing, paused, game over)
	+ Score (current and high score)

### Player

* Manages player movement and interactions
* Includes:
	+ Position, velocity, width, and height (for collision detection)
	+ Movement logic (left and right movement)

### Collision System (Observer)

* Manages collision detection and notification
* Includes:
	+ Observer pattern implementation
	+ Registers with Subjects (Player and Obstacles)
	+ `update()` method:
		- Calls `checkCollisions()` to detect collisions
		- Notifies Game State to update accordingly (e.g., displays collision text)

```c
class CollisionSystem {
public:
    void update() {
        // Check for collisions
        if (checkCollisions()) {
            // Notify Game State to update accordingly
            notifyGameState();
        }
    }

private:
    bool checkCollisions() {
        // Check for collisions between Player and Obstacles
        for (Obstacle obstacle : obstacles) {
            if (player.x < obstacle.x + obstacle.width &&
                player.x + player.width > obstacle.x &&
                player.y < obstacle.y + obstacle.height &&
                player.y + player.height > obstacle.y) {
                return true;
            }
        }

        // Check for collisions between Player and Collectibles
        for (Collectible collectible : collectibles) {
            if (player.x < collectible.x + collectible.width &&
                player.x + player.width > collectible.x &&
                player.y < collectible.y + collectible.height &&
                player.y + player.height > collectible.y) {
                return true;
            }
        }

        return false;
    }

    void notifyGameState() {
        // Display collision text
        displayCollisionText();
    }
};
```
### Collectibles

* Manages collectible item spawning and collection
* Includes:
	+ Spawning logic (random or predefined)
	+ Collection logic
	+ Scoring
	+ Types (different types of collectibles)

### Graphics

* Handles rendering of game elements using SplashKit
* Includes:
	+ Player rendering
	+ Obstacle rendering
	+ Collectible rendering
	+ Background rendering
	+ UI rendering (score, game over screen, etc.)

### Input

* Handles user input (keyboard or mouse events)
* Includes:
	+ Player movement input
	+ Pause/resume input
	+ Restart input

## Tasks

1. Set up SplashKit and create a basic game window
2. Implement game loop and game state management
3. Create player entity and implement movement
4. Implement obstacle spawning and movement
5. Implement Observer pattern for collision detection
	* Create Collision System (Observer)
	* Register Player and Obstacles as Subjects
	* Handle collision notifications and update game state
6. Add collision detection for player-obstacle and player-collectible interactions
7. Implement collectible item spawning and collection logic
8. Develop scoring system (time-based and collectible-based)
9. Create high score tracking system
10. Implement graphics rendering for game elements
11. Add user input handling for player movement

## Additional Considerations

* Error handling and debugging
* Performance optimization
* Audio implementation (optional)
* Testing and iteration
