# AICP TASK 8
#include <iostream>
#include <iomanip>
#include <vector>
#include <algorithm>

using namespace std;

const int NUM_BOATS = 10;
const int OPENING_HOUR = 10;
const int CLOSING_HOUR = 17;
const double HOURLY_RATE = 20.0;
const double HALF_HOUR_RATE = 12.0;

struct Boat {
    int boatNumber;
    double moneyTaken;
    double totalHoursHired;
    int returnHour;
};

void calculateDailyProfit(Boat& boat);
void findNextAvailableBoat(const vector<Boat>& boats);
void calculateTotalProfit(const vector<Boat>& boats);

int main() 
{
    vector<Boat> boats(NUM_BOATS);

    for (int i = 0; i < NUM_BOATS; ++i) 
	{
        boats[i].boatNumber = i + 1;
        boats[i].moneyTaken = 0.0;
        boats[i].totalHoursHired = 0.0;
        boats[i].returnHour = OPENING_HOUR;
    }

    cout << "Task 1 - Calculate the money taken in a day for one boat\n";
    for (int i = 0; i < NUM_BOATS; ++i) 
	{
        calculateDailyProfit(boats[i]);
    }

    cout << "\nTask 2 - Find the next boat available\n";
    findNextAvailableBoat(boats);

    cout << "\nTask 3 - Calculate the money taken for all the boats at the end of the day\n";
    calculateTotalProfit(boats);

    return 0;
}

void calculateDailyProfit(Boat& boat) 
{
    int startHour, endHour;
    double money;
    bool validInput = false;

    cout << "Enter the start hour for boat " << boat.boatNumber << " (10 - 17): ";
    cin >> startHour;

    while (!validInput) 
	{
        if (startHour >= OPENING_HOUR && startHour <= CLOSING_HOUR) 
		{
            validInput = true;
        } else {
            cout << "Invalid start hour. Enter a valid hour between 10 and 17: ";
            cin >> startHour;
        }
    }

    cout << "Enter the end hour for boat " << boat.boatNumber << " (start hour + 0.5 or start hour + 1): ";
    cin >> endHour;

    while (!validInput) 
	{
        if ((endHour == startHour + 0.5 || endHour == startHour + 1) && endHour <= CLOSING_HOUR) 
		{
            validInput = true;
        } else 
		{
            cout << "Invalid end hour. Enter a valid end hour (start hour + 0.5 or start hour + 1, not exceeding 17): ";
            cin >> endHour;
        }
    }

    if (endHour == startHour + 0.5) 
	{
        money = HALF_HOUR_RATE;
    } else 
	{
        money = HOURLY_RATE;
    }

    boat.moneyTaken += money;
    boat.totalHoursHired += (endHour - startHour);
    boat.returnHour = endHour;

    cout << "Boat " << boat.boatNumber << " hired for $" << money << ". Total money taken: $" << boat.moneyTaken << endl;
}

void findNextAvailableBoat(const vector<Boat>& boats) 
{
    int currentHour;

    cout << "Enter the current hour (10 - 17): ";
    cin >> currentHour;

    while (currentHour < OPENING_HOUR || currentHour > CLOSING_HOUR) 
	{
        cout << "Invalid hour. Enter a valid hour between 10 and 17: ";
        cin >> currentHour;
    }

    bool available = false;
    for (const Boat& boat : boats) 
	{
        if (currentHour >= boat.returnHour) 
		{
            available = true;
            cout << "Boat " << boat.boatNumber << " is available for hire.\n";
        }
    }

    if (!available) 
	{
        int earliestReturnHour = CLOSING_HOUR;
        for (const Boat& boat : boats) 
		{
            earliestReturnHour = min(earliestReturnHour, boat.returnHour);
        }
        cout << "No boats available. The earliest available boat will be at " << earliestReturnHour << ":00.\n";
    }
}

void calculateTotalProfit(const vector<Boat>& boats) 
{
    double totalProfit = 0.0;
    double totalHoursHired = 0.0;
    int unusedBoats = 0;
    int mostUsedBoat = 1;

    for (const Boat& boat : boats) 
	{
        totalProfit += boat.moneyTaken;
        totalHoursHired += boat.totalHoursHired;

        if (boat.totalHoursHired == 0) 
		{
            unusedBoats++;
        }

        if (boat.totalHoursHired > boats[mostUsedBoat - 1].totalHoursHired) 
		{
            mostUsedBoat = boat.boatNumber;
        }
    }

    cout << "Total money taken for all boats: $" << totalProfit << endl;
    cout << "Total number of hours boats were hired: " << totalHoursHired << " hours\n";
    cout << "Number of boats not used: " << unusedBoats << "\n";
    cout << "Boat " << mostUsedBoat << " was used the most with " << boats[mostUsedBoat - 1].totalHoursHired << " hours.\n";
}
 
