# Array
student.h
#include<iostream>
using namespace std;

class Student {
private:
	int id;
	string name;

public:
	Student();
	Student(int, string);
	Student(Student&);

	int getid();
	void setid(int);
	string getname();
	void setname(string);

};

cource.h

#include<iostream>
#include"Student.h"
using namespace std;

class Course {
private:
	string coursename;
	int courseid;
	int capacity;
	int currentlyEnrolled;
	Student* students;

public:
	Course();
	Course(int, string);
	Course(int, string, int);
	//~Course();
	int getcourseid();
	void setcourseid(int);
	string getcoursename();
	void setcoursename(string);
	int getcapacity();
	void setcapacity(int);
	int getcurrentlyEnrolled();
	void setcurrentlyEnrolled(int);

	void enroll(int, string);
	void drop(int);
	void display();// (display all loop)
	void resize();
	void shift(int);
	int search(int);//to search 

};


imp

#include<iostream>
#include"Course.h"
#include<string>
using namespace std;


Student::Student() {
	id = -1;
	name = "";

}
Student::Student(int id, string name)
{
	this->id = id;
	this->name = name;

}
void Student::setid(int id) 
{
	this->id = id;

}
int Student::getid()
{
	return id;
}
void Student::setname(string name)
{
	this->name = name;

}
string Student::getname()
{
	return name;
}

//***Course.h***

Course::Course() {
	courseid = 0;
	coursename = "";
	capacity = 20;
	currentlyEnrolled = 0;
	students = new Student[capacity];

}

Course::Course(int id, string name) {
	courseid = id;
	coursename = name;
	capacity = 10;
	currentlyEnrolled = 0;
	students = new Student[capacity];

}

Course::Course(int id, string name, int capacity) {
	courseid = id;
	coursename = name;
	capacity = 20;
	currentlyEnrolled = 0;
	students = new Student[capacity];

}
//Course::~Course() {
//	delete[] students; // Free dynamically allocated memory
//}

int Course::getcourseid()
{
	return courseid;
}
string Course::getcoursename() 
{
	return coursename;
}
int Course::getcapacity()
{
	return capacity;
}
int Course::getcurrentlyEnrolled() 
{
	return currentlyEnrolled;
}

void Course::setcourseid(int) {
	this->courseid = courseid;
}
void Course::setcoursename(string) {
	this->coursename = coursename;
}
void Course::setcapacity(int) 
{
	this->capacity = capacity;
}
void Course::setcurrentlyEnrolled(int) 
{
	this->currentlyEnrolled = currentlyEnrolled;
}

void Course::enroll(int id, string name)
{
	if (currentlyEnrolled == capacity) {
		resize();
		students[currentlyEnrolled].setname(name);
		students[currentlyEnrolled].setid(id);
		currentlyEnrolled++;

	}
	else {
		students[currentlyEnrolled].setid(id);
		students[currentlyEnrolled].setname(name);
		currentlyEnrolled++;

	}
}

void Course::resize()
{
	capacity = capacity + 10;
	Student* newstudent = new Student[capacity];
	for (int i = 0; i < currentlyEnrolled; i++)
	{
		newstudent[i].setid(students[i].getid());
		newstudent[i].setname(students[i].getname());

	}
	delete[]students;
	students = newstudent;
}

int Course::search(int id) {
	for (int i = 0; i < currentlyEnrolled; i++) {
		if (students[i].getid() == id) {
			return i;
		}
		else {
			continue;
		}
	}
	return -1;

}

void Course::shift(int index)
{
	for (int i = index; i < currentlyEnrolled; i++) {
		students[index].setid(students[index + 1].getid());
		students[index].setname(students[index + 1].getname());
	}
}

void Course::drop(int id) {
	int index = search(id);
	if (index == -1) {

	}
	else {
		students[index].setid(0);
		students[index].setname("");
		shift(index);
		currentlyEnrolled--;
	}
}

main

#include <iostream>
#include "Course.h"
#include <string>

using namespace std;

void showMenu() {
    cout << "\n\t\t========= Course Management Menu =========\n";
    cout << "\t1. Enroll a Student" << endl;
    cout << "\t2. Drop a Student" << endl;
    cout << "\t3. Search for a Student" << endl;
    cout << "\t4. Display Course Details" << endl;
    cout << "\t5. Exit" << endl;
    cout << "\t\t========================================\n";
    cout << "Enter your choice: ";

}

int main() {
    // Create a Course
    Course course(101, "Data Structure", 20);
    //Course course(102, "COAL", 10);
    //Course course(103, "Cyber Security", 30);

    int choice;
    do {
        showMenu();
        cin >> choice;
        system("CLS");
        switch (choice) {
        case 1: {
            // Enroll a Student
            int id;
            string name;
            cout << "Enter Student ID: ";
            cin >> id;
            cout << "Enter Student Name: ";
            cin.ignore(); // Ignore leftover newline character
            getline(cin, name);
            course.enroll(id, name);
            cout << "Student enrolled successfully." << endl;
            break;
        }
        case 2: {
            // Drop a Student
            int id;
            cout << "Enter Student ID to drop: ";
            cin >> id;
            course.drop(id);
            cout << "Student dropped successfully." << endl;
            break;
        }
        case 3: {
            // Search for a Student
            int id;
            cout << "Enter Student ID to search: ";
            cin >> id;
            int index = course.search(id);
            if (index != -1) {
                cout << "Student with ID " << id << " found at index " << index << "." << endl;
            }
            else {
                cout << "Student with ID " << id << " not found." << endl;
            }
            break;
        }
        case 4: {
            // Display Course Details
            cout << "\n\n\t**Course Details:**" << endl;
            cout << "\n\tCourse ID: " << course.getcourseid() << endl;
            cout << "\tCourse Name: " << course.getcoursename() << endl;
            cout << "\tCapacity: " << course.getcapacity() << endl;
            cout << "\tCurrently Enrolled: " << course.getcurrentlyEnrolled() << endl;
            // system("CLS");
            break;
        }
        case 5:
            cout << "* Exiting the program. Goodbye! *" << endl;
            break;

        default:
            cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 5);

    return 0;
}
