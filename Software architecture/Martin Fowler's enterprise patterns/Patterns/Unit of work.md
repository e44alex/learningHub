Паттерн Unit of Work позволяет упростить работу с различными репозиториями и дает уверенность, что все репозитории будут использовать один и тот же контекст данных.

Oбъект, реализующий этот паттерн отвечает за накопление информации о том какие объекты входят в транзакцию и каковы их изменния относительно исходных значений в хранилище. Основная работа производится в методе `commit()` который отвечает за вычисление изменений в сохранённых в UoW объектах и синхронизацию этих изменений с хранилищем (базой данных).

Обычно UOW тесно связан с Identity Map. Это позволяет избежать конфликтов изменений т.к. не допускает ситуации когда два объекта, представляющих один и тот же элемент данных в хранилище, изменены по-разному. Информация из Identity Map используется в методе `commit()` паттерна Unit of Work для вычисления разницы между исходными данными и накопленными изменениями.
![[Pasted image 20230725091523.png]]

```C#
///Unit of work aggregates multiple repos
public class UnitOfWork : IDisposable
{
    private OrderContext db = new OrderContext();
    private BookRepository bookRepository; 
    private OrderRepository orderRepository;
 
    public BookRepository Books
    {
        get
        {
            if (bookRepository == null)
                bookRepository = new BookRepository(db);
            return bookRepository;
        }
    }
 
    public OrderRepository Orders
    {
        get
        {
            if (orderRepository == null)
                orderRepository = new OrderRepository(db);
            return orderRepository;
        }
    }
 
    public void Save()
    {
        db.SaveChanges();
    }
 
    private bool disposed = false;
 
    public virtual void Dispose(bool disposing)
    {
        if (!this.disposed)
        {
            if (disposing)
            {
                db.Dispose();
            }
            this.disposed = true;
        }
    }
 
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
}

public class HomeController : Controller
{
	UnitOfWork unitOfWork;
	public HomeController()
	{
		unitOfWork = new UnitOfWork();
	}
	public ActionResult Index()
	{
		var books = unitOfWork.Books.GetAll();
		return View();
	}
}
```
