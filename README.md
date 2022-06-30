# СОДЕРЖАНИЕ
## Описание проекта
## Значения файлов
## Инструкция
## Исходный код проекта
# ОПИСАНИЕ
Лабиринт без лабиринта™ - это видеоигра о уничтожение врагов мешающие дойти до конечной цели для завершения игры с победой. Проект был создан исключительно для получения опыта в библеотеки pygame.
# ЗНАЧЕНИЯ ФАЙЛОВ
| Имя файла     | Значения               |
| ------------- |:----------------------:|
| labirint.py   | файл игры              |
| amogus.png    | текстура игрока и врага|
| laser.png     | текстура пули игрока   |
| back.png      | задний фон             |
| shaurma.png   | конечная цель          |
# ИНСТРУКЦИЯ
Запустите "labirint.exe" в закрепленном файле на GitHub, готово!
# ИСХОДНЫЙ КОД

```python
import pygame
from pygame import* #подключаем библеотеку pygame
pygame.init() 
'''Переменные для картинок'''
img_back = 'galaxy_2.jpg' 
img_hero = 'hero.png' 
img_enemy = 'enemy.png'
img_goal = 'goal.png'
img_bullet = 'bullet.png'
'''Доп.'''
win_width = 700
win_height = 500
back2 = (0, 0, 0)
display.set_caption('Лабиринт')
window = display.set_mode((win_width, win_height))
back = transform.scale(image.load(img_back), (win_width, win_height))
mw = pygame.display.set_mode((win_width,win_height))
mw.fill(back2)
'''Шрифт'''
font.init()
font = font.SysFont('Comic Sans MS',50)
win = font.render('YOU WIN!!!',True,(0,255,0))
lose = font.render('YOU LOSE! :(',True,(255,0,0))

'''Музыка'''
#mixer.init() #подключаем музыку к игре
#mixer.music.load('BLOOD_FLAME.ogg') #загружаем файл
#mixer.music.play()
'''Классы'''
class GameSprite(sprite.Sprite): #Класс - родитель для других классов
    def __init__(self, player_image, player_x, player_y, withd, height, speed):
        #Вызываем конструктор класса Sprite
        sprite.Sprite.__init__(self)
        #Каждый спрайт должен хранить свойство image - изображение
        self.image = transform.scale(image.load(player_image), (withd, height))    
        self.speed = speed
        #каждый спрайт должен хранить свойство rect - прямоугольник, в который он вписан
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
    #метод, отрисованный героя на окне
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))
class Player(GameSprite):
    def uptade(self): #метод передвижения
        keys = key.get_pressed() #подключаем клавиатуру
        if keys[K_a] and self.rect.x > 5:
            self.rect.x -= self.speed
        if keys[K_d] and self.rect.x < win_width - 20:
            self.rect.x += self.speed
        if keys[K_w] and self.rect.y >5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < win_height - 20:
            self.rect.y += self.speed
    #метод "выстрел" (используем место игрока, чтобы создать там пулю)    
    def fire(self):
        bullet = Bullet(img_bullet, self.rect.right, self.rect.centery, 15, 20, 15)
        bullets.add(bullet)      
class Enemy(GameSprite):
    side = 'left'
    def update(self):
        if self.rect.x <= 470:
            self.side = 'right'
        if self.rect.x >= win_width - 85:
            self.side = 'left'
        if self.side == 'left':
            self.rect.x -= self.speed
        else:
            self.rect.x += self.speed
class Enemy2(GameSprite):
    side = 'left'
    def update(self):
        if self.rect.x <= 100:
            self.side = 'right'
        if self.rect.x >= win_width - 250:
            self.side = 'left'
        if self.side == 'left':
            self.rect.x -= self.speed
        else:
            self.rect.x += self.speed
class Enemy3(GameSprite):
    side = 'up'
    def update(self):
        if self.rect.y <= 130:
            self.side = 'up'
        if self.rect.y >= win_width - 270:
            self.side = 'left'
        if self.side == 'left':
            self.rect.y -= self.speed
        else:
            self.rect.y += self.speed
class Enemy4(GameSprite):
    side = 'up'
    def update(self):
        if self.rect.y <= 10:
            self.side = 'up'
        if self.rect.y >= win_width - 340:
            self.side = 'left'
        if self.side == 'left':
            self.rect.y -= self.speed
        else:
            self.rect.y += self.speed
class Wall(sprite.Sprite):
    def __init__(self, red, green, blue, wall_x, wall_y, width, height):
        super().__init__()
        self.red = red
        self.green = green
        self.blue = blue
        self.w = width
        self.h = height
        #Каждый спрайт должен хранить свойство image - Surface - прямоугольная подложка
        self.image = Surface((self.w, self.h))
        self.image.fill((red,green,blue))
        #каждый спрайт должен хранить свойство rect - прямоугольник, в который он вписан
        self.rect = self.image.get_rect()
        self.rect.x = wall_x
        self.rect.y = wall_y
    def draw_wall(self):
        windows.blit(self.image, (self.rect.x, self.rect.y))
#класс спрайта-пули
class Bullet(GameSprite):
    #движение врага
    def update(self):
        self.rect.x += self.speed
        #исчезает, если дойдет до края экрана
        if self.rect.x > win_width+10:
            self.kill()
'''Окно игры'''
win_width = 700
win_height = 500
display.set_caption('Лабиринт')
window = display.set_mode((win_width, win_height))
back = transform.scale(image.load(img_back), (win_width, win_height))
'''Персонажи'''
hero = Player(img_hero, 5, win_height - 80, 30, 30, 5)
monster = Enemy(img_enemy, win_width - 80, 280, 65, 65, 6)
final = GameSprite(img_goal, 630, 420, 60, 60, 0)
monster2 = Enemy2(img_enemy, win_width - 600, 60, 65, 65, 6)
monster3 = Enemy3(img_enemy, win_width - 380, 130, 65, 65, 6)
monster4 = Enemy4(img_enemy, win_width - 120, 130, 65, 65, 5)
'''Счётчик очков'''
points = 0
'''Стены'''
w1 = Wall(255, 255, 255, 100, 20, 450, 10)
w2 = Wall(255, 255, 255, 100, 480, 350, 10)
w3 = Wall(255, 255, 255, 100, 20, 10, 380)
w4 = Wall(255, 255, 255, 200, 130, 10, 200)
w5 = Wall(255, 255, 255, 450, 130, 10, 360)
w6 = Wall(255, 255, 255, 300, 20, 10, 30)
w7 = Wall(255, 255, 255, 390, 120, 130, 10)
w8 = Wall(255, 255, 255, 300, 130, 10, 200)
w9 = Wall(255, 255, 255, 200, 280, 10, 200)
#w1 = GameSprite('way.png',win_width / 2 - win_width / 3, win_height / 2, 300, 50, 0)
#w2 = GameSprite('way.png', 370, 100, 50, 400, 0)
'''Группы спрайтов'''
bullets = sprite.Group()
monsters = sprite.Group()
walls = sprite.Group()
'''Добавление спрайтов в группу'''
monsters.add(monster)
monsters.add(monster2)
monsters.add(monster3)
monsters.add(monster4)
walls.add(w1)
walls.add(w2)
walls.add(w3)
walls.add(w4)
walls.add(w5)
walls.add(w6)
walls.add(w7)
walls.add(w8)
walls.add(w9)
'''Игровой цикл'''
game = True
finish = False
clock = time.Clock()
FPS = 30
while game:
    for e in event.get():
        if e.type == QUIT:
            game = False
        elif e.type == KEYDOWN:
            if e.key == K_SPACE:
                hero.fire()
    if finish != True:
        window.blit(back, (0,0))
        walls.draw(window)
        monsters.draw(window)
        monsters.update()
        hero.reset()
        hero.uptade()
        final.reset()
        bullets.draw(window)
        bullets.update()
        sprite.groupcollide(bullets, walls, True, False)
        if sprite.groupcollide(bullets, monsters, True, True):
            points+=1
        x = font.render(str(points), True, (255, 255, 255))
        window.blit(x, (20, 20))
        #Проигрыш
        if sprite.spritecollide(hero, monsters, False):
                hero.kill()
                finish = True
                window.blit(lose, (200,200))
        if sprite.spritecollide(hero, walls, False):
                hero.kill()
                finish = True
                window.blit(lose, (200,200))
        #Победа
        if sprite.collide_rect(hero,final):
            finish = True
            window.blit(win, (200,200))
    display.update()
    clock.tick(FPS)
```
