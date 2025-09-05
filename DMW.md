## *Lab Experiment No : 11*

## *Lab Experiment Name : Object Oriented Programming*

## *Submission Date : 19th August 2025*

----------
## *Code :*
```C
#include <bits/stdc++.h> // to include all standard headers
#include <fstream>

using namespace std;

/*----------------------- Static Variable ------------------------*/

static int total_marks = 0; // static variable to calculate total marks, to avoid copy in the objects, we can use it in all classes

/*----------------------- Generic function ------------------------*/

template <class T, class U>
U max_CT_sum(T a, T b, U c, U d) // a =10, b=11, c=10, d=9
{
    U sum_num = 0;
    // Find biggest among four
    U first;
    if (a >= b && a >= c && a >= d)
        first = a;
    else if (b >= a && b >= c && b >= d)
        first = b;
    else if (c >= a && c >= b && c >= d)
        first = c;
    else
        first = d;

    // Find second biggest among four
    U second;
    if ((a != first) && (a >= b && a >= c && a >= d))
        second = a;
    else if ((b != first) && (b >= a && b >= c && b >= d))
        second = b;
    else if ((c != first) && (c >= a && c >= b && c >= d))
        second = c;
    else
        second = d;

    // Find third biggest among four
    U third;
    if ((a != first && a != second) && (a >= b && a >= c && a >= d))
        third = a;
    else if ((b != first && b != second) && (b >= a && b >= c && b >= d))
        third = b;
    else if ((c != first && c != second) && (c >= a && c >= b && c >= d))
        third = c;
    else
        third = d;

    sum_num = first + second + third;
    return sum_num;
}

/*----------------------- Inheritence ------------------------*/

// Ruet <- ECE <- Running_Batches

class Ruet
{
public:
    virtual void print_class() // virtual function for runtime polymorphism
    {
        // Function Overriding
        cout << "------ RAJSHAHI UNIVERSITY OF ENGINEERING TECHNOLOGY (RUET) ------" << endl;
    }
};

class ECE : public Ruet
{

    int password = 987654321; // only accessible by the head of the dept, used it in friend class
public:
    void print_class() override
    {
        cout << "      -- Electrical & Computer Engineering (ECE) --      " << endl;
    }

    friend class Uni_Admin; // friend class declaration
};

class Running_Batches : public ECE
{
    int batch_year;

public:

    Running_Batches(int b) : batch_year(b)
    {
        cout << "Running Batch : " << batch_year << endl;
        switch (batch_year)
        {
        case 20:
            cout << "4th Year" << endl;
            break;
        case 21:
            cout << "3rd Year" << endl;
            break;
        case 22:
            cout << "2nd Year" << endl;
            break;
        case 23:
            cout << "1st Year" << endl;
            break;
        default:
            cout << "Graduated" << endl;
        }
    }
};

/*----------------------- Abstract class + Runtime polymorphism + Encapsulation ----------------------*/

// Person <- Faculty_members + Students + Staff
class Person // Abstract class
{

public:
    virtual void display() = 0; // pure virtual function
};

class Faculty_members : public Person
{
private:
    double salary; // encapsulated member

public:
    int faculty_id;
    string faculty_name;

    Faculty_members(string fn, int id = 0, double sal = 0.0) : faculty_id(id), salary(sal)
    {
        faculty_name = fn;
    }

    void display() override
    {
        cout << "Faculty name: " << faculty_name << endl;
        cout << "Faculty ID: " << faculty_id << endl;
        cout << endl;
    }

    friend void other_dept_Teachers(Faculty_members &f); // friend function declaration
    void set(string dept)                                // friend function usage
    {
        cout << "Faculty name: " << faculty_name << endl;
        cout << "Department: " << dept << endl;
        cout << endl;
    }
};

class Students : public Person // Student class
{                              // Encapsulation
private:
    int acc_no; // encapsulated member

protected:
    double cgpa; // encapsulation: protected member

public:
    char *student_name;
    int student_id; // encapsulated member
    Students(char *s_name = "", int i = 0, int acc = 0, double c = 0.0) : student_id(i), acc_no(acc), cgpa(c)
    {
        int sz = strlen(s_name);
        student_name = new char[sz + 1]; // dynamic memory allocation
        if (student_name == NULL)
        {
            cout << "Invalid!\n";
            exit(1);
        }
        strcpy(student_name, s_name);
    }

    ~Students() // destructor
    {
        delete[] student_name; // free dynamically allocated memory , to free dangling pointer
    }

    void display() override
    {
        cout << "Student name: " << student_name << endl;
        cout << "Student ID: " << student_id << endl;
        cout << endl;
    }
};

class Staff : public Person
{
public:
    int staff_id;
    string staff_name;
    Staff(string sn, int id = 0) : staff_id(id)
    {
        staff_name = sn;
    }

    void display() override
    {
        cout << "Staff name: " << staff_name << endl;
        cout << "Staff ID: " << staff_id << endl;
        cout << endl;
    }
};

/*----------------------- friend function + friend class + File handling--------------------*/

void other_dept_Teachers(Faculty_members &f) // friend function of Faculty_members
{
    //Accessing ECE class for other dept teacher
    f.set("Humanities");
}

class Uni_Admin // friend class of ECE
{

public:
    void Access_private_data(ECE &e)
    {   // nested try-catch for file operations
        try // outer try-catch for password protection
        {
            cout << "Enter password to access ECE private data:";
            int input;
            cin >> input;
            if (input != e.password) // Accessing the private member of ECE class
            {
                throw -1;
            }
            else
            {
                cout << "Access Granted! ECE private data is now accessible." << endl;
                try // inner try-catch for file operations
                {
                    ofstream file("ECE_PRIVATE_DATA.txt"); // open file   
                    if (!file.is_open())
                    {
                        throw -1;
                    }
                    file << "New confidential ECE entry.\n";
                    // File Info Input
                    file << "1.Student account no: 1087.\n2.Student cgpa: 3.88\n\n";
                    file << "1.Faculty account no: 9999.\n2.Faculty salary: 55000.00\n\n";
                    file << "1.Staff account no: 1000.\n2.Staff salary: 10000.00\n\n";

                    cout << "Confidential info written to file." << endl;
                    cout << "Data written successfully to ECE_PRIVATE_DATA.txt" << endl;
                    file.close();

                    ifstream infile("ECE_PRIVATE_DATA.txt"); // open file for reading
                    if (!infile.is_open())
                    {
                        cout << "Error opening file for reading!" << endl;
                        return;
                    }

                    cout << "\n--- Confidential Data from File ---\n";
                    string line;
                    while (getline(infile, line))
                    {
                        cout << line << endl;
                    }
                    infile.close();
                }
                catch (...) // inner catch for file operation
                {
                    cout << "File Error: File could not be opened!" << endl;
                }
            }
        }
        catch (...) // outer catch for password protection
        {
            cout << "Access denied! Incorrect password." << endl;
            cout << endl;
        }
    }
};

/*----------------------- Diamond Problem + Constructor Overloading + Operator Overloading ----------------------*/

// Students <- Theory_Marks + Lab_Marks -> Results // to avoid ambigiousness.

class Theory_Marks : virtual public Students // virtual inheritence to avoid diamond problem
{
public:
    int theory_marks;
    Theory_Marks(int t = 0) // constructor overloading
    {
        theory_marks = t;
    }

    void CT(int ct1, int ct2, double ct3, double ct4)
    {
        double sum = max_CT_sum(ct1, ct2, ct3, ct4);
        double average = sum / 3.0;
        average = ceil(average); // rounding up to the nearest higher integer
        cout << "CT average of best 3: " << average << endl;

        total_marks += average; //  static variable usage
    }

    void Assignment(int m1, int m2)
    {
        double assignment_avg = (m1 + m2) / 2.0;
        assignment_avg = ceil(assignment_avg); // rounding up to the nearest higher integer
        cout << "Assignment average: " << assignment_avg << endl;

        total_marks += assignment_avg; // static variable usage
    }

    void SemesterFinal(int m)
    {
        if (m < 15)
        {
            cout << "You have failed in this semester." << endl;
            cout << endl;
        }
        else if (m > 60)
        {
            cout << "Invalid marks." << endl;
            cout << endl;
        }
        else
        {
            cout << "Semester Final marks: " << m << endl;
            cout << "You have passed in this semester." << endl;
            cout << endl;

            total_marks += m; // static variable usage
        }
    }

    void Attendance(int att)
    {
        if (att < 50)
        {
            cout << "You are not allowed to sit for the final exam." << endl;
            cout << endl;
        }
        else if (att > 100)
        {
            cout << "Invalid attendance." << endl;
            cout << endl;
        }
        else
        {
            cout << "You are allowed to sit for the final exam." << endl;// cout << "Attendance: " << att << "%" << endl;
            cout << "Attendance marks: 10" << endl;
            cout << endl;

            total_marks += 10; // static variable usage
            theory_marks = total_marks;
        }
    }
};

class Lab_Marks : virtual public Students
{
public:
    int lab_marks;
    Lab_Marks(int l = 0) : lab_marks(l) {}

    void Board_viva(bool pass)
    {
        if (pass == true) // attended the board viva
        {
            cout << "Board Viva: Passed" << endl;
            cout << endl;

            lab_marks += 100; // static variable usage
            total_marks += 100;
        }
        else
        {
            cout << "Board Viva: Failed" << endl;
            cout << endl;
        }
    }
};

class Results : public Theory_Marks, public Lab_Marks
{
public:
    Results(int t = 0, int l = 0) : Theory_Marks(t), Lab_Marks(l) {}

    void display()
    {
        cout << "Theory: " << theory_marks << ", Lab: " << lab_marks

             << ", Total: " << theory_marks + lab_marks << endl;
    }
};

// Outside the class
// "+"Operator overloading
Results operator+(const Theory_Marks &t, const Lab_Marks &l) // to access the objects of two different classes
{
    return Results(t.theory_marks, l.lab_marks);
}


/*----------------------- Copy Constructor --------------------*/

class CourseReg
{
    string course_name;
    int course_fee;

public:
    CourseReg(string name = "", int fee = 300) : course_name(name), course_fee(fee) {}

    // Copy constructor
    CourseReg(const CourseReg &c)
    {
        course_name = c.course_name;
        course_fee = c.course_fee;
        // cout << "Copy Constructor called for CourseReg." << endl;
    }

    void display()
    {
        cout << "Course Name: " << course_name << ", Course Fee: " << course_fee << endl;
        cout << endl;
    }
};

//----------------------- Main Function --------------------//

int main()
{
    Ruet r;
    r.print_class();
    ECE e;
    e.print_class();
    Person *ptr;

    //-------------------- Menu --------------------
    cout << "-------------------- Menu --------------------" << endl;

    while (true)
    {
        cout << "Choose option (1-7)" << endl;
        cout << "1 -> Faculty Info: " << endl;
        cout << "2 -> Student Info: " << endl;
        cout << "3 -> Staff Info: " << endl;
        cout << "4 -> Course Registration Info: " << endl;
        cout << "5 -> Results Info: " << endl;
        cout << "6 -> Access ECE private data: " << endl;
        cout << "7 -> Non-Faculty Member Info: " << endl;
        cout << "8 -> Exit" << endl;

        int option;
        cout << "Enter your option: ";
        cin >> option;
        cout << endl;

        switch (option)
        {
        case 1: // Faculty Info
        {
            Faculty_members fm1("Hafsa Binte Kibria", 151024, 55000.00);
            Faculty_members fm2("Oishy Jyoti", 151041, 55000.00);

            // calls display function of Faculty_members class using base class pointer

            ptr = &fm1;
            ptr->display();
            ptr = &fm2;
            ptr->display();

            break;
        }

        case 2: // Student Info
        {
            Running_Batches rb(23); // running batch object

            Students s1("Tahsin Maria", 2310034, 1087);
            Students s2("Raidah Binte Zaman", 2310035, 1088);
            Students s3("Fahima Chowdhury", 2310054, 1107);


            // calls display function of Students class using base class pointer
            ptr = &s1;
            ptr->display();
            ptr = &s2;
            ptr->display();
            ptr = &s3;
            ptr->display();
            break;
        }

        case 3: // Staff Info
        {
            Staff st1("Md. Kamrul Hasan", 101);
            Staff st2("Mst. Fatema Begum", 102);

            // calls display function of Staffs class using base class pointer
            ptr = &st1;
            ptr->display();
            ptr = &st2;
            ptr->display();
            break;
        }

        case 4: // Course Registration Info
        {

            CourseReg crs1_obj("ECE 1204 : Object Oriented Programming");
            CourseReg crs2_obj = crs1_obj; // Copy constructor called
            crs1_obj.display();
            crs2_obj.display();
            break;

        }

        case 5: // Results Info
        {
            total_marks = 0; // reset total marks for each student

            // objects of Theory_Marks and Lab_Marks

            Theory_Marks t;
            Lab_Marks l;

            t.CT(19, 18, 19.5 , 14.5); // best 3 CTs marks count
            t.Assignment(8, 10);  // assignments count
            t.SemesterFinal(40);  // semester final count
            t.Attendance(80);     // attendance count

            l.Board_viva(true); // board viva pass

            using operator overloading    
            Results r = t + l;  // combine theory and lab marks 
            r.display();       // final display
            cout << endl;
            break;

        }

        case 6: // Access ECE private data
        {
            Uni_Admin admin; // friend class object
            ECE ece; // ECE class object
            admin.Access_private_data(ece); // friend class function call
            break;

        }
        case 7: // Non-Faculty Member Info
        {
            // friend function usage
            Faculty_members fm("Sadia Sharmin Runa");
            other_dept_Teachers(fm); // friend function call
            break;
        }
            {

                return 0;
            }

        case 8: // Exit
        {
            cout << "Exiting ...\n";
            cout << "..... Thank You .....\n";
            return 0;
        }

        default:
            cout << "Invalid Input! Please enter a valid option." << endl;
            break;
        }
    }
    return 0;
}
```

## *Output :* 
<p align="center">
<img src="https://github.com/user-attachments/assets/ba69c467-9b9a-4d35-8304-c37b3024992d">
</p>
<p align="center">
<img src="https://github.com/user-attachments/assets/a329f164-3194-4efc-9815-85b041e3ba8b">
</p>
<p align="center">
<img src="https://github.com/user-attachments/assets/81dd00cb-d6d1-43d4-8922-633a136291bd">
</p>
<p align="center">
<img src="https://github.com/user-attachments/assets/faca60b9-3bfa-4c57-bf40-09ebb5c76914">
</p>
<p align="center">
<img src="https://github.com/user-attachments/assets/3d596435-f51d-4730-a8c1-6b34c8f62dc6">
</p>
<p align="center">
<img src="https://github.com/user-attachments/assets/951f2fca-2199-4146-bdae-c407e6c6e9cd">
</p>
<p align="center">
<img src="https://github.com/user-attachments/assets/77c8b9e8-e52c-44ab-a433-1c9e8729c104">
</p>


## *Conclusion :*
<div align="justify">
The program effectively shows the output.<br>
</div>

----------
