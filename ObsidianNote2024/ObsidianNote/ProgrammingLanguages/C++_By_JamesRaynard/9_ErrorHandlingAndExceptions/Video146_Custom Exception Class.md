
If we write a custom exception class, usually we should derive from `std::exception`.

Normally, we would like it to store some string with related information to help figuring out what is going on. That means the custom exception class should have constructors to take in string. So we don't derive from `std::exception`, but derive from its sub class.

It needs a default copy constructor because it will be used when the exception object is copied into the special memory space.

We can override the `what()` member function if necessary.

```
class bad_student_grade : public std::out_of_range {
public:
	bad_student_grade(): std::out_of_range("invalid grade") {}
	bad_student_grade(const char* s): std::out_of_range(s) {}
	bad_student_grade(const string& s): std::out_of_range(s) {}

	bad_student_grade(const bad_student_grade& other) = default;
	bad_student_grade& operator= (const bad_student_grade& other) = default;

	//no override of virtual function, so no virtual destructor
};

class StudentGrade {
	int grade;
public:
	StudentGrade(int grade) : grade(grade) {
		if (grade < 0) {
			throw bad_student_grade("negative grade is invalid");
		}
		if (grade > 100) {
			throw bad_student_grade;
		}
	}
};

int main() {
	int result;
	cout << "enter a grade" << endl;
	cin >> result;
	try {
		StudentGrade(result);
	}
	catch(bad_student_grade& ex) {cout << ex.what() << endl;}
}
```
