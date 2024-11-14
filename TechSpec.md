### 1. **GameManager**

Manages the overall game state, including game progression, score, and game-over conditions in an infinite loop format.

#### **Variables**
- `score: int` - The player’s current score based on distance traveled. (P1)
- `isGameOver: boolean` - Indicates whether the game has ended. (P0)
- `monkey: Monkey` - Instance of the monkey character. (P0)
- `vines: List<Vine>` - List of vines that the monkey can grab. (P0)
- `obstacles: List<Obstacle>` - List of obstacles in the game. (P0)
- `isPaused: boolean` - Whether the game is paused. (P2)
- `time: double` - Tracks time passed for various game mechanics such as speed progression and obstacle appearance. (P1)

#### **Methods**
- **`initializeGame()`** (P0)
  - **Behavior**: Sets up the game state, initializes score, game-over state, the monkey, vines, obstacles, and other initial game components.
  - **Details**: The game begins by resetting all variables, placing the monkey at the starting point, and preparing the first vine.

- **`startGameLoop()`** (P0)
  - **Behavior**: Begins the infinite game loop. Continuously updates the state of the game, including vine swinging, obstacles, score updates, and checks for game over.
  - **Details**:
    - Starts a `setInterval` or `requestAnimationFrame` loop to continuously update the game state. 
    - Every iteration, the game will update the monkey's position, check if it collides with any obstacles, update the score based on the monkey’s progress, and add new vines and obstacles.

- **`updateScore(int)`** (P1)
  - **Behavior**: Adds points to the current score.
  - **Details**: Points are typically added based on distance traveled or successful interactions with obstacles (like avoiding obstacles or successfully jumping).

- **`spawnObstacle()`** (P0)
  - **Behavior**: Adds new obstacles to the game at random intervals or based on difficulty progression.
  - **Details**: Obstacles spawn at varying positions and heights, and their difficulty increases as the player progresses (faster movement, more frequent obstacles).

- **`checkGameOver()`** (P0)
  - **Behavior**: Determines whether the game is over based on the `isGameOver` state.
  - **Details**: Checks if the monkey collided with any obstacles. If so, `isGameOver` is set to `true`, ending the game.

- **`resetGame()`** (P1)
  - **Behavior**: Resets the game state for replay. This includes resetting score, position, obstacles, vines, and other relevant game components.
  - **Details**: Resets all variables to their initial state and reinitializes the game loop.

- **`updateGameLoop(deltaTime: float)`** (P0)
  - **Input**: `deltaTime` - The amount of time passed since the last frame.
  - **Behavior**: This method is called every frame. It updates the state of the game based on the elapsed time:
    - Moves the monkey and vines forward.
    - Increases difficulty by speeding up the vines and obstacles.
    - Spawns new obstacles at random intervals.
    - Checks if the monkey has collided with any obstacles.
  - **Details**: It helps simulate the continuous, infinite nature of the game.

- **`togglePause()`** (P2)
  - **Behavior**: Toggles whether the game is paused or playing.
  - **Details**: Pauses or resumes the game loop.

### 2. **Obstacles**

Manages obstacles that appear randomly and interact with the monkey’s movement.

#### **Variables**
- `position: Point` - Position of the obstacle in the game world. (P0)
- `typeOfObstacle: string` - Type of obstacle (e.g., "bird", "branch", etc.). (P1)
- `isActive: boolean` - Indicates whether the obstacle is currently active or not. (P0)
- `speed: double` - The speed at which the obstacle moves toward the monkey. (P1)
- `size: double` - Size of the obstacle for collision detection. (P0)

#### **Methods**
- **`spawnObstacle()`** (P0)
  - **Behavior**: Spawns a new obstacle at a random position or predetermined spawn points. Obstacle type and speed are randomized based on game difficulty. 
  - **Details**: Randomly determines the type (e.g., bird, falling branch), speed, and position. As the game progresses, the speed and frequency of obstacles increase.

- **`checkCollisionWithMonkey(monkey: Monkey)`** (P0)
  - **Input**: `monkey: Monkey` - The instance of the monkey to check for collisions with.
  - **Behavior**: Checks if the obstacle collides with the monkey.
  - **Details**: Uses the position and size of the obstacle and the monkey to detect any overlap. If a collision is detected, the game ends or deducts lives.

- **`resetObstacle()`** (P1)
  - **Behavior**: Resets the obstacle once it has moved out of the screen or has been interacted with by the monkey.
  - **Details**: Places the obstacle back into the game world in a position for the next iteration.

  ---


### 3. **Monkey Class**

- **Variables**
    - `position: Point` - Current position of the monkey on the screen. (P0)
    - `grabHeight: double` - The height along the vine at which the monkey grabs; affects jump trajectory and distance. (P2)
    - `swingSpeed: double` - Speed of the vine’s swing when the monkey jumps; influences horizontal distance. (P2)
    - `maxJumpHeight: double` - Maximum height the monkey can reach in a jump. (P1)
    - `jumpTrajectory: Point` - Calculated trajectory for the jump, determining the parabolic path based on the monkey’s grab position and swing speed. (P1)
    - `jumpInProgress: boolean` - Indicates whether the monkey is currently in a jump. (P0)

- **Methods**

    - **`calculateJumpTrajectory(grabHeight: number, swingSpeed: number): Point`** (P1)
        - **Input**:
            - `grabHeight: number` - Height along the vine where the monkey grabs, ranging from `0` (top of the vine) to `vine.length` (bottom).
            - `swingSpeed: number` - Current speed of the vine swing at the moment the monkey jumps.
        - **Output**: `Point` - Returns a `Point` object representing the initial velocity (both x and y components) of the jump.
        - **Behavior**:
            - Calculates the horizontal (`x`) and vertical (`y`) components of the jump based on `grabHeight` and `swingSpeed`.
            - **Horizontal Speed**: Derived from the `swingSpeed` and adjusted by `Math.sin(swingAngle)` to capture forward motion.
            - **Vertical Speed**: Proportional to `grabHeight` relative to the vine's length and `maxJumpHeight`, giving a parabolic shape based on how high the monkey grabs.
            - This initial velocity (stored in `jumpTrajectory`) dictates the monkey's path until it lands.
            - **Formula**:
                - `jumpTrajectory.x = swingSpeed * Math.cos(swingAngle)`
                - `jumpTrajectory.y = (grabHeight / vine.length) * maxJumpHeight`

    - **`jump()`** (P0)
        - **Behavior**: Initiates the jump by setting `jumpInProgress` to true and calculating the initial jump trajectory using `calculateJumpTrajectory`.
        - Updates the monkey’s position over time to follow a parabolic curve based on `jumpTrajectory`.
        - Adjusts the monkey’s `position.x` and `position.y` each frame according to the trajectory:
            - `position.x += jumpTrajectory.x * timeStep`
            - `position.y += jumpTrajectory.y * timeStep - (gravity * timeStep^2 / 2)`
            - Applies gravity to the `y` component for a realistic downward curve.
        - Ends the jump when the monkey lands on the next vine or the ground.

    - **`resetJump()`** (P0)
        - **Behavior**: Resets `jumpInProgress` to false, clears `jumpTrajectory`, and resets the monkey to a neutral position on a new vine after landing.
        
- **`landOnVine(vine: Vine, grabHeight: float)`** (P0)
  - **Input**: `vine: Vine`, `grabHeight: float` - The position and the height at which the monkey grabs the vine.
  - **Behavior**: Positions the monkey on the vine at the given height. The height affects how the swing behaves.
  - **Details**:
    - `monkey.position.x = vine.position.x`
    - `monkey.position.y = vine.position.y + grabHeight`

---

### ** 4. Vine Class**

Represents a vine for swinging, with a simplified approach to handle different lengths, swing speed, and the monkey’s grab point.

- **Variables**
    - `anchorPoint: Point` - The fixed anchor point of the vine on the screen. (P0)
    - `length: int` - The length of the vine. (P0)
    - `swingSpeed: double` - Controls the speed of the swing; the time taken to swing from one end to the other. (P2)
    - `currentAngle: double` - The current angle of the vine’s swing in radians, starting from the resting position. (P1)
    - `grabHeight: double` - Position along the vine where the monkey grabs, from `0` (top) to `length` (bottom). (P0)

- **Methods**

    - **`swing(timeStep: number)`** (P1)
        - **Input**: `timeStep: number` - The time increment for each frame update.
        - **Behavior**:
            - Updates `currentAngle` based on `swingSpeed` and `timeStep` to create a simple oscillating swing motion:
                - **Formula**: `currentAngle = Math.sin(timeStep * swingSpeed) * maxSwingAngle`
            - `maxSwingAngle` is a constant defining the maximum angle the vine reaches from the resting position.

    - **`getMonkeyPosition(): Point`** (P0)
        - **Output**: `Point` - Returns the position of the monkey along the vine’s arc, based on `grabHeight` and `currentAngle`.
        - **Behavior**:
            - Calculates the `x` and `y` position of the monkey:
                - `x = anchorPoint.x + Math.sin(currentAngle) * grabHeight`
                - `y = anchorPoint.y + Math.cos(currentAngle) * grabHeight`
            - Returns a `Point` object representing the monkey's position on the vine.

    - **`resetSwing()`** (P0)
        - **Behavior**: Resets `currentAngle` to zero to start a new swing cycle when needed.

---

### 5. **Point** (P0)

Represents a 2D point in space, used for positions and directional calculations such as `jumpTrajectory` for the monkey’s jump.

- **Variables**
    - `x: double` - The horizontal (x-axis) component of the point, representing position or velocity in the x-direction. (P0)
    - `y: double` - The vertical (y-axis) component of the point, representing position or velocity in the y-direction. (P0)

---

## [Monkey Vroom Vroom Architecture on Figma](https://www.figma.com/board/O0nuwyYQQlZSh1bG26ezDd/Monkey-Vroom-Vroom-Architecture?node-id=0-1&node-type=canvas&t=C6XpV7kCmZ9VqWIF-0)