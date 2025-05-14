using System.Drawing;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;

namespace TicTacToe
{
    public partial class MainWindow : Window
    {
        private char currentPlayer = 'X';
        private char[,] board;
        private int _boardSize = 4; 

        public MainWindow()
        {
            InitializeComponent();
            InitializeGame();
        }

        private void InitializeGame()
        {
            board = new char[_boardSize, _boardSize];
            StatusText.Text = $"Ход: {currentPlayer}";

            
            GameGrid.Children.Clear();
            GameGrid.Rows = _boardSize;
            GameGrid.Columns = _boardSize;

            for (int row = 0; row < _boardSize; row++)
            {
                for (int col = 0; col < _boardSize; col++)
                {
                    Button button = new Button
                    {
                        Tag = $"{row},{col}",
                        FontSize = 32,
                        Margin = new Thickness(2)
                    };
                    button.Click += Cell_Click;
                    GameGrid.Children.Add(button);
                }
            }

            ResetGame(); 
        }

private void Cell_Click(object sender, RoutedEventArgs e)
 {
     Button button = (Button)sender;
     string[] coordinates = button.Tag.ToString().Split(',');
     int row = int.Parse(coordinates[0]);
     int col = int.Parse(coordinates[1]);

     if (board[row, col] != '\0')
         return;

     board[row, col] = currentPlayer;
     button.Content = currentPlayer;

     if (CheckWin(currentPlayer))
     {
         MessageBox.Show($"Игрок {currentPlayer} победил!", "Победа");
         InitializeGame(); // Restart with the current size
         return;
     }

     if (IsBoardFull())
     {
         MessageBox.Show("Ничья!", "Игра окончена");
         InitializeGame(); // Restart with the current size
         return;
     }

     currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
     StatusText.Text = $"Ход: {currentPlayer}";
 }
 private bool CheckWin(char player)
{
   
    for (int row = 0; row < _boardSize; row++)
    {
        bool win = true;
        for (int col = 0; col < _boardSize; col++)
        {
            if (board[row, col] != player)
            {
                win = false;
                break;
            }
        }
        if (win) return true;
    }

    for (int col = 0; col < _boardSize; col++)
    {
        bool win = true;
        for (int row = 0; row < _boardSize; row++)
        {
            if (board[row, col] != player)
            {
                win = false;
                break;
            }
        }
        if (win) return true;
    }

    bool diag1Win = true;
    for (int i = 0; i < _boardSize; i++)
    {
        if (board[i, i] != player)
        {
            diag1Win = false;
            break;
        }
    }
    if (diag1Win) return true;

    bool diag2Win = true;
    for (int i = 0; i < _boardSize; i++)
    {
        if (board[i, _boardSize - 1 - i] != player)
        {
            diag2Win = false;
            break;
        }
    }
    if (diag2Win) return true;

    return false;
}
private bool IsBoardFull()
{
    for (int row = 0; row < _boardSize; row++)
    {
        for (int col = 0; col < _boardSize; col++)
        {
            if (board[row, col] == '\0')
                return false;
        }
    }
    return true;
}

private void ResetGame()
{
    currentPlayer = 'X';
    StatusText.Text = $"Ход: {currentPlayer}";

    for (int row = 0; row < _boardSize; row++)
    {
        for (int col = 0; col < _boardSize; col++)
        {
            board[row, col] = '\0';
        }
    }
    foreach (Button button in GameGrid.Children)
    {
        button.Content = "";
    }
}
        private void ResetButton_Click(object sender, RoutedEventArgs e)
        {
            InitializeGame(); 
        }


        private void SizeButton_Click(object sender, RoutedEventArgs e)
        {
            Button button = (Button)sender;
            if (int.TryParse(button.Tag.ToString(), out int size))
            {
                _boardSize = size;
                InitializeGame();
            }
        }
    }
}

