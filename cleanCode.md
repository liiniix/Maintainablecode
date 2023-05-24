## Style Guides

Every languages have their suggested style guides; they convey useful information of the code and make the code more maintainable.

For example, in `C#` class names are written in `PascalCase`, `Interface` names are prepended with `I`, `private` variable names are prepended with underscore.

```C#
public interface IWorkerQueue
{
}

public class ExampleEvents
{
    private bool _isValid;
    public IWorkerQueue workerQueue { get; init; }

}
```

In `C++`, variables names are written in lowercase, with underscore between words.

```C++
std::string table_name;
```

Sticking to style guide of a specific language will make code more maintainabl dramatically.

## Magic Numbers

We encounter some numbers (maybe used repeatedly) here and there in our code.

For example, number `7` in the following code is strange, though it is used to check whether the password legnth is shorter or equal to this number.

```C#
public class ValidatePassword {

    public void ValidatePasswordLength(String password) {
         if (password.length() >= 7) {
              throw new InvalidArgumentException("password");
         }
    }
}
```

If we name the number (by converting it to a variable), readability will increase.

```C#
public class ValidatePassword {
    public static final int MAX_PASSWORD_SIZE = 7;

    public void ValidatePasswordLength(String password) {
         if (password.length() >= MAX_PASSWORD_SIZE) {
              throw new InvalidArgumentException("password");
         }
    }
}
```

## Replace Nested Conditional with Guard Clauses

Use of extensive nesting will make code less readable.

```C#
double getPayAmount() {

    double result;

    if (_isDead) result = deadAmount();
    else {
        if (_isSeparated) result = separatedAmount();
        else {
            if (_isRetired) result = retiredAmount();
            else result = normalPayAmount();
        };
    }
     return result;
};
```

If we return earlier, the code will be more readable as nesting decreases.

```C#
double getPayAmount() {
    if (_isDead)      return deadAmount();
    if (_isSeparated) return separatedAmount();
    if (_isRetired)   return retiredAmount();

    return normalPayAmount();
}; 
```

## Write Self-documenting Code, Instead of Comments

'A comment is a failure to express yourself in code. If you fail, then write a comment; but try not to fail.', said Uncle Bob. Try to write self-explanatory, self-documenting code. Focus on intent; not technical details.

## Is it better to return null or empty collection?

Let's say we have a collection of pop songs and then we want to filter favorite songs from them.

```C#
public static IEnumerable<string> GetPopSongs()
{
}
```

```C#
public static IEnumerable<string> GetFavoritePopSongs()
{
}
```

If `GetPopSongs` returns `null` for no data, `GetFavoritePopSongs` needs to check `null` every time and codes get exception if null data is accessed accidentally.

```C#
public static IEnumerable<string> GetPopSongs()
{
    var popSongs = null;
    ...
}
```

```C#
public static IEnumerable<string> GetFavoritePopSongs()
{
    var popSongs = GetPopSongs();

    if(popSongs != null)
    {
        for(var popSong in popSongs)
        {
            ...
        }
    }
}
```

The problem can be solved by returning empty collection instead of null. In this case, other methods need not to check null every time it uses returns from the first method.

```C#
public static IEnumerable<string> GetPopSongs()
{
    var popSongs = IEnumerable.Empty<string>();
    ...
}
```

```C#
public static IEnumerable<string> GetFavoritePopSongs()
{
    var popSongs = GetPopSongs();
    
    for(var popSong in popSongs)
    {
        ...
    }
}
```

<!---
## Vertical code
-->

## Name Things Correctly

Always give self-explanatory, searchable, usable names to the entity, even if that requires long names. Unusable names make codes unusable easily.

```C#
int maximumAgeOfAllStudents;
int maximumAgeOfAllTeachers;
int maximumAgeOfAllStudentsAndTeachers;

public int GetMaximumAgeOfAllStudentsAndTeachers()
{

}
```
