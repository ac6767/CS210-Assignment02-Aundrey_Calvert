# CS210-Assignment02-Aundrey_Calvert

#include <iostream> 
#include <vector>
using namespace std; 

// ========================================================================================================================
// Search Functions 
// ========================================================================================================================

class SearchFunctions
{
    public: 
        // Iterative Binary Search
        static int IBinarySearch(const vector<int>& numbers, int target, int& comparisons)
        {
            int low = 0;
            int high = numbers.size() - 1;

            while (high >= low)
            {
                int mid = (high + low) / 2;
                comparisons++;

                if (numbers[mid] == target)
                {
                    return mid;
                }
                else if (numbers[mid] < target)
                {
                    low = mid + 1;
                }
                else
                {
                    high = mid - 1;
                }
            }
            return -1; // not found
        }

        // Recursive Binary Search
        // Each call examines one middle element and searches half the range:
        // T(n) = T(n/2) + O(1). Repeatedly halving n takes O(log n) calls.
        static int RBinarySearch(const vector<int>& numbers, int low, int high, int target, int& comparisons)
        {
            if (low > high) 
            {
                return -1;
            }
            
            int mid = (low + high) / 2;
            comparisons++;

            if (numbers[mid] == target)
            {
                return mid;
            }
            else if (numbers[mid] < target)
            {
                return RBinarySearch(numbers, mid + 1, high, target, comparisons);
            }
            else
            {
                return RBinarySearch(numbers, low, mid - 1, target, comparisons);
            }
        }

        // Linear Search
        static int LinearSearch(const vector<int>& numbers, int target, int& comparisons)
        {
            int numbersSize = numbers.size(); 

            for (int i = 0; i < numbersSize; i++)
            {
                comparisons++;
                if (numbers[i] == target)
                {
                    return i;
                }
            }

            return -1;
        }
};

// ========================================================================================================================
// Running the Test 
// ========================================================================================================================

class Test
{
    private: 
        static void runTest(const vector<int>& numbers, int target)
        {
            const string bold = "\033[1m";
            const string cyan = "\033[36m";
            const string underline = "\033[4m";
            const string reset = "\033[0m";

            int iterativeComparisons = 0;
            int recursiveComparisons = 0;
            int linearComparisons = 0;

            int IBinaryResult = SearchFunctions::IBinarySearch(numbers, target, iterativeComparisons);
            int RBinaryResult = SearchFunctions::RBinarySearch(numbers, 0, static_cast<int>(numbers.size()) - 1, target, recursiveComparisons);
            int LinearResult = SearchFunctions::LinearSearch(numbers, target, linearComparisons);
            
            // Boolean Valid/Invalid Expressions
            bool IBinaryValid = IBinaryResult != -1;
            bool RBinaryValid = RBinaryResult != -1;
            bool LinearValid = LinearResult != -1;

            // Displays 
            cout << cyan << bold << "RESULTS\n" << reset << "Iterative Target @ Index: " << cyan << IBinaryResult << reset << endl;
            cout << "Recursive Target @ Index: " << cyan << RBinaryResult << reset << endl;
            cout << "Linear Target @ Index: " << cyan << LinearResult << reset << endl;

            if (IBinaryValid && RBinaryValid && LinearValid)
            {
                cout << underline << "All searches found the target!\n" << reset << endl;
            }
            else
            {
                cout << underline << "No searches found the target!\n" << reset << endl;
            }

            // Comparison 
            cout << cyan << bold << "COMPARISONS\n" << reset << "Iterative Binary Comparisons: " << cyan << iterativeComparisons << reset << endl;
            cout << "Recursive Binary Comparisons: " << cyan << recursiveComparisons << reset << endl;
            cout << "Linear Comparisons: " << cyan << linearComparisons << reset << endl;
            if (IBinaryResult == RBinaryResult && RBinaryResult == LinearResult)
            {
                cout << underline << "All results match.\n" << reset << "\n=========================================================\n" << endl;
            }
        }

    public:
        Test()
        {
            runTest({2, 8, 15, 24, 31, 40}, 2);
            runTest({3, 9, 14, 18, 27, 35, 42}, 18);
            runTest({10, 25, 47, 63, 81, 99}, 99);
            runTest({4, 12, 19, 28, 33, 50}, -5);
            runTest({5, 12, 21, 27, 36, 44, 58}, 30);

        }
};

// ========================================================================================================================
// Main
// ========================================================================================================================

int main() 
{
    Test SearchTester;
    return 0;
}
