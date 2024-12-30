#### A game by Moein Avarzamani ####


# !!MUST INSTALL FOLLOWING MODULE!!
# pip install pygame
import pygame
from pygame import mixer
from pygame.locals import *


# Other needed modules
import random
import math
import sys


# Main initialization
pygame.init()



# Game display size
screen = pygame.display.set_mode((1000, 800))

# Game's name
pygame.display.set_caption("Dodge Ball")



# Icon settings
icon = pygame.image.load("Icon.png")
pygame.display.set_icon(icon)



# Background Image
global Backer
Background = pygame.image.load("Background1.jpg")
DiffBack = pygame.image.load("Sar.jpg")
ExtBack = pygame.image.load("Crack_Back.jpg")
Hell = pygame.image.load("HellBackground.jpg")



# Background Music
def Back_Music():
    mixer.music.load("BackgroundMusic.mp3")
    mixer.music.play(-1)


# Different music when extreme mode is played
global Rock
Rock = 0


# Extreme mode music
def Ext_Music():
    mixer.music.load("ExtremeMode.mp3")
    mixer.music.play(-1)


# Normal background music at start
Back_Music()


# Colors used in the future :)
Back_Color = pygame.Color('#1d3c45')
Button_Color = pygame.Color('#ADEFD1FF')
Overlay_Color = pygame.Color('#201e20')



# Extreme mode has a different color
global Button_Color_Ext
Button_Color_Ext = pygame.Color('#ADEFD1FF')


# Ever-changing difficulty name
global Diff_Name


# Default difficulty
Diff_Name = "Normal"



# Main menu
def Menu():


    # The other menus are turned off for better performance
    # (This repeats on all the menu function)
    global ext
    ext = False

    global diff
    diff = False

    global Surem
    Surem = False


    # Main while loop for the Main menu
    global menu
    menu = True

    while menu:

        # The game starts when you push either the Space key or the Enter key
        for events in pygame.event.get():

            if events.type == pygame.KEYDOWN:

                if events.key == pygame.K_SPACE or events.key == pygame.K_RETURN:

                    start_the_game()


            # The game closes when you push esc key or hit the x button on top right
            if events.type == pygame.QUIT:

                pygame.quit()

                sys.exit()


            if events.type == pygame.KEYDOWN:

                if events.key == pygame.K_ESCAPE:

                    pygame.quit()

                    sys.exit()



        # Main menu background color
        screen.fill(Back_Color)


        # The glorious ""Writte by Moein Zamani"" setting
        title_font = pygame.font.SysFont("gabriola", 100)
        title_text = title_font.render("Written by Moein Zamani", True, Button_Color)
        screen.blit(title_text, (100, 40))


        # Mouse position... pretty self explanatory
        MousePosition = pygame.mouse.get_pos()


        # The "Start Game" button settings
        if 370 < MousePosition[0] < 620 and 300 < MousePosition[1] < 342:
            Game_Start = pygame.draw.rect(screen, Overlay_Color, (370, 300, 250, 40))
            click = pygame.mouse.get_pressed()

            # When pressed: start the game
            if click[0] == 1:
                start_the_game()


        # The "Select Difficulty" button settings
        if 377 < MousePosition[0] < 618 and 500 < MousePosition[1] < 531:
            quit_game = pygame.draw.rect(screen, Overlay_Color, (377, 500, 240, 30))
            click = pygame.mouse.get_pressed()

            # When pressed: go to the Difficulty Menu
            if click[0] == 1:
                DiffMenu()


        #The "Exit Game" button settings
        if 401 < MousePosition[0] < 581.5 and 700 < MousePosition[1] < 731:
            pygame.draw.rect(screen, Overlay_Color, (401, 700, 180, 30))
            click = pygame.mouse.get_pressed()

            # When pressed: exit the game
            if click[0] == 1:
                pygame.quit()
                sys.exit()


        # The "Start Game" button's font, size and color
        bttn_font = pygame.font.Font("freesansbold.ttf",45)
        bttn_txt = bttn_font.render("Start Game", True, Button_Color)
        screen.blit(bttn_txt, (370, 300))


        # The "Select Difficulty" button's font, size and color
        bttn_font = pygame.font.Font("freesansbold.ttf", 30)
        bttn_txt = bttn_font.render("Select Difficulty", True, Button_Color)
        screen.blit(bttn_txt, (377, 500))


        # The "Exit Game" button's font, size and color
        bttn_font = pygame.font.Font("freesansbold.ttf", 30)
        bttn_txt = bttn_font.render("Exit Game", True, Button_Color)
        screen.blit(bttn_txt, (413, 700))


        # The "Selected Difficulty" button's font, size and color
        bttn_font = pygame.font.Font("freesansbold.ttf", 15)
        bttn_txt = bttn_font.render("Difficulty: " + Diff_Name, True, Button_Color)
        screen.blit(bttn_txt, (5, 780))




        # Necessary for actually showing the Main menu
        pygame.display.update()


# Ever-changing difficulty (Number of enemy balls)
global y

# Default number of enemies
y = 15


# The difficulty selection menu
# Pops up when the "Select Difficulty" button is pressed
def DiffMenu():

    global menu
    menu = False

    global Surem
    Surem = False

    global ext
    ext = False

    # Different color for the extreme difficulty
    global Button_Color_Ext


    global y
    global Diff_Name

    global Rock

    # Main while loop for the difficulty menu
    global diff
    diff = True

    while diff:


        # Background Image for the difficulty menu
        screen.fill((0,0,0))
        screen.blit(DiffBack,(0,0))


        # The game closes when you hit the x button on top right
        for events in pygame.event.get():

            if events.type == pygame.QUIT:
                pygame.quit()
                sys.exit()


             # Back to main menu when esc is pressed
            if events.type == pygame.KEYDOWN:

                if events.key == pygame.K_ESCAPE:

                    Menu()


        # Again... pretty self explanatory
        MousePosition = pygame.mouse.get_pos()



        # The "Extreme" button settings
        if 420 < MousePosition[0] < 563 and 150 < MousePosition[1] < 181:

            # The extreme button changes color when you hold your mouse over it
            # Because it's the toughest difficulty and it's epic
            Button_Color_Ext = pygame.Color('#FF0000')
            Extreme_Diff = pygame.draw.rect(screen, Overlay_Color, (426, 150, 150, 30))


            # When pressed: Set the number of enemies to 26 and return to the main menu
            # Music stops and takes you to the "Are you sure?" slide
            click = pygame.mouse.get_pressed()
            if click[0] == 1:
                y = 25
                Diff_Name = "Extreme"
                mixer.music.stop()
                You_Sure()


        # Default color when it's not held over by your mouse
        else:
            Button_Color_Ext = pygame.Color('#ADEFD1FF')



        # The "Hard" button settings
        if 650 < MousePosition[0] < 735 and 350 < MousePosition[1] < 381:
            Hard_Diff = pygame.draw.rect(screen, Overlay_Color, (648, 350, 87, 30))


            # When pressed: Set the number of enemies to 19 and return to the main menu
            click = pygame.mouse.get_pressed()
            if click[0] == 1:
                y = 18
                Diff_Name = "Hard"

                # Just incase something goes wrong with the music
                if Rock == 1:
                    Rock = 0
                    mixer.music.stop()
                    Back_Music()
                Menu()



        # The "Normal" button settings
        if 250 < MousePosition[0] < 375 and 350 < MousePosition[1] < 381:
            Normal_Diff = pygame.draw.rect(screen, Overlay_Color, (249, 350, 125, 30))


            # When pressed: Set the number of enemies to 16 and return to the main menu
            click = pygame.mouse.get_pressed()
            if click[0] == 1:
                y = 15
                Diff_Name = "Normal"

                # Just incase something goes wrong with the music
                if Rock == 1:
                    Rock = 0
                    mixer.music.stop()
                    Back_Music()
                Menu()



        # The "Easy" button settings
        if 650 < MousePosition[0] < 735 and 550 < MousePosition[1] < 581:
            Easy_Diff = pygame.draw.rect(screen, Overlay_Color, (650, 550, 83, 37))


            # When pressed: Set the number of enemies to 9 and return to the main menu
            click = pygame.mouse.get_pressed()
            if click[0] == 1:
                y = 8
                Diff_Name = "Easy"

                # Just incase something goes wrong with the music
                if Rock == 1:
                    Rock = 0
                    mixer.music.stop()
                    Back_Music()
                Menu()



        # The "Peaceful" button settings
        if 250 < MousePosition[0] < 394 and 550 < MousePosition[1] < 581:
            Peaceful_Diff = pygame.draw.rect(screen, Overlay_Color, (249, 550, 147, 30))


            # When pressed: Set the number of enemies to 26 and return to the main menu
            click = pygame.mouse.get_pressed()
            if click[0] == 1:
                y = 5
                Diff_Name = "Peaceful"

                # Just incase something goes wrong with the music
                if Rock == 1:
                    Rock = 0
                    mixer.music.stop()
                    Back_Music()
                Menu()


        # The "Extreme" button's font, size and color
        bttn_font = pygame.font.Font("freesansbold.ttf",35)
        bttn_txt = bttn_font.render("Extreme", True, Button_Color_Ext)
        screen.blit(bttn_txt, (430, 150))


        # The "Hard" button's font, size and color
        bttn_font = pygame.font.Font("freesansbold.ttf", 35)
        bttn_txt = bttn_font.render("Hard", True, Button_Color)
        screen.blit(bttn_txt, (650, 350))


        # The "Normal" button's font, size and color
        bttn_font = pygame.font.Font("freesansbold.ttf", 35)
        bttn_txt = bttn_font.render("Normal", True, Button_Color)
        screen.blit(bttn_txt, (250, 350))


        # The "Easy" button's font, size and color
        bttn_font = pygame.font.Font("freesansbold.ttf", 35)
        bttn_txt = bttn_font.render("Easy", True, Button_Color)
        screen.blit(bttn_txt, (650, 550))


        # The "Peaceful" button's font, size and color
        bttn_font = pygame.font.Font("freesansbold.ttf", 35)
        bttn_txt = bttn_font.render("Peaceful", True, Button_Color)
        screen.blit(bttn_txt, (250, 550))


        # Necessary for actually showing the Difficulty menu
        pygame.display.update()



# The "Are you sure?" menu function
def You_Sure():

    global Diff_Name
    global y

    global ext
    ext = False

    global diff
    diff = False

    global menu
    menu = False

    global Rock

    global Surem
    Surem = True

    while Surem:

        # Background color
        screen.fill((0,0,0))


        # The game closes when you hit the x button on top right
        for events in pygame.event.get():

            if events.type == pygame.QUIT:
                pygame.quit()
                sys.exit()


            # Back to difficulty menu when esc is pressed
            if events.type == pygame.KEYDOWN:

                if events.key == pygame.K_ESCAPE:
                    Back_Music()
                    Diff_Name = "Normal"
                    y = 15
                    Menu()


        # Again x2... pretty self explanatory
        MousePosition = pygame.mouse.get_pos()

        if 635 < MousePosition[0] < 746 and 508 < MousePosition[1] < 588:
            Easy_Diff = pygame.draw.rect(screen, Overlay_Color, (638, 508, 105, 80))


            click = pygame.mouse.get_pressed()
            if click[0] == 1:

                # The extreme music is played
                if Rock == 0:
                    Rock = 1
                    mixer.music.stop()
                    Ext_Music()
                Ext_Menu()


        if 240 < MousePosition[0] < 325 and 513 < MousePosition[1] < 588:
            Easy_Diff = pygame.draw.rect(screen, Overlay_Color, (240, 513, 83, 75))


            # Back to difficulty menu when "No" is pressed
            # Back to normal music
            # Back to normal difficulty
            click = pygame.mouse.get_pressed()
            if click[0] == 1:
                Back_Music()
                Diff_Name = "Normal"
                y = 15
                Menu()


        # The "Are you sure?" text's font, size and color
        title_font = pygame.font.SysFont("chiller", 100)
        title_text = title_font.render("ARE YOU SURE?", True, Button_Color)
        screen.blit(title_text, (240, 100))


        # The "There is no going back" text's font, size and color
        title_font = pygame.font.SysFont("chiller", 35)
        title_text = title_font.render("There is no going back", True, Button_Color)
        screen.blit(title_text, (375, 250))


        # The "Yes" button's font, size and color
        title_font = pygame.font.SysFont("chiller", 100)
        title_text = title_font.render("Yes", True, Button_Color)
        screen.blit(title_text, (640, 500))


        # The "No" button's font, size and color
        title_font = pygame.font.SysFont("chiller", 100)
        title_text = title_font.render("No", True, Button_Color)
        screen.blit(title_text, (240, 500))


        # Necessary for actually showing the "Are you sure?" menu
        pygame.display.update()



# The extreme mode menu
def Ext_Menu():
    global Surem
    Surem = False

    global menu
    menu = False

    global diff
    diff = False

    global ext
    ext = True


    while ext:
        # Background Image for the extreme menu
        screen.fill((0,0,0))
        screen.blit(ExtBack,(0,0))


        # The game closes when you hit the x button on top right
        for events in pygame.event.get():

            # The game starts when you push either the Space key or the Enter key

            if events.type == pygame.KEYDOWN:

                if events.key == pygame.K_SPACE or events.key == pygame.K_RETURN:
                    start_the_game()



            # The game closes when you hit the x button on top right
            if events.type == pygame.QUIT:
                pygame.quit()
                sys.exit()



            # The game closes when you hit the esc button
            if events.type == pygame.KEYDOWN:

                if events.key == pygame.K_ESCAPE:

                    pygame.quit()

                    sys.exit()


        # Again x3... pretty self explanatory
        MousePosition = pygame.mouse.get_pos()

        # The "Start Game" button settings
        if 375 < MousePosition[0] < 622 and 370 < MousePosition[1] < 418:
            Game_Start = pygame.draw.rect(screen, Overlay_Color, (378, 370, 243, 45))
            click = pygame.mouse.get_pressed()

            # When pressed: start the game
            if click[0] == 1:
                start_the_game()




        # The "Start Game" button's font, size and color
        title_font = pygame.font.SysFont("jokerman",45)
        title_txt = title_font.render("Start Game", True, "#FF0000")
        screen.blit(title_txt, (378, 355))


        # Necessary for actually showing the extreme menu
        pygame.display.update()




# Main Game
# (This is where it gets complicated)
def start_the_game():

    # Player's Image
    Player = pygame.image.load("golf-ball.png")

    # Player's start position (cordinates)
    xAxis = 500
    yAxis = 600

    # Player's speed (set to 0 for now)
    xAxis_change = 0
    yAxis_change = 0


    # Enemy settings
    # All of them are lists becuase they can change each game
    Enemy = []
    ExAxis = []
    EyAxis = []
    ExAxis_change = []
    EyAxis_change = []


    # The number of enemies **appearing** depending on what difficulty you chose
    global y
    NumOfEnemies = [1]
    NumOfEnemies2 = [x+y for x in NumOfEnemies]
    XX = []
    YY = []


    # Giving each individual enemy a different speed and a different start position
    for i in range(NumOfEnemies2[0]):

        # Random start position
        XX .append( round(random.uniform(-1, 1), 1) )
        YY .append( round(random.uniform(-1, 1), 1) )

        # Enemy's Image
        Enemy.append( pygame.image.load("Red_Ball.png") )

        # Random speed
        ExAxis.append(random.randint(100, 650))
        EyAxis.append(random.randint(100,350))

        # Giving the "i" total amount of different speeds to the "i" amount of enemies
        ExAxis_change.append(XX[i])
        EyAxis_change.append(YY[i])


    # Necessary for actually showing the player
    # Function called at line
    def player(x, y):
        screen.blit(Player, (x, y))


    # Necessary for actually showing the enemies
    # Function called at line
    def enemy(x, y, i):
        screen.blit(Enemy[i], (x, y))


    # When the player collides with an enemy
    def isCollision(PlayerX, PlayerY, EnemyX, EnemyY):

        # The distance between 2 points formula
        distance = math.sqrt((math.pow(EnemyX - PlayerX, 2)) + (math.pow(EnemyY - PlayerY, 2)))

        # If the distance is lower than 30 pixels...
        if distance < 30:

            # Play the "Dying" sound
            Hurt_Noise = mixer.Sound("Hurt.wav")
            Hurt_Noise.play()
            return True




    # Main loop
    # Mainly for controls
    running = True
    while running:
        global Backer
        if Diff_Name == "Extreme":
            Backer = Hell
        else:
            Backer = Background


        global diff
        diff = False

        global Surem
        Surem = False


        # The game background Image
        screen.fill((0, 0, 0))
        screen.blit(Backer, (0, 0))


        # The game closes when you hit the x button on top right
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False


            # The player's controls
            if event.type == pygame.KEYDOWN:


                # When the left arrow key is pushed, your horizontal speed changes to -1
                if event.key == pygame.K_LEFT:
                    xAxis_change = -1


                # When the right arrow key is pushed, your horizontal speed changes to +1
                if event.key == pygame.K_RIGHT:
                    xAxis_change = 1


                # When the up arrow key is pushed, your vertical speed changes to -1
                if event.key == pygame.K_UP:
                    yAxis_change = -1


                # When the left arrow key is pushed, your vertical speed changes to +1
                if event.key == pygame.K_DOWN:
                    yAxis_change = 1


            # When the buttons are released: speed goes back to 0 again, so the player stops moving
            if event.type == pygame.KEYUP:


                # Changing the horizontal speed back to 0 when the key is released
                if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                    xAxis_change = 0

                # Changing the vertical speed back to 0 when the key is released
                if event.key == pygame.K_UP or event.key == pygame.K_DOWN:
                    yAxis_change = 0


        # Player Movement
        # The player's cordinates changing
        xAxis += xAxis_change
        yAxis += yAxis_change


        # You cannot go beyond the screen border
        if xAxis <= 0:
            xAxis = 0
        elif xAxis >= 975:
            xAxis = 975
        if yAxis <= 0:
            yAxis = 0
        elif yAxis >= 775:
            yAxis = 775



        # Enemy Movement
        # The enemy's cordinates changing
        for i in range(NumOfEnemies2[0]):
            ExAxis[i] += ExAxis_change[i]
            EyAxis[i] += EyAxis_change[i]


            # The enemies cannot go beyond the screen border
            if ExAxis[i] <= 0:
                ExAxis_change[i] = -XX[i]
                XX[i] = - XX[i]

            elif ExAxis[i] >= 975:
                ExAxis_change[i] = -XX[i]
                XX[i] = - XX[i]

            if EyAxis[i] <= 0:
                EyAxis_change[i] = -YY[i]
                YY[i] = - YY[i]

            elif EyAxis[i] >= 775:
                EyAxis_change[i] = -YY[i]
                YY[i] = - YY[i]


            #If the player collides with an enemy stop the Game loop and go back to the main menu
            collision = isCollision(xAxis, yAxis, ExAxis[i], EyAxis[i])
            if collision:
                running = False


            # Calling the enemy display function
            enemy(ExAxis[i], EyAxis[i], i)


        # Calling the player display function
        player(xAxis, yAxis)


        # Necessary for everything
        pygame.display.update()



# Calling the main menu function
Menu()

# Hope you enjoyed