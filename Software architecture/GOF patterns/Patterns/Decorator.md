![[Pasted image 20230725084447.png]]

```Java
class SomeClass {
	public virtual void Operation() {
		// do smth.
	}
}

class DecoratedSomeClass extends SomeClass {
	private SomeClass _wrappee;
	
	public DecoratedSomeClass(SomeClass wrappee) {
		_wrappee = wrappee;
	}

	public override void Operation(){
		_wrappee.Operation();
		//add something
	}
}
```

```C#
public class SomeClass {
	public virtual void Operation() {}
}

public class DecoratedClass: SomeClass {
	[SomeAttribute] //attributes are also Decorators, however not so classical
	public override void Operation() {}
}
```
