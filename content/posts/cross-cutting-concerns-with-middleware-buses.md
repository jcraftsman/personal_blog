---
title: "Cross Cutting Concerns With Middleware Buses"
date: 2020-02-17T18:00:10+01:00
draft: true
---



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