# assignment19.1
1.
create external table 
customer_hive
(
 id String,name string,location string,age int
)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
with serdeproperties 
("hbase.columns.mapping"=":key,details:name,details:location,details:age")
tblproperties("hbase.table.name"="customer");

SELECT 
       MAX(s.age), 
       MIN(s.age) 
FROM customer_hive s
ORDER BY s.age;


2.

raw_data = LOAD '/home/acadgild/downloads/customers' USING PigStorage(',') AS (
           id:chararray,
           name:chararray,
           location:chararray,
           age:int,
        
);

STORE raw_data INTO 'hbase://customers' USING org.apache.pig.backend.hadoop.hbase.HBaseStorage(
'customers_data:name 
 customers_data:location 
 customers_data:age' 
 
);

PIG_CLASSPATH=/home/acadgild/lib/hbase/hbase.jar:/home/aadgild/lib/zookeeper/zookeeper-3.4.5-cdh4.4.0.jar /home/acadgild/bin/pig /home/acadgild/Load_HBase_Customers.pig

scan 'customers'
