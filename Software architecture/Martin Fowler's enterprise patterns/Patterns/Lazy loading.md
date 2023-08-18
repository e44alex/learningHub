![[Pasted image 20230725091937.png]]

```C#
class Context {
	private readonly Lazy<IMongoCollection<UserDocument>> _userCollectionLazy;

	public Context() {
		// init method assigns Lazy<T> where Lazy has lambda for object creation. 
		_userCollectionLazy = InitializeLazyCollection<UserDocument>(name); 
	}

	/*
	When you call .Value on Lazy<T>, it checks if value is created and invokes 
	factory lambda if needed.
	*/ 
	public IMongoCollection<UserDocument> UserCollection => _userCollectionLazy.Value;
}

protected Lazy<IMongoCollection<T>> InitializeLazyCollection<T>(string collectionName)
{
	return new Lazy<IMongoCollection<TDocument>>(() => 
		Database.GetCollection<TDocument>collectionName));
}
```