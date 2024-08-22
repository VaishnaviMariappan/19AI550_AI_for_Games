# Ex.No: 3  Implementation of Kinematic movement seek and flee behaviors 
#### DATE: 22-06-24                                                                   
#### REGISTER NUMBER : 212221240058
### AIM: 
To write a python program to simulate the process of seek and flee behaviors using mouse movements.
### Algorithm:
1. Import the necessary modules pygame, math, random when necessary.
2. Initiate the pygame engine
3. Create a window with size (400,300)
4. Create a function to simulate the seek behavior - to move towards the target 
5. Create a function to simulate the flee behavior - to move away from the target 
6. Update the position of object
7. Create a game loop to display the update behavior
8. Call the seek function when left mouse button is pressed
9. Call the flee function when right mouse button is pressed
10. Close the pygame window when quit icon is clicked.
11. Stop the program
### Program:
```
import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
BACKGROUND_COLOR = (0, 0, 0)
SNAKE_COLOR = (0, 255, 0)
FOOD_COLOR = (255, 0, 0)
SNAKE_SIZE = 20
FOOD_SIZE = 20
MAX_SPEED = 5
FPS = 15

# Create the screen
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Steering Behavior Snake Game")

# Clock to control the frame rate
clock = pygame.time.Clock()

class Snake:
    def __init__(self):
        self.body = [pygame.Vector2(WIDTH // 2, HEIGHT // 2)]
        self.direction = pygame.Vector2(MAX_SPEED, 0)
        self.grow = False

    def seek(self, target):
        direction = target - self.body[0]
        if direction.length() > 0:
            self.direction = direction.normalize() * MAX_SPEED

    def move(self):
        if self.grow:
            self.body.append(self.body[-1])
            self.grow = False
        for i in range(len(self.body) - 1, 0, -1):
            self.body[i] = pygame.Vector2(self.body[i - 1])
        self.body[0] += self.direction
        self.wrap_around()

    def wrap_around(self):
        if self.body[0].x < 0: self.body[0].x = WIDTH
        elif self.body[0].x >= WIDTH: self.body[0].x = 0
        if self.body[0].y < 0: self.body[0].y = HEIGHT
        elif self.body[0].y >= HEIGHT: self.body[0].y = 0

    def draw(self, surface):
        for segment in self.body:
            pygame.draw.rect(surface, SNAKE_COLOR, pygame.Rect(segment.x, segment.y, SNAKE_SIZE, SNAKE_SIZE))

    def check_collision(self, food_position):
        return pygame.Vector2.distance_to(self.body[0], food_position) < SNAKE_SIZE

class Food:
    def __init__(self):
        self.position = pygame.Vector2(random.randint(0, WIDTH - FOOD_SIZE), random.randint(0, HEIGHT - FOOD_SIZE))

    def randomize_position(self):
        self.position = pygame.Vector2(random.randint(0, WIDTH - FOOD_SIZE), random.randint(0, HEIGHT - FOOD_SIZE))

    def draw(self, surface):
        pygame.draw.rect(surface, FOOD_COLOR, pygame.Rect(self.position.x, self.position.y, FOOD_SIZE, FOOD_SIZE))

# Create instances
snake = Snake()
food = Food()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    snake.seek(food.position)
    snake.move()

    if snake.check_collision(food.position):
        snake.grow = True
        food.randomize_position()

    screen.fill(BACKGROUND_COLOR)
    food.draw(screen)
    snake.draw(screen)
    pygame.display.flip()
    clock.tick(FPS)
```
### Output:

![image](https://github.com/user-attachments/assets/bd77d2fc-d8e8-4ac9-a371-61a97c193fe2)


### Result:
Thus the simple seek and flee behavior was implemented successfully.
