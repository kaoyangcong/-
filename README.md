#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
#include <conio.h> //用于_getch()

const int BOARD_WIDTH = 10;
const int BOARD_HEIGHT = 20;

std::vector<std::vector<int>> board(BOARD_HEIGHT, std::vector<int>(BOARD_WIDTH, 0));

void drawBoard() {
    system("cls"); // 清屏
    for (int i = 0; i < BOARD_HEIGHT; ++i) {
        for (int j = 0; j < BOARD_WIDTH; ++j) {
            if (board[i][j] == 0)
                std::cout << ".";
            else
                std::cout << "#";
        }
        std::cout << std::endl;
    }
}

bool checkCollision(int x, int y) {
    if (x < 0 || x >= BOARD_WIDTH || y < 0 || y >= BOARD_HEIGHT)
        return true;
    return board[y][x] != 0;
}

void placePiece(int x, int y) {
    board[y][x] = 1; // 对于简化的示例，直接将方块放置为1
}

void clearFullRows() {
    for (int i = 0; i < BOARD_HEIGHT; ++i) {
        bool full = true;
        for (int j = 0; j < BOARD_WIDTH; ++j) {
            if (board[i][j] == 0) {
                full = false;
                break;
            }
        }
        if (full) {
            board.erase(board.begin() + i); // 删除满行
            board.insert(board.begin(), std::vector<int>(BOARD_WIDTH, 0)); // 在顶部插入新行
        }
    }
}

int main() {
    srand(static_cast<unsigned>(time(0))); // 初始化随机数生成器
    int pieceX = BOARD_WIDTH / 2;
    int pieceY = 0;

    while (true) {
        drawBoard();
        
        // 获取用户输入（简单的方式）
        if (_kbhit()) {
            char ch = _getch();
            if (ch == 'a') { // 左
                if (!checkCollision(pieceX - 1, pieceY)) 
                    pieceX--;
            } else if (ch == 'd') { // 右
                if (!checkCollision(pieceX + 1, pieceY)) 
                    pieceX++;
            } else if (ch == 's') { // 下
                if (!checkCollision(pieceX, pieceY + 1)) 
                    pieceY++;
            } else if (ch == 'q') { // 退出
                break;
            }
        }

        if (checkCollision(pieceX, pieceY + 1)) { // 检测是否可以下落
            placePiece(pieceX, pieceY);
            clearFullRows();
            pieceX = BOARD_WIDTH / 2; // 重置方块位置
            pieceY = 0; // 重置方块Y位置
        } else {
            pieceY++;
        }
        Sleep(100); // 设置每帧间隔，控制方块下落速度
    }

    return 0;
}
