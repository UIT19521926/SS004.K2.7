#include <iostream>
#include <time.h>
#include <conio.h>
#include <windows.h>
using namespace std;

bool gameover;
const int width = 60;
const int height = 30;
int snakex, snakey, fruitx, fruity,scores;
int tailX[100];
int tailY[100];
static int snaketail = 0;
enum edirection {STOP, LEFT, RIGHT, UP, DOWN};
edirection dir;
void SetUp();
void Draw();
void Input();
void Logic();

int main()
{
	SetUp();

	while (!gameover)
	{
		Draw();
		Input();
		Logic();
		Sleep(10);
	}
	return 1;
}
void SetUp()
{
	srand(time(NULL));
	gameover = false;
	dir = STOP;
	snakex = width / 2;
	snakey = height / 2;
	do
	{
		fruitx = rand() % (width - 2) + 1;
		fruity = rand() % (height - 2) + 1;
	} while (fruitx == snakex && fruity == snakey);
	scores = 0;

}
void Draw()
{
	system("cls");
	for (int i = 0; i < width  ; i++)
		cout << "x";
	cout << endl;
	for (int i = 1; i < height -1; i++)
	{
		for (int j = 0; j < width; j++)
		{
			if (j == 0)
				cout << "x";
			else if (j == width - 1)
				cout << "x" << endl;
			else if (j == snakex && i == snakey)
				cout << "0";
			else if (j == fruitx && i == fruity)
				cout << "*";
			else
			{
				bool print = false;
				for (int k = 0; k < snaketail; k++)
				{
					if (j == tailX[k] && i == tailY[k])
					{
						cout << "o";
						print = true;
					}
				}
				if (!print)
					cout << " ";
			}
		}
	}
	for (int i = 0; i < width ; i++)
		cout << "x";
	}
void Input()
{
	if (_kbhit())
	{
		switch (_getch())
		{
		case 'a':
			dir = LEFT;
			break;
		case 'd':
			dir = RIGHT;
			break;
		case 'w':
			dir = UP;
			break;
		case 's':
			dir = DOWN;
			break;
		default:
			break;
		}

	}
}
void Logic()
{
	for (int i = snaketail - 1; i > 0; i--)
	{
		tailX[i] = tailX[i - 1];
		tailY[i] = tailY[i - 1];
	}
	tailX[0] = snakex;
	tailY[0] = snakey;
	switch (dir)
	{
	case LEFT:
		snakex--;
		break;
	case RIGHT:
		snakex++;
		break;
	case UP:
	{
		snakey--;
		break;
	}
	case DOWN:
		snakey++;
		break;
	default:
		break;
	}
	if (snakex >= width || snakex < 0 || snakey < 0 || snakey >= height)
		gameover = true;
	if (snakex == fruitx && snakey == fruity)
	{
		scores += 10;
		snaketail += 1;
		fruitx = rand() % (width - 2) + 1;
		fruity = rand() % (height - 2) + 1;
	}

}
