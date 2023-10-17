# Testing

We want to create unit tests to test the method `SelectWord` of the `GameRepository` class in the [test_battles Repository](https://github.com/Software-Engineering-Red/test_battles). The code snippet below shows the method.

```csharp
public partial class GamePage : ContentPage
{
    ...

        /*!
	 * Uses the GameType to select a word from the list by its length:
	 * Easy : length < 7
	 * Medium : 7 <= length < 10
	 * Hard : length >= 10
	 */
    private string SelectWord(string gameType)
    {
        using var stream = FileSystem.OpenAppPackageFileAsync("wordList.txt").Result;
        using var reader = new StreamReader(stream);

        var contents = reader.ReadToEnd();
        var words = contents.Split("\n");
        if (gameType == "Easy")
        {
            words = words.Where(word => word.Length < 7).ToArray();
        }
        if (gameType == "Medium")
        {
            words = words.Where(word => word.Length >= 7 && word.Length < 10).ToArray();
        }
        if (gameType == "Hard")
        {
            words = words.Where(word => word.Length >= 10).ToArray();
        }
        return randomElement(words);
    }

    ...
}
```

The method takes a string `gameType` as input and returns a randomly selected word from the `wordList.txt` file.
If the gameType is "Easy" a word with the length less than 7 characters will be randomly selected. For "Medium" the randomly selected word will have between 7 and 9 characters and for "Hard" the word will be at least 10 characters long.

### Unit Tests

We are using the [xUnit](https://xunit.net/) library to write our unit tests.

#### Returns Valid Word Test
This unit test tests the main functionality of the `SelectWord` method. For this test instead of the attribute `Fact` we are using `Theory`. This attribute allows us to define `InlineData` for the unit test which means we can provide different sets of data for multiple runs.

In this unit test three sets of `InlineData` are provided which means the unit test will run three times, once for every gamemode. Additionally to providing the gamemode the inline data also contains the boundaries for the word that should be returned. If the word is not within that boundaries the test will fail.

```csharp
[Theory]
[InlineData("Easy", 0, 7)]
[InlineData("Medium", 7, 10)]
[InlineData("Hard", 10, int.MaxValue)]
public void SelectWordReturnsValidWordTest(string gameType, int minBound, int maxBound)
{
    var gamePage = new GamePage(gameType);
    var selectedWord = gamePage.SelectWord(gameType);

    Assert.NotNull(selectedWord);
    Assert.True(selectedWord.Length >= minBound && selectedWord.Length < maxBound);
}
```

Important in this unit tests are the `Asserts`. Asserts make sure that the state at the end of the test is what we expect. In this case we expect `selectedWord` not to be null, hence we are using the `NotNull` assertion.
We also expect the word to be within the given boundaries which is why we are using the `True` assertion to test for that condition.


#### Throws Null Reference Exception when GameType is Null Test
This is a very simple test that just makes sure the correct exception is thrown when someone provides the wrong value to the `SelectWord` method.

Here we are using `null` as the input value for the gamemode. We expect that a `NullReferenceException` is thrown therefore we are using the `Assert.Throws<NullReferenceException>` method.

```csharp
[Fact]
public void SelectWordThrowsNullReferenceExceptionWhenGameTypeIsNullTest()
{
    var gamePage = new GamePage("Easy");

    Assert.Throws<NullReferenceException>(() => gamePage.SelectWord(null));
}
```

### Reflection
#### DRY
Most of the unit tests are instantiate a new object of `GamePage` which violates the **DRY** (Don't repeat yourself) principle. Even though this portfolio entry is about testing we should still maintain a high standard of code quality.
To avoid having to instantiate the class over and over again we can make use of a setup method that runs before every unit test and creates the `GamePage` object. With the `xUnit` this is very simple as the constructor of the test class [functions as the setup method](https://xunit.net/docs/shared-context).
We would have to make the below changes to have our code comply with the DRY principle:

```csharp
    public class GamePageTests
    {
        private GamePage _gamePage;

        public GamePageTests() {
            _gamePage = new GamePage("Easy");
        }

        [Fact]
        public void SelectWordThrowsNullReferenceExceptionWhenGameTypeIsNullTest()
        {
            Assert.Throws<NullReferenceException>(() => _gamePage.SelectWord(null));
        }
    }
```

Now we can remove the creation of the `GamePage` in all methods and simply use the one we instantiate in the constructor (setup method).