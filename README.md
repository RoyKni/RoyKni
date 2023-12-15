import pygame as pg
import random
pg.init()
# Window game
screen = pg.display.set_mode((700, 700))
pg.display.set_caption("Snake Game")
# Var
score = highscore = 0
snake_part = 20
x = y = 200
x_change = y_change = 0
body_snake = []
length = 1
# Create food
food_x = random.randint(0, 19) * snake_part
food_y = random.randint(0, 19) * snake_part
# Snake speed
clock = pg.time.Clock()
speed = 3
# Def function


def check_col():
    if x < 0 or x > 700 or y < 0 or y > 700 or (x, y) in body_snake[:-1]:
        return False
    return True


def score_view():
    font = pg.font.Font(None, 36)
    if gameplay:
        score_txt = font.render(f"Score: {score}", True, (255, 255, 255))
        screen.blit(score_txt, (0, 0))
        hscore_txt = font.render(f"High Score: {highscore}", True, (255, 255, 255))
        screen.blit(hscore_txt, (170, 0))
    else:
        note_txt = font.render("PRESS SPACE TO PLAY AGAIN", True, (255, 255, 255))
        screen.blit(note_txt, (165, 350))


# Game loop
gameplay = True
while True:
    for event in pg.event.get():
        # Quit
        if event.type == pg.QUIT:
            pg.quit()
            quit()
        # Snake move
        if event.type == pg.KEYDOWN:
            if event.key == pg.K_LEFT:
                x_change -= snake_part
                y_change = 0
            elif event.key == pg.K_RIGHT:
                x_change = snake_part
                y_change = 0
            elif event.key == pg.K_UP:
                x_change = 0
                y_change -= snake_part
            elif event.key == pg.K_DOWN:
                x_change = 0
                y_change = snake_part
            elif event.key == pg.K_SPACE:
                gameplay = True
    # Clear screen
    screen.fill((0, 0, 0))
    score_view()
    if gameplay:
        # Update snake position
        x += x_change
        y += y_change
        # Add snake part
        body_snake.append((x, y))
        # Remove snake part
        if len(body_snake) > length:
            del body_snake[0]
        # Check snake eat food
        if x == food_x and y == food_y:
            length += 1
            score += 1
            if score > highscore:
                highscore = score
            # Random food
            food_x = random.randint(0, 19) * snake_part
            food_y = random.randint(0, 19) * snake_part
        # Draw snake
        for x, y in body_snake:
            pg.draw.rect(screen, (0, 127, 255), (x, y, snake_part, snake_part))
        # Draw food
        pg.draw.rect(screen, (255, 0, 0), (food_x, food_y, snake_part, snake_part))
        gameplay = check_col()
        clock.tick(speed)
    else:
        # Reset game
        x = y = 200
        x_change = y_change = 0
        body_snake = []
        length = 1
        score = 0
    # Update screen
    pg.display.update()
