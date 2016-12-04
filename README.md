# TheEthereumLottery
  Lottery Dapp on top of Ethereum which solves problem of random numbers in blockchain

# How to Play
  To play you need to pick 4 numbers (range 0-255) and provide them sorted to Play() function.  
  To win you need to hit at least 1 number out of 4 WinningNums which will be announced once every week
  (or more often if the lottery will become more popular). If you hit all of the 4 numbers you will win
  about 50 milion times more than you payed fot lottery ticket.  
  The exact values are provided as GuessXOutOf4 entries in Ledger - notice that
  they are provided in Wei, not Ether (10^18 Wei = Ether).  
  Use Withdraw() function to pay out.

# Cost
  At this moment, you need to send 0.1 ETH to play.  
  You can always verify cost of play function - every draw has "Price of ticket" field with exact cost measured in Wei (10^-18 ETH)

# Contract Address:
    0x7698392fff47d8d4CeE21295aD1F31b6ced9ad66

# JSON interface:

    [{"constant":false,"inputs":[{"name":"MyNum1","type":"uint8"},{"name":"MyNum2","type":"uint8"},{"name":"MyNum3","type":"uint8"},{"name":"MyNum4","type":"uint8"}],"name":"Play","outputs":[],"type":"function"},{"constant":false,"inputs":[{"name":"DrawNumber","type":"uint32"}],"name":"Withdraw","outputs":[],"type":"function"},{"constant":true,"inputs":[],"name":"Announcements","outputs":[{"name":"","type":"string"}],"type":"function"},{"constant":false,"inputs":[{"name":"DrawNumber","type":"uint32"}],"name":"Refund","outputs":[],"type":"function"},{"constant":true,"inputs":[],"name":"IndexOfCurrentDraw","outputs":[{"name":"","type":"uint256"}],"type":"function"},{"constant":true,"inputs":[{"name":"","type":"uint256"}],"name":"ledger","outputs":[{"name":"WinningNum1","type":"uint8"},{"name":"WinningNum2","type":"uint8"},{"name":"WinningNum3","type":"uint8"},{"name":"WinningNum4","type":"uint8"},{"name":"TheRand","type":"bytes32"},{"name":"TheHash","type":"bytes32"},{"name":"Guess4OutOf4","type":"uint256"},{"name":"Guess3OutOf4","type":"uint256"},{"name":"Guess2OutOf4","type":"uint256"},{"name":"Guess1OutOf4","type":"uint256"},{"name":"PriceOfTicket","type":"uint256"},{"name":"ExpirencyTime","type":"uint256"}],"type":"function"},{"constant":true,"inputs":[{"name":"DrawNumber","type":"uint8"},{"name":"PlayerAddress","type":"address"}],"name":"MyBet","outputs":[{"name":"Nums","type":"uint8[4]"}],"type":"function"},{"inputs":[],"type":"constructor"},{"anonymous":false,"inputs":[{"indexed":true,"name":"IndexOfDraw","type":"uint256"},{"indexed":false,"name":"TheHash","type":"bytes32"},{"indexed":false,"name":"PriceOfTicketInWei","type":"uint256"},{"indexed":false,"name":"WeiToWin","type":"uint256"}],"name":"NewDrawReadyToPlay","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"IndexOfDraw","type":"uint32"},{"indexed":false,"name":"WinningNumber1","type":"uint8"},{"indexed":false,"name":"WinningNumber2","type":"uint8"},{"indexed":false,"name":"WinningNumber3","type":"uint8"},{"indexed":false,"name":"WinningNumber4","type":"uint8"},{"indexed":false,"name":"TheRand","type":"bytes32"}],"name":"DrawReadyToPayout","type":"event"},{"anonymous":false,"inputs":[{"indexed":false,"name":"Wei","type":"uint256"}],"name":"PlayerWon","type":"event"},{"constant":true,"inputs":[{"name":"Num1","type":"uint8"},{"name":"Num2","type":"uint8"},{"name":"Num3","type":"uint8"},{"name":"Num4","type":"uint8"},{"name":"TheRandomValue","type":"bytes32"}],"name":"CheckHash","outputs":[{"name":"TheHash","type":"bytes32"}],"type":"function"}]

# About
  The advantage of TheEthereumLottery is that it is not using any pseudo-random numbers.  
  The winning numbers are known from the announcement of next draw - at this moment the values of GuessXOutOf4,
  and ticket price is publically available.  
  To unable cheating of contract owner, a hash (called "TheHash" in contract ledger) 
  equal to sha3(WinningNums, TheRand) is provided.  
  While announcing WinningNums the owner has to provide also a valid "TheRand" value, which satisfy 
  following expression: TheHash == sha3(WinningNums, TheRand).
  If hashes do not match - the player can refund his Ether used for lottery ticket.  
  As by default all non existing values equal to 0, from the perspective of blockchain
  the hashes do not match at the moment of announcing next draw from the perspective of blockchain.  
  This is why refund is possible only after 2 weeks of announcing next draw,
  this moment is called ExpirencyTime on contract Ledger.

# verify the code
verified code with comments (and Player JSON Interface in it):  
https://etherscan.io/address/0x7698392fff47d8d4cee21295ad1f31b6ced9ad66#code
