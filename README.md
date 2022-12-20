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
<p>&nbsp;</p>

# Arbori binari
Structura:
```c++
struct Node
{
    int date;
    Node *left, *right;
};
```
Schema logică:

<img src="https://media.geeksforgeeks.org/wp-content/uploads/BSTSearch.png" height=250px>

Declararea:
```c++
Node *root = NULL;
```

Funcție recursivă de creare a unui arbore, a cărui elemente se citesc de la tastatură (cu excepția radăcinii, care e dată inițial ca parametru)
```c++
void create(Node *&root, int rootVal)
{
    if (rootVal != 0)
    {
        root = new Node;
        root->date = rootVal;
        root->left = root->right = NULL;
        int temp;
        cout << "Nodul stang al lui " << rootVal << ": ";
        cin >> temp;
        create(root->left, temp);
        cout << "Nodul drept al lui " << rootVal << ": ";
        cin >> temp;
        create(root->right, temp);
    }
}
```
Funcție de înserare a unui nou element in arbore:
```c++
Node *insert(Node *&root, int date)
{
    if (root == NULL)
    {
        Node *node = new Node;
        node->date = date;
        node->left = NULL;
        node->right = NULL;
        return node;
    }

    if (date > root->date)
    {
        root->right = insert(root->right, date);
    }

    if (date < root->date)
    {
        root->left = insert(root->left, date);
    }

    return root;
}
```
*Atenție! ca această funcție sa meargă e nevoie ca rădăcina sa fie deja inițializată:*
```c++
// în main
Node *root = new Node;
cout << "Radacina: ";
cin >> root->date;
root->left = root->right = NULL;
```

Un arbore binar poate fi citit în-ordine, pre-ordine si post-ordine.

Exemplu:

<img src="https://lh5.googleusercontent.com/63bUUZdT3TPXbTrShHVMGycwVhcQ5Xgww21UqCI8dlQAQa_C8zCyU0GGPoI-5C-Ejt1RbAmlPqwf_-d5DdHo_DLR4I3Yb2Et9bTb9KlWvuk5hGF2nqUVFmOg4W94q_rJmqm0VVFK" height=250px>

Funcție de citire pre-ordine:
```c++
void preOrder(Node *root)
{
    if (root != NULL)
    {
        cout << root->date << " ";
        preOrder(root->left);
        preOrder(root->right);
    }
}
```
Funcție de citire în-ordine:
```c++
void inOrder(Node *root)
{
    if (root != NULL)
    {
        inOrder(root->left);
        cout << root->date << " ";
        inOrder(root->right);
    }
}
```
Funcție de citire post-ordine:
```c++
void postOrder(Node *root)
{
    if (root != NULL)
    {
        postOrder(root->left);
        postOrder(root->right);
        cout << root->date << " ";
    }
}
```
Funcție pentru a șterge un subarbore (sau arbore), rădăcina lui fiind dată ca parametru:
```c++
void deleteSubtree(Node *&root)
{
    if (root != NULL)
    {
        deleteSubtree(root->left);
        deleteSubtree(root->right);
        delete root;
        root = NULL;
    }
}
```
Funcție pentru a afișa node-urile terminale (care nu au copii) ale arboreului:
```c++
void terminalNodes(Node *root)
{
    if (root == NULL)
    {
        return;
    }
    if (root->left == NULL && root->right == NULL)
    {
        cout << root->date << " ";
    }
    if (root->left != NULL)
    {
        terminalNodes(root->left);
    }
    if (root->right != NULL)
    {
        terminalNodes(root->right);
    }
}
```
<p>&nbsp;</p>

<h3>Important! Oricare din aceste tipuri de date structurate poate deține mai multe câmpuri de informație, nu doar un integer numit date.</h3>

De exemplu, aici avem structura unui arbore binar din persoane:
```c++
struct Pers
{
    int id, varsta;
    char nume[100], prenume[100], gen;
    double bani;
    Pers *left, *right;
};
```
Iar aici a unei liste dublu înlănțuite din cărți:
```c++
struct Carte
{
    int anPublicare;
    char denumire[200], autori[200], editura[200];
    double pret;
    bool pentruCopii;
    Carte *next, *prev;
};
```