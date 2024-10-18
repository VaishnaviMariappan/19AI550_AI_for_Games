# Ex.No: 5  Implementation of Jumping behavior 
### DATE: 23-08-24                                                                      
### REGISTER NUMBER : 212221240058
### AIM: 
To write a python program to simulate Jumbing behavior. 
### Algorithm:
1. Start the program
2. Import the necessary modules
3. Initiate the pygame engine and window
4. Specify the necessary parameter for player height,depth,gravity,jump power. 
5. Create a game loop to simulate the continuous behavior.
6. If Quit button is pressed then quit the pygame window.
7. Move the player left when left button is pressed
8. Move the player right when right button is pressed
9. If space bar is pressed then enable the jump by increasing y axis value.
10. land the player and display the player at every timestep
11.  Stop the program
 ### Program:
 ```
import pygame
pygame.init()


width, height = 800, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Simple Jumping with Image")


black = (255, 255, 255)


sprite_image_filename = "C:/Users/vaishnavi/Documents/AiGames/onion.png"
sprite = pygame.image.load(sprite_image_filename)
sprite_width, sprite_height = sprite.get_size()


player_x = 100
player_y = height - sprite_height
player_velocity = 5
jump_power = -15
gravity = 1
is_jumping = False
vertical_speed = 0
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_x -= player_velocity
    if keys[pygame.K_RIGHT]:
        player_x += player_velocity
    if not is_jumping:
        if keys[pygame.K_SPACE]:
            is_jumping = True
            vertical_speed = jump_power
    if is_jumping:
        player_y += vertical_speed
        vertical_speed += gravity
        if player_y >= height - sprite_height:
            player_y = height - sprite_height
            is_jumping = False
    screen.fill(black)
    screen.blit(sprite, (player_x, player_y))
    pygame.display.flip()
    pygame.time.delay(30)

pygame.quit()

```

### Output:


![image](https://github.com/user-attachments/assets/61eddf2f-ee3c-4cef-b8cf-6bb2b3db31a9)

### Result:
Thus the simple jumping behavior  was implemented.
