import pygame
import sys

# Inicializar o Pygame
pygame.init()

# Definir as cores
PRETO = (0, 0, 0)
BRANCO = (255, 255, 255)

# Configurações da janela
largura = 800
altura = 600
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption("Pong")

# Definir variáveis do jogo
barra_largura = 10
barra_altura = 100
velocidade = 5

# Criar as barras (retângulos)
barra1 = pygame.Rect(50, altura // 2 - barra_altura // 2, barra_largura, barra_altura)
barra2 = pygame.Rect(largura - 50 - barra_largura, altura // 2 - barra_altura // 2, barra_largura, barra_altura)

# Definir a bola
bola = pygame.Rect(largura // 2, altura // 2, 10, 10)
bola_velocidade_x = velocidade
bola_velocidade_y = velocidade

# Loop principal do jogo
while True:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Movimentação das barras
    teclas = pygame.key.get_pressed()
    if teclas[pygame.K_w] and barra1.y > 0:
        barra1.y -= velocidade
    if teclas[pygame.K_s] and barra1.y < altura - barra_altura:
        barra1.y += velocidade
    if teclas[pygame.K_UP] and barra2.y > 0:
        barra2.y -= velocidade
    if teclas[pygame.K_DOWN] and barra2.y < altura - barra_altura:
        barra2.y += velocidade

    # Movimentação da bola
    bola.x += bola_velocidade_x
    bola.y += bola_velocidade_y

    # Colisão com as bordas da tela
    if bola.y <= 0 or bola.y >= altura - 10:
        bola_velocidade_y = -bola_velocidade_y
    if bola.x <= 0 or bola.x >= largura - 10:
        bola_velocidade_x = -bola_velocidade_x

    # Colisão com as barras
    if bola.colliderect(barra1) or bola.colliderect(barra2):
        bola_velocidade_x = -bola_velocidade_x

    # Limpar a tela
    tela.fill(PRETO)

    # Desenhar as barras e a bola
    pygame.draw.rect(tela, BRANCO, barra1)
    pygame.draw.rect(tela, BRANCO, barra2)
    pygame.draw.ellipse(tela, BRANCO, bola)

    # Atualizar a tela
    pygame.display.update()
