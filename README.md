# towergame
## main
```
#include "Random.h"

int main()
{
	srand((unsigned)time(NULL));

	SetConsoleView();

	Random* rando = new Random();

	while (true)
	{
		int i = rand() % 15;

		gotoXY(0, 0);
		setColor(i);
		rando->drawMap();
		gotoXY(49, 19);

		rando->keyInput();
	}

	return 0;
}
```
### console.h
```
#pragma once
#include <iostream>
#include <conio.h>
#include <Windows.h> 
#include <time.h>

using namespace std;

extern void gotoXY(int x, int y);
extern void setColor(int color);
extern void clrscr();
extern void beep(int tone, int delay);
extern int GetKeyDown();
extern void SetConsoleView();
```
#### console.cpp
```
#include "Console.h"

void gotoXY(int x, int y)
{
    HANDLE hOut;
    COORD Cur;
    hOut = GetStdHandle(STD_OUTPUT_HANDLE);
    Cur.X = x;
    Cur.Y = y;
    SetConsoleCursorPosition(hOut, Cur);
}
void setColor(int color)
{
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), color);
}
void clrscr()
{
    system("cls");
}
void beep(int tone, int delay)
{
    Beep(tone, delay);
}

int GetKeyDown()
{
    if (_kbhit() != 0)
    {
        return _getch();
    }

    return 0;
}

void SetConsoleView()
{
    system("mode con:cols=50 lines=20");
    system("title 0.0000001의 탑");
}
```
##### random.h
```
#pragma once
#include "Console.h"

#define LEFT 75
#define RIGHT 77

class Random
{
private:
	int num;
	int up;
	int down;

	int result;
	int ch;
public:
	int getGrade();
	void keyInput();
	void drawMap();
};
```
###### random.cpp
```
#include "Random.h"

int Random::getGrade()
{
    num = rand() % 100;

    // down 퍼센트
    if (num < 50)
    {
        return 2;
    }
    else //if (num < 99)
    {
        return 1;
    }
    //else
    //{
    //    return 3;
    //}
}

void Random::keyInput()
{
	place();

	result = up - down;

	//0이하로는 X
	if (result < 0)
	{
		result = 1;
		up = 0;
		down = 0;
	}
	else if (result == 99)
	{
		return;
	}

	//입력
	int ch = GetKeyDown();

	if (ch == 0xE0 || ch == 0)
	{
		ch = _getch();
		switch (ch)
		{
		case LEFT:
		case RIGHT:
			getGrade();
			break;
		}

		if (getGrade() == 1)
		{
			up++;
		}
		else //if (getGrade() == 2)
		{
			down++;
		}
		//else if (getGrade() == 3)
		//{
		//	down = down + 10;
		//}
	}
	else //나가기
	{
		ch = tolower(ch);

		if (ch == 'q')
		{
			clrscr();
			exit(0);
		}
	}
	
	gotoXY(22, 11);
	cout << result + 1 << "층" << endl;
}

void Random::drawMap()
{
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "|               |" << "             " << "|               |" << endl;
	cout << "        ◁       " << "             " << "         ▷      " << endl;
	int i = rand() % 255;
	gotoXY(22, 11);
	setColor(i);
	cout << result + 1 << "층" << endl;
}
```
