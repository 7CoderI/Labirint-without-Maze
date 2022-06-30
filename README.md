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

'''python
from pygame import* #подключаем библеотеку pygame
'''Переменные для картинок'''
img_back = 'back.png'
img_hero = 'amogus.png'
img_enemy = 'amogus.png'
img_goal = 'shaurma.png'
img_bullet = 'laser.png'
'''Музыка'''
#mixer.init() # подключаем музыку к игре
#mixer.music.load('ASAp.ogg') # загружаем файл.
#mixer.music.play()
'''Шрифт'''
font.init()
font = font.SysFont('Comic Sans MS',50)
win = font.render('YOU WIN!!!',True,(255,255,0))
lose = font.render('YOU LOSE:(!',True,(255,255,255))
count = 0
'''Классы'''
class GameSprite(sprite.Sprite): # Класс - родитель для других классов.
    def __init__(self, player_image, player_x, player_y, width, height, speed):
        #Вызываем конструктор класса Sprite
        sprite.Sprite.__init__(self)
        # Каждый спрайт должен хранить свойство image - изображение
        self.image = transform.scale(image.load(player_image), (width, height))
        self.speed = speed
        #  каждый спрайт должен хранить свойство rect - прямоугольник, в который он вписан
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
    # метод, отрисовывающий героя на окне
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))
class Player(GameSprite):
    def update(self): # метод передвижения
        keys = key.get_pressed() #подключаем клавиатуру.
        if keys[K_a] and self.rect.x > 5:
            self.rect.x -= self.speed
        if keys[K_d] and self.rect.x < win_width - 80:
            self.rect.x += self.speed
        if keys[K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < win_height - 80:
            self.rect.y += self.speed
    # метод "выстрел" (используем место игрока, чтобы создать там пулю)
    def fire(self):
        bullet = Bullet(img_bullet, self.rect.right, self.rect.centery, 15, 20, 15)
        bullets.add(bullet)

class Enemy(GameSprite):
    side = "left"
    def update(self):
        if self.rect.x <= 470:
            self.side = "right"
        if self.rect.x >= win_width - 85:
            self.side = "left"
        if self.side == "left":
            self.rect.x -= self.speed
        else:
            self.rect.x += self.speed
class Enemy2(GameSprite):
    side = "left"
    def update(self):
        if self.rect.x <= 100:
            self.side = "right"
        if self.rect.x >= win_width - 250:
            self.side = "left"
        if self.side == "left":
            self.rect.x -= self.speed
        else:
            self.rect.x += self.speed
class Enemy3(GameSprite):
    side = "up"
    def update(self):
        if self.rect.y <= 130:
            self.side = "up"
        if self.rect.y >= win_width - 270:
            self.side = "left"
        if self.side == "left":
            self.rect.y -= self.speed
        else:
            self.rect.y += self.speed
class Enemy4(GameSprite):
    side = "up"
    def update(self):
        if self.rect.y <= 10:
            self.side = "up"
        if self.rect.y >= win_width - 340:
            self.side = "left"
        if self.side == "left":
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
        # Каждый спрайт должен хранить свойство image - Surface - прямоугольная подложка.
        self.image = Surface((self.w, self.h))
        self.image.fill((red,green,blue))
        # каждый спрайт должен хранить свойство rect - прямоугольник, в который он вписан
        self.rect = self.image.get_rect()
        self.rect.x = wall_x
        self.rect.y = wall_y
    def draw_wall(self):
        window.blit(self.image, (self.rect.x, self.rect.y))
# класс спрайта-пули  
class Bullet(GameSprite):
    # движение врага
    def update(self):
        self.rect.x += self.speed
        # исчезает, если дойдет до края экрана
        if self.rect.x > win_width+10:
            self.kill()
'''Окно игры'''
win_width = 700
win_height = 500
display.set_caption("Лабиринт")
window = display.set_mode((win_width, win_height))
back = transform.scale(image.load(img_back),(win_width,win_height))
'''Персонажи'''
hero = Player(img_hero, 5, win_height - 80, 40,40,10)
monster = Enemy(img_enemy, win_width - 80, 280,65,65 ,2)
final = GameSprite(img_goal, win_width - 120, win_height - 80,65,65, 0)
monster2 = Enemy2(img_enemy, win_width - 600, 60,65,65 ,2)
monster3 = Enemy3(img_enemy, win_width - 380, 130,65,65 ,2)
monster4 = Enemy4(img_enemy, win_width - 120, 130,65,65 ,2)
'''Стены'''
w1 = Wall(154, 205, 50, 100, 20 , 450, 10)
w2 = Wall(154, 205, 50, 100, 480, 350, 10)
w3 = Wall(154, 205, 50, 100, 20 , 10, 380)
w4 = Wall(154, 205, 50, 200, 130, 10, 200)
w5 = Wall(154, 205, 50, 450, 130, 10, 360)
w6 = Wall(154, 205, 50, 300, 20, 10, 30)
w7 = Wall(154, 205, 50, 390, 120, 130, 10)
w8 = Wall(154, 205, 50, 300, 130, 10, 200)
w9 = Wall(154, 205, 50, 200, 280, 10, 200)
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
FPS = 60
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
        monsters.update()
        text_count = font.render(str(count),True,(255,255,255))
        window.blit(text_count, (10,10))
        monsters.draw(window)
        hero.reset()
        hero.update()
        final.reset()
        bullets.draw(window)
        bullets.update()
        collides = sprite.groupcollide(bullets, monsters, True, True)
        for c in collides:
           c.kill()
           count = count + 1


        #Проигрыш
        if sprite.collide_rect(hero,monster) or sprite.collide_rect(hero,monster2) or sprite.collide_rect(hero, w1) or sprite.collide_rect(hero,w2) or sprite.collide_rect(hero,w3) or sprite.collide_rect(hero,w4):
            finish = True
            window.blit(lose, (200,200))
        #Победа
        if sprite.collide_rect(hero,final):
            finish = True
            window.blit(win, (200,200))
    
    display.update()
    clock.tick(FPS)
'''
