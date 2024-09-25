DATE: 2024-07-25
TIME: 14:46
STATE:
TAGS: 
# NOTE

Today I learned that my object null reference is caused by not mapping my player but calling it in my sut.
I took note of that below: 
[[Code From My PlayerDataStorage]].

& Also, on the above/linked file, I need to check what implication it has when I remove the return player without the score and with the score because when I run the swagger ui, it seems as though nothing gets too affected if I just call the Func<> (FirstOrDefault) without the .Inclued Func<> So I'm not too sure if there's a big difference or not.