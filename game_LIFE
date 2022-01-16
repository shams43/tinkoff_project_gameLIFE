import pygame
import numpy as np

WHITE = pygame.Color('white')
BLACK = pygame.Color('black')
GREEN = pygame.Color('green')


def alive_cells(board, x, y):
    c = 0
    for i in range(3):
        for j in range(3):
            if i == 1 and j == 1:
                continue
            else:
                if board[(x - 1 + i + board.shape[0]) % board.shape[0], (y - 1 + j + board.shape[1]) % board.shape[1]]:
                    c += 1
    return c


class Board:
    def __init__(self, width, height):
        self.width = width
        self.height = height
        self.board = np.zeros((width, height), dtype=np.int32)

        self.left = 10
        self.top = 10
        self.cell_size = 30

    def set_view(self, left, top, cell_size):
        self.left = left
        self.top = top
        self.cell_size = cell_size

    def render(self, surface):
        for i in range(self.height):
            for j in range(self.width):
                if self.board[i, j] == 0:
                    pygame.draw.rect(surface, WHITE, (
                        self.left + j * self.cell_size, self.top + i * self.cell_size, self.cell_size, self.cell_size),
                                     width=1)
                if self.board[i, j] == 1:
                    pygame.draw.rect(surface, GREEN, (
                        self.left + j * self.cell_size + 1, self.top + 1 + i * self.cell_size, self.cell_size - 2,
                        self.cell_size - 2))

    def get_cell(self, mouse_pos: (int, int)):
        for i in range(self.height):
            for j in range(self.width):
                if self.top + i * self.cell_size <= mouse_pos[1] <= self.top + self.cell_size * (i + 1):
                    if self.left + j * self.cell_size <= mouse_pos[0] <= self.left + self.cell_size * (j + 1):
                        return j, i

    def on_click(self, cell_coords: (int, int)):
        if cell_coords:
            y = cell_coords[0]
            x = cell_coords[1]
            self.board[x, y] = (self.board[x, y] + 1) % 2

    def get_click(self, mouse_pos: (int, int)):
        cell = self.get_cell(mouse_pos)
        self.on_click(cell)


class Life(Board):
    def next_move(self):
        board = self.board.copy()
        for i in range(self.board.shape[0]):
            for j in range(self.board.shape[1]):
                if board[i, j] == 0 and alive_cells(board, i, j) == 3:
                    self.board[i, j] = 1
                if board[i, j] == 1 and not 2 <= alive_cells(board, i, j) <= 3:
                    self.board[i, j] = 0


if __name__ == '__main__':
    screen = pygame.display.set_mode((800, 800))
    running = True
    life = Life(30, 30)
    life.set_view(20, 20, 20)
    play = False
    fps = 10
    clock = pygame.time.Clock()
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            if event.type == pygame.MOUSEBUTTONDOWN:
                if event.dict['button'] == 3:
                    if play:
                        play = False
                    else:
                        play = True
                if not play and event.dict['button'] == 1:
                    life.get_click(event.pos)
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    if play:
                        play = False
                    else:
                        play = True

        screen.fill((0, 0, 0))
        if play:
            life.next_move()
        clock.tick(fps)
        life.render(screen)
        pygame.display.flip()
