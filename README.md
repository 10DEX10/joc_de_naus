# joc_de_naus
python



En aquest repositori penjaré els fitxers d'un joc de nauys fet en Python amb la llibreria Pygame.

Els gràfics son a la carpeta **imatges**



import time
from pygame.locals import *
import pygame, random

AMPLADA = 800
ALTURA = 600
BACKGROUND_IMAGE = 'assets/fons.png'
MAROON = (153,76,0)
MAGENTA = (255,0,255)
BLACK = (0,0,0)
GRIS = (80,80,80)
RED = (255,0,0)
BLUE = (0,0,255)
WHITE = (255,255,255)

vala1 = pygame.image.load('assets/vala1.png')
vala2 = pygame.image.load('assets/vala2.png')
boom = pygame.image.load('assets/boom.png')
vala = 'assets/vala.mp3'
guanyador = 0
pantalla_actual = 1

# Jugador 1:
player_image = pygame.image.load('assets/nave1.png')
player_rect = player_image.get_rect(midbottom=(AMPLADA // 2, ALTURA - 10))
velocitat_nau = 9

# Jugador 2
player_image2 = pygame.image.load('assets/nave2.png')
player_rect2 = player_image2.get_rect(midbottom=(AMPLADA // 2, ALTURA - 500))
velocitat_nau2 = 9


# vides:
vides_jugador1 = 3
vides_jugador2 = 3
vides_jugador1_image = pygame.image.load('assets/corazon1.png')
vides_jugador2_image = pygame.image.load('assets/corazon2.png')


# Bala rectangular blanca:
bala_imatge = pygame.Surface((13,24)) #definim una superficie rectangle de 4 pixels d'ample i 10 d'alçada
bala_imatge.fill(WHITE) #pintem la superficie de color blanc
bales_jugador1 = [] #llista on guardem les bales del jugador 1
bales_jugador2 = [] #llista on guardem les bales del jugador 2
velocitat_bales = 10
temps_entre_bales = 400 #1 segon
temps_ultima_bala_jugador1 = 0 #per contar el temps que ha passat des de que ha disparat el jugador 1
temps_ultima_bala_jugador2 = 0 #per contar el temps que ha passat des de que ha disparat el jugador 2


pygame.init()
pygame.mixer.music.load('assets/music1.mp3')
pygame.mixer.music.play()
pantalla = pygame.display.set_mode((AMPLADA, ALTURA))
pygame.display.set_caption("assets") # Arcade

# Control de FPS
clock = pygame.time.Clock()
fps = 40

def imprimir_pantalla_fons(image):
    # Imprimeixo imatge de fons:
    background = pygame.image.load(image).convert()
    pantalla.blit(background, (0, 0))

while True:
    #contador
    current_time = pygame.time.get_ticks()




    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()

        if pantalla_actual == 1:
            if event.type == KEYDOWN:
                if event.key == K_3:
                    pygame.quit()
                if event.key == K_1:
                    pantalla_actual = 3
                if event.key == K_2:
                    pantalla_actual = 2

        if pantalla_actual == 2:
            imprimir_pantalla_fons(BACKGROUND_IMAGE)
            text0 = "Friend or Enemy?"
            text1 = "Programació:"
            text2 = "Gràfics:"
            text3 = "Música:"
            text4 = "Sons:"
            text5 = "Xavi i mohammed"
            text6 = "Yuri O"
            text7 = "Freesound.org"
            text8 = "mohammed"
            font0 = pygame.font.SysFont(None, 100)
            font1 = pygame.font.SysFont(None, 60)
            font2 = pygame.font.SysFont(None, 50)
            img0 = font0.render(text0, True, BLACK)
            img1 = font1.render(text1, True, GRIS)
            img2 = font1.render(text2, True, GRIS)
            img3 = font1.render(text3, True, GRIS)
            img4 = font1.render(text4, True, GRIS)
            img5 = font2.render(text5, True, BLUE)
            img6 = font2.render(text6, True, BLUE)
            img7 = font2.render(text7, True, BLUE)
            img8 = font2.render(text8, True, BLUE)
            pantalla.blit(img0, (60, 60))
            pantalla.blit(img1, (100, 170))
            pantalla.blit(img5, (200, 220))
            pantalla.blit(img2, (100, 270))
            pantalla.blit(img8, (200, 320))
            pantalla.blit(img3, (100, 370))
            pantalla.blit(img6, (200, 420))
            pantalla.blit(img4, (100, 470))
            pantalla.blit(img7, (200, 520))
            if event.type == KEYDOWN:
                if event.key == K_SPACE:
                    pantalla_actual = 1

        if pantalla_actual == 4 :
            for i in bales_jugador1:
                bales_jugador1.remove(i)
            for i in bales_jugador2:
                bales_jugador2.remove(i)
            if event.type == KEYDOWN:
                if event.key == K_SPACE:
                    pantalla_actual = 1
                    vides_jugador1 = 3
                    vides_jugador2 = 3




        if pantalla_actual == 3:

            # controlar trets de les naus
            if event.type == KEYDOWN:
                #jugador 1

                if event.key == K_UP and current_time - temps_ultima_bala_jugador1 >= temps_entre_bales:
                    bales_jugador1.append(pygame.Rect(player_rect.centerx - 2, player_rect.top, 4, 10))
                    temps_ultima_bala_jugador1 = current_time
                    pygame.mixer.music.load(vala)
                    pygame.mixer.music.play()
                # jugador 2
                if event.key == K_w and current_time - temps_ultima_bala_jugador2 >= temps_entre_bales:
                    bales_jugador2.append(pygame.Rect(player_rect2.centerx - 2, player_rect2.bottom -10, 4, 10))
                    temps_ultima_bala_jugador2 = current_time
                    pygame.mixer.music.load(vala)
                    pygame.mixer.music.play()




    if pantalla_actual == 4:
        imprimir_pantalla_fons('assets/fons.png')
        text = "Player " + str(guanyador) + " Wins!"
        font = pygame.font.SysFont(None, 100)
        img = font.render(text,True, RED)
        pantalla.blit(img,(175,250))


    if pantalla_actual == 1:
        imprimir_pantalla_fons("assets/fons.png")
        text0 = "Friend or Enemy?"
        text1 = "1. Partida nova"
        text2 = "2. Crèdits"
        text3 = "3. Sortir"
        font0 = pygame.font.SysFont(None, 100)
        font1 = pygame.font.SysFont(None, 50)
        img0 = font0.render(text0, True, BLACK)
        img1 = font1.render(text1, True, MAROON)
        img2 = font1.render(text2, True, MAGENTA)
        img3 = font1.render(text3, True, RED)
        pantalla.blit(img0, (60, 60))
        pantalla.blit(img1, (100, 200))
        pantalla.blit(img2, (100, 300))
        pantalla.blit(img3, (100, 400))


    if pantalla_actual == 3:
        # Moviment del jugador 1
        keys = pygame.key.get_pressed()
        if keys[K_LEFT]:
            player_rect.x -= velocitat_nau
        if keys[K_RIGHT]:
            player_rect.x += velocitat_nau

        # Moviment del jugador 2
        if keys[K_a]:
            player_rect2.x -= velocitat_nau2
        if keys[K_d]:
            player_rect2.x += velocitat_nau2

        # Mantenir al jugador dins de la pantalla:
        player_rect.clamp_ip(pantalla.get_rect())
        player_rect2.clamp_ip(pantalla.get_rect())


        #dibuixar el fons:
        imprimir_pantalla_fons(BACKGROUND_IMAGE)

        #Actualitzar i dibuixar les bales del jugador 1:
        for bala in bales_jugador1: # bucle que recorre totes les bales
            bala.y -= velocitat_bales # mou la bala
            if bala.bottom < 0 or bala.top > ALTURA: # comprova que no ha sortit de la pantalla
                bales_jugador1.remove(bala) # si ha sortit elimina la bala
            else:
                pantalla.blit(vala1, bala) # si no ha sortit la dibuixa
            # Detectar col·lisions jugador 2:
            if player_rect2.colliderect(bala):  # si una bala toca al jugador1 (el seu rectangle)
                print("1")
                bales_jugador1.remove(bala)  # eliminemd la bala
                pantalla.blit(boom, player_rect2)
                vides_jugador2 = vides_jugador2 - 1
                # mostrem una explosió
                # eliminem el jugador 1 (un temps)
                # anotem punts al jugador 1


        # Actualitzar i dibuixar les bales del jugador 2:
        for bala in bales_jugador2:
            bala.y += velocitat_bales
            if bala.bottom < 0 or bala.top > ALTURA:
                bales_jugador2.remove(bala)
            else:
                pantalla.blit(vala2, bala)
            # Detectar col·lisions jugador 1:
            if player_rect.colliderect(bala):  # si una bala toca al jugador1 (el seu rectangle)
                print("2")
                bales_jugador2.remove(bala)  # eliminem la bala
                pantalla.blit(boom, player_rect)
                vides_jugador1 = vides_jugador1 - 1
                # mostrem una explosió
                # eliminem el jugador 1 (un temps)
                # anotem punts al jugador 1



        # dibuixar vides
        if vides_jugador1 >= 3:
            pantalla.blit(vides_jugador1_image, (700, 550))
        if vides_jugador1 >= 2:
            pantalla.blit(vides_jugador1_image, (720, 550))
        if vides_jugador1 >= 1:
            pantalla.blit(vides_jugador1_image, (740, 550))
        if vides_jugador1 <= 0:
            pantalla_actual = 4
            # vides_jugador2 = 0
        # dibuixar vides
        if vides_jugador2 >= 3:
            pantalla.blit(vides_jugador2_image, (100, 50))
        if vides_jugador2 >= 2:
            pantalla.blit(vides_jugador2_image, (80, 50))
        if vides_jugador2 >= 1:
            pantalla.blit(vides_jugador2_image, (60, 50))


        if vides_jugador2 <=0 or vides_jugador1 <=0:
            guanyador = 1
            if vides_jugador1 <= 0:
                guanyador = 2
            pantalla_actual = 4
            # vides_jugador1 = 0

    if pantalla_actual == 3:
    #dibuixar els jugadors:
        pantalla.blit(player_image2, player_rect2)
        pantalla.blit(player_image, player_rect)
    pygame.display.update()
    clock.tick(fps)
