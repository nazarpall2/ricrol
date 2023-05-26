# ricrol
import pygame
class Herou:
    def __init__(self, x, y, w, h,  speedx ,speedy,fillename):
        self.rect = pygame.Rect(x, y, w, h)
        self.png = pygame.image.load(fillename)
        self.speedx = speedx
        self.speedy = speedy
    def draw(self, screen):
        screen.blit(self.png, [self.rect.x,self.rect.y])
def pause(screen):
    fps = pygame.time.Clock()
    while True:
        for event in pygame.event.get():
            if event.type == pygame.KEYUP:
                if event.key == pygame.K_p:

        
                    return
        fps.tick(1)
    
class Enemy:
    def __init__(self, x, y, w, h,startx,finishx,speed, fillename):
        self.startx = startx
        self.finishx = finishx
        self.speed = speed
        self.rect = pygame.Rect(x, y, w, h)
        self.png = pygame.image.load(fillename)
    def draw(self, screen):
        screen.blit(self.png, [self.rect.x,self.rect.y])
    def run(self):
        self.rect.x += self.speed
        if self.rect.x > self.finishx:
            self.speed *= -1
        if self.rect.x < self.startx:
            self.speed *= -1
    

    def setText(self, text):
        self.text = text
        self.font = pygame.font.Font(None, 18).render(text, True, (100, 100, 100))
class Wall:
    def __init__(self, color, x, y, w, h):
        self.rect = pygame.Rect(x, y, w, h)
        self.color = color

    def draw(self, window):
        pygame.draw.rect(window, self.color, self.rect)
    def setText(self, text):
        self.text = text
        self.font = pygame.font.Font(None, 18).render(text, True, (100, 100, 100))
enemy = []

pygame.init()
screen = pygame.display.set_mode((500, 500))
fps = pygame.time.Clock()
walls = []
walls.append(Wall((255, 0, 0), 0, 150, 200, 5))
walls.append(Wall((255, 0, 0), 120, 150, 100, 5)) 
walls.append(Wall((255, 0, 0), 200, 150, 100, 5))
walls.append(Wall((255, 0, 0), 400, 65, 5, 85))
walls.append(Wall((255, 0, 0), 300, 70, 5, 85))
walls.append(Wall((255, 0, 0), 400, 150, 100, 5))
walls.append(Wall((255, 0, 0), 400, 150, 100, 5)) 
walls.append(Wall((255, 0, 0), 125, 70, 175, 5)) 


herous = Herou(35,250,35,10,0,0,"herou (1).png")


enemy.append(Enemy(100,100,70,10,1,140,0.2,"enemy.png"))
enemy.append(Enemy(50,72,70,10,1,280,0.3,"enemy.png"))
enemy.append(Enemy(100,26,70,10,12,500,1,"enemy.png"))
enemy.append(Enemy(78,80,70,10,1,200,5,"enemy.png"))
enemy.append(Enemy(50,30,70,10,1,200,3,"enemy.png"))
enemy.append(Enemy(21,230,70,10,1,189,1,"enemy.png"))
    

winLable = pygame.font.Font(None, 50).render("ТИ ПЕРЕМІГ!!!", True, (10, 100, 100))
loseLable = pygame.font.Font(None, 50).render("Ти програв", True, (20, 230, 2))
game = True
while game:
    for i in range(len(enemy)):
        enemy[i].run()
    
    for event in pygame.event.get():

        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_d:
                herous.speedx = 1
            if event.key == pygame.K_p:
                pause(screen)

            if event.key == pygame.K_a:
                herous.speedx = -1
            if event.key == pygame.K_w:
                herous.speedy = -1

            if event.key == pygame.K_s:
                herous.speedy = 1
    for i in range( len(enemy) ):
        if enemy[i].rect.colliderect(herous.rect):
              
            enemy.pop(i)
            break
            
    
            
    
    
            
        
    
    
            
    herous.rect.y += herous.speedy
    herous.rect.x += herous.speedx
    
    screen.fill( (100, 156, 0))

    if len(enemy) == 0:
        screen.fill( (0, 156, 0))
        screen.blit(winLable, [250, 250])
        pygame.display.flip()
        game = False
    
    herous.draw(screen)
    
    for i in range(len(enemy)):
        enemy[i].draw(screen)
    for i in range(len(walls)):
        walls[i].draw(screen)
    pygame.display.flip()
    for i in range( len(walls) ):
        if walls[i].rect.colliderect(herous.rect):
            screen.fill( (0, 156, 0))
            screen.blit(loseLable, [250, 250])
            pygame.display.flip()
              
            game = False
    
    




    fps.tick(99)
