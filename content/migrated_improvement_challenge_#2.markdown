Title: improvement challenge #2
Date: 2016-11-11 11:00

finished two more excersises for the linux online course i'm doing
 right now. its next stage about IPC is delayed so i have time to
 refresh some c++ design patterns. writing adapter and facade samples.

##trivia time!

###which code snippet is faster and why?  
move `A` is fast, copy is slow, default vector c-tor is negligible


	A a;
	// first snippet
	vector<A> b {std::move(a)};
	// second snippet
	vector<A> b;
	b.emplace_back(std::move(a));

well.. that is the same anyways, right? it is not actually.  
second snippet does exactly what we thought - one `A move c-tor call`  
but first calls one more copy c-tor after that.

- `a` moves to `initializer_list` 
- `vector constructor` __copies__ content of `initializer_list` to the
vector internals 
