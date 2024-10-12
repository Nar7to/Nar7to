 👋 Hi, I'm Abdalazeez 


 #include <iostream>
#include <string>
using namespace std;

const int Max_Size = 100;

struct op {
    string operation = " ";
    int value = 0;
    int value_new = 0;
    int value_index = 0;
};

/**
 * add
 * deleteelement
 * edit
 * undo
 * redo
 * display .
 */

void add(int list[], int &top, int &element, op operationStack[], int &opTop) {
    if (top < Max_Size - 1) {
        list[++top] = element;
        operationStack[++opTop] = {"add", element};
        cout << "Added: " << element << endl;
    } else {
        cout << "Stack is full !!" << endl;
    }
}

void deleteelement(int list[], int &top, op operationStack[], int &opTop) {
    if (top >= 0) {
        int element = list[top--];
        operationStack[++opTop] = {"delete", element};
        cout << "Deleted: " << element << endl;
    } else {
        cout << "Stack Is empty !!" << endl;
    }
}

void edit(int list[], int &top, int &index, int &newelement, op operationStack[], int &opTop) {
    if (index >= 0 && index <= top) {
        int oldelement = list[index];
        list[index] = newelement;
        operationStack[++opTop] = {"edit", oldelement, newelement, index};
        cout << "Edited: " << oldelement << " to " << newelement << endl;
    } else {
        cout << "Index out of range!" << endl;
    }
}

void undo(int list[], int &top, op operationStack[], int &opTop, op redoStack[], int &redoTop) {
    if (opTop >= 0) {
        redoStack[++redoTop] = operationStack[opTop];
        string action = operationStack[opTop--].operation;

        if (action == "add") {
            deleteelement(list, top, operationStack, opTop);
        } else if (action == "delete") {
            add(list, top, redoStack[redoTop].value, operationStack, opTop);
        } else if (action == "edit") {
            int idx = redoStack[redoTop].value_index;
            list[idx] = redoStack[redoTop].value;
            cout << "Undid edit: " << redoStack[redoTop].value_new << " to " << redoStack[redoTop].value << endl;
        }
    } else {
        cout << "No actions to undo!" << endl;
    }
}

void redo(int list[], int &top, op redoStack[], int &redoTop, op operationStack[], int &opTop) {
    if (redoTop >= 0) {
        string action = redoStack[redoTop].operation;

        if (action == "add") {
            add(list, top, redoStack[redoTop].value, operationStack, opTop);
        } else if (action == "delete") {
            deleteelement(list, top, operationStack, opTop);
        } else if (action == "edit") {
            int idx = redoStack[redoTop].value_index;
            list[idx] = redoStack[redoTop].value_new;
            cout << "Redid edit: " << redoStack[redoTop].value << " to " << redoStack[redoTop].value_new << endl;
        }
        redoTop--;
    } else {
        cout << "No actions to redo!" << endl;
    }
}

void display(int list[], int top) {
    cout << "Stack: ";
    for (int i = 0; i <= top; ++i) {
        cout << list[i] << " ";
    }
    cout << endl;
}

void printOperations(op operationStack[], int opTop) {
    cout << "Operations performed:\n";
    for (int i = 0; i <= opTop; ++i) {
        cout << operationStack[i].operation << ' '
             << operationStack[i].value << ' '
             << operationStack[i].value_new << endl;
    }
}

int main() {
    int list[Max_Size];
    op operationStack[Max_Size];
    op redoStack[Max_Size];

    int top = -1;
    int opTop = -1;
    int redoTop = -1;

    int choice, element, index;

    do {
        cout << "1. Add element\n";
        cout << "2. Delete element\n";
        cout << "3. Edit element\n";
        cout << "4. Undo\n";
        cout << "5. Redo\n";
        cout << "6. Display stack\n";
        cout << "7. Print all operations\n";
        cout << "8. Exit\n";
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter element to add: ";
                cin >> element;
                add(list, top, element, operationStack, opTop);
                break;
            case 2:
                deleteelement(list, top, operationStack, opTop);
                break;
            case 3:
                cout << "Enter index to edit: ";
                cin >> index;
                cout << "Enter new element: ";
                cin >> element;
                edit(list, top, index, element, operationStack, opTop);
                break;
            case 4:
                undo(list, top, operationStack, opTop, redoStack, redoTop);
                break;
            case 5:
                redo(list, top, redoStack, redoTop, operationStack, opTop);
                break;
            case 6:
                display(list, top);
                break;
            case 7:
                printOperations(operationStack, opTop);
                break;
            case 8:
                cout << "Exiting…" << endl;
                break;
            default:
                cout << "Invalid choice, please try again." << endl;
        }
    } while (choice != 8);

    return 0;
}
