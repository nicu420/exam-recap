# Liste simplu înlănțuite

Structura:
```c++
struct Node
{
    int date;
    Node *next;
};
```
Schema logică:

![image](https://user-images.githubusercontent.com/84264524/208507806-8ce6c51b-37cb-4e79-b0eb-0c628fb89b9e.png)

Declarare:
```c++
Node *head = NULL;
```
Funcție de adaugare a unui nou node, la sfârșitul listei, cu datele date ca parametru:
```c++
void push(Node *&head, int date)
{
    Node *element = new Node;
    element->date = date;
    element->next = NULL;

    if (head == NULL)
    {
        head = element;
    }
    else
    {
        Node *iter = head;
        while (iter->next != NULL)
        {
            iter = iter->next;
        }
        iter->next = element;
    }
}
```
Funcție de ștergere a ultimului node:
```c++
void pop(Node *&head)
{
    if (head == NULL)
    {
        return;
    }
    if (head->next == NULL)
    {
        delete head;
        head = NULL;
        return;
    }
    Node *iter = head;
    while (iter->next->next != NULL)
    {
        iter = iter->next;
    }
    delete iter->next;
    iter->next = NULL;
}
```
Funcție de afișare a listei:
```c++
void printList(Node *head)
{
    while (head != NULL)
    {
        cout << head->date << " ";
        head = head->next;
    }
}
```
Funcție (recursivă) de ștergere a listei:
```c++
void deleteList(Node *&head)
{
    if (head == NULL)
    {
        return;
    }
    else
    {
        deleteList(head->next);
        head = NULL;
        delete head;
    }
}
```
<p>&nbsp;</p>

# Liste dublu înlănțuite
Structura:
```c++
struct Node
{
    int date;
    Node *prev, *next;
};
```
Schema logică:

![image](https://user-images.githubusercontent.com/84264524/208507632-50d114ec-d298-4516-8337-0b569544c5dc.png)

Declarare:
```c++
Node *head = NULL, *tail = NULL;
```
Funcție de adaugare a unui nou node, la sfârșitul listei, cu datele date ca parametru:
```c++
void push(Node *&head, Node *&tail, int date)
{
    Node *element = new Node;
    element->date = date;
    element->next = NULL;

    if (head == NULL)
    {
        element->prev = NULL;
        head = tail = element;
    }
    else
    {
        tail->next = element;
        element->prev = tail;
        tail = element;
    }
}
```
Funcție de ștergere a ultimului node:
```c++
void pop(Node *&head, Node *&tail)
{
    if (head == NULL)
    {
        return;
    }
    else
    {
        Node *toDelete = tail;
        tail = tail->prev;
        tail->next = NULL;
        delete toDelete;
    }
}
```
Funcțiile de afișare și ștergere listei simplu înlănțuite merg și aici:
```c++
void printList(Node *head)
{
    while (head != NULL)
    {
        cout << head->date << " ";
        head = head->next;
    }
}
```
```c++
void deleteList(Node *&head)
{
    if (head == NULL)
    {
        return;
    }
    else
    {
        deleteList(head->next);
        head = NULL;
        delete head;
    }
}
```
<p>&nbsp;</p>

# Conceptul de stivă și coadă (LIFO și FIFO)

Funcțiile push și pop folosite anterior sunt bazate pe principiul LIFO (Last In First Out), adică ultimul adăugat va fi primul șters de funcția pop (stivă)

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20220714004311/Stack-660x566.png" height=250px>

Există însă și o altă implementare, bazată pe principiul FIFO (First In First Out) (coadă)

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20220816162225/Queue.png" height=250px>

Așadar, la cozi, funcția de ștergere a unui node ar trebui sa îl șteargă pe primul

Pentru liste simplu înlănțuite:
```c++
void dequeue(Node *&head)
{
    if (head == NULL)
    {
        return;
    }
    Node *toDelete = head;
    head = head->next;
    delete toDelete;
}
```
Pentru liste dublu înlănțuite:
```c++
void dequeue(Node *&head)
{
    if (head == NULL)
    {
        return;
    }
    Node *toDelete = head;
    head = head->next;
    head->prev = NULL;
    delete toDelete;
}
```
Desigur, cu puțin chin putem scrie funcții care să șteargă un anumit node dupa poziția sa in listă sau după o valoare pe care o deține:

Pentru liste simplu înlănțuite:
```c++
void deleteByValue(Node *head, int date)
{
    Node *iter;
    while (head != NULL && head->date != date)
    {
        iter = head;
        head = head->next;
    }
    if (head->date == date)
    {
        Node *toDelete = head;
        iter->next = iter->next->next;
        delete toDelete;
    }
}
```
```c++
// începând cu 0
void deleteFromPosition(Node *head, int position)
{
    if (head == NULL)
    {
        return;
    }
    if (position == 0)
    {
        Node *toDelete = head;
        head = head->next;
        delete toDelete;
        return;
    }
    int i = 1;
    while (head != NULL && i < position)
    {
        head = head->next;
        i++;
    }
    if (i == position)
    {
        Node *toDelete = head->next;
        head->next = head->next->next;
        delete toDelete;
    }
}
```
Pentru liste dublu înlănțuite:
```c++
void deleteByValue(Node *head, int date)
{
    while (head != NULL && head->date != date)
    {
        head = head->next;
    }
    if (head->date == date)
    {
        Node *toDelete = head;
        head->next->prev = head->prev;
        head->prev->next = head->next;
        delete toDelete;
    }
}
```
```c++
// incepând de la 0
void deleteFromPosition(Node *head, int position)
{
    if (head == NULL)
    {
        return;
    }
    if (position == 0)
    {
        Node *toDelete = head;
        head = head->next;
        head->prev = NULL;
        delete toDelete;
        return;
    }
    int i = 1;
    while (head != NULL && i < position)
    {
        head = head->next;
        i++;
    }
    if (i == position)
    {
        Node *toDelete = head->next;
        head->next = head->next->next;
        head->next->prev = head;
        delete toDelete;
    }
}
```