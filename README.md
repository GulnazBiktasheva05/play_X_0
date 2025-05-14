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
