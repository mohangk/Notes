http://cpp-next.com/archive/2009/08/want-speed-pass-by-value/

 •	If you need a copy of or intend to modify the value sent in, without wanting to change the actual object passed: pass by value.
 •	If you intend to modify the value sent in, and want these changes to affect the actual object passed: pass by reference.
 •	If you do not want to change the object passed, but believe it is beneficial to avoid copying: pass by const reference.
 •	If you take parameters by value, reference or const reference, and believe there are valuable optimizations which can be achieved using the knowledge that the input parameter is a temporary: also allow to pass by rvalue reference.


&& —> rvalue reference