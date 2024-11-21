import pygame
import sys

# Initialize Pygame
pygame.init()

# Screen dimensions and setup
WIDTH, HEIGHT = 800, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Flying and Walking Bird")

# Colors
WHITE = (255, 255, 255)
BLUE = (135, 206, 250)
GROUND_COLOR = (100, 50, 0)

# Clock
clock = pygame.time.Clock()

# Bird attributes
bird = pygame.Rect(100, 300, 50, 50)  # x, y, width, height
bird_color = (255, 255, 0)
bird_velocity = 0
gravity = 0.5
is_flying = False

# Ground level
GROUND = 350

# Main game loop
def main():
    global bird_velocity, is_flying

    while True:
        screen.fill(BLUE)
        
        # Draw ground
        pygame.draw.rect(screen, GROUND_COLOR, (0, GROUND, WIDTH, HEIGHT - GROUND))
        
        # Event handling
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_RETURN:  # Fly
                    bird_velocity = -10
                    is_flying = True
                elif event.key == pygame.K_SPACE:  # Walk
                    is_flying = False
        
        # Update bird position
        if is_flying:
            bird.y += bird_velocity
            bird_velocity += gravity
        else:
            bird.y = GROUND - bird.height  # Walk on the ground
        
        # Prevent the bird from falling below the ground
        if bird.y > GROUND - bird.height:
            bird.y = GROUND - bird.height
            bird_velocity = 0
        
        # Draw bird
        pygame.draw.rect(screen, bird_color, bird)
        
        # Update display
        pygame.display.flip()
        
        # Frame rate
        clock.tick(30)

if __name__ == "__main__":
    main()
