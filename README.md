Tic-Tac-Toe-in-c-
=================
// tictac.cpp : Defines the entry point for the console application.
//

//I tried it to make it general, n*n matrix. So ticTacToeSize = n*n
//if n < 3 the game cannot be played.
//Also the minimum value to display in the tic tac toe game is ticTacToeMinValue = 1

//The game is for 2 players only and will be played by one-one moves. 
//If the user press characters other than input displayed continuously more than 5 times game will be over.
//Checking for winninability : it is checked in horizontal, vertical and in cross(identity matrix) directions.
//                             If continuous positions are found for any player in selected direction than that player //                             wins. If its not found than match is tied.
                                 
                                 
#include "stdafx.h"
#include <string>
#include <iostream>
#include <vector>
#include <algorithm>
#include <math.h>
#include <sstream>

#define square(x)x*x
const int ticTacToeMinValue= 1;
const int maxRow = 3;
const int ticTacToeSize = square(maxRow);//9
using namespace std;

bool checkForWinningPosition(const std::vector<int>& vtUser)
{
	//1  2  3
	//4  5  6
	//7  8  9

	if(vtUser.size() < 3)return false;
	//check for horizontal match
	//eg : {1,2,3}, {4,5,6}, {7,8,9}
	for(int i = 0; i < maxRow; ++i)
	{
		int j = 1 + i*maxRow;//element at first column
		for(int k = j; k < j+maxRow; ++k)//check each element of selected row
		{
			if(std::find(vtUser.begin(), vtUser.end(), k) == vtUser.end())
			{
				break;
			}
			if(k == j+maxRow - 1)return true;//it reaches here only of each element of selected row is found
		}
	}

	//check for vertical match
	//eg : {1,4,7}, {2,5,8}, {1,3,9}
	for(int i = 1; i <= maxRow; ++i)
	{
		for(int j = 0; j < maxRow; ++j)
		{
			int k = i + j*maxRow;
			if(std::find(vtUser.begin(), vtUser.end(), k) == vtUser.end())
			{
				break;
			}
			if(j == maxRow - 1)return true;//it reaches here only of each element of selected row is found
		}
	}

	//check for identity matrix match
	//eg : {1,5,9}
	for(int i = 0; i < maxRow; ++i)
	{
		int j = (i + 1) + i*maxRow;
		if(std::find(vtUser.begin(), vtUser.end(), j) == vtUser.end())
		{
			break;
		}
		if(i == maxRow - 1)return true;
	}

	//check for other identity matrix match
	//eg : {3,5,7}
	for(int i = 0; i < maxRow; ++i)
	{
		int j = maxRow*(i+1) - i;
		if(std::find(vtUser.begin(), vtUser.end(), j) == vtUser.end())
		{
			break;
		}
		if(i == maxRow - 1)return true;
	}
	return false;
}

void printTicTacToe(const std::vector<int>& vtUser1, const std::vector<int>& vtUser2)
{
	std::vector<string> vtTicTacToe;
	for(int i = ticTacToeMinValue; i <= ticTacToeSize; ++i)
	{
		if(find(vtUser1.begin(), vtUser1.end(), i )!= vtUser1.end())
		{
			vtTicTacToe.push_back("X");
		}
		else if(find(vtUser2.begin(), vtUser2.end(), i )!= vtUser2.end())
		{
			vtTicTacToe.push_back("Y");
		}
		else
		{
			stringstream ss;
			ss << i;
			vtTicTacToe.push_back(ss.str());
		}	
	}
	int count = 0;
	for(int i = 0; i < maxRow; ++i)
	{
		for(int j = 0; j <= maxRow; ++j)
			cout<<"----";
		cout<<endl;
		for(int j = 0; j < maxRow; ++j)
			cout<<"| "<<vtTicTacToe[count++]<<"  ";
		cout<<"|"<<endl;
		for(int j = 0; j < maxRow; ++j)
			cout<<"|    ";
		cout<<"|"<<endl;
	}
	for(int i = 0; i <= maxRow; ++i)
			cout<<"----";
	cout<<endl;
}

int _tmain(int argc, _TCHAR* argv[])
{
	if(maxRow < 3)
	{
		cout<<"You cannot play this game as minimum row size for tic tac toe game is 3"<<endl;
		system("PAUSE");
		return 0;
	}
	//get all the elements
	std::vector<int> remInputs;
	for(int i = ticTacToeMinValue; i <= ticTacToeSize; ++i)
		remInputs.push_back(i);

	std::vector<int> vtUser1, vtUser2;
	printTicTacToe(vtUser1, vtUser2);

	cout<<"Lets play tic tac toe game. Its a two player game. Player 1 operation will be marked X. Player 2 operation will be marked Y."<<endl;

	bool hasUser1Won = false, hasUser2Won = false;
	while(1)
	{
		int maxInvalidIputCount = 0;
		while(1)
		{
			cout<<endl;
			cout<<"Player 1, please pick your position from the following"<<endl;
			for(int i = 0; i < (int)remInputs.size(); ++i)
				cout<<remInputs[i]<<", ";
			cout<<endl;

			int user1Pos;
			cin>>user1Pos;
			if(user1Pos < ticTacToeMinValue || user1Pos > ticTacToeSize || find(remInputs.begin(), remInputs.end(), user1Pos) == remInputs.end()) 
			{
				if(maxInvalidIputCount > 5)
				{
					cout<<"GAME OVER, you entered more than 5 wrong input"<<endl;
					system("PAUSE");
					return 0;
				}
				cin.clear();
				cin.ignore(10000, '\n'); 
				++maxInvalidIputCount;
			}
			else
			{
				vtUser1.push_back(user1Pos);
				remInputs.erase(find(remInputs.begin(), remInputs.end(), user1Pos));
				break;
			}
		}
		printTicTacToe(vtUser1, vtUser2);
		if(hasUser1Won = checkForWinningPosition(vtUser1))break;
		if(remInputs.size() == 0)break;

		maxInvalidIputCount = 0;
		while(1)
		{
			cout<<endl;
			cout<<"Player 2, please pick your position from the following"<<endl;
			for(int i = 0; i < (int)remInputs.size(); ++i)
				cout<<remInputs[i]<<", ";
			cout<<endl;

			int user2Pos;
			cin>>user2Pos;
			if(user2Pos < ticTacToeMinValue || user2Pos> ticTacToeSize || find(remInputs.begin(), remInputs.end(), user2Pos) == remInputs.end()) 
			{
				if(maxInvalidIputCount > 5)
				{
					cout<<"GAME OVER, you entered more than 5 wrong input"<<endl;
					system("PAUSE");
					return 0;
				}
				cin.clear();
				cin.ignore(10000, '\n'); 
				++maxInvalidIputCount;
			}
			else
			{
				vtUser2.push_back(user2Pos);
				remInputs.erase(find(remInputs.begin(), remInputs.end(), user2Pos));
				break;
			}
		}
		printTicTacToe(vtUser1, vtUser2);
		if(hasUser2Won = checkForWinningPosition(vtUser2))break;
		if(remInputs.size() == 0)break;
	}
	if(hasUser1Won)cout<<"Player 1 won"<<endl;
	else if(hasUser2Won)cout<<"Player 2 won"<<endl;
	else cout<<"Match tied"<<endl;
	system("PAUSE");
	return 0;
}
