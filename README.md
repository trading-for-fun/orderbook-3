# Order Book Programming Problem
Order Book was a small project that I did over a weekend as a coding challenge for an interview that I had.

## Contents

- [Problem Background](#problem-background)
- [Problem](#problem)
- [Solution](#solution)
    - [How to build](#how-to-build)
    - [How to run](#how-to-run)
    - [Solution Description](#solution-description)
    - [Questions](#questions)
    - [CPU Profiling](#cpu-profiling-of-the-pricer-program)
    - [Memory Profiling](#memory-profiling-of-the-pricer-program)
- [License](#license)
---

## Problem Background

Suppose your great aunt Gertrude dies and leaves you $3000 which you decide you want to invest in the Acme Internet Widget Company (stock symbol:AIWC). You are willing to pay up to $30 per share of AIWC. So you log in to your online trading account and enter a limit order: "BUY 100 AIWC @ $30". It's a limit order because that's most you're willing to pay. You'd be willing to pay less than $30, but not more than $30. Your order will sit around in the market until you get your 100 shares. A limit order to buy is called a "bid".

But you're not the only prospective buyer for AIWC stock. Others want to buy it too. One is bidding $31/share for 200 shares, while another is bidding $29/share for 300 shares. When Warren Buffett wants to sell 225 shares, he's obviously going to take the highest price he can get for each share. So he hits the $31 bid first, selling 200 shares. Then he sells his remaining 25 shares to you at $30/share. Your bid size reduced by 25, leaving 75 shares still to be bought.

Suppose you eventually get the full 100 shares at some price. Next year, you decide to buy a new computer and you need $4500 for it, and luckily the value of AIWC has appreciated by 50%. So you want to sell your 100 shares of AIWC stock for at least $45/share. So you enter this limit order: "SELL 100 AIWC @ $45". A limit order to sell is called an "ask".

But you're not the only prospective seller of AIWC stock. There's also an ask for $44/share and an ask for $46/share. If Alan Greenspan wants to buy AIWC, he's obviously going to pay as little as possible. So he'll take the $44 offer first, and only buy from you at $45 if he can't buy as much as he wants at $44.

The set of all standing bids and asks is called a "limit order book", or just a "book". You can buy a data feed from the stock market, and they will send you messages in real time telling you about changes to the book. Each message either adds an order to the book, or reduces the size of an order in the book (possibly removing the order entirely). You can record these messages in a log file, and later you can go back and analyze your log file.

## Problem

Your task is to write a program, Pricer, that analyzes such a log file. Pricer takes one command-line argument: `target-size`. Pricer then reads a market data log on standard input. As the book is modified, Pricer prints (on standard output) the total expense you would incur if you bought target-size shares (by taking as many asks as necessary, lowest first), and the total income you would receive if you sold target-size shares (by hitting as many bids as necessary, highest first). Each time the income or expense changes, it prints the changed value.

Read more about the problem in the [`Problem.html`](http://htmlpreview.github.io/?https://github.com/panaali/orderbook/blob/master/problem.html) file.

## Solution


### How to build

    $ cd PROJECT_PATH/bin
    $ cmake .
    $ cmake --build . --target all -- -j 4

### How to run

    $ ./pricer target-size input-file How to run using console input:
    $ ./pricer target-size

### Solution Description
The program coded in C++ and uses a multimap for storing the orders and an unordered_map for looking up the orders based on their order-id. The source code style lint-ed based on [“**Google C++ Style Guide**”](https://google.github.io/styleguide/cppguide.html). The program has been checked with `valgrind` for memory leakage. The result of the Profiling shows that on the pricer.in file and size-target 200, at peak just 5.1 megabyte used. Look at the end of this page for more info about the profiling.
The code could be optimized for more clarity using better design pattern but not done because of the time constraints.

### Questions
#### What is the time complexity for processing an Add Order message?
A STL multimap been used for storing the orders sorted by their prices. Since multimap implemented using red-black tree, the insert have runtime complexity of O(log n).

#### What is the time complexity for processing a Reduce Order message?
A STL unordered_map been used for storing the order-id to an iterator into the orders multimap. Since unordered_map implemented using hash map, the lookup have runtime complexity of O(1) and erasing from orders multimap using iterator have runtime complexity of O(1) thus in total it takes time complexity of Reduce Order is O(1).

#### If your implementation were put into production and found to be too slow, what ideas would you try out to improve its performance?
• CPU Profiling and Optimization and rewriting parts that are bottlenecks
• Memory optimization using tools like valgrind
• Using GPU computing
• Using data types that have less overheads • Using better Hash functions
• Using cache where it’s possible


### CPU Profiling of the Pricer program
![CPU Profiling](https://github.com/panaali/orderbook/blob/master/img/CPU_Profiling.png)

### Memory Profiling of the Pricer program
![Memory Profiling](https://github.com/panaali/orderbook/blob/master/img/Memory_Profiling.png)

## License[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)
To the extent possible under law, [panaali](https://github.com/panaali) has waived all copyright and related or neighboring rights to this work.
