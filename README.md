# httpQL
run SQL via http protocol

Specification only, No implementation, No opinion, Simple, Flexible, Powerful


The demon is a httpQL implementation in Node.js. You are free to write your own implementation in any language. 


CRUD Operations 

The HTTP methods, which are used for RESTful Web Services, map neatly to the common SQL statements:
 
 
    CRUD OPERATION	            HTTP METHOD	          SQL STATEMENT
    
     Create	                      POST	                 INSERT
     Read	                        GET	                  SELECT
     Update	                      PUT      	            UPDATE
     Delete	                      DELETE	               DELETE
     
     
     




1. SQL select :  
 
                    select * from your_table
                    
    
        
      http:
      
                     GET /your_table
      URL :                
      
                    ...context/your_table?select=*   
                    
                    
                    
                    
                    
2. SQL select with where, order by, sort clause:

                    select * from your_table where type='xxx' order by 'yyy' asc
          
      http:
      
                     GET /your_table    
          
      URL :  
                    
                    where type='xxx' must be encoded as type%3D%27xxx%27
                    
                    ...context/your_table?select=*
                                                  &where=type%3D%27xxx%27
                                                  &orderby=yyy
                                                  &asc_desc=asc
3. SQL select multiple table :

                    select * from a, b, c where a.id=b.customerID .....
                    
                    You should create a view for this complex sql as your_view
    
     http:
      
                     GET /your_view    
    
     URL:             
     
                    ...context/your_view?select=* 
        
        
        
4. SQL insert

           INSERT INTO your_table (column_name1, column_name2, column_name3) 
           VALUES ("value1", "value2", "value3");       
           
     http: 
     
             POST /your_table

             {
                "column_name1": "value1",
                "column_name2": "value2",
                "column_name3": "value3"
             }
        
    
    URL:   
              
              must provide security pin for update, insert, delete, no pin required for select 
              
              ...context/your_table?pin=123456789    
               
                {
                    "column_name1": "value1",
                    "column_name2": "value2",
                    "column_name3": "value3"
                }
                
                
                
        
5. SQL update all columns: 

                   UPDATE your_table
                      SET id = 2, 
                          column_name1 = "value1", 
                          column_name2 = "value2", 
                          column_name3 = "value3"
                    WHERE id = 2;        
                    
                    
     http: 
             
            
             PUT /your_table?where=id=2

             {
                "id": 2, 
                "column_name1": "value1",
                "column_name2": "value2",
                "column_name3": "value3"
             }
                    
                    
    URL:    
    
           where clause  id = 2 must be encoded as id%3D2
        
           ...context/your_table? where=id%3D2 & pin=123456789 

             {
                "id": 2, 
                "column_name1": "value1",
                "column_name2": "value2",
                "column_name3": "value3"
             }


6. SQL update only some of the columns:

                 UPDATE your_table
                 SET column_name3 = "value3"
                 WHERE id = 2;

     http: 
             
             
            
             PATCH /your_table?where=id=2

             {
                  "column_name3": "value3"
             }
             

     URL:    
      
              where id = 2 must be encoded as id%3D2
              
                ...context/your_table? where=id%3D2 & pin=123456789 

                     {
                       "column_name3": "value3"
                     }
             
             
     Note: 
     
              The difference between PUT and PATCH is that PUT must update all fields to make it idempotent. 
              This fancy word basically means that you must always get the same result no matter how many times it is executed. 
              This is important in network traffic, because if you’re in doubt whether your request has been lost during transmission, 
              you can just send it again without worrying about messing up the resource’s data.           
              
              
              

7. SQL delete:


               DELETE FROM your_table WHERE id = 2;
               

      http: 
             
               DELETE /your_table?where=id=2
               
               
      URL:     
      
                where id = 2 must be encoded as id%3D2
              
                ...context/your_table? where=id%3D2 & pin=123456789 



              
