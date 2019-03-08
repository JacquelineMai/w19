---
num: "lect16"
desc: "Using Linked Lists"
ready: false
pdfurl: /lectures/CS16_Lecture16.pdf
annotatedpdfurl: /lectures/CS16_Lecture16_ann.pdf
annotatedready: false
lecture_date: 2019-03-07
---

**HW #5 due 3/15!**

**Dot operator review:**
* When you have a struct object, use the dot operator to access the member variables by name
`
struct Box{    
            Int w;
            Int h
        }
`
* If you have a Box object known as b1, you can access the width of b1 with: `b1.w`

* When to use the arrow `->` ?
  * When you have a pointer to a struct type, and you want to access the member variables of the object that the pointer points to
      * `Box* boxptr = &b1;`
      * Now to access b1’s width from the pointer, you need to dereference the pointer first, then use the dot operator to access the member variable
        * `(\*boxptr).w`
* **Note:** The dot operator has higher precedence than the dereference operator in ordinary order of operations. That is why you must enclose the dereferencing in parentheses, so you access the Box object first
  * `*boxptr.w` will try to get the w member variable of boxptr first, then try to dereference
    * This is an error bc the pointer itself has no member variable w
  * `boxptr->w`
      * This is a shortcut for the previous notation. As though you are “pointing” at any part of the struct 

* The `new` operator creates variables in dynamic memory- this area of memory is known as the **heap** or **freestore**

**Linked List Example: Cards** 
* 4 suits, 13 denominations

       ` A 2 3 4 5 6 7 8 9 10 J Q K`
  * Heart         
  * Diamond
  * Spade
  * Club

* Can this be stored as an array? 
* You could make an array 13 units long, store characters at a given pos

* Or can create a `struct`:
    * Two attributes: suit and denomination
      * `char suit`
      * `int denomination // could this be a float? char? string?
* Choose types based on how you are planning to use this struct
* With integers, treat Jack, Queen, King as 11, 12, 13
`
struct Card{
    char suit;
    int denomination;
}
`

* Create a struct to hold a hand of cards
* How will you store a hand of cards?
  * You won’t necessarily know the size of the hand 
* An array? 52 possible cards, so you would have a 52-array with a lot of empty spaces. This would be inefficient.
* A dynamic array? This will work to hold your initial hand of cards, but what if you want to add cards? If the player receives any additional cards, will have to create a new dynamic array that is slightly larger, copy over existing values and add new ones, then delete original array
  * A lot of memory swapping, also inefficient
  
* A linked list? Can add or remove cards from any location in the hand fairly easily

* What if we start with this code:
`
struct Hand{
    Card c;
    Card* next;
}
`

* How do we add new cards? 
  * The Hand has no way to link to any other cards-
  * It just holds one card, a link to another, but that next card cannot link to any other card since that card is not part of a Hand (a Card ptr cannot point to another Hand)
  * We could fix this by changing the Card* to a Hand*. Then we can link a bunch of Hands together, where each Hand holds a single Card and a ptr to another Hand
  * This is weird and repetitive (why are we connecting hands, rather than cards?)

* How to go about changing this?
  * Add a head and tail pointer to Hand, add a Card* next to the CARD struct
  * Now the Card behaves as a node, and the Hand tracks the start and end of the list
  * The Cards in between are linked to each other w their next pointers

* Initializing a Hand:
  * At first, should be empty, so want head and tail to be NULL
    * `Hand myHand; //creates a Hand object not a Hand pointer`
  * Access head and tail pointers with dot operator
    * `myHand.head = NULL`
    * `myHand.tail = NULL` 
  * To add a Card to the Hand:
      * Non-dynamic: 
        `Card cr;
         Card* cp = &cr
         myHand.head = cp;
         cp->next = NULL
         myHand.tail = cp
         `
  * Now a linked list with a single item
  * Now, to add more cards, look at the first (head of Hand), then walk down list (following the pointers from each card)
    * add the card to the end, when you reach a NULL pointer

