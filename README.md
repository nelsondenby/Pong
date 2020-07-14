# Pong
#A shitty game of pong!

import pygame
import random
pygame.init ()

screenwidth = 1500
screenheight = 800
#background = (0,0,screenwidth,screenheight)
win = pygame.display.set_mode ((screenwidth, screenheight))

pygame.display.set_caption ('PONG')

clock = pygame.time.Clock ()

class player(object):
    def __init__ (self, x, y, width, height, colour):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.vel = 15
        self.rectangle = (self.x,self.y,self.width,self.height)
        self.colour = colour


    def draw (self, win):
        pygame.draw.rect (win, self.colour, self.rectangle)

    def move (self, direc):
        if direc == 'up':
            self.y -= self.vel
        elif direc == 'down':
            self.y += self.vel
        self.rectangle = (self.x,self.y,self.width,self.height)

class ball(object):
    def __init__ (self, horfacing):
        self.x = (screenwidth//2)
        self.y = (screenheight//2)
        self.radius = 10
        self.colour = (0,0,0)
        self.vel = 15
        self.horfacing = horfacing
        self.yfact = 3

    def draw (self,win):
        pygame.draw.circle (win, self.colour, (self.x,self.y), self.radius)

    def move (self, start):
        global gamestart
        if start:
            if self.horfacing == 'right':
                self.x += self.vel
            if self.x >= p2.x and self.y in range(p2.y, (p2.y + p2.height)):
                self.horfacing = 'left'
                self.yfact = random.randint(-5,5)

                
            if self.horfacing == 'left':
                self.x -= self.vel
            if self.x <= (p1.x + p1.width + self.radius) and self.y in range(p1.y, (p1.y + p1.height)):
                self.horfacing = 'right'
                self.yfact = random.randint(-5,5)

            if self.y <= 0 or self.y >= screenheight:
                self.yfact = (self.yfact * -1)
                print ('hit')

            if self.x <= 0 :
                gamestart = False
                self.x = (screenwidth//2)
                self.y = (screenheight//2)
                win.blit(text1, ((750,400),(75,75)))
                
                    
                
            if self.x >= screenwidth :
                gamestart = False
                self.x = (screenwidth//2)
                self.y = (screenheight//2)
                win.blit (text2, (750,400))
                
                    
                
                
            self.y += self.yfact
        
            
                

def drawgamewindow ():
    win.fill ((255,255,255))
    p1.draw (win)
    p2.draw (win)
    b1.draw (win)
    

font = pygame.font.SysFont ('arial', 30)
text1 = font.render ('Player 2 Scored!',1, (255,0,0))
text2 = font.render ('Player 1 Scored!',1, (255,0,0))
p1 = player (x = 20,y = 10,width = 20,height = 100, colour = (255,0,0))
p2 = player (x = (screenwidth - 40), y = 10, width = 20, height = 100, colour = (0,255,0))
b1 = ball ('right')
gamestart = False
run = True

#main loop
while run:
    clock.tick (64)
        
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            run = False

    keys = pygame.key.get_pressed ()

    if keys[pygame.K_w] and p1.y > 0:
        p1.move('up')
    
    if keys[pygame.K_s] and (p1.y + p1.height) < screenheight:
        p1.move('down')

    if keys[pygame.K_UP] and p2.y > 0:
        p2.move('up')
    
    if keys[pygame.K_DOWN] and (p2.y + p2.height) < screenheight:
        p2.move('down')

    if keys[pygame.K_SPACE]:
        if not gamestart:
            gamestart = True
        else:
            gamestart = False
        
    b1.move (gamestart)
    drawgamewindow ()        
    pygame.display.flip ()

pygame.quit ()
