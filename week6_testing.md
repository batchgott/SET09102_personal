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


### Mocking
Mocking is used in unit tests to isolate code by creating fake versions of external components. These mocks allow us to control how these external components behave during testing, to ensure that the tests focus on the specific part of the code under examination while simulating different scenarios. This makes the unit tests more reliable and consistent.

Specifically in the unit tests we wrote for the `SelectWord` method we do not need to use mocking as we do not rely on any external objects.

