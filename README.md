
void insertLine(const string& text) {
    LineNode* newNode = new LineNode(text);

    if (!head) {
        // If the linked list is empty, set both head and currentLine to the new node
        head = currentLine = newNode;
    } else {
        if (currentLine) {
            // If there is a current line (cursor is positioned), insert after the current line
            newNode->next = currentLine->next;
            newNode->prev = currentLine;
            if (currentLine->next) {
                currentLine->next->prev = newNode;
            }
            currentLine->next = newNode;
            currentLine = newNode;
        } else {
            // If no current line, insert at the end of the linked list
            LineNode* temp = head;
            while (temp->next) {
                temp = temp->next;
            }
            temp->next = newNode;
            newNode->prev = temp;
            currentLine = newNode;
        }
    }

    // Push the operation onto the undo stack to track changes
    undoStack.push("insert");
}
Explanation:

Create a New Node:

cpp
Copy code
LineNode* newNode = new LineNode(text);
Creates a new LineNode with the specified text.
Check if the Linked List is Empty:

cpp
Copy code
if (!head) {
    head = currentLine = newNode;
}
If the linked list is empty (head is null), set both head and currentLine to the new node.
Insert After the Current Line (If Available):

cpp
Copy code
if (currentLine) {
    newNode->next = currentLine->next;
    newNode->prev = currentLine;
    if (currentLine->next) {
        currentLine->next->prev = newNode;
    }
    currentLine->next = newNode;
    currentLine = newNode;
}
If there is a current line (cursor is positioned), insert the new node after the current line.
Update pointers to maintain the doubly linked list.
Insert at the End (If No Current Line):

cpp
Copy code
else {
    LineNode* temp = head;
    while (temp->next) {
        temp = temp->next;
    }
    temp->next = newNode;
    newNode->prev = temp;
    currentLine = newNode;
}
If there is no current line, insert the new node at the end of the linked list.
Traverse the list to find the last node.
Update Undo Stack:


undoStack.push("insert");
Pushes the "insert" operation onto the undo stack to track changes for possible undo operations.
In summary, the insertLine function inserts a new line with the specified text at the current cursor position in the text editor. It appropriately updates the linked list, cursor position, and undo stack to reflect the changes


#deletion function 


void deleteLine() {
    if (currentLine) {
        LineNode* temp = currentLine;
        if (currentLine->prev) {
            currentLine->prev->next = currentLine->next;
        } else {
            head = currentLine->next;
        }
        if (currentLine->next) {
            currentLine->next->prev = currentLine->prev;
        }
        currentLine = currentLine->next;
        delete temp;
    }

    undoStack.push("delete");
}
Explanation:

Check If There is a Current Line:


if (currentLine) {
Ensures that there is a current line to delete.
Backup Current Line and Update Pointers:


LineNode* temp = currentLine;
if (currentLine->prev) {
    currentLine->prev->next = currentLine->next;
} else {
    head = currentLine->next;
}
if (currentLine->next) {
    currentLine->next->prev = currentLine->prev;
}
temp is a temporary pointer to the current line that will be deleted.
Updates the pointers to skip the current line in the doubly linked list.
Move Cursor to Next Line:


currentLine = currentLine->next;
Moves the cursor to the next line after deletion.
Delete Current Line:


delete temp;
Deallocates the memory occupied by the node representing the current line.
Update Undo Stack:

cpp
Copy code
undoStack.push("delete");
Pushes the "delete" operation onto the undo stack to track changes for possible undo operations.
In summary, the deleteLine function deletes the line at the current cursor position in the text editor. It updates the linked list by adjusting pointers to bypass the deleted line, moves the cursor to the next line, deletes the node representing the deleted line, and updates the undo stack to track the deletion operation.









#key points and important parts of project

Private Members:
LineNode* head

Represents the head of the linked list containing lines of text.
Each node in the linked list (LineNode) holds a line of text and pointers to the next and previous nodes.
LineNode* currentLine

Represents the current cursor position in the text editor.
This pointer points to the LineNode where the cursor is currently positioned.
stack<string> undoStack

A stack used to implement undo functionality.
It keeps track of the operations performed on the text editor, allowing for the reversal of the last operation.
Public Member Functions:
Constructor (TextEditor())

Initializes the head and currentLine pointers to nullptr when a TextEditor object is created.
Destructor (~TextEditor())

Calls the clear() function to deallocate memory used by the linked list when a TextEditor object goes out of scope or is explicitly deleted.
void insertLine(const string& text)

Inserts a new line with the specified text at the current cursor position.
Updates the undoStack to keep track of the operation.
void deleteLine()

Deletes the current line.
Updates the undoStack to keep track of the operation.
void moveCursorDown()

Moves the cursor down to the next line if available.
void moveCursorUp()

Moves the cursor up to the previous line if available.
void display() const

Displays the content of the text editor, indicating the current cursor position with ">".
void undo()

Undoes the last operation (insert or delete) based on the information stored in the undoStack.
void clear()

Clears all lines in the text editor.
Deallocates memory used by the linked list.
Clears the undoStack.
Summary:
The TextEditor class provides a simple interface for text editing operations, maintains the state of the text (linked list of lines), and implements undo functionality using a stack. The destructor ensures proper cleanup of allocated memory when the object is no longer needed.










#output of code

![image](https://github.com/haris461/data-structure-project-notepad/assets/158203393/33eff88f-1f13-43fd-8afc-88fb3005bc14)

