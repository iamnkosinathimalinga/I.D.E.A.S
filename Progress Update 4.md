DATE: 2024-08-03
TIME: 19:52
STATE: #child #developing #AddOn 
TAGS:  [Work 1](Work%201.md) [P.U](./P.U.md) 

# NOTE

```
Today is going a bit slow, I got a bit side tracked and lost a lot of hours from last night, Today though things seem to be looking rather proficient and sturdy, last night I also learnt about " _MapToScoreEntity_ " &  why we don't even need to " _MapToScore_ " => It's quite simple and logical frankly: We already " _MapToPlayerEntity_ " which contains a _List<ScoreEntity>_ which Maps a 'PlayerScore' to the 'PlayerEntityScore'
ergo when returning a 'PlayerEntity' from the database you can simply 'MapToPlayer' & the 'PlayerEntity' should return a 'Player' with 'Scores'.

I also got a bit of insight on a feature(rule) that I never got around to working on because my progress was slow, so through my little yet pragmatic overview of how <FluentValidator> works, it basically handles things that normally(currently) my project checks via a series of 'If-else' statements, which become cumbersome and are hard to read at times if you want to validate a lot of things like if a 'PlayerId' is > 1 & the 'PlayerName' is not null, and is a valid string, etc...So instead of doing all of that yourself you have a class with the package <FluentValidator> installed which does all of these checks for you, and that is, of course how you remove logic from the store(which should not really have any validation rules in it, as it just stores) or atleast I'm told. Anyway you then obviously inject the class with your <FluentValidator> in to your BusinessLogic.Interactors and parse it the parameters you would like to validate. Oh, I also remember that I learned the <FluentValidator> class  only returns types Boolean, I am yet to implement the use of the validator so that's just something I will have to see and work through myself to understand why, all in all, I learned a fair bit from last night's conversatin with Ntokozo, and came to a bit of an idea: 

Of course this is hypothetical as I have reason to believe it might not even work, lol, but it's worth a shot, so in my CreateGameOutput I want my 'ResponseMessage' to return a message that interpolates the 'GameId' which was created, this of course sounds simple ("in theory") - to implement but I guess we shall see how true that is when we get to it.

Oh, and also I want to change my solution's - codebase to use primary constructors and new list initialization expressions: which I think look something like this: (P.C) 

< class CreateGame(IGameDataStore gameDataStore) : ICreateGame 
private readonly IGameDataStore _gameDataStore = gameDataStore; /> (end of P.C)

(N.L.I) < new List<Score> = [] > (end of N.L.I)

< #AddOn > #AddOn Oh, and one more thing, I need to move my business logic tests to the BusinessLogicLayerTests file, they are in PlayerStorageTests now, this is wrong. 
=> <Optionally> I can also implement the use of 'Test.Fixtures' for my 'BusinessLogicTests' and 'PlayerDataStorage'
#AddOn \\ *End Of* < * #AddOn * />
```


$$
**R.C.P** =>  
1. **Interpolating Game IDs:** Your idea of interpolating the `GameId` into the response message for `CreateGameOutput` is a nice touch! It adds a personal touch to the feedback you provide after creating a game. In practice, you’ll likely have a template string (maybe something like `"Game {GameId} created successfully!"`) that you can populate with the actual `GameId` value. Keep an eye out for any edge cases (e.g., what if the `GameId` is null or invalid?).
2.  **Mapping Entities and Scores:** Your understanding of mapping entities and scores seems spot on! When dealing with data models and their relationships, it’s crucial to have efficient ways to map between them. By already having a `MapToPlayerEntity` method that includes a list of `ScoreEntity`, you’re well-prepared. When retrieving a `PlayerEntity` from the database, you can indeed use the existing mapping logic to get a `Player` with associated scores. It’s a great way to keep your code concise and maintainable.
3.  **Fluent Validation:** Ah, the joys of validation! You’ve stumbled upon a powerful tool with **Fluent Validation**. It’s fantastic for handling complex validation rules without cluttering your business logic. By encapsulating validation logic in a separate class, you achieve separation of concerns. Plus, the fluent syntax makes it readable and expressive. When you inject the validator into your interactors, you’re following a clean architecture pattern. Remember to handle the Boolean return values appropriately—maybe log validation failures or provide user-friendly error messages.
$$
#### References

[KeyMap](KeyMap.md#^b66171) & [KeyMap](KeyMap.md#^b5963b) - **P.C**, **N.L.I**, **R.C.P **is used in this **P.U**