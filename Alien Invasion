# Import modul pygame
import pygame

# Import abc untuk abstrak kelas dan abstract method
from abc import ABC, abstractmethod

# Import random untuk logika spawn
import random

# Import pygame.local dari modul pygame
from pygame.locals import (
    RLEACCEL,
    K_UP,
    K_DOWN,
    K_LEFT,
    K_RIGHT,
    K_ESCAPE,
    K_SPACE,
    KEYDOWN,
    QUIT,
)
    
# Mengeset panjang dan tinggi screen
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# Fungsi untuk menggambar teks dalam screen
def draw_text(surface, text, size, x, y):
    font = pygame.font.SysFont("Arial Black  ", size)
    text_surface = font.render(text, True, (255, 255, 255))
    text_rect = text_surface.get_rect()
    text_rect.center = (x, y)
    surface.blit(text_surface, text_rect)
    
# Class entity yang menurunkan 'ABC' merupakan class abstrak yang memiliki abstract method update untuk digunakan oleh semua entity turunan
class Entity(ABC):
    @abstractmethod
    def update(self):
        pass

# Entity player sebagai entity yang digunakan user untuk bermain
class Player(Entity, pygame.sprite.Sprite):
    def __init__(self):
        super(Player, self).__init__()
        self.sprite=[]
        self.sprite.append(pygame.image.load('assets/spaceship1.png').convert())
        self.sprite.append(pygame.image.load('assets/spaceship2.png').convert())
        self.sprite.append(pygame.image.load('assets/spaceship3.png').convert())
        self.sprite.append(pygame.image.load('assets/spaceship4.png').convert())
        for sprite in self.sprite:
            sprite.set_colorkey((255,255,255), RLEACCEL)
        
        self.current_sprite=0
        self.surf = self.sprite[self.current_sprite]
        self.rect = self.surf.get_rect()
        # Agar player memulai game ditengah kiri screen
        self.rect.y = (SCREEN_HEIGHT // 2) - 50
        self.life=3
        self.alive=True
        
    # Player bergerak berdasarkan pencetan keyboard user
    def update(self, pressed_keys):
        #Sprite berpindah setiap frame
        self.current_sprite += 0.2
        if self.current_sprite >= len(self.sprite):
            self.current_sprite=0
        self.surf=self.sprite[int(self.current_sprite)]
        
        # Player akan bergerak sesuai tombol keyboard yang ditekan saat masih hidup
        if self.alive:
            if pressed_keys[K_UP]:
                self.rect.move_ip(0, -5)
            if pressed_keys[K_DOWN]:
                self.rect.move_ip(0, 5)
            if pressed_keys[K_LEFT]:
                self.rect.move_ip(-5, 0)
            if pressed_keys[K_RIGHT]:
                self.rect.move_ip(5, 0)
                
            # Logika untuk mengunci pergerakan player tetap dalam screen
            if self.rect.left < 0:
                self.rect.left = 0
            if self.rect.right > SCREEN_WIDTH:
                self.rect.right = SCREEN_WIDTH
            if self.rect.top <= 0:
                self.rect.top = 0
            if self.rect.bottom >= SCREEN_HEIGHT:
                self.rect.bottom = SCREEN_HEIGHT

# Entity bullet digunakan sebagai peluru laser yang ditembakan player untuk menghancurkan enemy
class Bullet(Entity, pygame.sprite.Sprite):
    def __init__(self):
        super(Bullet, self).__init__()
        self.sprite = []
        self.sprite.append(pygame.image.load('assets/shot1_1.png'))
        self.sprite.append(pygame.image.load('assets/shot1_2.png'))
        self.sprite.append(pygame.image.load('assets/shot1_3.png'))
        self.sprite.append(pygame.image.load('assets/shot1_4.png'))
        self.sprite.append(pygame.image.load('assets/shot1_5.png'))
        
        self.current_sprite = 0
        self.surf = self.sprite[self.current_sprite]
        self.rect = self.surf.get_rect(center=player.rect.center)
    
    # Bullet bergerak ke arah kanan dan akan hilang ketika sudah menyentuh ujung kanan layar screen
    def update(self):
        self.current_sprite += 0.2
        if self.current_sprite >= len(self.sprite):
            self.current_sprite=1
        self.surf=self.sprite[int(self.current_sprite)]
        self.rect.move_ip(10, 0)
        if self.rect.left < 0:
            self.kill
            
# Entity enemy sebagai lawan untuk player
class Enemy(Entity, pygame.sprite.Sprite):
    def __init__(self):
        super(Enemy, self).__init__()
        self.sprite=[]
        self.sprite.append(pygame.image.load('assets/Alien1.png').convert())
        self.sprite.append(pygame.image.load('assets/Alien2.png').convert())
        self.sprite.append(pygame.image.load('assets/Alien3.png').convert())
        
        for sprite in self.sprite:
            sprite.set_colorkey((255,255,255), RLEACCEL)
        
        self.current_sprite=0
        self.surf = self.sprite[self.current_sprite]
        # spawn random
        self.rect = self.surf.get_rect(
            center=(
                random.randint(SCREEN_WIDTH + 20, SCREEN_WIDTH + 100),
                random.randint(0, SCREEN_HEIGHT),
            )
        )
        # speed random
        self.speed = random.randint(5, 20)
    
    # Enemy bergerak ke kiri dan akan menghilang jika sudah melewati ujung kiri layar screen
    def update(self):
        self.current_sprite += 0.03
        if self.current_sprite >= len(self.sprite):
            self.current_sprite=1
        self.surf=self.sprite[int(self.current_sprite)]
        self.rect.move_ip(-self.speed, 0)
        if self.rect.right < 0:
            self.kill()
 
# Entity planet digunakan sebagai penghias latar game
class Planet(Entity, pygame.sprite.Sprite):
    def __init__(self):
        super(Planet, self).__init__()
        self.sprite = []
        self.sprite.append(pygame.image.load('assets/planet1.png'))
        self.sprite.append(pygame.image.load('assets/planet2.png'))
        self.sprite.append(pygame.image.load('assets/planet3.png'))
        self.sprite.append(pygame.image.load('assets/planet4.png'))
        self.sprite.append(pygame.image.load('assets/planet5.png'))
        self.current_sprite = 0
        self.surf = self.sprite[self.current_sprite]
         # spawn random
        self.rect = self.surf.get_rect(
            center=(
                random.randint(SCREEN_WIDTH + 20, SCREEN_WIDTH + 100),
                random.randint(0, SCREEN_HEIGHT),
            )
        )
        
    # Planet bergerak ke kiri dan akan menghilang jika sudah melewati ujung kiri layar screen
    def update(self):
        self.current_sprite += 0.1
        if self.current_sprite >= len(self.sprite):
            self.current_sprite=0
        self.surf=self.sprite[int(self.current_sprite)]
        self.rect.move_ip(-5, 0)
        if self.rect.right < 0:
            self.kill()

#Entity explosion digunakan ketika player/musuh dihancurkan
class Explosion(Entity, pygame.sprite.Sprite):
    def __init__(self,center):
        super(Explosion,self).__init__()
        self.sprite = []
        self.sprite.append(pygame.image.load('assets/Explosion1_1.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_2.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_3.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_4.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_5.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_6.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_7.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_8.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_9.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_10.png'))
        self.sprite.append(pygame.image.load('assets/Explosion1_11.png'))
        self.current_sprite=0
        
        self.surf=self.sprite[self.current_sprite]
        self.rect=self.surf.get_rect(center=center)
    
    # Ketika explosion di spawn akan dijalankan animasi explosion kemudian kill proses explosion
    def update(self):
        self.current_sprite += 0.5
        if self.current_sprite >= len(self.sprite):
            self.current_sprite=10
            self.kill()
        self.surf=self.sprite[int(self.current_sprite)]        
 
# Tombol replay untuk memulai game kembali   
class Replay(pygame.sprite.Sprite):
    exist=False
    def __init__(self):
        super(Replay ,self).__init__()
        self.surf = pygame.image.load("assets/replay.png")
        self.rect = self.surf.get_rect()
        self.rect.x=(SCREEN_WIDTH-self.surf.get_width())/2
        self.rect.y=(SCREEN_HEIGHT-self.surf.get_height())/2 + 10
    
    def clicked(self,mouse_pos):
        return self.rect.collidepoint(mouse_pos)

# Tombol exit untuk menghentikan permainan 
class Exit(pygame.sprite.Sprite):
    exist=False
    def __init__(self):
        super(Exit ,self).__init__()
        self.surf = pygame.image.load("assets/exit.png")
        self.rect = self.surf.get_rect()
        self.rect.x=(SCREEN_WIDTH-self.surf.get_width())/2
        self.rect.y=(SCREEN_HEIGHT-self.surf.get_height())/2 + 60
    
    def clicked(self,mouse_pos):
        return self.rect.collidepoint(mouse_pos)
        
# Initialize pygame
pygame.init()
pygame.display.set_caption('Alien Invasion')
icon=pygame.image.load("assets/alien1.png")
icon.set_colorkey((255,255,255),RLEACCEL)
pygame.display.set_icon(icon)

# Membuat screen dengan lebar SCREEN_WIDTH dan tinggi SCREEN_HEIGHT
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT), pygame.SRCALPHA)

# Membuat custom event dalam game
# Spawn enemy setiap interval 0,75 detik
ADDENEMY = pygame.USEREVENT + 1
pygame.time.set_timer(ADDENEMY, 750)

# Spawn planet setiap interval 6 detik
ADDPLANET = pygame.USEREVENT + 2
pygame.time.set_timer(ADDPLANET, 6000)

# Menambah score setuao interval 1 detik
ADDSCORE = pygame.USEREVENT + 3
pygame.time.set_timer(ADDSCORE, 1000)

# Membuat objek player
player = Player()

# Create groups sprite untuk masing-masing entity dan all sprite untuk menampung semua entity
# - group masing-masing sprite dibuat karena masing-masing sprite memiliki fungsi yang berbeda sehingga diperlukan group untuk memisahkan entity
# - group all sprite digunakan untuk merender semua entity
# - entity player tidak perlu memiliki groupnya sendiri dikarenakan hanya terdiri dari 1 objek
enemies = pygame.sprite.Group()
planets = pygame.sprite.Group()
bullets = pygame.sprite.Group()
explosions=pygame.sprite.Group()
all_sprites = pygame.sprite.Group()
all_sprites.add(player)

# Setup clock untuk framerate dan frekuensi tembakan
clock = pygame.time.Clock()
last_shot = pygame.time.get_ticks()
last_hit = pygame.time.get_ticks()

# Load dan menjalankan music dalam game (looping)
pygame.mixer.music.load("assets/music.mp3")
pygame.mixer.music.play(loops=-1)

# Load sound effect
blaster=pygame.mixer.Sound("assets/blaster.mp3")
collision_sound = pygame.mixer.Sound("assets/Collision.ogg")

# Mengeset volume sound effect
blaster.set_volume(0.5)
collision_sound.set_volume(100)

# Variabel yang digunakan dalam game
running=True
paused=False
lose=False
score=0

# Main loop
while running:
    
    # Menjalankan event dalam game
    for event in pygame.event.get():
        # Event ketika tombol ditekan
        if event.type == KEYDOWN:
            # Jika user menekan tombol esc, game akan di pause
            if event.key == K_ESCAPE:
                # fungsi ini hanya dapat dijalankan jika user belum kalah
                if not lose:
                    pygame.mixer.music.pause()
                    # toggle to pause the game
                    paused = not paused
                    
            # Jika player masi hidup dan menekan tombol space maka akan ditembakan laser
            elif player.alive and event.key == K_SPACE:
                now = pygame.time.get_ticks()
                # Frekuensi laser 0,25 detik pertembakan
                if now - last_shot > 250:
                    blaster.play()
                    new_bullet = Bullet()
                    bullets.add(new_bullet)  
                    all_sprites.add(new_bullet)
                    last_shot = now 
        
        
        
        elif event.type == pygame.MOUSEBUTTONDOWN:
            mouse_pos = pygame.mouse.get_pos()
            # Jika tombol replay di klik maka permainan akan di replay
            if Replay.exist:
                if replaybutton.clicked(mouse_pos):
                    lose=not lose
                    score=0
                    for entity in all_sprites:
                        entity.kill()
                    player=Player()
                    all_sprites.add(player)
            # Jika tombol exit di klik maka permainan akan berakhir
            if Exit.exist:
                if exitbutton.clicked(mouse_pos):
                    running=False
                
        # Jika user menekan tombol x pada menu screen maka game akan berhenti
        elif event.type == QUIT:
            running = False
                
        # Menambah entity enemy
        elif event.type == ADDENEMY:
            new_enemy = Enemy()
            enemies.add(new_enemy)
            all_sprites.add(new_enemy)
        
        # Menambah entity planet
        elif event.type == ADDPLANET:
            new_planet = Planet()
            planets.add(new_planet)
            all_sprites.add(new_planet)

        # Menambah Score 1 setiap 1 detik
        elif event.type == ADDSCORE:
            score+=1
    
    if not paused and not lose:
        # Game berjalan dalam 30 fps
        clock.tick(30)
        # Membuat lokasi tombol replay & exit tidak berfungsi ketika belum dipanggil
        Replay.exist=False
        Exit.exist=False
        
        #Unpause music ketika game sedang tidak di pause
        pygame.mixer.music.unpause()        
        
        # Variabel untuk menyimpan keyboard yang user tekan
        pressed_keys = pygame.key.get_pressed()
        # Update player berdasarkan variabel pressed_keys
        player.update(pressed_keys)
        
        # Update entity
        enemies.update()
        planets.update()
        bullets.update()
        explosions.update()
               
        # Mengisi screen dengan background space
        bg = pygame.image.load("assets/bg.png")
        screen.blit(bg,(0,0))

        # Menggambar semua sprite
        for entity in all_sprites:
            screen.blit(entity.surf, entity.rect)
        
        # Text untuk score & life
        draw_text(screen, "Score: {}".format(score), 25, SCREEN_WIDTH // 2, 10)
        draw_text(screen, "Life: {}".format(player.life), 15, 30, 10)
        
        # Mengecek apakah player menabrak musuh
        if player.alive:
            if pygame.sprite.spritecollideany(player, enemies):
                now=pygame.time.get_ticks()
                # Player akan kebal selama 1 detik setelah menabrak musuh
                if now-last_hit > 1000:
                    # Jika iya maka player akan kalah
                    new_explosion=Explosion(player.rect.center)
                    explosions.add(new_explosion)
                    all_sprites.add(new_explosion)
                    collision_sound.play()
                    player.life-=1
                    last_hit=now
                    if player.life==0:
                        player.alive=False
                        player.kill()
                        lose=not lose
        
        # Ketika enemy terkena laser player maka enemy & laser akan hancur dan score + 5
        collisions = pygame.sprite.groupcollide(bullets, enemies, True, True)
        for bullet, collided_enemies in collisions.items():
            for enemy in collided_enemies:
                collision_sound.play()
                score += 5
                new_explosion=Explosion(enemy.rect.center)
                explosions.add(new_explosion)
                all_sprites.add(new_explosion)   
       
    # Paused         
    elif paused:
        clock.tick(0)
        draw_text(screen, "GAME PAUSED", 30, SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2)
    
    # Jika user kalah maka akan ditampilkan "Game Over" kemudian tombol replay dan exit akan muncul
    elif lose:
        clock.tick(30)
        # Menonaktifkan pertambahan score ketika kalah
        pygame.time.set_timer(ADDSCORE, 0)
        
        bg = pygame.image.load("assets/bg.png")
        screen.blit(bg,(0,0))
        draw_text(screen, "Game Over", 30, (SCREEN_WIDTH // 2), (SCREEN_HEIGHT // 2) - 60)
        draw_text(screen, f"Your Score : {score}", 20, (SCREEN_WIDTH // 2), (SCREEN_HEIGHT // 2) - 30)
        replaybutton=Replay()
        exitbutton=Exit()
        Replay.exist=True
        Exit.exist=True
        all_sprites.add(replaybutton,exitbutton)
        enemies.update()
        planets.update()
        bullets.update()
        explosions.update()
        for entity in all_sprites:
            screen.blit(entity.surf, entity.rect)      

    pygame.display.flip()
