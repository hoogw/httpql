# HttpQL



[ HttpQL specification v1.0 ](https://transparentgov.net/httpql/index.html)

## Run SQL via http protocol

The HTTP methods, which are used for RESTful Web Services, map neatly to the common SQL statements:
 
 
   | CRUD OPERATION	 |    HTTP METHOD	  | SQL STATEMENT    |
   |:----------------:|:----------------:|:----------------:|
   |  Create	       |    POST	        |   INSERT         |
   |  Read	          |    GET	        |   SELECT         |
   |  Update	       |    PUT      	  |   UPDATE         |
   |  Delete	       |    DELETE	     |   DELETE         |
     
     
     




## 1. SQL select
> select * from your_table
                    
    
##### http
> GET /your_table


##### URL                
> https://...context.../your_table



##### Example 1
                  
  Get all arcgis server list

> select * from rest_url 

  [https://transparentgov.net:3200/restapi/rest_url](https://transparentgov.net:3200/restapi/rest_url)



##### Example 2

  Get all socrata open data domain
                 
> select * from domain_list

  [https://transparentgov.net:3200/restapi/domain_list](https://transparentgov.net:3200/restapi/domain_list)
                                   
                    
                    
                    
## 2. SQL select with where, order by, sort etc.

> select * from your_table where type='xxx' order by 'yyy' asc
          
##### http
      
> GET /your_table    
          
##### URL 
                     
> https://...context.../your_table?select=*

>                                           &where=type%3D%27xxx%27

>                                           &orderby=yyy

>                                           &asc_desc=asc


>  Note:  where clause type='xxx' must be encoded as type%3D%27xxx%27

##### Example 1

Get all arcgis server list sort by name
                                    
> select * from rest_url where type='folder' or type='hub' order by name asc

[https://transparentgov.net:3200/restapi/rest_url?select=*&orderby=name&asc_desc=asc&where=type%3D'folder'%20or%20type%3D'hub'](https://transparentgov.net:3200/restapi/rest_url?select=*&orderby=name&asc_desc=asc&where=type%3D'folder'%20or%20type%3D'hub')



##### Example 2
 Get all socrata open data domain sort by organization
 
 >select * from domain_list order by organization asc
 
[https://transparentgov.net:3200/restapi/domain_list?select=*&orderby=organization&asc_desc=asc](https://transparentgov.net:3200/restapi/domain_list?select=*&orderby=organization&asc_desc=asc)
                     






## 3. SQL select multiple table :

                    select * from a, b, c where a.id=b.customerID .....
                    
                    You should create a view for this complex sql as your_view
    
     http:
      
                     GET /your_view    
    
     URL:             
     
                    https://...context.../your_view?select=* 
        
        
        
## 4. SQL insert

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
              
              https://...context.../your_table?pin=123456789    
               
                {
                    "column_name1": "value1",
                    "column_name2": "value2",
                    "column_name3": "value3"
                }
                
                
                
        
## 5. SQL update all columns: 

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
        
           https://...context.../your_table? where=id%3D2 & pin=123456789 

             {
                "id": 2, 
                "column_name1": "value1",
                "column_name2": "value2",
                "column_name3": "value3"
             }


## 6. SQL update only some of the columns:

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
              
                https://...context.../your_table? where=id%3D2 & pin=123456789 

                     {
                       "column_name3": "value3"
                     }
             
             
     Note: 
     
              The difference between PUT and PATCH is that PUT must update all fields to make it idempotent. 
              This fancy word basically means that you must always get the same result no matter how many times it is executed. 
              This is important in network traffic, because if you’re in doubt whether your request has been lost during transmission, 
              you can just send it again without worrying about messing up the resource’s data.           
              
              
              

## 7. SQL delete:


               DELETE FROM your_table WHERE id = 2;
               

      http: 
             
               DELETE /your_table?where=id=2
               
               
      URL:     
      
                where id = 2 must be encoded as id%3D2
              
                https://...context.../your_table? where=id%3D2 & pin=123456789 



              
  
      