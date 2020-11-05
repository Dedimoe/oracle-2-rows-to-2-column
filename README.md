# oracle-2-rows-to-2-column
oracle query for 2 rows became 2 columns

1. create the table :
```
create table test ( App_ID Number, User_Id Number, strKey varchar2(10), strValue varchar2(20));
```

2. Insert the data :
```
insert into kvtest select 1, 100, 'Color', 'Blue' from dual;
insert into kvtest select 1, 200, 'Location', 'USA' from dual;
commit;
```

The output will be :
```
The output of select * from kvtest:
    APP_ID    USER_ID STRKEY     STRVALUE            
---------- ---------- ---------- --------------------
         1        200 Location   USA                 
         1        100 Color      Blue      
```


I'd like to combine key:values by an app_id regardless of the user_id which in this example will always be limited to 2 rows (meaning there will not be 3 user_ids per app_id, only 2).

```
My desired output would look like this:
    APP_ID LOCATION                         COLOR                           
---------- -------------------------------- ------------
         1 USA                              Blue       
```

3. The solutions :
```
with rws as (
  select app_id, strkey, strvalue from kvtest
)
  select * from rws
  pivot ( 
    min ( strvalue ) for strkey in (
      'Color' color, 'Location' location
    )
  );
```

The output :
```
APP_ID   COLOR   LOCATION   
       1 Blue    USA   
```

Copas from (this)[https://asktom.oracle.com/pls/apex/f?p=100:11:0::::P11_QUESTION_ID:9538490400346215005].
