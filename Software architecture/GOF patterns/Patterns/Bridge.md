![[Pasted image 20230725083546.png]]
![[Pasted image 20230725083819.png]]
``` C#
abstract class Abstraction
{
    protected abstract Implementor Implementor { get; set; }

    public virtual void Operation() => Implementor.OperationImpl();
}

abstract class Implementor
{
    public abstract void OperationImpl();
}
```