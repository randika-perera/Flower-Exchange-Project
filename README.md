# Flower Exchange Project

## Organized by LSEG Sri Lanka


- The flower exchange is a system which supports basic trading.
- **Trader Application** - Traders can submit buy or sell orders for flowers via the Trader

#### Application.

- **Exchange Application** - will process the incoming order against existing orders in the

#### order container(known as Order Book) and do a full or partial execution.

- Every order is replied with an Execution Report by the **Exchange Application** indicating

#### the status of the order.

- Orders sometimes could be rejected due to quantity limitations, invalid flower type, etc.

## Flower Exchange Story


## Flower Exchange – High Level Architecture

##### Trader

##### Application

### Exchange

### Execution Report (s) Application

### orders.csv Order(s)

### execution_rep.csv


## Flower Exchange – A simpler implementation (2nd option)

### orders.csv Exchange Application execution_rep.csv

#### Basically, read an orders.csv fila and produce an execution_rep.csv file


## Flower Exchange – Input order

#### Field

#### Name

#### Type Possible Values Mandatory Notes

```
Client Order ID String Alpha numeric string (max 7 chars) Yes This unique ID identifies the submitted order
```
```
Instrument String {Rose, Lavender, Lotus, Tulip, Orchid) Yes We will limit the instruments for these 5 types only
```
```
Side Int 1 : Buy ; 2 : Sell Yes Specifies if the input order is a buy order or a sell order
```
```
Price double Price > 0.0 Yes Price of one unit
```
```
Quantity int {10, 20, 30, .... , 1000} Order size
must be a a multiple of 10. Min 10, Max 1000
Yes Quantity of the order
```

## Flower Exchange – Output Execution Report

#### Field

#### Name

#### Type Possible Values Mandatory Notes

```
Client Order ID String Alpha numeric string (max 7 chars) Yes This is the Client Order ID of the submitted order
Order ID String Alpha numeric string Yes System generated unique order ID
Instrument String {Rose, Lavender, Lotus, Tulip, Orchid) Yes We will limit the instruments for these 5 types only
Side Int 1 : Buy ; 2 : Sell Yes Specifies if the order is a buy order or a sell order
Price double Price > 0.0 Yes Price of one unit
Quantity int {10, 20, 30, .... , 1000} Order size must be a a multiple of 10. Min 10,
Max 1000
```
```
Yes Quantity of the order
```
```
Status int 0 1 – – NewRejected
2 – Fill
3 - Pfill
```
```
Yes The status of the execution report
```
```
Reason String Max 50 chars No Contains the reject reason, when an order is not accepted into the
system due to validation failure
Transaction Time String YYYYMMDD-HHMMSS.sss Yes Every execution report should have the transaction time in the given format. This data can be used to check the speed and
optimize your code
```

## Order book – Example 1

- Exchange app maintains one order book for each Instrument (flower type). Since there are 5 types
    of Instruments, there will be five separate order books in our system.
- Orderbook has two sides. Buy (blue), Sell (pink)
- Initially the orderbook is empty
- The order book receives an order. (Side = Sell, Price = 55.00, Qty = 100)
- This order will go inside sell side of the order book, and an execution report with status “New” will
    be disseminated
Orderbook : Rose (Initially empty)
Order ID Qty Price Price Qty Order ID

```
Orderbook : Rose – (After the 100 @ 55.00 order enters the book)
Order ID Qty Price Price Qty Order ID
55.00 100 ord
```
100 @ 55.

#### Side = 1 : Buy Side

#### Side = 2 : Sell Side

###### Exchange App


## Order Book - Example 2

Orderbook : Rose (Initial state)
Order ID Qty Price Price Qty Order ID

State - 1
Order ID Qty Price Price Qty Order ID
55.00 100 ord

1. Sell 100 @ 55.
2. Sell 100 @ 45.

```
State – 2
Order ID Qty Price Price Qty Order ID
45.00 100 ord
55.00 100 ord
```
3. Buy 100 @ 35.

```
State – Final
Order ID Qty Price Price Qty Order ID
ord3 100 35.00 45.00 100 ord
55.00 100 ord
```

## Order Book - Example 2

### Exchange App


## Order Book - Example 3 (A FILL execution)

```
Orderbook : Rose (Initial state)
Order ID Qty Price Price Qty Order ID
```
```
State - 1
Order ID Qty Price Price Qty Order ID
55.00 100 ord
```
1. Sell 100 @ 55.
2. Sell 100 @ 45.

```
State – 2
Order ID Qty Price Price Qty Order ID
45.00 100 ord
55.00 100 ord
```
3. Buy 100 @ 45.

```
State – Final
Order ID Qty Price Price Qty Order ID
55.00 100 ord
```
###### Aggressive order (an incoming order

###### which executes)

Passive order (an incoming order which goes
into the OB without exeucuting)


## Order Book - Example 3

### Exchange App


## Order book

- Buy side of the orderbook is sorted in the ascending order of the price. (Higher the

#### buy price, more attractive the order)

- Sell side of the orderbook is sorted in the descending order of the price. (Lower the

#### sell price, more attractive the order)

- Orders with the same price are ordered in the time priority. (Priority sequence)

```
Orderbook : Rose
Pr.Seq Order ID Trader ID Qty Price Price Qty Trader ID Order ID Pr.Seq
```
##### 1 ord4 Tr04 100 4.00 5.00 200 Tr02 ord2 1

##### 1 ord8 Tr08 200 3.00 5.00 100 Tr03 ord3 2

##### 2 ord7 Tr02 200 3.00 6.00 100 Tr01 ord1 1


## Order Book - Example 4 (A PFILL execution)

Orderbook : Rose (Initial state)
Order ID Qty Price Price Qty Order ID

State - 1
Order ID Qty Price Price Qty Order ID
55.00 100 ord

1. Sell 100 @ 55.
2. Sell 100 @ 45.

```
State – 2
Order ID Qty Price Price Qty Order ID
45.00 100 ord
55.00 100 ord
```
3. Buy 200 @ 45.

```
State – Final
Order ID Qty Price Price Qty Order ID
ord3 100 45.00 55.00 100 ord
```

## Order Book - Example 4

### Exchange App


## Order Book - Example 5 (A FILL execution with a twist)

Orderbook : Rose (Initial state)
Order ID Qty Price Price Qty Order ID

State - 1
Order ID Qty Price Price Qty Order ID
ord1 100 55.

1. Buy 100 @ 55.
2. Buy 100 @ 65.

```
State – 2
Order ID Qty Price Price Qty Order ID
ord2 100 65.
ord1 100 55.
```
3. Sell 300 @ 1.

```
State – Final
Order ID Qty Price Price Qty Order ID
1.00 100 ord
```
###### The execution price of an

###### aggressive order is decided

###### by the orderbook


## Order Book - Example 5

### Exchange App


## Order Book - Example 6

Orderbook : Rose (Initial state)
Order ID Qty Price Price Qty Order ID

1. Buy 100 @ 55.
2. Buy 100 @ 65.00 4. Buy 100 @ 2.
    State – State 3
    Order ID Qty Price Price Qty Order ID
       1.00 100 ord

State – 2
Order ID Qty Price Price Qty Order ID
ord2 100 65.
ord1 100 55.

3. Sell 300 @ 1.
    Final state
    Order ID Qty Price Price Qty Order ID


## Order Book - Example 6

### Exchange App


## Flower Exchange – Input validations

- An order will be rejected if

#### It does not contain a required field

#### It is for an invalid Instrument

#### It contains an invalid side

#### Its price is not greater than 0

#### Its quantity is not a multiple of 10

#### Its quantity is outside the range (min = 10 max = 1000)

- A Rejected execution report is generated when validations

#### are failed


## Order Book - Example 7 (Input Validations)

### Exchange App

##### A validation failure results in a

##### Rejected Execution Report


## Flower Exchange – Verify your system

```
22
```
- We have provided you 6 sample input files (order.csv) and corresponding sample output files

##### (exec_rep.csv) files in the previous examples.

- When developing your system, test your system with the sample input files we provided and verify your

##### system produces the same output as in the sample execution_rep.csv files

##### SampleOrder1.csv (give this

as an input to your app)

### Exchange

### system

##### execReports1.csv (compare

```
with the sample execution_rep.csv
given in the slides)
```
##### Compare :

- exeReports.csv generated by your system matches the sample execution_rep.csv – Pass
- exeReports.csv generated by your system produces a different output to sample execution_rep.csv – Fix your bugs, and re-

###### test

- Note that the order of the execution reports in your file does not have to be the same as ours. But the content should be the

###### same.


## Flower Exchange – Evaluation

- Demonstration and code review – You will demonstrate the system and we will do a code review with you.
- Project will be evaluated for the design, coding practices, efficiency of the program and the speed of the

##### program.

- During the demo, we will test your application against our test data (orders.csv) and verify the functional

##### accuracy.

- To test the speed of the program we will input a large orders.csv and measure the time taken to produce the

##### resulting exec_reports.csv (Note that Transaction Time is a mandatory field in the Execution Report)



