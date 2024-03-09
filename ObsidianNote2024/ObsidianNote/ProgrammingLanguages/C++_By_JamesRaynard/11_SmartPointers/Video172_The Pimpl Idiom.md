
The `pimpl` idiom is short for pointer to implementation, also known as compiler firewall. As it usually introduces rigid separation of client code and implementation code.

To use `pimpl` idiom to achieve handle body pattern, we have the handle class to store a pointer of body class as private data member. And the public member functions of handle class will just forward the call to the corresponding member functions of the body class object through the pointer.

To see an example with traditional pointer.
The body class header file but with implementation to be concise.
```
class Date_impl {
	int day;
	int month;
	int year;
public:
	Date_impl(int day, int month, int year): day(day), month(month), year(year) {}
	void set_day(int d) {day = d;}
	void print() {cout << day << "/" << month << "/" << year << endl;}
}
```
The handle class object header file.
```
class Date_imple; // forward declaration

class Date {
	Date_impl* pImpl;
public:
	Date(int day, int month, int year);
	~Date();
	void set_day(int day);
	void print();
};
```
The handle class `cpp` file.
```
#include "Date.h"
#include "Date_impl.h"

Date::Date(int day, int month, int year) {
	pImpl = new Date_imple(day, month, year);
}

Date::~Date() {
	delete pImpl;
}

void Date::set_day(int day) {
	pImpl->set_day(day);
}

void Date::print() {
	pImpl->print();
}
```
The main `cpp` file.
```
#include "Date.h"

int main() {
	Date date(16, 11, 2019);
	date.print();
	date.set_day(17);
	date.print();
}
```


We can do this with `unique_ptr` as well. The body class and the main class can stay the same. All we need to change is the handle class.
The handle class header file.
```
#include <memory>

class Date {
	std::unique_ptr<Date_impl> pImpl;
public:
	Date(int day, int month, int year);
	~Date();
	Date(Date&&) noexcept; // move constructor
	Date& operator= (Date&&) noexcept; // move assignment operator
	void set_day(int d);
	void print();
}
```
The handle class `cpp` file.
```
#include "Date.h"
#include "Date_impl.h"

Date::Date(int day, int month, int year) {
	pImpl = std::make_unique<Date_impl>(day, month, year);
}
Date::~Date() = default;
Date::Date(Date&&) noexcept = default;
Date& Date::operator= (Date&&) noexcept = default;

void Date::set_day(int d) {
	pImpl->set_day(d);
}
void Date::print() {
	pImpl->print();
}
```
