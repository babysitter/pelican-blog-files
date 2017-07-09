Title: improvement challenge #3
Date: 2016-11-16 23:00

actually i am not doing very good job right now :(  
added adapter class and object patterns into my github repo.
 going to overtake the linux course tasks and add factories patterns.

##trivia time!

###which code snippet is better and why?  
`T` is a random class, `g` and `foo` are function calls

	// first snippet
	foo(shared_ptr<T>(new T), g());
	// second snippet
	foo(make_shared<T>(), g());

first of all `make_shared` performs one heap-allocation, whereas calling the `shared_ptr c-tor` [performs two](http://en.cppreference.com/w/cpp/memory/shared_ptr).

but in this case perfomance is not the worst part - we should be aware of exceptions!
[c++faq](https://isocpp.org/wiki/faq/exceptions#ctors-can-throw) says that exceptions in c-tor is fine.

	void f() {
	 X x;            // If X::X() throws, the memory for x itself will not leak
	 Y* p = new Y(); // If Y::Y() throws, the memory for *p itself will not leak
	}

exceptions in `T c-tor` by itself is ok in our snippets, but if `g()` is not `noexcept` that means it could throw an exception. so in c++ `The order of evaluation of arguments is unspecified` hence this could happen in the first (slow) snippet:

- first heap allocation has performed `new T`
- function `g()` has called
- `g()` throws an exception
- `delete` for the first allocation will never be performed
- say hello to your memory leak!
