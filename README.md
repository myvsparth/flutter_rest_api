# FLUTTER REST API
 How to integrate REST API in Flutter

## Introduction:
 In this article we will learn how to integrate REST API in flutter app. As we know that now a days almost all the app uses remote data using APIs. This article will be the crucial part for any developer who wants to make their future in flutter. So let’s understand step by step how to integrate rest api in flutter.

 We are using http plugin for call REST API from the app. http will provide get, post, put, read etc method for send and receive data from remote locations.

 We have used dummy sample for rest api REST API Sample URL for this article. You can use your project API.

## Output:
![List of Employees](https://raw.githubusercontent.com/myvsparth/flutter_rest_api/master/screenshots/1.png)
![Add New Employee](https://raw.githubusercontent.com/myvsparth/flutter_rest_api/master/screenshots/2.png)

# Plugin Required: http: ^0.12.0+2

## Programming Steps:
1. First and basic step to create new application in flutter. If you are a beginner in flutter then you can check my blog Create a first app in Flutter. I have created app named as “flutter_rest_api”

2. Open the pubspec.yaml file in your project and add the following dependencies into it.
```
dependencies:
 flutter:
   sdk: flutter
 cupertino_icons: ^0.1.2
 http: ^0.12.0+2
```

3. Create new file named as “rest_api.dart” for configure rest api url and functions for send and receive data. Following is the programming implementation for rest api in app.
```
import 'dart:convert';
 
import 'package:http/http.dart' as http;
 
class URLS {
 static const String BASE_URL = 'http://dummy.restapiexample.com/api/v1';
}
 
class ApiService {
 static Future<List<dynamic>> getEmployees() async {
   // RESPONSE JSON :
   // [{
   //   "id": "1",
   //   "employee_name": "",
   //   "employee_salary": "0",
   //   "employee_age": "0",
   //   "profile_image": ""
   // }]
   final response = await http.get('${URLS.BASE_URL}/employees');
   if (response.statusCode == 200) {
     return json.decode(response.body);
   } else {
     return null;
   }
 }
 
 static Future<bool> addEmployee(body) async {
   // BODY
   // {
   //   "name": "test",
   //   "age": "23"
   // }
   final response = await http.post('${URLS.BASE_URL}/create', body: body);
   if (response.statusCode == 200) {
     return true;
   } else {
     return false;
   }
 }
}
```

4. Now, we will consume rest api methods in our app. We have created a widget to display all the employee list. Open main.dart and implement following code.

```
import 'package:flutter/material.dart';
import 'package:flutter_rest_api/rest_api.dart';
 
void main() => runApp(MyApp());
 
class MyApp extends StatelessWidget {
 @override
 Widget build(BuildContext context) {
   return MaterialApp(
     theme: ThemeData(
       primarySwatch: Colors.purple,
     ),
     home: EmployeePage(),
   );
 }
}
 
class EmployeePage extends StatefulWidget {
 @override
 _EmployeePageState createState() => _EmployeePageState();
}
 
class _EmployeePageState extends State<EmployeePage> {
 @override
 Widget build(BuildContext context) {
   return Scaffold(
     appBar: AppBar(
       title: Text('Flutter REST API'),
     ),
     body: FutureBuilder(
       future: ApiService.getEmployees(),
       builder: (context, snapshot) {
         final employees = snapshot.data;
         if (snapshot.connectionState == ConnectionState.done) {
           return ListView.separated(
             separatorBuilder: (context, index) {
               return Divider(
                 height: 2,
                 color: Colors.black,
               );
             },
             itemBuilder: (context, index) {
               return ListTile(
                 title: Text(employees[index]['employee_name']),
                 subtitle: Text('Age: ${employees[index]['employee_age']}'),
               );
             },
             itemCount: employees.length,
           );
         }
         return Center(
           child: CircularProgressIndicator(),
         );
       },
     ),
     floatingActionButton: FloatingActionButton(
       onPressed: () {
         Navigator.push(
           context,
           MaterialPageRoute(
             builder: (context) => AddNewEmployeePage(),
           ),
         );
       },
       tooltip: 'Increment',
       child: Icon(Icons.add),
     ),
   );
 }
}
```

5. To add new employee we have created another page called AddNewEmployeePage
Which will add new employee in the list. Following is the implementation for that I have created widget in main.dart file.

```
class AddNewEmployeePage extends StatefulWidget {
 AddNewEmployeePage({Key key}) : super(key: key);
 
 _AddNewEmployeePageState createState() => _AddNewEmployeePageState();
}
 
class _AddNewEmployeePageState extends State<AddNewEmployeePage> {
 final _employeeNameController = TextEditingController();
 final _employeeAge = TextEditingController();
 @override
 Widget build(BuildContext context) {
   return Scaffold(
     appBar: AppBar(
       title: Text('New Employee'),
     ),
     body: Center(
       child: Padding(
         padding: EdgeInsets.all(10),
         child: Column(
           children: <Widget>[
             TextField(
               controller: _employeeNameController,
               decoration: InputDecoration(hintText: 'Employee Name'),
             ),
             TextField(
               controller: _employeeAge,
               decoration: InputDecoration(hintText: 'Employee Age'),
               keyboardType: TextInputType.number,
             ),
             RaisedButton(
               child: Text(
                 'SAVE',
                 style: TextStyle(
                   color: Colors.white,
                 ),
               ),
               color: Colors.purple,
               onPressed: () {
                 final body = {
                   "name": _employeeNameController.text,
                   "age": _employeeAge.text,
                 };
                 ApiService.addEmployee(body).then((success) {
                   if (success) {
                     showDialog(
                       builder: (context) => AlertDialog(
                         title: Text('Employee has been added!!!'),
                         actions: <Widget>[
                           FlatButton(
                             onPressed: () {
                               Navigator.pop(context);
                               _employeeNameController.text = '';
                               _employeeAge.text = '';
                             },
                             child: Text('OK'),
                           )
                         ],
                       ),
                       context: context,
                     );
                     return;
                   }else{
                     showDialog(
                       builder: (context) => AlertDialog(
                         title: Text('Error Adding Employee!!!'),
                         actions: <Widget>[
                           FlatButton(
                             onPressed: () {
                               Navigator.pop(context);
                             },
                             child: Text('OK'),
                           )
                         ],
                       ),
                       context: context,
                     );
                     return;
                   }
                 });
               },
             )
           ],
         ),
       ),
     ),
   );
 }
}
```

6. Great, you are done with flutter rest api integration. Run this project in device or emulator and check the output.
Conclusion:
In this article we have learned how to implement rest api in flutter. We have used and get and post method of http plugin.

Git: https://github.com/myvsparth/flutter_rest_api

## Related Tags: Flutter, REST API, Android, iOS
