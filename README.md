# Malicious-contracts-using-similarABI
External calls to malicious contracts that have similar looking ABI 


/**
 * Attack breakdown
  
 1. the fact about Malicious.sol is that it will generate the same ABI as Helper.sol eventhough it has different code within it.
 
 2. This is because ABI only contains function definitions for public variables, functions and events. So Malicious.sol can be typecasted as Helper.sol.
 
 3.  Now because Malicious.sol can be typecasted as Helper.sol, 
    a malicious owner can deploy Good.sol with the address of Malicious.sol instead of Helper.sol 
    and users will believe that he is indeed using Helper.sol to create the eligibility list.
  
 4. In our case, the scam will happen as follows. The scammer will first deploy Good.sol --- with the address of Malicious.sol. 
  
 5. Then when the user enters the eligibility list using addUserToList function which will work fine because the code for this function is same within Helper.sol and Attack.sol.
  
 6. when the tries to call isUserEligible with his address ---
    this function will always return false because it calls Malicious.sol's isUserEligible function which always returns false 
    except when its the owner itself, which was not supposed to happen.
PREVENTION 
1. Make the address of the external contract public and also get your external contract verified so that all users can view the code
2. instead of typecasting an address into a contract inside the constructor -- Create a new contract
   So instead of doing Helper(_helper) where you are typecasting _helper address into a contract--- which may or may not be the Helper contract, 
    create an explicit new helper contract instance using new Helper().
    contract Good {
    Helper public helper;
    constructor() {
        helper = new Helper();
}

*/
