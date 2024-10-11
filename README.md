 👋 Hi, I'm Abdalazeez 


#include <iostream>

using namespace std;



const int MAX_SIZE = 100; //(stack) ????? ??????



struct op{//???? ???????? (stack)



string operation  = "  ";

int value = 0;

int value_new = 0;

int value_index  = 0;};



void add(int list[], int &top, int &element ,op operationStack[], int &opTop) {



    if (top < MAX_SIZE - 1) {

        top++;

        opTop++;

        list[top] = element;

        operationStack[opTop].operation = "add";

        operationStack[opTop].value = element;

        cout << "Added: " << element << endl;}



     else {

        cout << "Stack is full !!" << endl;}

}



void deleteelement(int list[], int &top, op operationStack[], int &opTop) {

    if (top >= 0) {

        int element = list[top--];

        opTop++;

        operationStack[opTop].operation = "delete";

        operationStack[opTop].value = element;

        cout << "Deleted: " << element << endl;

    } else {

        cout << "Stack is empty !!" << endl;

    }

}



void edit(int list[], int top, int &index, int &newelement, op operationStack[], int &opTop) {

    if (index >= 0 && index <= top) {

        int oldelement = list[index];

        list[index] = newelement;

        opTop++;

        operationStack[opTop].operation = "edit";

        operationStack[opTop].value_index = index ;

        operationStack[opTop].value = oldelement;

        operationStack[opTop].value_new = newelement;

        cout << "Edited: " << oldelement << " to " << newelement << endl;

    } else {

        cout << "Index out of range!" << endl;

    }

}



void undo(int list[], int &top, op operationStack[], int &opTop) {

    int temp ,oldelement;



    if (opTop >= 0) {

        string action = operationStack[opTop].operation;

        int element;



        if (action == "add") {

            element = operationStack[opTop].value;

            deleteelement(list, top, operationStack, opTop);

            cout << "Undid add: " << element << endl;}



         else

            if (action == "delete") {

            element = operationStack[opTop].value;

            add(list, top, element, operationStack, opTop);

            cout << "Undid delete: " << element << endl;}



         else if (action == "edit") {

          int editIndex = operationStack[opTop].value_index;

          oldelement = operationStack[opTop].value;

          temp = operationStack[opTop].value;

          operationStack[opTop].value = operationStack[opTop].value_new;

          operationStack[opTop].value_new = temp;

          list[editIndex] = oldelement;

          cout << "Undid edit: " << oldelement << endl;

        }

    } else {

        cout << "No actions to undo!" << endl;

    }

}



void display(int list[], int top) {

    cout << " stack: ";

    for (int i = 0; i <= top; ++i) {

        cout << list[i] << " ";

    }

    cout << endl;

}



void printOperations(op operationStack[], int opTop) {

    cout << "Operations performed:\n";

    for (int i = 0; i <= opTop; ++i) {

       cout << operationStack[i].operation <<' ' << operationStack[i].value <<' '<< operationStack[i].value_new <<' ' <<endl;

    }

}

int main() {



    int list[MAX_SIZE];

    op operationStack[MAX_SIZE];

    int top = -1; //

    int opTop = -1; //



    int choice;

    int element;

    int index;



    do {

        cout << "1. Add element\n";

        cout << "2. Delete element\n";

        cout << "3. Edit element\n";

        cout << "4. Undo\n";

        cout << "5. Display stack\n";

        cout << "6. Print all operations\n";

        cout << "7. Exit\n";

        cout << "Choose an option: ";

        cin >> choice;



        switch (choice) {

            case 1:

                cout << "Enter element to add: ";

                cin >> element;

                add(list, top, element, operationStack, opTop);

                cout<<endl;

                cout<<"========================================="<<endl;

                break;

            case 2:

                deleteelement(list, top, operationStack, opTop);

                cout<<endl;

                cout<<"========================================="<<endl;

                break;

            case 3:

                cout << "Enter index to edit: ";

                cin >> index;

                cout << "Enter new element: ";

                cin >> element;

                edit(list, top, index, element, operationStack, opTop);

                cout<<endl;

                cout<<"========================================="<<endl;

                break;

            case 4:

                undo(list, top, operationStack, opTop);

                cout<<endl;

                cout<<"========================================="<<endl;

                break;

            case 5:

                display(list, top);

                cout<<endl;

                cout<<"========================================="<<endl;

                break;

            case 6:

                printOperations(operationStack, opTop);

                cout<<endl;

                cout<<"========================================="<<endl;

                break;

            case 7:

                cout << "Exiting..." << endl;

                break;

            default:

                cout << "Invalid choice, please try again. :)" << endl;

        }

    } while (choice != 7);



    return 0;

}

