## ontology layer for entity specification, interpreter
### ontology construction
ontology configuration:
````json
{
   "component":"hr_system",
   "ontology":[
      {
         "attributes":{
            "class":"Address",
            "id": "String",
            "description":"String",
            "x":"Float",
            "y":"Float",
         }
      },
      {
         "attributes":{
            "class":"Employee",
            "id": "String",
            "name":"String",
            "age":"Integer",
            "birthday":"Date",
            "gender":"String"
         },
         "relationships":[
            {
               "name": "report_to",
               "target": "Employee",
            },
            {
               "name": "live_in",
               "target": "Address",
            }  
         ],
         "actions":[
            {
               "name":"send birthday greeting",
               "assessment":"birthday>now-day(3) && birthday<now",       
               "intent":{
                  "precondition":"birthday<now",
                  "target": "Employee",
                  "inputs":["name","birthday"]
                  "action":"send_birthday_greeting"
               }
            }
         ]
      }
   ]
}

{
   "component":"common",
   "ontology":[
      {
         "attributes":{
            "class":"Address",
            "id": "String",
            "description":"String",
            "x":"Float",
            "y":"Float",
         }
      }
   ]
}
````
### parser
system will generate outline code base on the configuration. https://github.com/WillCaptain/outline
````javascript
//macro will be interpreted in entitir interpreter, won't return x, the x here is for type inference
module macro;
let __macro_fetch__ = <a>(condition:Bool)->a;
let __macro_query__ = <a>(condition:Bool)->[a];
let __macro_create_intent__ = (x,channel,method)->{};

//this is created for the ontology configuration
module common;
outline Address = {
   description: String,
   x: Float,
   y: Float
};

let address = Address();

let fetch_address = (condition:Bool)->{
   __macro_fetch__<Address>(condition);
};

module hr_system;
import * from common;

outline Employee = {
   id: String,
   name: String,
   gender: String,
   age: Integer,
   birthday: Date, 
   explain: String,
   _report_to: String,  //this is an extra attribute for relationship
   _live_in: String
};

let employee = Employee();//empty entity for future reference

let fetch_employee = (condition:Bool)->{
   let result = __macro_fetch__<Employee>(condition);
   result{
      report_to = ()-> fetch_employee(employee.id==this._report_to),
      live_in = ()-> fetch_address(address.id==this._live_in)
   }
};

let query_employee = (condition:Bool)->{
   let result = __macro_query__<Employee>(condition);
   result.map(r->r{
      report_to = ()-> fetch_employee(employee.id==this._report_to),
      live_in = ()-> fetch_address(address.id==this._live_in)
   })
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
   .change_report_manager_action(Employee(id="2")) //get one employee to do a action, action name is "change_report_manager_action"
5. Employee(id="1")
   .report_to //get relation. employee report_to manager, so report_to will get his manager
   .name  //get manager's name
6. Employee.create(id="1",name="Will") //class action
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
