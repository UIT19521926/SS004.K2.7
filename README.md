#include <iostream>
#include <string>
#include <cstdlib>
#include <windows.h>
#include <conio.h>
using namespace std;

HANDLE console = GetStdHandle(STD_OUTPUT_HANDLE);
COORD CursorPosition;

void gotoXY(int x, int y);

void gotoXY(int x, int y)
{
    CursorPosition.X = x; // Locates column
    CursorPosition.Y = y; // Locates Row
    SetConsoleCursorPosition(console,CursorPosition); // Sets position for next thing to be printed
}
void ClearScreen()
{
   DWORD n;
  DWORD size;
  COORD coord = {0};
  CONSOLE_SCREEN_BUFFER_INFO csbi;
  HANDLE h = GetStdHandle ( STD_OUTPUT_HANDLE );
  GetConsoleScreenBufferInfo ( h, &csbi );
  size = csbi.dwSize.X * csbi.dwSize.Y;
  FillConsoleOutputCharacter ( h, TEXT ( ' ' ), size, coord, &n );
  GetConsoleScreenBufferInfo ( h, &csbi );
  FillConsoleOutputAttribute ( h, csbi.wAttributes, size, coord, &n );
  SetConsoleCursorPosition ( h, coord );
}
struct _POINT{
    int x,y;
};
class SNAKE{
private:
    struct _POINT A[100];
    int Leng;
public:
    SNAKE(){
        Leng = 3;
        A[0].x = 10;A[0].y = 10;
        A[0].x = 11;A[0].y = 10;
        A[0].x = 12;A[0].y = 10;
    }
    void DiChuyen(int Huong){
        for (int i = Leng-1; i>0; i--) A[i] = A[i-1];
        if (Huong ==0)      A[0].x = A[1].x+1;
        else if (Huong ==1) A[0].y = A[1].y+1;
        else if (Huong ==2) A[0].x = A[1].x-1;
        else if (Huong ==3) A[0].y = A[1].y-1;
    }
    void Ve(){
        for (int i=0;i<Leng;i++){
            gotoXY(A[i].x,A[i].y);
            printf("X");
        }

    }
};


int main()
{
    SNAKE s;
    char t;
    int Huong;

    while (1){
        if (kbhit()){
            t = getch();
            if (t=='d') Huong = 0;
            if (t=='x') Huong = 1;
            if (t=='a') Huong = 2;
            if (t=='w') Huong = 3;
        }
        s.Ve();
        Sleep(200);
        s.DiChuyen(Huong);
        ClearScreen();
        //SYSTEM("cls");
        //clrscr();
    }
    return 0;
}
