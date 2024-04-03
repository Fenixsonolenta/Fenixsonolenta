import pygame
import sys

# Inicializando o Pygame
pygame.init()

# Definindo as constantes
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
GROUND_HEIGHT = 50
WHITE = (255, 255, 255)
BLUE = (0, 0, 255)

# Criando a tela do jogo
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Simple Mario")

# Definindo a classe Player
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(BLUE)
        self.rect = self.image.get_rect()
        self.rect.center = (SCREEN_WIDTH // 2, SCREEN_HEIGHT - GROUND_HEIGHT - 25)
        self.vel_y = 0

    def update(self):
        # Aplicando gravidade
        self.vel_y += 1
        self.rect.y += self.vel_y
        if self.rect.bottom > SCREEN_HEIGHT - GROUND_HEIGHT:
            self.rect.bottom = SCREEN_HEIGHT - GROUND_HEIGHT
            self.vel_y = 0

    def jump(self):
        self.vel_y = -15

# Criando o grupo de sprites e adicionando o player
all_sprites = pygame.sprite.Group()
player = Player()
all_sprites.add(player)

# Loop principal do jogo
running = True
while running:
    # Verificando eventos
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                player.jump()

    # Atualizando
    all_sprites.update()

    # Desenhando
    screen.fill(WHITE)
    all_sprites.draw(screen)
    pygame.display.flip()

    # Limitando a taxa de quadros
    pygame.time.Clock().tick(30)

# Saindo do Pygame
pygame.quit()
sys.exit()
