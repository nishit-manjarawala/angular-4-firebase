# angular-4-firebase

now we will create new project by type command : ng new angular4firebase

it take littel bit time wait for it

you can see it completed now we will change directory to our project and run command : ng serve 

it will run our app on port 4200

i am using code editor for edit my code 

now in code editor it support command line also to open command line in code press ctrl + open quote

now i will create new component by command : ng g c employees

now in employees create two new component for that in cmd change directory to employees component :cd src\app\employees

now create component:

ng g c employee

ng g c employee-list

clear file src\app\app.component.html


add main emaployees components tag in src\app\app.component.html as below
-----------------------------------------------
<div class="container">
	
  <app-employees></app-employees>
  
</div>
------------------------------------------------
i am using bootstrape so i use container tag for design if you want to use copy th cdn of bootstrap and paste it in index.html which is located in root of project


now i will call both sub component  employee AND employee-list in src\app\employees\employees.component.html as below
-------------------------------------
<h2 class="jumbotron text-center">Angular Crud With Firebase</h2>

<div class="row">
	
  <div class="col-md-7">
	
    <app-employee></app-employee>
    
  </div>
  
  <div class="col-md-5">
	
    <app-employee-list></app-employee-list>
    
  </div>
  
</div>

-------------------------------------

go to google firebase https://console.firebase.google.com and create firebase database and copy api details and paste it in src\environments\environment.ts as below:

export const environment = {

  production: false,
  
  firebaseConfig:{
  
    apiKey: "AIzaSyDkxh1PCK2flRPGX8Niinc9Uim0lvsIwNU",
    
    authDomain: "fir-demo-5a193.firebaseapp.com",
    
    databaseURL: "https://fir-demo-5a193.firebaseio.com",
    
    projectId: "fir-demo-5a193",
    
    storageBucket: "fir-demo-5a193.appspot.com",
    
    messagingSenderId: "136132574212"
    
  }
  
};


now in database RULES tab configure read and write true so anyone can read and write as below and publish it:

{

  "rules": {
  
    ".read": true,
    
    ".write": true 
    
  }
  
}

--------------------------------------------------------------------------------------------------
install firebase library to our project : got to https://github.com/angular/angularfire2 for install it run command : npm install firebase angularfire2 --save

run this command for updated firebase : npm install firebase@4.8.0

now import fire base moduel and firebase database moduel in src\app\app.module.ts as below:

import {AngularFireModule} from 'angularfire2';

import {AngularFireDatabaseModule} from 'angularfire2/database';


and import environment file as below:

import {environment} from '../environments/environment';


now import AngularFireDatabaseModule and initialize angular model in src\app\app.module.ts as below:

imports: [

    BrowserModule,
    
	AngularFireDatabaseModule,
	
    AngularFireModule.initializeApp(environment.firebaseConfig) //here environment variable come from import and firebaseConfigconfig variable come from that we have define our firebase configuration
    
  ],
  
----------------------------------------------------------------------------------------------

now for create services for our project for opratiuon with firebase database:

-> create data folder shared in our main component employess path will be like this: src\app\employees\shared

-> in new terminal change directory to shared folder : cd src\app\employees\shared

-> for create service model i will write command in shared directory : ng g class employee.model


it will create class :

export class Employee.Model {

}

now remove .model from class as below :

export class Employee{

}

now in that Employee class we will diclare our variable for employee as below:

export class Employee{

	$key:string;//this property define for unique key for operation on data
	
    name:string;
    
    position:string;
    
    office:string;
    
    salary:number;
    
}

now we will create service in shared folder as below:

ng g s employee

in this employee service path : src\app\employees\shared\employee.service.ts we will import our employee class where we define employee variable as below:

import {Employee} from './employee.model';

now we will create instance of class as : selectedEmployee:Employee = new Employee();

our service page now look like below:
--------------------------------------------
import { Injectable } from '@angular/core';

import {Employee} from './employee.model';

@Injectable()

export class EmployeeService {

  selectedEmployee:Employee = new Employee();
  
  constructor() { }
  
}

--------------------------------------------

now we will inject this employee service to our main src\app\employees\employees.component.ts as below:

import {EmployeeService} from './shared/employee.service';

after import service we have to inject it to providers

as below:

providers:[EmployeeService]


now we will create object of our employee service in constructor as below:

constructor(private employeeService : EmployeeService) { }

our full code src\app\employees\employees.component.ts will look like below:
------------------------------------------------

import { Component, OnInit } from '@angular/core';

import {EmployeeService} from './shared/employee.service';

@Component({

  selector: 'app-employees',
  
  templateUrl: './employees.component.html',
  
  styleUrls: ['./employees.component.css'],
  
  providers:[EmployeeService]
  
})

export class EmployeesComponent implements OnInit {

  constructor(private employeeService : EmployeeService) { }

  ngOnInit() {
  
  }

}
------------------------------------------------

now as main employees component we will import our service in employee component and employee-list component 

src\app\employees\employee\employee.component.ts code look as below:
-------------------------------------------------

import { Component, OnInit } from '@angular/core';

import {EmployeeService} from '../shared/employee.service';

@Component({

  selector: 'app-employee',
  
  templateUrl: './employee.component.html',
  
  styleUrls: ['./employee.component.css']
  
})

export class EmployeeComponent implements OnInit {

  constructor(private employeeService : EmployeeService) { }

  ngOnInit() {
  }

}
-------------------------------------------------
src\app\employees\employee-list\employee-list.component.ts code look as below:
-------------------------------------------------

import { Component, OnInit } from '@angular/core';

import {EmployeeService} from '../shared/employee.service';

@Component({

  selector: 'app-employee-list',
  
  templateUrl: './employee-list.component.html',
  
  styleUrls: ['./employee-list.component.css']
  
})

export class EmployeeListComponent implements OnInit {

  constructor(private employeeService : EmployeeService) { }

  ngOnInit() {
  }

}

----------------------------------------------------------------------------------

for work with form first we have to import ngfrom in our component : src\app\employees\employee\employee.component.ts

import {NgForm} from '@angular/forms';

now we have to import form moduel in our app.mouel.ts which is in rout path of app as : src\app\app.module.ts

import {FormsModule} from '@angular/forms';

now FormsModule declare in imports as below
----------------------------------------------
imports: [

    BrowserModule,
    
	AngularFireDatabaseModule,
	
    AngularFireModule.initializeApp(environment.firebaseConfig),
    
    FormsModule
    
  ]

-------------------------------------------------
now we will create form for add employee and edit employee in src\app\employees\employee\employee.component.html as below:

first we will define form tag:

<form #employeeForm="ngForm" (ngSubmit)="onSubmit(employeeForm)"></form>

where #employeeForm="ngForm" is id of form and (ngSubmit) is action when we submit form and onSubmit(employeeForm) is function which get all data of employeeForm to the onSubmit function

now in form i will add text box for name as below

<div class="form-group">
	
	<label for="">Name</label>
	
	<input type="text" class="form-control" name="name"  #name="ngModel" [(ngModel)]="employeeService.selectedEmployee.name">
	
</div>

where #name="ngModel" use for bind field with model 

we have used [(ngModel)]="employeeService.selectedEmployee.name" where employeeService is private variable which we have define in constructor for employee service and selectedEmployee is instance we have created in service model and name is property we declare in class



now i will add all column as above and full code for src\app\employees\employee\employee.component.html will be as below:
-----------------------------------------------------------------------------------

<form #employeeForm="ngForm" (ngSubmit)="onSubmit(employeeForm)">

    <div class="form-group">
    
        <label for="">Name</label>
	
        <input type="text" class="form-control" name="name" #name="ngModel" [(ngModel)]="employeeService.selectedEmployee.name" placeholder="Full Name">
	
    </div>
    
    <div class="form-group">
    
        <label for="">Position</label>
	
        <input type="text" class="form-control" name="position" #name="ngModel" [(ngModel)]="employeeService.selectedEmployee.position" placeholder="Position">
	
    </div>
    
    <div class="form-group">
    
        <label for="">Office</label>
	
        <input type="text" class="form-control" name="office" #name="ngModel" [(ngModel)]="employeeService.selectedEmployee.office" placeholder="Office">
	
    </div>
    
    <div class="form-group">
    
        <label for="">Salary</label>
	
        <input type="text" class="form-control" name="salary" #name="ngModel" [(ngModel)]="employeeService.selectedEmployee.salary" placeholder="Salary">
	
    </div>
    
    <div class="form-group">
    
        <button type="submit" class="btn btn-default">Submit</button>
	
        <button type="button" class="btn btn-default">Delete</button>
	
        <button type="button" class="btn btn-default">Reset</button>
	
    </div>
    
</form>
--------------------------------------------------------------------------------------
Now lets start implementing crud

in src\app\employees\shared\employee.service.ts import database and database list

import {AngularFireDatabase,AngularFireList} from 'angularfire2/database';

now we will create object of AngularFireDatabase as below in constructor

constructor(private firebase:AngularFireDatabase) { }

now we will define employeeList of any datatype for our employees list

employeeList:AngularFireList<any>;

now we will create function for get data from fire base database as follow:
---------------------------------------------------------------------------------

getData(){

	this.employeeList=this.firebase.list('employees');
	
	return this.employeeList;
	
}

-------------------------------------------------------------------------------
now we will create new function for insert data in firebase database
-------------------------------------------------------------------------------

insertEmployee(employee : Employee){

	this.employeeList.push({
	
	  name:employee.name,
	  
	  position:employee.position,
	  
	  office:employee.office,
	  
	  salary:employee.salary,
	  
	});
	
}
-------------------------------------------------------------------------------

import AngularFireList in src\app\employees\employee-list\employee-list.component.ts:

import {AngularFireList} from 'angularfire2/database';

for declare employeelist variable of Employee data type import employee service model:

import { Employee } from '../shared/employee.model';

define variable:

employeelist:AngularFireList<Employee>;

in ngOnInit we will fetch data for show in list:
-----------------------------------------------------------------------------------
ngOnInit() {

    this.employeeService.getData();
    
  }
-----------------------------------------------------------------------------------
now for insert and update we will create function in src\app\employees\employee\employee.component.ts
---------------------------------------------------------------------------------
onSubmit(form:NgForm){

    this.employeeService.insertEmployee(form.value);
    
  }
---------------------------------------------------------------------------------

Now insert record in for and check database you can see record is added :)

for reset form we will create function:
-------------------------------------------------------------------------------------
resetForm(form?:NgForm){ //i have use ? mark because we will call this function on ngOnInit for reset form and that time form value not assign so 

	if(form!=null) //check if form not then only it will null form
	
    form.reset();
    
    this.employeeService.selectedEmployee = {
    
      $key:null,
      
      name:"",
      
      position:"",
      
      office:"",
      
      salary:0//salary is number type so assign it 0
      
    }
    
  }
-------------------------------------------------------------------------------------
call reset function after insert in onSubmit function as below:
-------------------------------------------------------------------------------------
onSubmit(form:NgForm){

    this.employeeService.insertEmployee(form.value);
    
    this.resetForm(form);
    
  }
-------------------------------------------------------------------------------------
call reset on ngOnInit:
-------------------------------------------------------------------------------------
ngOnInit() {

    this.resetForm();
    
  }
-------------------------------------------------------------------------------------
call reset function on reset button as below:

<button type="button" class="btn btn-default" (click)="resetForm()">Reset</button>

------------------We Will see littel bit for validation you can skip this -----------------------------

you can use required to fields in form if you use required and you didn't written in it it will add ng-invalid and if you add text it will remove ng-invalid and add class ng-valid

when form load all field is ng-untouched class if you write anything in field it will change remove ng-untouched class and add class ng-dirty and ng-touched

so for validation i will add following css in src\styles.css:

input.ng-invalid.ng-dirty{

    border:1px solid red;
    
}

if we want to stop submiting value if form has not valid value we can disable our submit button as below:

<button type="submit" class="btn btn-default" [disabled]="!employeeForm.valid">Submit</button>

------------------------------------------------------------------------------------------------------------

Lets back to the main topic :)

for showing the list go to src\app\employees\employee-list\employee-list.component.ts :

we have declare employeelist:AngularFireList<Employee>;
	
but we want it's in array for dispaly to web so we will change it as:

employeelist:Employee[];


in  ngOnInit() we have fetch data  as below:
------------------------------------------------------------------------------------------------------------
ngOnInit() {

    var x= this.employeeService.getData();
    
    x.snapshotChanges().subscribe(item=>{
    
      this.employeelist=[];
      
      item.forEach(element=>{
      
        var y=element.payload.toJSON();
	
        y["$key"]=element.key;
	
        this.employeelist.push(y as Employee);
	
      });
      
    });
    
  }
  
------------------------------------------------------------------------------------------------------------
now we will display that data in src\app\employees\employee-list\employee-list.component.html
------------------------------------------------------------------------------------------------------------

<ul class="list-group hover">

  <li class="list-group-item" *ngFor="let employee of employeelist">
  
    {{employee.name}} - {{employee.position}}
    
  </li>
  
</ul>

------------------------------------------------------------------------------------------------------------
now you can check out put in browser there will be disolay list of employee

now we will will form for update when click on the list for that add onclick event to lis as  below:
------------------------------------------------------------------------------------------------------------
<ul class="list-group hover">

  <li class="list-group-item" *ngFor="let employee of employeelist" (click)="onItemClick(employee)" >
  
    {{employee.name}} - {{employee.position}}
    
  </li>
  
</ul>
------------------------------------------------------------------------------------------------------------
create function onItemClick inside src\app\employees\employee-list\employee-list.component.ts
------------------------------------------------------------------------------------------------------------
onItemClick(emp : Employee){

    this.employeeService.selectedEmployee=Object.assign({},emp); // by this we will give copy of emp to selectedEmployee
    
  }
------------------------------------------------------------------------------------------------------------
now need to add one more field for key in the form we will use hidden filed for that

<input type="hidden" name="$key" $key="ngModel" [(ngModel)]="employeeService.selectedEmployee.$key" >

now we will create update function in src\app\employees\shared\employee.service.ts
------------------------------------------------------------------------------------------------------------
updateEmployee(emp : Employee){

    this.employeeList.update(emp.$key,{
    
      name:emp.name,
      
      position:emp.position,
      
      office:emp.office,
      
      salary:emp.salary
      
    })
    
  } 
------------------------------------------------------------------------------------------------------------
now we will call updateEmployee function in onSubmit function in src\app\employees\employee\employee.component.ts

we are using same button for insert and update for that we will modify some code in onSubmit function in src\app\employees\employee\employee.component.ts

-----------------------------------------------------------------------------------------------------------
onSubmit(form:NgForm){

    if(form.value.$key==null)
    
      this.employeeService.insertEmployee(form.value);
      
    else
    
    this.employeeService.updateEmployee(form.value);
    
    this.resetForm(form);
  }
-----------------------------------------------------------------------------------------------------------
now you can see update working properly :)

now for delete first if there is no record selected delete button will hide in : src\app\employees\employee\employee.component.html
<button type="button" class="btn btn-default" *ngIf="employeeService.selectedEmployee.$key!=''">Delete</button>

now we will create function for delete and we will call it on delete button as below

<button type="button" class="btn btn-default" *ngIf="employeeService.selectedEmployee.$key!=null" (click)="onDelet(employeeForm)">Delete</button>

create function in services for delete in : src\app\employees\shared\employee.service.ts
-----------------------------------------------------------------------------------------------------------
deleteEmployee(key:string){

    this.employeeList.remove(key);
    
  }
-----------------------------------------------------------------------------------------------------------
now call this function on onDelet in src\app\employees\employee\employee.component.ts
-----------------------------------------------------------------------------------------------------------
onDelet(form:NgForm){

    if(confirm("Are you sure want to delete?")==true){
    
      this.employeeService.deleteEmployee(form.value.$key);
      
	  this.resetForm(form);
	  
    }
    
  }
-----------------------------------------------------------------------------------------------------------

check Your Delete functionality working :)

Thank You.....
