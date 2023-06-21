贪吃蛇简易代码


import pygame
import random

# 游戏窗口的宽度和高度
window_width = 640
window_height = 480

# 蛇身每个方块的大小
block_size = 20

# 初始化Pygame
pygame.init()

# 创建游戏窗口
window = pygame.display.set_mode((window_width, window_height))
pygame.display.set_caption('贪吃蛇')

clock = pygame.time.Clock()

# 定义颜色
black = pygame.Color(0, 0, 0)
white = pygame.Color(255, 255, 255)
red = pygame.Color(255, 0, 0)

# 蛇的初始位置和移动方向
snake_head = [window_width / 2, window_height / 2]
snake_body = [[window_width / 2, window_height / 2],
              [window_width / 2 - block_size, window_height / 2],
              [window_width / 2 - 2 * block_size, window_height / 2]]
direction = 'RIGHT'

# 食物的初始位置
food = [random.randrange(1, (window_width // block_size)) * block_size,
        random.randrange(1, (window_height // block_size)) * block_size]

# 游戏结束标志
game_over = False

# 游戏主循环
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                direction = 'RIGHT'
            elif event.key == pygame.K_LEFT:
                direction = 'LEFT'
            elif event.key == pygame.K_UP:
                direction = 'UP'
            elif event.key == pygame.K_DOWN:
                direction = 'DOWN'

    # 移动蛇头位置
    if direction == 'RIGHT':
        snake_head[0] += block_size
    elif direction == 'LEFT':
        snake_head[0] -= block_size
    elif direction == 'UP':
        snake_head[1] -= block_size
    elif direction == 'DOWN':
        snake_head[1] += block_size

    # 检查蛇头是否与食物重合
    if snake_head[0] == food[0] and snake_head[1] == food[1]:
        # 生成新的食物位置
        food = [random.randrange(1, (window_width // block_size)) * block_size,
                random.randrange(1, (window_height // block_size)) * block_size]
    else:
        # 移除蛇尾
        snake_body.pop()

    # 检查蛇头是否与蛇身重合
    for block in snake_body:
        if snake_head[0] == block[0] and snake_head[1] == block[1]:
            game_over = True

    # 添加新的蛇头
    snake_body.insert(0, list(snake_head))

    # 清空窗口
    window.fill(black)

    # 绘制蛇身
    for block in snake_body:
        pygame.draw.rect(window, white, pygame.Rect(block[0], block[1], block_size, block_size))

    # 绘制食物
