### Debounce 
```
function debounce(f, ms) {
  var state = null;
  var COOLDOWN = 1;
  
  return function() {
    if (state) return;
    f.apply(this, arguments);
    state = COOLDOWN;
    setTimeout(function() { state = null }, ms);
  }

}

function f(x) { alert(x) }
var f = debounce(f, 1000);

f(1); // Відразу
f(2); 

setTimeout( function() { f(3) }, 100); // -
setTimeout( function() { f(4) }, 1100); // +
setTimeout( function() { f(5) }, 1500); // -

```
### requestAnimationFrame

http://plnkr.co/edit/QKkOvATRaCMQBXSbbCdN?p=preview


### Immutable
```
var map1 = Immutable.Map({a:1, b:2, c:3});
var map2 = map1.set('b', 50);
map1.get('b'); // 2
map2.get('b'); // 50
console.log(map1.get('a'));
console.log(map2)
    
var list = Immutable.List([ 1, 2, 3 ]);
list.push('333');
console.log(list.toJS());
```
### Manifest file
```
<html manifest="assets/cache.manifest">

CACHE MANIFEST
	# v.1.8.2

	CACHE:
	../../assets/icon/All_Restaurants_Sprite.png
	../../assets/icon/Icon_Personalization.png

	../../assets/fonts/Montserrat-Light.otf

	../../app/scripts/jquery-3.1.1.min.js
	NETWORK:
	*
```
### Factory design pattern
```
function Factory() {
    this.createEmployee = function (type) {
        var employee;
 
        if (type === "fulltime") {
            employee = new FullTime();
        } else if (type === "parttime") {
            employee = new PartTime();
        } else if (type === "temporary") {
            employee = new Temporary();
        } else if (type === "contractor") {
            employee = new Contractor();
        }
 
        employee.type = type;
 
        employee.say = function () {
            log.add(this.type + ": rate " + this.hourly + "/hour");
        }
 
        return employee;
    }
}
 
var FullTime = function () {
    this.hourly = "$12";
};
 
var PartTime = function () {
    this.hourly = "$11";
};
 
var Temporary = function () {
    this.hourly = "$10";
};
 
var Contractor = function () {
    this.hourly = "$15";
};
 
// log helper
var log = (function () {
    var log = "";
 
    return {
        add: function (msg) { log += msg + "\n"; },
        show: function () { alert(log); log = ""; }
    }
})();
 
function run() {
    var employees = [];
    var factory = new Factory();
 
    employees.push(factory.createEmployee("fulltime"));
    employees.push(factory.createEmployee("parttime"));
    employees.push(factory.createEmployee("temporary"));
    employees.push(factory.createEmployee("contractor"));
    
    for (var i = 0, len = employees.length; i < len; i++) {
        employees[i].say();
    }
 
    log.show();
}
```

### Abscract factory
```
function Employee(name) {
    this.name = name;
 
    this.say = function () {
        log.add("I am employee " + name);
    };
}
 
function EmployeeFactory() {
 
    this.create = function(name) {
        return new Employee(name);
    };
}
 
function Vendor(name) {
    this.name = name;
 
    this.say = function () {
        log.add("I am vendor " + name);
    };
}
 
function VendorFactory() {
 
    this.create = function(name) {
        return new Vendor(name);
    };
}
 
// log helper
var log = (function () {
    var log = "";
 
    return {
        add: function (msg) { log += msg + "\n"; },
        show: function () { alert(log); log = ""; }
    }
})();
 
function run() {
    var persons = [];
    var employeeFactory = new EmployeeFactory();
    var vendorFactory = new VendorFactory();
 
    persons.push(employeeFactory.create("Joan DiSilva"));
    persons.push(employeeFactory.create("Tim O'Neill"));
    persons.push(vendorFactory.create("Gerald Watson"));
    persons.push(vendorFactory.create("Nicole McNight"));
 
    for (var i = 0, len = persons.length; i < len; i++) {
        persons[i].say();
    }
 
    log.show();
}
```

### Decorator

```
var User = function(name) {
    this.name = name;
 
    this.say = function() {
        log.add("User: " + this.name);
    };
}
 
var DecoratedUser = function(user, street, city) {
    this.user = user;
    this.name = user.name;  // ensures interface stays the same
    this.street = street;
    this.city = city;
 
    this.say = function() {
        log.add("Decorated User: " + this.name + ", " +
                   this.street + ", " + this.city);
    };
}
 
// logging helper
 
var log = (function() {
    var log = "";
 
    return {
        add: function(msg) { log += msg + "\n"; },
        show: function() { alert(log); log = ""; }
    }
})();
 
function run() {
 
    var user = new User("Kelly");
    user.say();
 
    var decorated = new DecoratedUser(user, "Broadway", "New York");
    decorated.say();
 
    log.show();
}
```

# Proxy
```
function GeoCoder() {
 
    this.getLatLng = function(address) {
        
        if (address === "Amsterdam") {
            return "52.3700° N, 4.8900° E";
        } else if (address === "London") {
            return "51.5171° N, 0.1062° W";
        } else if (address === "Paris") {
            return "48.8742° N, 2.3470° E";
        } else if (address === "Berlin") {
            return "52.5233° N, 13.4127° E";
        } else {
            return "";
        }
    };
}
 
function GeoProxy() {
    var geocoder = new GeoCoder();
    var geocache = {};
 
    return {
        getLatLng: function(address) {
            if (!geocache[address]) {
                geocache[address] = geocoder.getLatLng(address);
            }
            log.add(address + ": " + geocache[address]);
            return geocache[address];
        },
        getCount: function() {
            var count = 0;
            for (var code in geocache) { count++; }
            return count;
        }
    };
};
 
// log helper
 
var log = (function() {
    var log = "";
 
    return {
        add: function(msg) { log += msg + "\n"; },
        show: function() { alert(log); log = ""; }
    }
})();
 
function run() {
    var geo = new GeoProxy();
 
    // geolocation requests
 
    geo.getLatLng("Paris");
    geo.getLatLng("London");
    geo.getLatLng("London");
    geo.getLatLng("London");
    geo.getLatLng("London");
    geo.getLatLng("Amsterdam");
    geo.getLatLng("Amsterdam");
    geo.getLatLng("Amsterdam");
    geo.getLatLng("Amsterdam");
    geo.getLatLng("London");
    geo.getLatLng("London");
 
    log.show();
}
```
# Tasks 

### 1
Реалізувати таймер за допомогою requestAnimationFrame.

### 2
Реалізувати Object clarkKent і зробити з нього супермена за домомогою декоратора.

### 3
Реалізувати проксі для отримання статусу квитка. ( Перевіряти чи дійсний квиток чи ні за датою ) 

### 4
Реалізувати фабрику, для створення квитків (квитки бувають різних типів) 

### 5 
Створіть батьківський клас Human, який буде приймати 2 аргументи (ім"я та вік), та має метод sayHello. Також створіть 2 підкласи - Man і Toy які будуть наслідувати клас продукту, а також мати власні методи: doShopping(data), drink(data).

### 6
Написати фукнцію, яка буде викликатись лише тоді, коли юзер закінчив скролл.

## Homework

Проект:
- при першому відкритті, потрібно створити юзера і зберегти в локалсторедж
- створити json файл з даними події ( футбольний матч, кіно, театр і так далі)
- записати дані з json файлу в локальну фейкову api
* щось наприклад https://github.com/typicode/json-server
- отримати дані з api та вивести в консоль


