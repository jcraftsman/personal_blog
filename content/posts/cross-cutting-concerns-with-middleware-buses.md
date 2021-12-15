---
title: "Cross Cutting Concerns With Pipes and filters / Middleware Buses"
date: 2020-02-17T18:00:10+01:00
draft: true
---


- <https://codeopinion.com/separating-concerns-with-pipes-filters/>
- <https://www.enterpriseintegrationpatterns.com/PipesAndFilters.html>


```java
 public String doSomething(String input)
{
      //Logging
        System.out.println("entering business method with:"+input);

        //Security check for authorization of action (business-method) 

        //transactionality
        try
        {
            //Start new session and transaction
            
            //Some business logic
            
            //Commit transaction
        }
        catch(Exception e)
        {
            //Rollback transaction 
        }
        finally
        {
            //Close session
        }
        
        //Logging
        System.out.println("exiting business method with:"+input);

      return input;
}
```

Example:
- Logging
- Validation
- Caching
- Retry

Example:
- Decrypt
- Authenticate
- Dedulicate