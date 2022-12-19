# Liste simplu înlănțuite

structura:
```c++
struct Node
{
    int date;
    Node *next;
};
```
schema logică:

![image](https://user-images.githubusercontent.com/84264524/208507806-8ce6c51b-37cb-4e79-b0eb-0c628fb89b9e.png)

declarare:
```c++
Node *head = NULL;
```
funcție de adaugare a unui nou node, la sfârșitul listei, cu datele date ca parametru:
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
funcție de ștergere a ultimului node:
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
funcție de afișare a listei:
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
funcție (recursivă) de ștergere a listei:
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
structura:
```c++
struct Node
{
    int date;
    Node *prev, *next;
};
```
schema logică :

![image](https://user-images.githubusercontent.com/84264524/208507632-50d114ec-d298-4516-8337-0b569544c5dc.png)

declarare:
```c++
Node *head = NULL, *tail = NULL;
```
funcție de adaugare a unui nou node, la sfârșitul listei, cu datele date ca parametru:
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
funcție de ștergere a ultimului node:
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
funcțiile de afișare și stergere listei simplu înlănțuite merg și aici:
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
