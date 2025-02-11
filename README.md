# LuMoPlay-2025
https://replit.com/@luismonsalved/LuMoPlay-2025?v=1
print("Hi people")
# Import der Pygame-Bibliothek
import pygame

# Import der random-Bibliothek für Zufall
import random

# Initialisierung von Pygame
pygame.init()

# Initialisierung des Fensters
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode([screen_width, screen_height])

# Definition der Variablen für die Spielschleife
running = True
game_active = True

# Laden des Hintergrundbildes
background_image = pygame.image.load('background.png')

# Laden des Spielfigurbildes
player_image = pygame.image.load('player.png')
player_image = pygame.transform.scale_by(player_image, 0.5)

# Laden des Apfelbildes
apple_image = pygame.image.load('apple.png')
apple_image = pygame.transform.scale_by(apple_image, 0.15)

# Erstellen eines Rechtecks für das Bild der Spielfigur
player_rect = player_image.get_rect()

# Definition der Variablen für die x- und y-Koordinate der Spielfigur
player_x = screen_width / 2 - player_image.get_width() / 2
player_y = screen_height - player_image.get_height()

# Apfelrechtecke definieren
apples = []
for i in range(5):
    apple_rect = apple_image.get_rect()
    apple_x = random.randint(0, screen_width - apple_image.get_width())
    apple_y = random.randint(-500, -100)
    apple_rect.topleft = (apple_x, apple_y)
    apples.append(apple_rect)

score = 0
font = pygame.font.SysFont(None, 40)

# Spielschleife
while running and game_active:
    # Schleife für die Spielmechanik/-logik
    while game_active:
        # Anzeigen des Hintergrundbildes
        screen.blit(background_image, (0, 0))

        # Anzeigen der Spielfigur
        screen.blit(player_image, (player_x, player_y))
        player_rect.topleft = (player_x, player_y)

        # Über alle Apfelrechtecke iterieren
        for apple_rect in apples:
            # Anzeigen eines Apfels für jedes Rechteck
            screen.blit(apple_image, apple_rect)

            # Den Apfel abwärts bewegen
            apple_rect.y += 1

            # Auf Kollision prüfen
            collision = player_rect.colliderect(apple_rect)

            # Wenn sie miteinander kollidieren, sollen die x- und y-Koordinaten jedes Apfels neu gesetzt werden
            if collision:
                score += 1
                apple_rect.x = random.randint(0, screen_width - apple_image.get_width())
                apple_rect.y = random.randint(-500, -100)

            if apple_rect.y > screen_height:
                game_active = False

        for event in pygame.event.get():
            # Überprüfen, ob das Kreuz geklickt wurde
            if event.type == pygame.QUIT:
                running = False
                game_active = False

        # Alle möglichen gedrückten Tasten bekommen
        keys = pygame.key.get_pressed()

        # Prüfen, ob die linke Pfeiltaste gedrückt wird und die Spielfigur innerhalb des Spielfelds ist
        if keys[pygame.K_LEFT] and player_x > 0:
            # Die Spielfigur nach links bewegen
            player_x -= 1

        # Prüfen, ob die rechte Pfeiltaste gedrückt wird und die Spielfigur innerhalb des Spielfelds ist
        if keys[pygame.K_RIGHT] and player_x < screen_width - player_image.get_width():
            # Die Spielfigur nach rechts bewegen
            player_x += 1

        # Score anzeigen
        score_text = font.render(f"Score: {score}", True, "white")
        screen.blit(score_text, (10, 10))

        # Die gesamte Spieloberfläche aktualisieren
        pygame.display.flip()

    # Schleife für das Spielende
    while running and not game_active:
        # Text für Game Over
        game_over_text = font.render("Game Over", True, "red")

        # Die Mitte des Bildschirms berechnen
        game_over_text_mid_x = screen_width / 2 - game_over_text.get_width() / 2
        game_over_text_mid_y = screen_height / 2 - game_over_text.get_height() / 2

        # Game Over mittig anzeigen
        screen.blit(game_over_text, (game_over_text_mid_x, game_over_text_mid_y))

        # Abfragen ob das X gedrückt wurde und sonst die gesamte Spieloberfläche aktualisieren
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        # Die gesamte Spieloberfläche aktualisieren
        pygame.display.flip()
