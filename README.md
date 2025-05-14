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
        private int _boardSize = 4; // Default board size

        public MainWindow()
        {
            InitializeComponent();
            InitializeGame();
        }

        private void InitializeGame()
        {
            board = new char[_boardSize, _boardSize];
            StatusText.Text = $"Ход: {currentPlayer}";

            // Remove existing buttons and create them based on the _boardSize
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

            ResetGame(); // Reset the game state after creating the buttons
        }
