# Infix to Postfix Conversion in C++

## Description

The Infix to Postfix Conversion project in C++ is a versatile tool for transforming mathematical expressions from infix to postfix notation. This program utilizes a stack-based algorithm to ensure efficient and accurate conversion.

## Features
- **Efficient Algorithm:** Utilizes a stack-based algorithm for accurate and optimized conversion.
- **User-Friendly Interface:** Allows users to input complex mathematical expressions with ease.
- **Operator and Parentheses Support:** Handles a variety of mathematical operators and manages parentheses appropriately.
- **Clear Output:** Presents the converted postfix expression in a readable format.
- **Error Handling:** Robust validation and error handling for gracefully managing invalid expressions.

## How it works
Infix to postfix conversion is a process used in computer science and programming to convert an infix expression (like what you typically write) into a postfix expression (where operators come after their operands). This conversion is often helpful when evaluating expressions.

The algorithm for converting infix to postfix generally involves using a stack to keep track of operators and ensuring that the output maintains the correct order of operations.

**Here's a simplified explanation:**

-Initialize an empty stack to keep track of operators.

-Initialize an empty output string to store the postfix expression.

-start scanning the infix expression from left to right.

**For each symbol in the infix expression:**
If it's an operand (like a number or variable), add it to the output string.
If it's an operator, pop operators from the stack and add them to the output until the stack is empty or the top operator has lower precedence. Then push the current operator onto the stack.
If it's an opening parenthesis '(', push it onto the stack.
If it's a closing parenthesis ')', pop operators from the stack and add them to the output until an opening parenthesis is encountered. Pop and discard the opening parenthesis.
After scanning the entire infix expression, pop any remaining operators from the stack and add them to the output.
Let's take an example to illustrate:

**Infix: A + B * C - D / E**

Start with an empty stack and an empty output string.
Scan the infix expression:
A is an operand, so add it to the output.
is an operator, push it onto the stack.
B is an operand, add it to the output.
has higher precedence than +, so push it onto the stack.
C is an operand, add it to the output.
has lower precedence than *, so pop * from the stack and add it to the output. Then push - onto the stack.
D is an operand, add it to the output.
/ has higher precedence than -, so push it onto the stack.
E is an operand, add it to the output.
After scanning the entire expression, pop any remaining operators from the stack and add them to the output. 

**The final postfix expression is: A B C * + D E / - as showm in the output**

<img width="487" alt="Infix" src="https://github.com/AfrahSaud36/InfixToPostfix/assets/138797663/cbc77411-70a1-40ae-81f6-38d6a089b9d0">


## Mathematically
<img width="460" alt="mmath" src="https://github.com/AfrahSaud36/InfixToPostfix/assets/138797663/3bcf2965-b934-458e-ac9f-5f7788f8afb1">



## Usage

1. **Input:** Enter the desired infix mathematical expression.
2. **Output:** The program processes the input and displays the corresponding postfix expression.
3. **Error Handling:** Correct any input errors as indicated by the program.

## Implementation

The entire project is implemented in C++, emphasizing code efficiency, readability, and adherence to best practices. The code structure allows accessibility for both beginners and experienced programmers.





**Enjoy converting your infix expressions effortlessly with the Infix to Postfix Conversion in C++!**
```cpp
/*             Infix to Postfix program 
               Data Structure ||  CS214 */
#include<iostream>
#include<string>
using namespace std;


struct node {
    char data;
    struct node *next;
};

struct node *TOP = NULL;

bool isEmpty(){
    if (TOP == NULL )
        return true;
    else
        return false;
}


char peek(){
    return TOP->data;
}
// Push function inserts value in stack.

void push (char x)
{
    struct node *NewNode;
    NewNode = new (node);
    NewNode->data = x;
    NewNode->next = NULL;

    if(TOP == NULL) TOP = NewNode;
    else
    {
        NewNode->next = TOP;
        TOP=NewNode;
    }
}



// Pop function to remove an item from stack.


char pop ()
{
    struct node *T;
    char x;
    T=TOP;
    x= T->data;
    TOP = TOP->next;
    return x;
}




int priority (char alpha){
    switch (alpha){
        case '+': return 1;
        case '-': return 1;
        case '*': return 2;
        case '/': return 2;
        case '%': return 2;
        case '^': return 3;

        default: return 0;
    }
}

bool IsOperand(char C)
{
    if(C >= '0' && C <= '9') return true;
    if(C >= 'a' && C <= 'z') return true;
    if(C >= 'A' && C <= 'Z') return true;
    return false;
}

string INtoPOST(string infix)
{
    int i=0;
    string postfix = "";

    while(infix[i]!='\0')
    {

// if operand add to the postfix expression
        if(IsOperand(infix[i]))
        {
            postfix += infix[i];
            i++;
        }
// if opening bracket then push the stack
        else if(infix[i]=='(')
        {
            push(infix[i]);
            i++;
        }
// if closing bracket encounted then keep popping from stack until
// closing a pair opening bracket is not encountered
        else if(infix[i]==')')
        {
            while(TOP->data!='(')
                postfix += pop();

            pop();
            i++;
        }
        else
        {

            if (infix[i] != ('+')&& infix[i] !=('-') && infix[i] !=('*')&& infix[i] != '/' && infix[i]!='%'
                && infix[i] !='^' )

                return "\n OPPS :(! you entered an invalid operator  \n";

            while (!isEmpty() && priority(infix[i]) <= priority(peek())){
                postfix += pop();
            }
            push(infix[i]);
            i++;
        }
    }
    while(!isEmpty()){
        postfix += pop();
    }


    return postfix ;
}

int main()
{
    char infix[100];
    string postfix;
    int c;
    system("Color 0F");
    do {
        cout << "\t ______________________________________________________\n\t|\t\tEnter your expression to convert\t   |  \n\t|________________________________________________________| ";
        cout << "\n\t|\t\tPlease use the next operators: \t\t   |\n\t|_____________________________________________________|\n \t|\t^\t|  for exponents\t\t\t   |\n";
        cout << "\t| / and * and % | for division and multiplication and Mod  |\n\t|    - and +\t| for subtraction and addition\t\t   |\n\t|_____|__________________________________________________|\n";
        cout << "\t|\t    (You can use parentheses as well)\t\t   |\n\t|___________________________________________________|\n\n";
        cin.getline(infix, 100);
        if (infix[0] == '\0') {
            cout << " There is no expression to convert please enter your expression \n";
        }
        else
            cout << "\n Your expression after being converted to Postfix : \n " << INtoPOST(infix)<<endl; //it will print postfix conversion
        cout<<"\n Do you want to try again ? PRESS 1 or 2 \n 1- YES \n 2- NO  \n ";
        cin>>c;
        cin.getline(infix, 100);
        cout << "\n  " << INtoPOST(infix); //it will print postfix conversion
    }
    while (c==1);
    return 0;
}
```

