# Testing

We want to create unit tests to test the methods of the `GameRepository` class. (The code snippet is not showing the whole class, it only shows the part that is discussed in the portfolio)

```csharp
    public class GameRepository
    {
        string _dbPath;
        private SQLiteConnection conn;

        public GameRepository(string dbPath)
        {
            _dbPath = dbPath;
        }

        public void Init()
        {
            conn = new SQLiteConnection(_dbPath);
            conn.CreateTable<Game>();
        }

        public void Add(Game game)
        {
            Init();
            conn.Insert(game);
        }

        public void Delete(int id)
        {
            Init();
            conn.Delete<Game>(id);
        }

        ...
    }
```

The GameRepository is used to interact with an SQLite database. It has methods for initializing the database, adding new game data to the database, and deleting game data. The **Init** method sets up a connection to the database and creates a table to store game data. The **Add** method inserts a new game record into the database, and the **Delete** method removes a game record by its ID. This class simplifies the process of managing game-related data in a SQLite database for a mobile app.

### Unit tests

```csharp
using System.Collections.Generic;
using Moq;
using Xunit;

public class GameRepositoryTests
{
    private Mock<SQLiteConnection> mockConnection;
    private GameRepository gameRepository;

    public GameRepositoryTests()
    {
        mockConnection = new Mock<SQLiteConnection>("mock.db");
        gameRepository = new GameRepository(dbPath);

        typeof(GameRepository)
        .GetField("conn", BindingFlags.NonPublic | BindingFlags.Instance)
        .SetValue(repository, mockConnection.Object);
    }

    [Fact]
    public void TestAdd_InsertGameIntoDatabase()
    {
        var game = new Game { Type = GameOperation.Addition, Score = 20 };

        repository.Add(game);
        mockConnection.Verify(conn => conn.Insert(game), Times.Once);
    }

    [Fact]
    public void TestDelete_RemoveGameFromDatabase()
    {
        var gameId = 1;

        repository.Delete(gameId);
        mockConnection.Verify(conn => conn.Delete<Game>(gameId), Times.Once);
    }
}
```

There is a few things going on in this code snippet so let us break it down bit by bit.

#### Mocking
Mocking is used in unit tests to isolate code by creating fake versions of external components. These mocks allow us to control how these external components behave during testing, to ensure that the tests focus on the specific part of the code under examination while simulating different scenarios. This makes the unit tests more reliable and consistent.

Specifically in this example we are creating a mock for `SQLiteConnection` as we do not want to test this class, we just want to use it to test our methods.

#### Reflection
The `SQLiteConnection` in the `GameRepository` is set to private and there is no public setter which means we need to make use of one of C#'s more advanced features called [Reflection](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/reflection-and-attributes/).

With Reflection we can use the `GetField` method to search for a field with the name "conn" inside of the type `GameRepository`. Additionally we are providing the flags `BindingFlags.NonPublic` and `BindingFlags.Instance` to specify the search to non-public fields that are not static.
With `.SetValue(repository, mockConnection.Object)` we effectivly inject the `mockConnection` into the `GameRepository` instance so that it uses the mock instead of creating a real `SQLiteConnection`.

#### Testing with *Verify*
In the unit tests we make use of the `Verify` method of a mock to ensure the `Add` and `Delete` method of the `SQLiteConnection` have been called once. If the methods are not called or are called more than once the tests will fail. 
