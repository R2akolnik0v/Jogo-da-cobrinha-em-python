# Jogo-da-cobrinha-

import pygame
import random

# Inicialização do Pygame
pygame.init()

# Configurações da janela
largura, altura = 600, 400
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption("Jogo da Cobrinha")

# Cores
branco = (255, 255, 255)
verde = (0, 255, 0)
vermelho = (255, 0, 0)
preto = (0, 0, 0)

# Configurações do jogo
tamanho_bloco = 10
velocidade = 15
clock = pygame.time.Clock()

def gerar_comida():
    x = random.randint(0, (largura // tamanho_bloco) - 1) * tamanho_bloco
    y = random.randint(0, (altura // tamanho_bloco) - 1) * tamanho_bloco
    return [x, y]

def desenhar_cobra(lista_corpo):
    for bloco in lista_corpo:
        pygame.draw.rect(tela, verde, [bloco[0], bloco[1], tamanho_bloco, tamanho_bloco])

def jogo():
    fim = False
    x, y = largura // 2, altura // 2
    dx, dy = 0, 0
    corpo = []
    comprimento = 1
    comida = gerar_comida()

    while not fim:
        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                fim = True
            elif evento.type == pygame.KEYDOWN:
                if evento.key == pygame.K_LEFT and dx == 0:
                    dx, dy = -tamanho_bloco, 0
                elif evento.key == pygame.K_RIGHT and dx == 0:
                    dx, dy = tamanho_bloco, 0
                elif evento.key == pygame.K_UP and dy == 0:
                    dx, dy = 0, -tamanho_bloco
                elif evento.key == pygame.K_DOWN and dy == 0:
                    dx, dy = 0, tamanho_bloco

        x += dx
        y += dy

        # Verificar colisão com bordas
        if x < 0 or x >= largura or y < 0 or y >= altura:
            fim = True

        corpo.append([x, y])
        if len(corpo) > comprimento:
            del corpo[0]

        # Verificar colisão com o próprio corpo
        for segmento in corpo[:-1]:
            if segmento == [x, y]:
                fim = True

        # Verificar se comeu a comida
        if x == comida[0] and y == comida[1]:
            comida = gerar_comida()
            comprimento += 1

        # Desenhar tela
        tela.fill(preto)
        pygame.draw.rect(tela, vermelho, [comida[0], comida[1], tamanho_bloco, tamanho_bloco])
        desenhar_cobra(corpo)
        pygame.display.update()
        clock.tick(velocidade)

    pygame.quit()

jogo()
