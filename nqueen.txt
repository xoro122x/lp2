#include <iostream>
using namespace std;

class Nqueen
{

private:
	int n;
	int board[10][10];
	int pass;

public:
	Nqueen(int val)
	{
		n = val;
		for (int i = 0; i < n; i++)
		{
			for (int j = 0; j < n; j++)
			{
				board[i][j] = 0;
			}
		}
		pass = 0;
	}
	bool isSafe(int row, int col)
	{
		int i, j;

		for (i = 0; i < col; i++)
			if (board[row][i])
				return false;	

		for (i = row, j = col; i >= 0 && j >= 0; i--, j--)
			if (board[i][j])
				return false;

		for (i = row, j = col; j >= 0 && i < n; i++, j--)
			if (board[i][j])
				return false;

		return true;
	}
	void printSolution()
	{
		for (int i = 0; i < n; i++)
		{
			for (int j = 0; j < n; j++)
			{
				if (board[i][j])
					cout << "Q ";
				else
					cout << ". ";
			}
			cout << endl;
		}

		cout << "the no. of passes are:" << pass << endl;
	}

	bool solveNQUtil(int col)
	{

		if (col >= n)
			return true;

		for (int i = 0; i < n; i++)
		{

			if (isSafe(i, col))
			{

				board[i][col] = 1;

				if (solveNQUtil(col + 1))
					return true;

				board[i][col] = 0;
			}
		printSolution();
		pass++;
		}
		return false;
	}
};

int main()
{

	int n;

	cout << "Enter the no. of queens: ";
	cin >> n;
	Nqueen queen(n);
	queen.solveNQUtil(0);
	queen.printSolution();

	return 0;
}
