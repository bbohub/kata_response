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


##### US1, US2, US3: 

public interface BankService {

void depositService(int clientId, int accountNumber, int amountOfDeposit);

void withdrawalService(int clientId, int accountNumber, int amountOfWithdrawal);

AccountOperationsDTO getAccountOperationsDTO(int clientId, int accountNumber, Date startDate, Date endDate);

}

@Service
public class BankServiceImpl implements BankService {

	@Autowired
	private DatabaseService dbService;

	@Override
	public void depositService(int clientId, int accountNumber, int amountOfDeposit){
	
		BankAccount bankAccount = dbService.getBankAccount(clientId, accountNumber);
		int currentBankAccountAmount = bankAccount.getCurrentBankAccountAmount();
		currentBankAccountAmount += amountOfDeposit;
		bankAccount.setCurrentBankAccountAmount(currentBankAccountAmount);
		dbService.updateBankAccount(bankAccount);
	
	}
	
	@Override
	public void withdrawalService(int clientId, int accountNumber, int amountOfWithdrawal){
	
		BankAccount bankAccount = dbService.getBankAccount(clientId, accountNumber);
		int currentBankAccountAmount = bankAccount.getCurrentBankAccountAmount();
		currentBankAccountAmount -= amountOfWithdrawal;
		bankAccount.setCurrentBankAccountAmount(currentBankAccountAmount);
		dbService.updateBankAccount(bankAccount);
	
	}
	
	@Overidde
	public AccountOperationsDTO getAccountOperationsDTO(int clientId, int accountNumber, Date startDate, Date endDate){
	
		
		AccountOperations accountOperations = dbService.getAccountOperations(clientId, accountNumber, startDate, endDate);
		return loadAccountOperationsDTO(accountOperations);
		
	}
	
	private AccountOperationsDTO loadAccountOperationsDTO(accountOperations){
		
		AccountOperationsDTO accountOperationsDTO = new AccountOperationsDTO();
		accountOperationsDTO.setClientId(accountOperations.getClientId());
		accountOperationsDTO.setAccountNumber(accountOperations.getAccountNumber());
		
		List<OperationDTO> operationsList = new ArrayList<>();
		
		for(i=0; i<accountOperations.getOperations().length(); i++){
		
		operationsList.add(new OperationDTO(accountOperations.getOperations().get(i).getDate(),accountOperations.getOperations().get(i).getOperation(),accountOperations.getOperations().get(i).getAmount(),accountOperations.getOperations().get(i).getBalance()););
		
		}
		
		accountOperationsDTO.setOperationsList(operationsList);
		
		return accountOperationsDTO;
	}
	
	public DatabaseService getDatabaseService(){
		return dbService;
	}
	
	public void setDatabaseService(DatabaseService dbService){
		this.dbService= dbService;
	}
}

public class AccountOperationsDTO {
	
	private int clientId;
	private int accountNumber;
	private List<OperationDTO> operationsList;
	
	public AccountOperationsDTO(int clientId, int accountNumber, List<OperationDTO> operationsList){
		this.clientId = clientId;
		this.accountNumber = accountNumber;
		this.operationsList = operationsList;
	}
	
	//getter and setter
}

public class OperationDTO {

	private Date date;
	private String operation;
	private double amount;
	private double balance;

	//Constructor getter and setter
	

}