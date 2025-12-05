## Initiating a class 
``` typescript
class Dog { 
name: string
breed: string
age: number
owner?: string

constructor(name: string, breed: string, age: number, owner?: string){ 
	this.name = name; 
	this.breed = breed;
	this.age = age;
	this.owner = owner
	}
	
	Bark(){
		console.log("Woof")
	}
	
	birthday(){ 
		this.age += 1
	}
	
	


}

//Intiate an Instance of the class 
const Gojo = new Dog('Gojo', 'poodle', '3')

//Prints "woof"
Gojo.bark()

//Prints name 
console.log(Gojo.name)

// Would be undefined because we aren't returning age in the birthday method
const dogsBirthday = Gojo.birthday()

// This is how we would do it if the method DOESN'T Return
Gojo.birthday()
const dogsBirthday = Gojo.age


```