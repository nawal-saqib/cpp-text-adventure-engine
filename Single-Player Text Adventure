#include <iostream>
using namespace std;
#include <cstdlib> // for rand()
#include <ctime>   // for time()
#include <vector>
#include <fstream>

// Function declarations
void show_menu();
void start_game();
void load_game();
void save_game(int playerHP, int playerX, int playerY, vector<string>& inventory);
void battle(vector<string>& inventory, int& playerHP);

int main() {
    int choice = 0;
    bool running = true;
    srand(time(0)); // seed random

    while (running) {
        show_menu();
        if (!(cin >> choice)) {
        cin.clear();
        cin.ignore(1000, '\n');
        cout << "Invalid input. Enter a number.\n";
        continue;
    }
        
        switch (choice) {
            case 1:
                start_game();
                break;
            case 2:
                load_game();
                break;
            case 3:
                cout << "Exiting game...\n";
                running = false;
                break;
            default:
                cout << "Invalid choice. Try again.\n\n";
                break;
        }
    }

    return 0;
}

// Function definitions
void show_menu() {
    cout << "\n===== RPG Game =====\n";
    cout << "1. Start New Game\n";
    cout << "2. Load Game\n";
    cout << "3. Exit\n";
    cout << "Enter your choice: ";
}

void start_game() {
    vector<string> inventory;
    inventory.push_back("Potion");
    int playerHP = 100;

    cout << "Starting new game...\n";
    cout << "HP: " << playerHP << " | Potions: " << inventory.size() << endl;

    vector<vector<char>> map = {
        {'.', '.', '.', 'E', '.'},
        {'.', 'T', '.', '.', '.'},
        {'P', '.', '.', 'E', '.'},
        {'.', '.', '.', '.', '.'},
        {'.', '.', 'E', '.', '.'}
    };

    int playerX = 2;
    int playerY = 0;

    char move;

    while (true) {
        cout << "\n--- Map ---\n";
        for (int i = 0; i < map.size(); i++) {
            for (int j = 0; j < map[i].size(); j++) {
                cout << map[i][j] << " ";
            }
            cout << endl;
        }

        cout << "\nMove (W/A/S/D, V to save, Q to quit): ";
        cin >> move;

        map[playerX][playerY] = '.';

        // Movement
        if (move == 'w' || move == 'W') playerX--;
        else if (move == 's' || move == 'S') playerX++;
        else if (move == 'a' || move == 'A') playerY--;
        else if (move == 'd' || move == 'D') playerY++;
        else if (move == 'v' || move == 'V') {
            save_game(playerHP, playerX, playerY, inventory);
            map[playerX][playerY] = 'P';
            continue;
        }
        else if (move == 'q' || move == 'Q') break;
        else {
            cout << "Invalid move!\n";
        }

        // Boundary check
        if (playerX < 0) playerX = 0;
        if (playerX >= map.size()) playerX = map.size() - 1;
        if (playerY < 0) playerY = 0;
        if (playerY >= map[0].size()) playerY = map[0].size() - 1;

        // Check tile
        if (map[playerX][playerY] == 'E') {
            cout << "Enemy encountered!\n";
            battle(inventory, playerHP);
            map[playerX][playerY] = '.'; 
        }

        if (map[playerX][playerY] == 'T') {
            inventory.push_back("Potion");
            cout << "You found a potion!\n";
            map[playerX][playerY] = '.'; 
        }

        map[playerX][playerY] = 'P';
    }
}

void save_game(int playerHP, int playerX, int playerY, vector<string>& inventory) {
    ofstream file("savegame.txt");

    if (file.is_open()) {
        file << playerHP << endl;
        file << playerX << endl;
        file << playerY << endl;
        file << inventory.size() << endl;

        file.close();
        cout << "Game saved successfully!\n";
    } else {
        cout << "Error saving game!\n";
    }
}

void load_game() {
    ifstream file("savegame.txt");

    if (!file.is_open()) {
        cout << "No saved game found!\n";
        return;
    }

    int playerHP, playerX, playerY, potionCount;

    file >> playerHP;
    file >> playerX;
    file >> playerY;
    file >> potionCount;

    file.close();

    vector<string> inventory;
    for (int i = 0; i < potionCount; i++) {
        inventory.push_back("Potion");
    }

    cout << "Game loaded successfully!\n";
    cout << "HP: " << playerHP << endl;
    cout << "Position: (" << playerX << ", " << playerY << ")" << endl;
    cout << "Potions: " << inventory.size() << endl;

}

void battle(vector<string>& inventory, int& playerHP) {
    int enemyHP = 80;
    int playerAttack = 20;
    int enemyAttack = 15;
    int choice;

    cout << "\n--- Battle Start! ---\n";

    while (playerHP > 0 && enemyHP > 0) {
        cout << "\nYour HP: " << playerHP << " | Enemy HP: " << enemyHP << endl;
        cout << "Potions: " << inventory.size() << endl;
        bool isDefending = false;

        while (true) {
        cout << "1. Attack\n";
        cout << "2. Defend\n";
        cout << "3. Use Potion\n";
        cout << "Choose action: ";
        if (!(cin >> choice)) {
            cin.clear();
            cin.ignore(1000, '\n');
            cout << "Invalid input! Try again.\n";
            continue;
    }

        // Player turn
        if (choice == 1) {
            cout << "You attack the enemy!\n";
            enemyHP -= playerAttack;
            if (enemyHP < 0) enemyHP = 0;
            break;
        }

        else if (choice == 2) {
            cout << "You brace for the enemy attack!\n";
            isDefending = true;
            break;
        }

        else if (choice == 3) {
            if (!inventory.empty()) {
            cout << "You used a potion! +20 HP\n";
            playerHP += 20;
            inventory.pop_back();
            cout << "Potions left: " << inventory.size() << endl;

            if (playerHP > 100) playerHP = 100; // cap HP
            break;
            } else {
            cout << "No potions left!\n";
        }
    }

        else {
            cout << "Invalid choice! Try again.\n";
        }
        }
        // Check if enemy defeated
        if (enemyHP <= 0) {
            cout << "Enemy defeated!\n";
            break;
        }

        // Enemy turn (random)
        int enemyMove = rand() % 2;

        if (enemyMove == 0) {
            cout << "Enemy attacks!\n";
            if (isDefending) {
                cout << "You reduced the damage!\n";
                playerHP -= enemyAttack / 2;
            } else {
                playerHP -= enemyAttack;
            }
            if (playerHP < 0) playerHP = 0;
        } else {
            cout << "Enemy hesitates...\n";
        }

        // Check if player defeated
        if (playerHP <= 0) {
            cout << "You were defeated...\n";
            break;
        }
    }

    cout << "--- Battle End ---\n";
}
