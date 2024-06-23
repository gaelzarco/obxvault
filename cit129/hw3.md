### Pseudocode for HW3

  1. Write "Please input number of items to purchase"
  2. Input itemsPurchasedAmount
  3. If itemsPurchasedAmount < 1 or itemsPurchasedAmount > 30, Error "Invalid amount of items purchased. Please input a value between 1 and 30, inclusive."
  4. Display "***ITEM NAME ITEM PRICE***"
  5. Set count = 0
  6. totalCost = 0
  7. totalCostBetween = 0
  8. While count < itemsPurchasedAmount
      - Write "Please input item name:"
      - Input itemName
        - Loop
          - Write "Please input item price:"
          - If itemPrice < 0 or itemPrice > 500, Error "Invalid item price. Please input a value between 0 and 500, inclusive."
          - Set totalCost = totalCost + itemPrice
          - if itemPrice >= 10 and itemPrice <= 20, Set totalCostBetween = totalCostBetween + itemPrice 
          - Display itemName + " " + itemPrice
        - End Loop
  9. End While
  10. Set averagePrice = totalCost / itemsPurchasedAmount
  11. Display "*** REPORT ***"
  12. Display "Total of all purchases = $" + totalCost
  13. Display "Total of all purchases between $10 and $20 = $" + totalCostBetween
  14. Display "Average price for all purchases = $" + averagePrice
      
