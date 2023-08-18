![[Pasted image 20230811105456.png]]

In computer programming, the **specification pattern** is a particular [software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern "Software design pattern"), whereby [business rules](https://en.wikipedia.org/wiki/Business_rules "Business rules") can be recombined by chaining the business rules together using [boolean logic](https://en.wikipedia.org/wiki/Boolean_algebra "Boolean algebra"). The pattern is frequently used in the context of [domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design "Domain-driven design").

A specification pattern outlines a business rule that is combinable with other business rules. In this pattern, a unit of business logic inherits its functionality from the abstract aggregate Composite Specification class. The Composite Specification class has one function called IsSatisfiedBy that returns a boolean value. After instantiation, **the specification is "chained" with other specifications, making new specifications easily maintainable, yet highly customizable business logic**. Furthermore, upon instantiation the business logic may, through method invocation or [inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control "Inversion of control"), have its state altered in order to become a delegate of other classes such as a persistence repository.

As a consequence of performing runtime composition of high-level business/domain logic, the Specification pattern is a convenient tool for converting ad-hoc user search criteria into low level logic to be processed by repositories.

Since a specification is an encapsulation of logic in a reusable form it is very simple to thoroughly unit test, and when used in this context is also an implementation of the humble object pattern.

```CSharp
public interface ISpecification<T>
{
    bool IsSatisfiedBy(T candidate);
    ISpecification<T> And(ISpecification<T> other);
    ISpecification<T> AndNot(ISpecification<T> other);
    ISpecification<T> Or(ISpecification<T> other);
    ISpecification<T> OrNot(ISpecification<T> other);
    ISpecification<T> Not();
}

public abstract class LinqSpecification<T> : CompositeSpecification<T>
{
    public abstract Expression<Func<T, bool>> AsExpression();
    public override bool IsSatisfiedBy(T candidate) => AsExpression().Compile()(candidate);
}

public abstract class CompositeSpecification<T> : ISpecification<T>
{
    public abstract bool IsSatisfiedBy(T candidate);
    public ISpecification<T> And(ISpecification<T> other) => new AndSpecification<T>(this, other);
    public ISpecification<T> AndNot(ISpecification<T> other) => new AndNotSpecification<T>(this, other);
    public ISpecification<T> Or(ISpecification<T> other) => new OrSpecification<T>(this, other);
    public ISpecification<T> OrNot(ISpecification<T> other) => new OrNotSpecification<T>(this, other);
    public ISpecification<T> Not() => new NotSpecification<T>(this);
}

public class AndSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> left;
    ISpecification<T> right;

    public AndSpecification(ISpecification<T> left, ISpecification<T> right)
    {
        this.left = left;
        this.right = right;
    }

    public override bool IsSatisfiedBy(T candidate) => left.IsSatisfiedBy(candidate) && right.IsSatisfiedBy(candidate);
}

public class AndNotSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> left;
    ISpecification<T> right;

    public AndNotSpecification(ISpecification<T> left, ISpecification<T> right)
    {
        this.left = left;
        this.right = right;
    }

    public override bool IsSatisfiedBy(T candidate) => left.IsSatisfiedBy(candidate) && !right.IsSatisfiedBy(candidate);
}

public class OrSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> left;
    ISpecification<T> right;

    public OrSpecification(ISpecification<T> left, ISpecification<T> right)
    {
        this.left = left;
        this.right = right;
    }

    public override bool IsSatisfiedBy(T candidate) => left.IsSatisfiedBy(candidate) || right.IsSatisfiedBy(candidate);
}
public class OrNotSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> left;
    ISpecification<T> right;

    public OrNotSpecification(ISpecification<T> left, ISpecification<T> right)
    {
        this.left = left;
        this.right = right;
    }

    public override bool IsSatisfiedBy(T candidate) => left.IsSatisfiedBy(candidate) || !right.IsSatisfiedBy(candidate);
}

public class NotSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> other;
    public NotSpecification(ISpecification<T> other) => this.other = other;
    public override bool IsSatisfiedBy(T candidate) => !other.IsSatisfiedBy(candidate);
}
```

## Usage example

```CSharp
var overDue = new OverDueSpecification();
var noticeSent = new NoticeSentSpecification();
var inCollection = new InCollectionSpecification();

// Example of specification pattern logic chaining
var sendToCollection = overDue.And(noticeSent).And(inCollection.Not());

var invoiceCollection = Service.GetInvoices();

foreach (var currentInvoice in invoiceCollection)
{
    if (sendToCollection.IsSatisfiedBy(currentInvoice))
    {
        currentInvoice.SendToCollection();
    }
}
```