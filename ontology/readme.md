## ontology layer for entity specification, interpreter
self defined ontology specifications. it would be like this: 
1. Employee(id="1") //query entity by id, will get only one employee
2. Employee
   .query(age>30,onbording_data<now)//query list from database, "," means && opertion: age>30 && onbording_data<now  
   .filter(gender=Gender.Male) //not from database, from the list
   .order_by(gender,desc)
   .limit(3)
   .first
   .explain(status) //explain attribute status
4. Employee(id="1"||name="Will")
   .do_action_1 //get one employee to do a action, action name is "do_acion_1"
5. Employee(id="1")
   .report_to //get relation. employee report_to manager, so report_to will get his manager
   .name  //get manager's name
6. Employee
   .created(my_action(emp)) //event
7. global_action(Employee(id="1").get)  //global action
