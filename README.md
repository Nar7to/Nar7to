 👋 Hi, I'm Abdalazeez 
 
/**
 * add
 * deleteelement
 * edit
 * undo
 * redo
 * display .
 **/

#include <iostream>
#include <cstring>
using namespace std;

const int Max_size = 5;

struct op {
    string operation = " ";
    int value = 0;
    int value_new = 0;
    int value_index = 0;
};

void push(int Stack[], int &top, int element);

bool isEmpty(int &top) {
    return top == -1;}


bool isFull(int &top) {
    return top >= Max_size - 1;}


bool isOperationStackFull(int &optop) {
    return optop >= Max_size - 1;}


//first in first out in operationStack[]....//#fraj_alserksy natarite altanjra
op temp[Max_size];
void fifo(op operationStack[], int &optop) {
    op temp[Max_size];
    int temptop = -1;

    while(!isEmpty(optop)){
        temp[++temptop] = operationStack[optop--];
    }
    temptop--;

    while(!isEmpty(temptop)){
        operationStack[++optop] = temp[temptop--];
    }
}



int add_element(int Stack[], int &top, int element, op operationStack[], int &optop) {
    if (isFull(top)) {
        cout << "Stack is full !!" << endl;
        return 0;}

     else {
        if (isOperationStackFull(optop)) {
            cout << "Operation stack is full. Removing the oldest operation." << endl;
            fifo(operationStack, optop);}

        operationStack[++optop] = {"add", element};
        push(Stack, top, element);}
        return 0;
}

void push(int Stack[], int &top, int element) {
    Stack[++top] = element;
    cout << "Added: " << element << endl;}



void delete_element(int Stack[], int &top, op operationStack[], int &optop) {
    if (isEmpty(top)) {
        cout << "Stack is empty !!" << endl;}

     else {
        int element = Stack[top--];
        if (isOperationStackFull(optop)) {
            cout << "Operation stack is full. Removing the oldest operation." << endl;
            fifo(operationStack, optop); }

        operationStack[++optop] = {"delete", element};
        cout << "Deleted: " << element << endl;
    }
}


void edit(int Stack[], int &top, int index, int newelement, op operationStack[], int &optop) {
    if (index >= 0 && index <= top) {
        int oldelement = Stack[index];
        Stack[index] = newelement;
        if (isOperationStackFull(optop)) {
            cout << "Operation stack is full. Removing the oldest operation." << endl;
            fifo(operationStack, optop);}

        operationStack[++optop] = {"edit", oldelement, newelement, index};
        cout << "Edited: " << oldelement << " to " << newelement << endl;   }

  else {
        cout << "Index out of range!" << endl;}
    }


void undo(int Stack[], int &top, op operationStack[], int &optop, op redoStack[], int &redotop) {
    if (isEmpty(optop)) {
        cout << "No actions to undo!" << endl; }

    else {
        redoStack[++redotop] = operationStack[optop];
        string action = operationStack[optop].operation;

        if (action == "add") {
            delete_element(Stack, top, operationStack, optop);}

         else if (action == "delete") {
            add_element( Stack, top, redoStack[redotop].value, operationStack,  optop);}

         else if (action == "edit") {
            int indx = redoStack[redotop].value_index;
            Stack[indx] = redoStack[redotop].value;
            cout << "Undid edit: " << redoStack[redotop].value_new << " to " << redoStack[redotop].value << endl;}
    }
}


void redo(int Stack[], int &top, op redoStack[], int &redotop, op operationStack[], int &optop) {
    if (isEmpty(redotop)) {
        cout << "No actions to redo!" << endl;}

     else {
        string action = redoStack[redotop].operation;

        if (action == "add") {
            add_element(Stack, top, redoStack[redotop].value, operationStack, optop);}

         else
            if (action == "delete") {
            delete_element(Stack, top, operationStack, optop);}

         else
            if (action == "edit") {
            int indx = redoStack[redotop].value_index;
            Stack[indx] = redoStack[redotop].value_new;
            cout << "Redid edit: " << redoStack[redotop].value << " to " << redoStack[redotop].value_new << endl;
        }

        redotop--;
    }
}


void display(int Stack[], int top) {
    cout << "Stack: ";
    for (int i = 0; i <= top; ++i) {
        cout << Stack[i] << " ";
    }
    cout << endl;
}


void printOperations(op operationStack[], int optop) {
    cout << "Operations performed:\n";
    for (int i = 0; i <= optop; ++i) {
        cout << operationStack[i].operation << ' '
             << operationStack[i].value << ' '
             << operationStack[i].value_new << endl;
    }
}

int main() {
    int Stack[Max_size];
    op operationStack[Max_size];
    op redoStack[Max_size];

    int top = -1;
    int optop = -1;
    int redotop = -1;
    int choice;
    int element;
    int index;

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
                add_element(Stack, top, element, operationStack, optop);
                cout << endl;
                cout << "=================================="<<endl;
                break;

            case 2:
                delete_element(Stack, top, operationStack, optop);
                cout << endl;
                cout << "=================================="<<endl;
                break;

            case 3:
                cout << "Enter index to edit: ";
                cin >> index;
                cout << "Enter new element: ";
                cin >> element;
                edit(Stack, top, index, element, operationStack, optop);
                cout << endl;
                cout << "=================================="<<endl;
                break;

            case 4:
                undo(Stack, top, operationStack, optop, redoStack, redotop);
                cout << endl;
                cout << "=================================="<<endl;
                break;

            case 5:
                redo(Stack, top, redoStack, redotop, operationStack, optop);
                cout << endl;
                cout << "=================================="<<endl;
                break;

            case 6:
                cout << endl;
                cout << "=================================="<<endl;
                display(Stack, top);
                cout << endl;
                cout << "=================================="<<endl;
                break;

            case 7:
                cout << endl;
                cout << "=================================="<<endl;
                printOperations(operationStack, optop);
                cout << endl;
                cout << "=================================="<<endl;
                break;

            case 8:
                cout << "Exiting ..." << endl;
                cout << endl;
                cout << "****************** THE END ******************"<<endl;
                break;

            default:
                cout << "Invalid choice, please try again." << endl;
        }
    } while (choice != 8);

    return 0;
}
