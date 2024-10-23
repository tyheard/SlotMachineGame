#include <iostream>
#include <cstdlib>
#include <ctime>

class SlotMachine {
public:
    void spinReels();
    void displayReels();
    int checkPayout();
    
private:
    char reels[3];
    char symbols[5] = {'A', 'B', 'C', 'D', 'E'};
};

void SlotMachine::spinReels() {
    // Randomly generate symbols for each reel
    for (int i = 0; i < 3; i++) {
        reels[i] = symbols[rand() % 5];
    }
}

void SlotMachine::displayReels() {
    std::cout << "Reels: ";
    for (int i = 0; i < 3; i++) {
        std::cout << reels[i] << " ";
    }
    std::cout << std::endl;
}

int SlotMachine::checkPayout() {
    // Simple payout logic (e.g., all 3 symbols match)
    if (reels[0] == reels[1] && reels[1] == reels[2]) {
        return 100; // win payout
    }
    return 0; // no payout
}

class Player {
public:
    Player(int startingBalance);
    void play(SlotMachine& machine);
    int getBalance();

private:
    int balance;
};

Player::Player(int startingBalance) : balance(startingBalance) {}

void Player::play(SlotMachine& machine) {
    char choice;
    do {
        machine.spinReels();
        machine.displayReels();
        int payout = machine.checkPayout();
        if (payout > 0) {
            std::cout << "You won " << payout << "!\n";
            balance += payout;
        } else {
            std::cout << "You lost!\n";
            balance -= 10; // Cost per spin
        }

        std::cout << "Balance: " << balance << "\n";
        std::cout << "Spin again? (y/n): ";
        std::cin >> choice;
    } while (choice == 'y' && balance > 0);

    if (balance <= 0) {
        std::cout << "Game over! You ran out of money.\n";
    }
}

int Player::getBalance() {
    return balance;
}

int main() {
    srand(static_cast<unsigned>(time(0))); // Initialize random seed
    SlotMachine machine;
    Player player(100); // Start with a balance of 100

    std::cout << "Welcome to Lucky Streak Showdown Slot Machine!\n";
    player.play(machine);

    return 0;
}

