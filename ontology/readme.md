## ontology layer for entity specification, interpreter
### ontology construction
ontology configuration:
````json
{
   "env":{
      "db_connection":"hr"
   },
   "ontology":[{
      "class":"Employee",
      "source":"employee",
      "connection": "$db_connection",
      "name":"String",
      "age":"Integer",
      "onboarding_date":"String",
      "gender":"String",
      "report_to":"",
      "do_action_1":"acion id"
   }]
}
````
### parser
system will generate outline code base on the configuration. https://github.com/WillCaptain/outline
````javascript
//macro will be interpreted in entitir interpreter, won't return x, the x here is for type inference
let __macro_fetch__ = (x, condition:Bool)->x;
let __macro_query__ = (x, condition:Bool)->[x];

//this is created for the ontology configuration
let employee = {
   name = "",
   gender="male",
   age=0,
   onbording_date="",
   explain=""
   do_action_1 = 
};

//query  a list ontologies
let employee_query = (employee:Employee, query: Employee->[Employee]) ->{
   query(employee) 
};
````

### compile json to entitir code

### ontology operations via entitir code
self defined ontology specifications. it would be like this: 
1. Employee(id="1") //query entity by id, will get only one employee
2. Employee
   .query(age>30,onboarding_date<now)//query list from database, "," means && opertion: age>30 && onbording_data<now  
   .filter(gender="male") //not from database, from the list
   .order_by(gender)
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

### compile entitir code into outline code
````javascript
//1.Employee(id="1") will be translate into outline grammar
__macro_fetch__(employee, employee.id=="1")

//2. Employee.query
_macro_query__(employee,employee.age>30 && employee.onboarding_date<date())
.filter(x->x.gender=="male")
.order_by(x->x.gender)
.limit(3)
.first()

//3. Employee(id="1"||name="Will")
__macro_fetch__(employee, employee.id=="1" || employee.name=="Will")
.do_action_1

//
````
### type inference
gcp will do the type inference

### SQL interpration
interpret into SQL
