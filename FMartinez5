#include <iostream>
#include <chrono>
#include <vector>
#include <iomanip>
#include <ctime>
#include <cstdlib>
using namespace std;



#include <windows.h>
#define sleep(n) Sleep(1000 * n)  //Windows Sleep in ms

#define clear() system("cls")



/*Function where input is a vector of int vectors.
 It goes through all elements and prints them out.
 If the element is greater than 64 than the output is a char. Else the output is an int*/

void printBoard(const vector< vector <int> >& board)
{
	for (int i = 0; i < board.size(); i++)
	{
		for (int j = 0; j < board[0].size(); j++)
		{
			if (board[i][j] > 64)
				cout << setw(3) << right << (char)board[i][j];
			else
				cout << setw(3) << right << board[i][j];
		}
		cout << endl;
	}
}

int main()
{
	srand(time(0));
	cout << "In this game, pairs of letters will be hidden on a rectangular grid. \nOnce the grid is established you will be asked to enter the numbers of two slots.\n"
		"If the letters of those slots match, they will stay in view.\n"
		"If not, after 3 seconds the letters will dissapear and be replaced by the numbers.\n"
		"Your job is to find all the pairs\n\n\n\n";
	int rows;
	int cols;
	int product; //rows * cols

	//loop that takes input for rows and cols until product is even and between 16 and 64
	do
	{
		cout << "Size requirement: product of row x col must be between 16 and 64 and be even\n"
			"Enter rows: ";
		cin >> rows;
		cout << "Enter columns: ";
		cin >> cols;
		product = rows * cols;
	} while (product < 16 || product > 64 || product % 2 != 0);

	cout << "\n\nAllowing 30 seconds per pair\n";
	int minutes = (int)(30 * (product * .5) * (0.0166666666666667)); //calculationg the number of minutes for the program to exit
	int seconds = ((30 * product / 2) % 60); //calculationg the number of seconds after the minutes are up till the program exits
	cout << "You will have " << minutes << " minutes and " << seconds << " seconds to find the pairs.\n";
	cout << "Let's Play\n\n";

	//create variable that stores the starting time, and the time when the program will exit when time runs out

	auto start = std::chrono::system_clock::now();
	std::chrono::time_point<std::chrono::system_clock> end;
	chrono::seconds  timelimit(minutes * 60 + seconds);
	end = std::chrono::system_clock::now() + timelimit;


	/*creating a vector of int vectors that will contain elements that number the elements from 1 to
	product. This will be the vector that will be outputed. */
	vector<vector<int>> output;
	int number = 1;
	for (int i = 0; i < rows; i++)
	{
		vector<int> line;
		for (int j = 0; j < cols; j++)
		{
			line.push_back(number);
			cout << setw(3) << right << line[j] << " ";
			number += 1;
		}
		output.push_back(line);
		cout << endl;
	}
	cout << "\n\n" << endl;

	/* creating an int vector that will be the rows for the letters vector which is a vector that contains int vector. This will be the vector that holds the pairs
	I made letters have row x column elements*/
	vector<int> charline(cols);
	vector<vector<int>> letters(rows, charline);
	//these variabkles  are the row and column index I will use to insert a random generated letter into letters
	int xcor;
	int ycor;
	int count = 0;
	int pairs = product / 2;

	/*In these loops I am assiging the pairs a random position in the letters matrix.
	If the spot is empty the letter is assigned in the spot. If the spot already has a letter, then another spot is randomly choosen.
	This continues until all the spots are filled by the pairs */
	for (int i = 65; i < (65 + (pairs)); i++)
	{
		count = 0;
		while (count != 2)
		{

			xcor = rand() % cols;
			ycor = rand() % rows;



			if (letters[ycor][xcor] == '\0')
			{
				letters[ycor][xcor] = i;
				count++;

			}

		}
	}

	// variables that will store the players choices. Match stores the number of matches 
	int firstchoice;
	int secondchoice;
	int match = 0;
	//this loop stops when the number of matches is equal to the number of pairs. 
	while (match < pairs)
	{
		do // this block makes sure that the input is inbound and that the choices inputted are different numbers
		{
			do
			{
				cout << "Enter first slot to view: ";
				cin >> firstchoice;
			} while (firstchoice > product);
			do
			{
				cout << "Enter second choice: ";
				cin >> secondchoice;
			} while (secondchoice > product);
		} while (firstchoice == secondchoice);

		// these variables are the coordinates for the positions chosen by the player
		int xcora, ycora, xcorb, ycorb;

		ycora = (firstchoice - 1) / cols;
		xcora = (firstchoice - 1) % cols;
		ycorb = (secondchoice - 1) / cols;
		xcorb = (secondchoice - 1) % cols;

		//these loops  displays the board with the the numbers, the letters where the players chose for this round, and the pairs that were already matched
		for (int i = 0; i < rows; i++)
		{
			for (int j = 0; j < cols; j++)
			{
				if (output[i][j] > 64)
				{
					cout << setw(3) << right << (char)output[i][j];
					continue;
				}

				if (i == ycora && j == xcora)
					cout << setw(3) << right << (char)letters[ycora][xcora];
				else if (i == ycorb && j == xcorb)
					cout << setw(3) << right << (char)letters[ycorb][xcorb];
				else
					cout << setw(3) << right << output[i][j];
			}
			cout << endl;
		}
		//checks to see if the players matched. If there is a match, match is raised by 1, the numbers are replaced by the number of the char, and match is displayed. If the numbers chosen are already matched or their is no match the proper messages are outputted
		if (output[ycora][xcora] > 64)
			cout << "Previously matched";
		else if (letters[ycora][xcora] == letters[ycorb][xcorb])
		{
			cout << "\aMatch\n\n" << endl;
			match++;
			output[ycora][xcora] = (int)letters[ycora][xcora];
			output[ycorb][xcorb] = (int)letters[ycorb][xcorb];
		}
		else
			cout << "No Match \n\n" << endl;


		sleep(3);
		clear();
		//when time runs out the proper message is displayed, the boards are outputted and the program terminates
		if (chrono::system_clock::now() > end)
		{
			cout << "Time has expired\nYou revealed: \n";
			printBoard(output);

			cout << "\nAll the pairs were at: \n";
			printBoard(letters);
			exit(0);


		}
	}
	// When match is == to pairs you will exit teh while loop and win the game
	auto finish = chrono::system_clock::now();
	auto elapsed_secs = chrono::duration_cast<chrono::seconds>(finish - start).count();

	int mins = elapsed_secs / 60;
	int secs = elapsed_secs - mins * 60;
	cout << "\nAll matched withen " << mins << " minutes and " << secs << " seconds";
	exit(0);



	return 0;
}
