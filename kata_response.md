# Bank account kata
Think of your personal bank account experience When in doubt, go for the simplest solution

# Requirements
- Deposit and Withdrawal
- Account statement (date, amount, balance)
- Statement printing
 
# User Stories
##### US 1:
**In order to** save money  
**As a** bank client  
**I want to** make a deposit in my account  
 
##### US 2: 
**In order to** retrieve some or all of my savings  
**As a** bank client  
**I want to** make a withdrawal from my account  
 
##### US 3: 
**In order to** check my operations  
**As a** bank client  
**I want to** see the history (operation, date, amount, balance)  of my operations  


Proposition de solution: 

##### US1: 
The service will do
1. Call the service to get current amount of client's bank account from database
2. Add the deposit amount to the current amount
3. Update in the database the new current amount for the bank client


##### US2:
The service will do
1. Call the service to get current amount of client's bank account from database
2. Make the withdrawal from the current amount
3. Update in the database the new current amount for the bank client


##### US3:
The service will do
1. Call the service to get operations' list between two dates of client's bank account from database
2. For each operation, load the details of operation, date, amount and balance in a business object list
3. According to the customer's choice, display this list in the IHM or launch the printing service 