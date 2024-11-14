# Ex.No: 11  Mini Project 
#### DATE: 08/10/2024                                                                            
#### REGISTER NUMBER : 212221240058
### AIM: 
The aim of the game is to shoot and pop balloons for points, preventing them from reaching the bottom. The objective is to achieve the highest possible score.

### ALGORITHM:


Here’s an algorithm outlining the steps for this "Balloon Shooter" game implemented using Tkinter:

1. **Initialize Game Settings and UI Elements:**
   - Set up the game window dimensions and properties (width, height, and title).
   - Create a canvas to represent the game background and set the sky-blue background color.
   - Define game settings such as balloon colors, sizes, speeds, cannon dimensions, bullet properties, and the initial score.

2. **Create and Position the Cannon:**
   - Calculate the initial position of the cannon at the bottom center of the canvas.
   - Draw the cannon as a rectangle and store its canvas ID.

3. **Define Game Functions:**
   
   - **Create Balloons (`create_balloon`):**
     - Randomly choose a horizontal starting position and color for each balloon.
     - Randomly assign each balloon a horizontal movement direction.
     - Draw the balloon at the top of the canvas.
     - Schedule balloon creation at regular intervals (2 seconds).

   - **Move Balloons (`move_balloons`):**
     - Move each balloon down by a set step size.
     - Adjust horizontal movement according to the balloon's direction.
     - Check for boundary conditions to reverse the balloon's direction when it hits the screen edges.
     - If a balloon reaches the bottom of the screen, trigger the game over condition.

   - **Shoot Bullet (`shoot_bullet`):**
     - When the space bar is pressed, create a bullet at the current cannon position.
     - Add the bullet to the bullets list for movement and collision checks.

   - **Move Bullets (`move_bullets`):**
     - Move each bullet upward at a fixed speed.
     - Delete bullets that go off-screen.
     - For each bullet, check for collisions with balloons:
       - If a collision is detected, remove the bullet and balloon, update the score, and display the updated score.

   - **Collision Detection (`check_collision`):**
     - Check if a bullet's position overlaps with a balloon's position, indicating a collision.

   - **Move Cannon Left and Right (`move_left`, `move_right`):**
     - Move the cannon left or right based on arrow key presses, ensuring it stays within canvas boundaries.

   - **Game Over (`game_over`):**
     - Stop balloon and bullet movement by disabling game activity.
     - Display a message box with the final score and close the game window.

4. **Event Bindings and Game Loop:**
   - Bind the space bar to the `shoot_bullet` function.
   - Bind left and right arrow keys to move the cannon.
   - Start a loop to create and move balloons and bullets at set intervals to animate the game elements.

5. **Run the Game:**
   - Start the Tkinter main loop to display the game window and handle user interactions continuously.

This algorithm structures the game mechanics, animation, and player interactions, allowing for smooth gameplay.

### PROGRAM:

```python
from tkinter import Canvas, Tk, messagebox, font
from random import randrange, choice

# Set up game window
canvas_width = 800
canvas_height = 400

root = Tk()
root.title("Balloon Shooter")


c = Canvas(root, width=canvas_width, height=canvas_height, background="sky blue")
c.pack()

# Game settings
balloon_colors = ['red', 'blue', 'green', 'yellow', 'purple']
balloon_width = 50
balloon_height = 65
balloon_speed = 250  # Delay between each movement step of the balloons
balloon_step = 5     # Movement step size for falling balloons
horizontal_step = 3  # Step size for horizontal movement
cannon_width = 100
cannon_height = 30
bullet_radius = 5
cannon_color = "gray"
bullet_color = "black"
score = 0
game_active = True  # Flag to track the game's active state

# Cannon position
cannon_x1 = (canvas_width // 2) - cannon_width // 2
cannon_y1 = canvas_height - cannon_height - 20
cannon_x2 = cannon_x1 + cannon_width
cannon_y2 = cannon_y1 + cannon_height

cannon = c.create_rectangle(cannon_x1, cannon_y1, cannon_x2, cannon_y2, fill=cannon_color)

# Score text
game_font = font.nametofont("TkFixedFont")
game_font.config(size=18)
score_text = c.create_text(10, 10, anchor="nw", font=game_font, fill="darkblue", text="Score: " + str(score))

balloons = []
bullets = []

# Initialize balloon direction
def create_balloon():
    """Creates a new balloon at a random horizontal position at the top."""
    if game_active:  # Only create balloons if the game is active
        x = randrange(50, canvas_width - 50)
        y = 0  # Start balloons at the top of the screen
        color = choice(balloon_colors)
        direction = choice([-1, 1])  # Randomly choose direction (-1 for left, 1 for right)
        balloon = c.create_oval(x, y, x + balloon_width, y + balloon_height, fill=color, outline=color)
        balloons.append((balloon, direction))  # Store balloon and its direction
        root.after(2000, create_balloon)  # Create new balloons every 2 seconds

def move_balloons():
    """Moves the balloons downwards and implements simple AI for horizontal movement."""
    global score
    for balloon_data in balloons[:]:  # Iterate over a copy of the list
        balloon, direction = balloon_data  # Unpack balloon and its direction
        c.move(balloon, 0, balloon_step)  # Move balloons down by step size
        (balloonx1, balloony1, balloonx2, balloony2) = c.coords(balloon)

        # Move the balloon horizontally based on its direction
        c.move(balloon, direction * horizontal_step, 0)

        # Check if balloon has moved out of bounds
        if balloonx1 < 0:  # If balloon moves out of the left boundary
            direction = 1  # Change direction to right
        if balloonx2 > canvas_width:  # If balloon moves out of the right boundary
            direction = -1  # Change direction to left

        # Update the balloon's position
        balloons[balloons.index(balloon_data)] = (balloon, direction)

        if balloony2 > canvas_height:  # If balloon falls off the screen
            game_over()
            return  # End the game
    root.after(balloon_speed, move_balloons)

def shoot_bullet(event):
    """Creates and shoots a bullet from the cannon when space bar is pressed."""
    if game_active:  # Only shoot bullets if the game is active
        (x1, y1, x2, y2) = c.coords(cannon)
        bullet_x = (x1 + x2) / 2
        bullet = c.create_oval(bullet_x - bullet_radius, y1 - 10, bullet_x + bullet_radius, y1 - 10 - bullet_radius * 2, fill=bullet_color)
        bullets.append(bullet)

def move_bullets():
    """Moves the bullets upwards and checks for collisions with balloons."""
    global score
    for bullet in bullets[:]:  # Iterate over a copy of the bullets list
        c.move(bullet, 0, -10)  # Move bullets upwards by 10 pixels
        (bx1, by1, bx2, by2) = c.coords(bullet)
        if by2 < 0:  # Bullet goes off-screen
            bullets.remove(bullet)
            c.delete(bullet)
        else:
            for balloon_data in balloons[:]:  # Iterate over a copy of the balloons list
                balloon = balloon_data[0]  # Get the balloon
                if check_collision(bullet, balloon):
                    bullets.remove(bullet)
                    c.delete(bullet)
                    balloons.remove(balloon_data)  # Remove the entire tuple (balloon, direction)
                    c.delete(balloon)
                    score += 10
                    c.itemconfigure(score_text, text="Score: " + str(score))
                    break  # Exit the loop once a bullet hits a balloon
    root.after(50, move_bullets)

def check_collision(bullet, balloon):
    """Check if a bullet hits a balloon."""
    (bx1, by1, bx2, by2) = c.coords(bullet)
    (balx1, baly1, balx2, baly2) = c.coords(balloon)
    return bx1 < balx2 and bx2 > balx1 and by1 < baly2 and by2 > baly1

def move_left(event):
    """Moves the cannon left when left arrow key is pressed."""
    (x1, y1, x2, y2) = c.coords(cannon)
    if x1 > 0:
        c.move(cannon, -20, 0)

def move_right(event):
    """Moves the cannon right when right arrow key is pressed."""
    (x1, y1, x2, y2) = c.coords(cannon)
    if x2 < canvas_width:
        c.move(cannon, 20, 0)

def game_over():
    """Handles game over scenario."""
    global game_active
    game_active = False  # Set the game active flag to False
    messagebox.showinfo("Game Over", f"Your final score is: {score}")
    root.quit()  # Close the game window

# Bind the space bar to shooting bullets
root.bind("<space>", shoot_bullet)
# Bind the left and right arrow keys to cannon movement
root.bind("<Left>", move_left)
root.bind("<Right>", move_right)

# Game loop
root.after(1000, create_balloon)
root.after(1000, move_balloons)
root.after(1000, move_bullets)

root.mainloop()
#final
```



### OUTPUT:
![WhatsApp Image 2024-11-14 at 19 54 59_631e36de](https://github.com/user-attachments/assets/57350c85-fda2-4ae5-9f1e-b5dc72d4a922)




### RESULT:
Thus, a game to shoot and pop balloons for points using AI is created.
