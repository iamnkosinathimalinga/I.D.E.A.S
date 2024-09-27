2024-07-10 11:43

State: #baby

Tags: [[C#]]  [[Engineering]]

# CSharp - Target

  
Methods are executed blocks of code.  
  
public static void Main(string[] args)  
{  
Console.ReadLine();  
} // End of Main() method  
  
Main() method syntax:  
  
The Main method has two keywords before it:  
  
static – Later in the book when we look at classes and methods in detail, we will become familiar with the use of the keyword static. For now, just forget about static and simply accept its use in the code as shown.  
  
void – This means that when all the lines of code within a method are executed, no value will be returned from the method – it is a void return. We will see more about this later in the book when we look at methods in detail.  
  
The Main method has some text within the brackets:  
  
string[] – We will see more about this later in the book when we look at arrays in detail. Essentially, it means that the Main() method can accept, be given, input values that are of type string. The [] means that it is an array of strings, one or more string values. So the Main Method can aceept multiple string types.  
  
args is the name of the "string[]" array


Namespaces  
  
A namespace may be thought of as a storage area for some classes, which may also contain methods.  
  
A namespace can be likened to the folders that we keep our files in. We create different folders to hold different files in a structure that best suits our system  
  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Threading.Tasks;  
namespace ConsoleVersion1  
{  
public class Program  
{  
public static void Main(string[] args)  
{  
Console.ReadLine(;  
} // End of Main() method  
} // End of class  
} // End of namespace


A class exists inside a namespace.  
A class can contain variables, for example, forename, surname, and dateofbirth.  
A class can contain methods, for example, CalculateInsurancePremium().  
  
  
Classes are contained within namespaces.  
  
> Allow us to create our own type using other types, methods, and variables from any namespace you're using.  
  
A class for the type Pizza, to define that all pizzas have:  
  
• A pizza base  
• A pizza sauce  
• Toppings  
  
Once we define the blueprint class for the pizza, we will be able to use the class to create specific types of pizza. For example, we can create a Hawaiian pizza or a vegetarian pizza. The two classes, Hawaiian pizza and vegetarian pizza, are called instances of the class, and each instance will contain a pizza base, a pizza sauce, and a topping(s).  
  
Another Example would be:  
  
  
A class for the type InsuranceQuote to define that all quotes must have  
An applicant's forename  
  
• An applicant's surname  
  
• An applicant's date of birth  
  
• A method to calculate the insurance premium  
  
Once we define the blueprint class for "InsuranceQuote" , we will be able to use the class to create specific types of InsuranceQuote. For example, we can create a CarInsuranceQuote or a HomeInsuranceQuote. The two classes, CarInsuranceQuote and HomeInsuranceQuote, are called instances of the class, and each instance will contain an applicant's forename, an applicant's surname, an applicant's date of birth, and a method to calculate the insurance premium.  
  
The term instance has been used to describe our copy of the class (it can be denoted to an OBJECT)

Classes Diction:  
  
property , field or member: denotes to variable, they are called property ,field or member when inside a CLASS

References
