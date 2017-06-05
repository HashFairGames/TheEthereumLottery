# TheEthereumLottery
  Lottery Dapp on top of Ethereum which solves problem of random numbers in blockchain

# How to Play
  To play you need to pick 4 numbers (range 0-255) and provide them sorted to Play() function.  
  To win you need to hit at least 1 number out of 4 WinningNums which will be announced once every week
  (or more often if the lottery will become more popular). If you hit all of the 4 numbers you will win
  about 10 million times more than you payed for lottery ticket.  
  The exact values are always provided as GuessXOutOf4 entries in Ledger - notice that
  they are provided in Wei, not Ether (10^18 Wei = Ether).  
  Use Withdraw() function to pay out.

# How much you can win?
  At the moment if you guess X out of 4 Winning Numbers you will win:  
  1 out of 4:      0.01 ETH  
  2 out of 4:      0.06 ETH  
  3 out of 4:      8.8  ETH  
  4 out of 4: 10,000    ETH  

# Cost
  At the moment, you need to send 0.001 ETH to play.  
  You can always verify cost of play function - every draw has "Price of ticket" field with exact cost measured in Wei (10^-18 ETH)  
  The Play() function will reject any amount of ETH which does not equal "Price of ticket" field, so you can not loose your ETH
  on invalid bet (non sorted values will also be rejected). Any other function is free and reject any ETH you send to it.  

# Contract Address:
    0x9473BC8BB575Ffc15CB2179cd9398Bdf5730BF55

# JSON interface:

    [{"constant":true,"inputs":[],"name":"Announcements","outputs":[{"name":"","type":"string"}],"payable":false,"type":"function"},{"constant":true,"inputs":[],"name":"IndexOfCurrentDraw","outputs":[{"name":"","type":"uint256"}],"payable":false,"type":"function"},{"constant":true,"inputs":[{"name":"","type":"uint256"}],"name":"ledger","outputs":[{"name":"WinningNum1","type":"uint8"},{"name":"WinningNum2","type":"uint8"},{"name":"WinningNum3","type":"uint8"},{"name":"WinningNum4","type":"uint8"},{"name":"ClosingHash","type":"bytes32"},{"name":"OpeningHash","type":"bytes32"},{"name":"Guess4OutOf4","type":"uint256"},{"name":"Guess3OutOf4","type":"uint256"},{"name":"Guess2OutOf4","type":"uint256"},{"name":"Guess1OutOf4","type":"uint256"},{"name":"PriceOfTicket","type":"uint256"},{"name":"ExpirationTime","type":"uint256"}],"payable":false,"type":"function"},{"constant":true,"inputs":[{"name":"TheRand","type":"bytes32"}],"name":"CheckHash","outputs":[{"name":"OpeningHash","type":"bytes32"}],"payable":false,"type":"function"},{"constant":true,"inputs":[{"name":"DrawIndex","type":"uint8"},{"name":"PlayerAddress","type":"address"}],"name":"MyBet","outputs":[{"name":"Nums","type":"uint8[4]"}],"payable":false,"type":"function"},{"constant":true,"inputs":[],"name":"referral_fee","outputs":[{"name":"","type":"uint8"}],"payable":false,"type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"referral_ledger","outputs":[{"name":"","type":"uint256"}],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"MyNum1","type":"uint8"},{"name":"MyNum2","type":"uint8"},{"name":"MyNum3","type":"uint8"},{"name":"MyNum4","type":"uint8"}],"name":"Play","outputs":[],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"DrawIndex","type":"uint32"}],"name":"Withdraw","outputs":[],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"DrawIndex","type":"uint32"}],"name":"Refund","outputs":[],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"MyNum1","type":"uint8"},{"name":"MyNum2","type":"uint8"},{"name":"MyNum3","type":"uint8"},{"name":"MyNum4","type":"uint8"},{"name":"ref","type":"address"}],"name":"PlayReferred","outputs":[],"payable":true,"type":"function"},{"constant":false,"inputs":[],"name":"Withdraw_referral","outputs":[],"payable":false,"type":"function"},{"constant":false,"inputs":[],"name":"Deposit_referral","outputs":[],"payable":true,"type":"function"},{"anonymous":false,"inputs":[{"indexed":true,"name":"IndexOfDraw","type":"uint256"},{"indexed":false,"name":"OpeningHash","type":"bytes32"},{"indexed":false,"name":"PriceOfTicketInWei","type":"uint256"},{"indexed":false,"name":"WeiToWin","type":"uint256"}],"name":"NewDrawReadyToPlay","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"IndexOfDraw","type":"uint32"},{"indexed":false,"name":"WinningNumber1","type":"uint8"},{"indexed":false,"name":"WinningNumber2","type":"uint8"},{"indexed":false,"name":"WinningNumber3","type":"uint8"},{"indexed":false,"name":"WinningNumber4","type":"uint8"},{"indexed":false,"name":"TheRand","type":"bytes32"}],"name":"DrawReadyToPayout","type":"event"},{"anonymous":false,"inputs":[{"indexed":false,"name":"Wei","type":"uint256"}],"name":"PlayerWon","type":"event"}]


# About
  The advantage of TheEthereumLottery is that it uses secret random value which only owner knows (called TheRand).
  A hash of TheRand (called OpeningHash) is announced at the beginning of every draw (lets say draw number N) - 
  at this moment ticket price and the values of GuessXOutOf4 are publicly available and can not be changed.  
  When draw N+1 is announced in a block X, a hash of block X-1 is assigned to ClosingHash field of draw N. 
  After few minutes, owner announces TheRand which satisfy following expression: sha3(TheRand)==drawN.OpeningHash
  then Rand32B=sha3(TheRand, ClosingHash) is calculated an treated as a source for WinningNumbers.  
  For design purposes at this moment ClosingHash is changed to Rand32B as it might be more interesting for someone 
  watching lottery ledger to see that number instead of hash of some block.  

# Why this way?
  This approach (1) unable players to cheat, because as long as no one knows TheRand, 
  no one can predict what WinningNums will be, (2) unable the owner to influence the WinningNums (in order to
  reduce average amount won) because OpeningHash=sha3(TheRand) was public before bets were made, and (3) reduces 
  owner capability of playing it's own lottery and making winning bets to very short window of one
  exactly the same block as new draw was announced - so anyone, with big probability, can think that if winning
  bet was made in this particular block - probably it was the owner, especially if no more bets were made 
  at this block (which is very likely).

# Timings
  Withdraw is possible only after TheRand was announced (once every weekend, few minutes after opening next week draw).  
  Players can use Refund function in order to refund their ETH used to make bet if the owner will not announce 
  TheRand in ~2 weeks.  
  That moment is called ExpirationTime on contract Ledger (which is visible from JSON interface).





# verify the code
verified code with comments (and Player JSON Interface in it):   
https://etherscan.io/address/0x9473BC8BB575Ffc15CB2179cd9398Bdf5730BF55#code   
