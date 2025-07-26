#Final-Project
Project: Categorizing

Code:
```cpp
  #include <iostream>
using namespace std;

// ---------- Node ------------
struct Node {
    int data;
    Node* next;
};

// --------- Linkedlist Class ---------
class Linkedlist {
private:
    Node* head;

public:
    Linkedlist() {
        head = nullptr;
    }

    void insertNode(int val) {
        Node* newNode = new Node();
        newNode->data = val;
        newNode->next = nullptr;

        if (head == nullptr) {
            head = newNode;
        } else {
            Node* temp = head;
            while (temp->next != nullptr) {
                temp = temp->next;
            }
            temp->next = newNode;
        }

        cout << "Inserted: " << val << endl;
    }

    void deleteNode(int val) {
        if (head == nullptr) {
            cout << "List is empty." << endl;
            return;
        }

        if (head->data == val) {
            Node* temp = head;
            head = head->next;
            delete temp;
            cout << "Deleted." << endl;
            return;
        }

        Node* curr = head;
        while (curr->next && curr->next->data != val)
            curr = curr->next;

        if (curr->next == nullptr) {
            cout << "Value not found." << endl;
            return;
        }

        Node* temp = curr->next;
        curr->next = temp->next;
        delete temp;
        cout << "Deleted." << endl;
    }

    void updateNode(int oldVal, int newVal) {
        Node* temp = head;
        while (temp) {
            if (temp->data == oldVal) {
                temp->data = newVal;
                cout << "Updated." << endl;
                return;
            }
            temp = temp->next;
        }
        cout << "Value not found." << endl;
    }

    void displayList() {
        if (head == nullptr) {
            cout << "List is empty." << endl;
            return;
        }

        Node* temp = head;
        cout << "Linked List: ";
        while (temp) {
            cout << temp->data << " ";
            temp = temp->next;
        }
        cout << endl;
    }
};

// ---------- Sorting ----------
void merge(int arr[], int l, int m, int r) {
    int n1 = m - l + 1, n2 = r - m;
    int* L = new int[n1];
    int* R = new int[n2];

    for (int i = 0; i < n1; i++) L[i] = arr[l + i];
    for (int i = 0; i < n2; i++) R[i] = arr[m + 1 + i];

    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2)
        arr[k++] = (L[i] <= R[j]) ? L[i++] : R[j++];

    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];

    delete[] L;
    delete[] R;
}

void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}

int partition(int arr[], int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++)
        if (arr[j] < pivot)
            swap(arr[++i], arr[j]);
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// --------- Binary Search ---------
int binarySearch(int arr[], int size, int key) {
    int low = 0, high = size - 1;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (arr[mid] == key) return mid;
        else if (arr[mid] < key) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}

int main() {
    Linkedlist list;
    int choice;

    do {
        cout << "=== Categorizing App ===" << endl;
        cout << "1. Linked List Operations" << endl;
        cout << "2. Sort Array (Merge or Quick)" << endl;
        cout << "3. Binary Search" << endl;
        cout << "4. Exit" << endl;
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
        case 1: {
            int subChoice;
            do {
                cout << "-- Linked List Menu --" << endl;
                cout << "1. Insert" << endl;
                cout << "2. Delete" << endl;
                cout << "3. Update" << endl;
                cout << "4. Display" << endl;
                cout << "5. Back" << endl;
                cout << "Choice: ";
                cin >> subChoice;

                if (subChoice == 1) {
                    int val;
                    cout << "Enter value to insert: ";
                    cin >> val;
                    list.insertNode(val);
                } else if (subChoice == 2) {
                    int val;
                    cout << "Enter value to delete: ";
                    cin >> val;
                    list.deleteNode(val);
                } else if (subChoice == 3) {
                    int oldVal, newVal;
                    cout << "Enter old value: ";
                    cin >> oldVal;
                    cout << "Enter new value: ";
                    cin >> newVal;
                    list.updateNode(oldVal, newVal);
                } else if (subChoice == 4) {
                    list.displayList();
                } else if (subChoice != 5) {
                    cout << "Invalid sub-choice!" << endl;
                }
            } while (subChoice != 5);
            break;
        }

        case 2: {
            int n;
            cout << "Enter number of elements: ";
            cin >> n;
            int* arr = new int[n];
            cout << "Enter elements: ";
            for (int i = 0; i < n; i++) cin >> arr[i];

            int sortType;
            cout << "1. Merge Sort" << endl;
            cout << "2. Quick Sort" << endl;
            cout << "Choose sorting method: ";
            cin >> sortType;

            if (sortType == 1)
                mergeSort(arr, 0, n - 1);
            else if (sortType == 2)
                quickSort(arr, 0, n - 1);
            else
                cout << "Invalid sort type!" << endl;

            cout << "Sorted Array: ";
            for (int i = 0; i < n; i++) cout << arr[i] << " ";
            cout << endl;

            delete[] arr;
            break;
        }

        case 3: {
            int n;
            cout << "Enter number of elements: ";
            cin >> n;
            int* arr = new int[n];
            cout << "Enter sorted elements: ";
            for (int i = 0; i < n; i++) cin >> arr[i];

            int key;
            cout << "Enter value to search: ";
            cin >> key;

            int result = binarySearch(arr, n, key);
            if (result != -1)
                cout << "Value found at index " << result << endl;
            else
                cout << "Value not found." << endl;

            delete[] arr;
            break;
        }

        case 4:
            cout << "Thank you for using the Categorizing App!" << endl;
            break;

        default:
            cout << "Invalid input!" << endl;
        }

    } while (choice != 4);

    return 0;
}
```

Output:
PS D:\Final Project - Categorizing> g++ categorizing.cpp
PS D:\Final Project - Categorizing> ./a.exe
=== Categorizing App ===
1. Linked List Operations
2. Sort Array (Merge or Quick)
3. Binary Search
4. Exit
Enter choice: 1
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 1
Enter value to insert: 1
Inserted: 1
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 1
Enter value to insert: 2
Inserted: 2
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 1
Enter value to insert: 3
Inserted: 3
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 1
Enter value to insert: 4
Inserted: 4
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 1
Enter value to insert: 4
Inserted: 4
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 1
Enter value to insert: 5
Inserted: 5
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 2
Enter value to delete: 3
Deleted.
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 3
Enter old value: 1    
Enter new value: 2
Updated.
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 4
Linked List: 2 2 4 4 5 
-- Linked List Menu --
1. Insert
2. Delete
3. Update
4. Display
5. Back
Choice: 5
=== Categorizing App ===
1. Linked List Operations
2. Sort Array (Merge or Quick)
3. Binary Search
4. Exit
Enter choice: 2
Enter number of elements: 5
Enter elements: 9
8
0
1
2
1. Merge Sort
2. Quick Sort
Choose sorting method: 1
Sorted Array: 0 1 2 8 9 
=== Categorizing App ===
1. Linked List Operations
2. Sort Array (Merge or Quick)
3. Binary Search
4. Exit
Enter choice: 2
Enter number of elements: 8
Enter elements: 7
6
45
2
3
4
5
9
1. Merge Sort
2. Quick Sort
Choose sorting method: 2
Sorted Array: 2 3 4 5 6 7 9 45
=== Categorizing App ===
1. Linked List Operations
2. Sort Array (Merge or Quick)
3. Binary Search
4. Exit
Enter choice: 3
Choose sorting method: 2
Sorted Array: 2 3 4 5 6 7 9 45
=== Categorizing App ===
1. Linked List Operations
2. Sort Array (Merge or Quick)
3. Binary Search
4. Exit
Enter choice: 3
Enter number of elements: 5
Enter sorted elements: 2
3
4
5
Choose sorting method: 2
Sorted Array: 2 3 4 5 6 7 9 45
=== Categorizing App ===
1. Linked List Operations
2. Sort Array (Merge or Quick)
3. Binary Search
4. Exit
Enter choice: 3
Enter number of elements: 5
Enter sorted elements: 2
3
4
5
1. Linked List Operations
2. Sort Array (Merge or Quick)
3. Binary Search
4. Exit
Enter choice: 3
Enter number of elements: 5
Enter sorted elements: 2
3
4
5
Enter number of elements: 5
Enter sorted elements: 2
3
4
5
4
5
5
6
Enter value to search: 4
Value found at index 2
=== Categorizing App ===
1. Linked List Operations
2. Sort Array (Merge or Quick)
3. Binary Search
4. Exit
Enter choice: 4
Thank you for using the Categorizing App!
PS D:\Final Project - Categorizing> & 'd:\Final Project - Categorizing\categorizing.cpp'
PS D:\Final Project - Categorizing>
![categorozing](https://github.com/jinaljain0705/Final-Project---Categorizing/blob/main/Output/Output-1.png)
![categorozing](https://github.com/jinaljain0705/Final-Project---Categorizing/blob/main/Output/Output-2.png)
![categorozing]()
![categorozing]()
![categorozing]()
