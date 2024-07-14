import pygame
import sys
import random
import math

# Inicializar pygame
pygame.init()

# Configuración de la pantalla
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("¿Me perdonas amor?")

# Colores
WHITE = (255, 255, 255)
PURPLE = (200, 150, 200)

# Fuentes
font = pygame.font.Font(None, 36)
big_font = pygame.font.Font(None, 48)

# Botones
class Button:
    def __init__(self, text, position):
        self.text = text
        self.position = position
        self.clicked = False

    def draw(self, screen, big=False):
        button_font = big_font if big else font
        text_surface = button_font.render(self.text, True, PURPLE)
        text_rect = text_surface.get_rect(center=self.position)
        screen.blit(text_surface, text_rect)

    def is_clicked(self, pos):
        return self.position[0] - 100 < pos[0] < self.position[0] + 100 and \
               self.position[1] - 25 < pos[1] < self.position[1] + 25

# Función principal
def main():
    yes_button = Button("Sí, te perdono amorcito", (SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2 - 50))
    no_button = Button("No", (SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2 + 50))
    show_message = False

    while True:
        screen.fill(WHITE)

        # Dibujar botones
        yes_button.draw(screen)
        no_button.draw(screen)

        # Manejar eventos
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            elif event.type == pygame.MOUSEBUTTONDOWN:
                pos = pygame.mouse.get_pos()
                if yes_button.is_clicked(pos):
                    # Mostrar mensaje y corazones alrededor
                    show_message = True
                elif no_button.is_clicked(pos):
                    yes_button.position = (yes_button.position[0], yes_button.position[1] + 10)
                    no_button.position = (no_button.position[0], no_button.position[1] - 10)

        if show_message:
            # Mostrar el mensaje "Gracias amorcito" en el centro
            screen.fill(WHITE)  # Limpiar la pantalla
            message = "Gracias amorcito"
            message_surface = big_font.render(message, True, PURPLE)
            message_rect = message_surface.get_rect(center=(SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2))
            screen.blit(message_surface, message_rect)

            # Dibujar corazones alrededor del mensaje
            draw_hearts_around_message(screen, (SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2))

        pygame.display.flip()

# Función para dibujar corazones alrededor del mensaje
def draw_hearts_around_message(screen, position):
    for _ in range(30):  # Mostrar solo 30 corazones
        heart = pygame.image.load("heart.png")  # Asegúrate de tener un archivo de imagen llamado "heart.png"
        heart = pygame.transform.scale(heart, (30, 30))  # Ajusta el tamaño de los corazones
        angle = random.uniform(0, 2 * math.pi)  # Ángulo aleatorio para la posición de cada corazón
        offset_x = int(150 * random.uniform(0.5, 1.0) * math.cos(angle))  # Desplazamiento en X
        offset_y = int(150 * random.uniform(0.5, 1.0) * math.sin(angle))  # Desplazamiento en Y
        heart_rect = heart.get_rect(center=(position[0] + offset_x, position[1] + offset_y))
        screen.blit(heart, heart_rect)
        pygame.display.update()
        pygame.time.delay(100)  # Esperar 0.1 segundos entre cada corazón


if __name__ == "__main__":
    main()

