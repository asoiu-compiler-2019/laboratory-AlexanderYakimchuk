# compiler
The compiler is written in Python 3 without third-party dependencies. The language is somewhat between C ++ and Pascal. There are also built-in data types like Event, TicketType, Attendee and basic operations between them.

# lexical rules
```
nondigit ::= _ | a | ... | z | A | ... | Z | _
digit ::= 0 | ... | 9
operator ::= + | - | * | / | = | == | != | <= | >= | and | or | > | < |. | []
sign ::= + | -
intLiteral ::= [sign] digit {digit}
realLiteral ::= [sign] digit {digit} “.“ digit {digit}
variableIdentifier ::= nondigit {nondigit | digit}
unicodeCharacter — any unicode characher except "
stringLiteral ::= “{unicodeCharacter}”
Characters are also string.
boolLiteral ::= true | false

type ::= int | string | bool | real | event | ticketType | attendee
```
All lexical analysis functionality lies in the lexer module.
1. lexer.py - class Lexer that performs token splitting
2. token.py - class Token with token type, value and position
3. token_types.py - allowed token types and logical groups
# syntax rules
```
baseFieldDeclaration ::= type identifier ‘;’

reservedType ::= event | ticketType | attendee | arr
creator ::= Event | TicketType | Attendee
arrayFieldDeclaration ::= arr type identifier ';' | '=' 'Array' '('expression |{',' expression} ')' ';'
reservedFieldDeclaration ::= reservedType identifier ';' | '=' creator '(' identifier '=' expression | {',' identifier '=' expression} ')' ';' 

expression ::= ‘-’ expression | expression logic_op expression | expression binary_op expression | expression compare_op expression | identifier [ ‘(’ [ expression {‘,’ expression } ] ‘)’ ] | ‘(’ expression ‘)’ | intLiteral | realLiteral | stringLiteral | boolLiteral

logic_op ::= and | or

binary_op ::= + | - | * | / | . | [

compare_op ::= == | <= | >= | < | >

assignment ::= identifier ‘=’ expression ';'

statement ::= “if” statement expression [ “else” statement ] | “while” statement expression | ';' | assignment ‘;’ | identifier ‘(’ [ expression {‘,’ expression } ] ‘)’ ‘;’ |
‘{’ { statement } ‘}’ | ‘;’
```
Implemented in my_parser module
1. my_parser.py - class Parser. Parsing is performed according to the recursive descent algorithm.
2. expressions.py - structure to store expression tree and calculate its value
3. variables.py - built-in data types
# semantic
Also implemented in my_parser:
1. comp_types.py - structures to compare and transform types
2. commands.py - Declaration, Assigment, IfStatement and WhileStatement commands. Each of these classes implements the "execute" method. Type conformance is checked at runtime.
# examples
A simple piece of code that does bubble sorting.
```
arr int a = Array(1, 4, 2, 3, 4, 5, 9, 8, 7, 1);
int i = 0;
int j = 0;
while i < a.len - 1 {
    j = i;
    while j < a.len {
        if a[i] > a[j] {
            int t = a[i];
            a[i] = a[j];
            a[j] = t;
        }
        j = j + 1;
    }
    i = i + 1;
}
i = 0;
while i < a.len {
    print(a[i]);
    i = i + 1;
}
```
Output:
```
1
1
2
3
4
4
5
7
8
9
```

Example with built in types:
```
event e = Event(name="My event", quantity=200);
e = e + 100;
print(e.quantity);


ticketType t1 = TicketType(name="VIP", quantity=10, price=999.99);
ticketType t2 = TicketType(name="Standart", quantity=100, price=99.99);

e = e + t1 + t2;

print(e.ticket_types[0].name);
print(e.ticket_types[1].name);
```
Output:
```
300
"VIP"
"Standart"
```
