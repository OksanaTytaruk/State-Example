# State Example

Цей репозиторій містить приклад патерну проектування "Стан".

## Патерн Стан (State)

Патерн "Стан" дозволяє об'єкту змінювати свою поведінку при зміні його внутрішнього стану. Об'єкт буде виглядати так, наче він змінив свій клас.

### Приклад коду:

```csharp
using System;

// Інтерфейс стану
public interface IState
{
    void Handle(Context context);
}

// Конкретні стани
public class ConcreteStateA : IState
{
    public void Handle(Context context)
    {
        Console.WriteLine("Обробка у стані A");
        context.State = new ConcreteStateB(); // Перехід у стан B
    }
}

public class ConcreteStateB : IState
{
    public void Handle(Context context)
    {
        Console.WriteLine("Обробка у стані B");
        context.State = new ConcreteStateA(); // Перехід у стан A
    }
}

// Контекст
public class Context
{
    private IState _state;

    public Context(IState state)
    {
        _state = state;
    }

    public IState State
    {
        get => _state;
        set
        {
            _state = value;
            Console.WriteLine("Перехід у новий стан.");
        }
    }

    public void Request()
    {
        _state.Handle(this);
    }
}

class Program
{
    static void Main(string[] args)
    {
        Context context = new Context(new ConcreteStateA());
        
        context.Request(); // Обробка у стані A
        context.Request(); // Обробка у стані B
    }
}
