DATE: 2024-07-29
TIME: 09:50
STATE: #child  #developing 
TAGS: [[P.U]] [[C]] [[3 - Tags/Work]] [[Block-11-Assignment]] [[Deprecated]]
# NOTE

### This Code Is Implementing Logic we already had on playerStorage & rendering redundant & useless, it's good for understanding the setting up of a joining entity, as it makes it very straightforward and configurable.
### ~~`Step 1: Define Entities~~`

`~~Let's define the Game, Player, and GamePlayer entities.~~`

#### `~~Game.cs~~`

`~~csharp~~`
`~~Step 1: Define Entities~~`
`~~Let's define the Game, Player, and GamePlayer entities.~~`

`~~Game.cs~~`
`~~csharp~~`
`~~Copy code~~`
`~~public class Game~~`
`~~{~~`
    `~~public int Id { get; set; }~~`
    `~~public DateOnly GameDate { get; set; }~~`
    `~~public string GameLeader { get; set; }~~`

    `~~public ICollection<GamePlayer> GamePlayers { get; set; }~~`
`~~}~~`
`~~Player.cs~~`
`~~csharp~~`
`~~Copy code~~`
`~~public class Player~~`
`~~{~~`
    `~~public int Id { get; set; }~~`
    `~~public string FirstName { get; set; }~~`
    `~~public string LastName { get; set; }~~`
    
    `~~public ICollection<GamePlayer> GamePlayers { get; set; }~~`
`~~}~~`
`~~GamePlayer.cs~~`
`~~csharp~~`
`~~Copy code~~`
`~~public class GamePlayer~~`
`~~{~~`
    `~~public int GameId { get; set; }~~`
    `~~public Game Game { get; set; }~~`

    `~~public int PlayerId { get; set; }~~`
    `~~public Player Player { get; set; }~~`

    `~~public ICollection<Score> Scores { get; set; }~~`
`~~}~~`
`~~Score.cs~~`
`~~csharp~~`
`~~Copy code~~`
`~~public class Score~~`
`~~{~~`
    `~~public int Id { get; set; }~~`
    `~~public int QuestionId { get; set; }~~`
    `~~public bool ClappedFirst { get; set; }~~`
    `~~public int PlayerAnswerQuality { get; set; }~~`
    `~~public int Points { get; set; }~~`

    `~~public int GamePlayerId { get; set; }~~`
    `~~public GamePlayer GamePlayer { get; set; }~~`
`~~}~~`
`~~Step 2: Configure Many-to-Many Relationship~~`
`~~Configure the relationship in the DbContext.~~`

`~~GameDbContext.cs~~`
`~~csharp~~`
`~~Copy code~~`
`~~public class GameDbContext : DbContext~~`
`~~{~~`
    `~~public DbSet<Player> Players { get; set; }~~`
    `~~public DbSet<Game> Games { get; set; }~~`
    `~~public DbSet<GamePlayer> GamePlayers { get; set; }~~`
    `~~public DbSet<Score> Scores { get; set; }~~`

    `~~protected override void OnModelCreating(ModelBuilder modelBuilder)~~`
    `~~{~~`
        `~~modelBuilder.Entity<GamePlayer>()~~`
            `~~.HasKey(gp => new { gp.GameId, gp.PlayerId });~~`

        `~~modelBuilder.Entity<GamePlayer>()~~`
            `~~.HasOne(gp => gp.Game)~~`
            `~~.WithMany(g => g.GamePlayers)~~`
            `~~.HasForeignKey(gp => gp.GameId);~~`

        `~~modelBuilder.Entity<GamePlayer>()~~`
            `~~.HasOne(gp => gp.Player)~~`
            `~~.WithMany(p => p.GamePlayers)~~`
            `~~.HasForeignKey(gp => gp.PlayerId);~~`
    `~~}~~`
`~~}~~`
`~~Step 3: Implement Repositories and Services~~`
`~~IGameRepository.cs~~`
`~~csharp~~`
`~~Copy code~~`
`~~public interface IGameRepository~~`
`~~{~~`
    `~~void CreateGame(Game game);~~`
    `~~void AddPlayerToGame(int gameId, int playerId);~~`
    `~~IEnumerable<Player> GetPlayersInGame(int gameId);~~`
    `~~void AddScoreToPlayerInGame(int gameId, int playerId, Score score);~~`
`~~}~~`
`~~GameRepository.cs~~`
`~~csharp~~`
`~~Copy code~~`
`~~public class GameRepository : IGameRepository~~`
`~~{~~`
    `~~private readonly GameDbContext _context;~~`

    `~~public GameRepository(GameDbContext context)~~`
    `~~{~~`
        `~~_context = context;~~`
    `~~}~~`

    `~~public void CreateGame(Game game)~~`
    `~~{~~`
        `~~_context.Games.Add(game);~~`
        `~~_context.SaveChanges();~~`
    `~~}~~`

    `~~public void AddPlayerToGame(int gameId, int playerId)~~`
    `~~{~~`
        `~~var gamePlayer = new GamePlayer { GameId = gameId, PlayerId = playerId };~~`
        `~~_context.GamePlayers.Add(gamePlayer);~~`
        `~~_context.SaveChanges();~~`
    `~~}~~`

    `~~public IEnumerable<Player> GetPlayersInGame(int gameId)~~`
    `~~{~~`
        `~~return _context.GamePlayers~~`
            `~~.Include(gp => gp.Player)~~`
            `~~.Where(gp => gp.GameId == gameId)~~`
            `~~.Select(gp => gp.Player)~~`
            `~~.ToList();~~`
    `~~}~~`

    `~~public void AddScoreToPlayerInGame(int gameId, int playerId, Score score)~~`
    `~~{~~`
        `~~var gamePlayer = _context.GamePlayers~~`
            `~~.FirstOrDefault(gp => gp.GameId == gameId && gp.PlayerId == playerId);~~`

        `~~if (gamePlayer != null)~~`
        `~~{~~`
            `~~gamePlayer.Scores.Add(score);~~`
            `~~_context.SaveChanges();~~`
        `~~}~~`
    `~~}~~`
`~~}~~`
`~~Step 4: Update DI Configuration~~`
`~~Update the Program.cs to include the new repository.~~`

`~~Program.cs~~`
`~~csharp~~`
`~~Copy code~~`
`~~public class Program~~`
`~~{~~`
    `~~public static void Main(string[] args)~~`
    `~~{~~`
        `~~var builder = WebApplication.CreateBuilder(args);~~`

        `~~// Add services to the container.~~`
        `~~builder.Services.AddDbContext<GameDbContext>(options =>~~`
            `~~options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));~~`

        `~~builder.Services.AddScoped<IGameRepository, GameRepository>();~~`
        `~~builder.Services.AddScoped<IPlayerDataStore, PlayerDataStorage>();~~`
        `~~builder.Services.AddScoped<IPlayerMapper, PlayerMapper>();~~`
        `~~builder.Services.AddScoped<IScoreMapper, ScoreMapper>();~~`

        `~~builder.Services.AddControllers();~~`
        `~~builder.Services.AddEndpointsApiExplorer();~~`
        `~~builder.Services.AddSwaggerGen();~~`

        `~~var app = builder.Build();~~`

        `~~// Configure the HTTP request pipeline.~~`
        `~~if (app.Environment.IsDevelopment())~~`
        `~~{~~`
            `~~app.UseSwagger();~~`
            `~~app.UseSwaggerUI();~~`
        `~~}~~`

        `~~app.UseHttpsRedirection();~~`
        `~~app.UseAuthorization();~~`
        `~~app.MapControllers();~~`
        `~~app.Run();~~`
    `~~}~~`
`~~}~~`
`~~Step 5: Write Tests~~`
`~~GameRepositoryTests.cs~~`
`~~csharp~~`
`~~Copy code~~`
`~~public class GameRepositoryTests~~`
`~~{~~`
    `~~[Fact]~~`
    `~~public void CreateGame_ShouldAddGameToDatabase()~~`
    `~~{~~`
        `~~// Arrange~~`
        `~~var options = new DbContextOptionsBuilder<GameDbContext>()~~`
            `~~.UseInMemoryDatabase(databaseName: "GameDatabase")~~`
            `~~.Options;~~`

        `~~using (var context = new GameDbContext(options))~~`
        `~~{~~`
            `~~var repository = new GameRepository(context);~~`
            `~~var game = new Game { GameDate = DateOnly.FromDateTime(DateTime.Today), GameLeader = "Leader" };~~`

            `~~// Act~~`
            `~~repository.CreateGame(game);~~`

            `~~// Assert~~`
            `~~var createdGame = context.Games.FirstOrDefault(g => g.GameLeader == "Leader");~~`
            `~~createdGame.Should().NotBeNull();~~`
        `~~}~~`
    `~~}~~`

    `~~[Fact]~~`
    `~~public void AddPlayerToGame_ShouldAddPlayerToGame()~~`
    `~~{~~`
        `~~// Arrange~~`
        `~~var options = new DbContextOptionsBuilder<GameDbContext>()~~`
            `~~.UseInMemoryDatabase(databaseName: "GameDatabase")~~`
            `~~.Options;~~`

        `~~using (var context = new GameDbContext(options))~~`
        `~~{~~`
            `~~var repository = new GameRepository(context);~~`
            `~~var game = new Game { GameDate = DateOnly.FromDateTime(DateTime.Today), GameLeader = "Leader" };~~`
            `~~var player = new Player { FirstName = "John", LastName = "Doe" };~~`

            `~~context.Games.Add(game);~~`
            `~~context.Players.Add(player);~~`
            `~~context.SaveChanges();~~`

            `~~// Act~~`
            `~~repository.AddPlayerToGame(game.Id, player.Id);~~`

            `~~// Assert~~`
            `~~var gamePlayer = context.GamePlayers.FirstOrDefault(gp => gp.GameId == game.Id && gp.PlayerId == player.Id);~~`
            `~~gamePlayer.Should().NotBeNull();~~`
        `~~}~~`
    `~~}~~`

    `~~[Fact]~~`
    `~~public void GetPlayersInGame_ShouldReturnPlayersInGame()~~`
    `~~{~~`
        `~~// Arrange~~`
        `~~var options = new DbContextOptionsBuilder<GameDbContext>()~~`
            `~~.UseInMemoryDatabase(databaseName: "GameDatabase")~~`
            `~~.Options;~~`

        `~~using (var context = new GameDbContext(options))~~`
        `~~{~~`
            `~~var repository = new GameRepository(context);~~`
            `~~var game = new Game { GameDate = DateOnly.FromDateTime(DateTime.Today), GameLeader = "Leader" };~~`
            `~~var player1 = new Player { FirstName = "John", LastName = "Doe" };~~`
            `~~var player2 = new Player { FirstName = "Jane", LastName = "Doe" };~~`

            `~~context.Games.Add(game);~~`
            `~~context.Players.AddRange(player1, player2);~~`
            `~~context.GamePlayers.Add(new GamePlayer { Game = game, Player = player1 });~~`
            `~~context.GamePlayers.Add(new GamePlayer { Game = game, Player = player2 });~~`
            `~~context.SaveChanges();~~`

            `~~// Act~~`
            `~~var players = repository.GetPlayersInGame(game.Id);~~`

            `~~// Assert~~`
            `~~players.Should().Contain(p => p.FirstName == "John");~~`
            `~~players.Should().Contain(p => p.FirstName == "Jane");~~`
        `~~}~~`
    `~~}~~`

    `~~[Fact]~~`
    `~~public void AddScoreToPlayerInGame_ShouldAddScoreToPlayer()~~`
    `~~{~~`
        `~~// Arrange~~`
        `~~var options = new DbContextOptionsBuilder<GameDbContext>()~~`
            `~~.UseInMemoryDatabase(databaseName: "GameDatabase")~~`
            `~~.Options;~~`

        `~~using (var context = new GameDbContext(options))~~`
        `~~{~~`
            `~~var repository = new GameRepository(context);~~`
            `~~var game = new Game { GameDate = DateOnly.FromDateTime(DateTime.Today), GameLeader = "Leader" };~~`
            `~~var player = new Player { FirstName = "John", LastName = "Doe" };~~`

            `~~context.Games.Add(game);~~`
            `~~context.Players.Add(player);~~`
            `~~context.GamePlayers.Add(new GamePlayer { Game = game, Player = player });~~`
            `~~context.SaveChanges();~~`

            `~~var score = new Score { QuestionId = 1, ClappedFirst = true, PlayerAnswerQuality = 5, Points = 10 };~~`

            `~~// Act~~`
            `~~repository.AddScoreToPlayerInGame(game.Id, player.Id, score);~~`

            `~~// Assert~~`
            `~~var gamePlayer = context.GamePlayers~~`
                `~~.Include(gp => gp.Scores)~~`
                `~~.FirstOrDefault(gp => gp.GameId == game.Id && gp.PlayerId == player.Id);~~`

            `~~gamePlayer.Scores.Should().Contain(s => s.QuestionId == 1);~~`
        `~~}~~`
    `~~}~~`
`~~}~~`
`~~This setup should give you a good foundation for working with the "Game" concept, allowing you to create games, add players to them, record scores, and retrieve players in a game using Entity Framework. Adjust the classes and methods as necessary to fit your exact requirements and architecture.~~`
`~~Copy code~~`

`~~`  `public class Game {     public int Id { get; set; }     public DateOnly GameDate { get; set; }     public string GameLeader { get; set; }      public ICollection<GamePlayer> GamePlayers { get; set; } }`~~``

``~~#### Player.cs~~``

``~~csharp~~``

``~~Copy code~~``

``~~`public class Player {     public int Id { get; set; }     public string FirstName { get; set; }     public string LastName { get; set; }          public ICollection<GamePlayer> GamePlayers { get; set; } }`~~``

``~~#### GamePlayer.cs~~``

``~~csharp~~``

``~~Copy code~~``

``~~`public class GamePlayer {     public int GameId { get; set; }     public Game Game { get; set; }      public int PlayerId { get; set; }     public Player Player { get; set; }      public ICollection<Score> Scores { get; set; } }`~~``

``~~#### Score.cs~~``

``~~csharp~~``

``~~Copy code~~``

``~~`public class Score {     public int Id { get; set; }     public int QuestionId { get; set; }     public bool ClappedFirst { get; set; }     public int PlayerAnswerQuality { get; set; }     public int Points { get; set; }      public int GamePlayerId { get; set; }     public GamePlayer GamePlayer { get; set; } }`~~``

``~~### Step 2: Configure Many-to-Many Relationship~~``

``~~Configure the relationship in the `DbContext`.~~``

``~~#### GameDbContext.cs~~``

``~~csharp~~``

``~~Copy code~~``

``~~`public class GameDbContext : DbContext {     public DbSet<Player> Players { get; set; }     public DbSet<Game> Games { get; set; }     public DbSet<GamePlayer> GamePlayers { get; set; }     public DbSet<Score> Scores { get; set; }      protected override void OnModelCreating(ModelBuilder modelBuilder)     {         modelBuilder.Entity<GamePlayer>()             .HasKey(gp => new { gp.GameId, gp.PlayerId });          modelBuilder.Entity<GamePlayer>()             .HasOne(gp => gp.Game)             .WithMany(g => g.GamePlayers)             .HasForeignKey(gp => gp.GameId);          modelBuilder.Entity<GamePlayer>()             .HasOne(gp => gp.Player)             .WithMany(p => p.GamePlayers)             .HasForeignKey(gp => gp.PlayerId);     } }`~~``

``~~### Step 3: Implement Repositories and Services~~``

``~~#### IGameRepository.cs~~``

``~~csharp~~``

``~~Copy code~~``

``~~`public interface IGameRepository {     void CreateGame(Game game);     void AddPlayerToGame(int gameId, int playerId);     IEnumerable<Player> GetPlayersInGame(int gameId);     void AddScoreToPlayerInGame(int gameId, int playerId, Score score); }`~~``

``~~#### GameRepository.cs~~``

``~~csharp~~``

``~~Copy code~~``

``~~`public class GameRepository : IGameRepository {     private readonly GameDbContext _context;      public GameRepository(GameDbContext context)     {         _context = context;     }      public void CreateGame(Game game)     {         _context.Games.Add(game);         _context.SaveChanges();     }      public void AddPlayerToGame(int gameId, int playerId)     {         var gamePlayer = new GamePlayer { GameId = gameId, PlayerId = playerId };         _context.GamePlayers.Add(gamePlayer);         _context.SaveChanges();     }      public IEnumerable<Player> GetPlayersInGame(int gameId)     {         return _context.GamePlayers             .Include(gp => gp.Player)             .Where(gp => gp.GameId == gameId)             .Select(gp => gp.Player)             .ToList();     }      public void AddScoreToPlayerInGame(int gameId, int playerId, Score score)     {         var gamePlayer = _context.GamePlayers             .FirstOrDefault(gp => gp.GameId == gameId && gp.PlayerId == playerId);          if (gamePlayer != null)         {             gamePlayer.Scores.Add(score);             _context.SaveChanges();         }     } }`~~``

``~~### Step 4: Update DI Configuration~~``

``~~Update the `Program.cs` to include the new repository.~~``

``~~#### Program.cs~~``

``~~csharp~~``

``~~Copy code~~``

``~~`public class Program {     public static void Main(string[] args)     {         var builder = WebApplication.CreateBuilder(args);          // Add services to the container.         builder.Services.AddDbContext<GameDbContext>(options =>             options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));          builder.Services.AddScoped<IGameRepository, GameRepository>();         builder.Services.AddScoped<IPlayerDataStore, PlayerDataStorage>();         builder.Services.AddScoped<IPlayerMapper, PlayerMapper>();         builder.Services.AddScoped<IScoreMapper, ScoreMapper>();          builder.Services.AddControllers();         builder.Services.AddEndpointsApiExplorer();         builder.Services.AddSwaggerGen();          var app = builder.Build();          // Configure the HTTP request pipeline.         if (app.Environment.IsDevelopment())         {             app.UseSwagger();             app.UseSwaggerUI();         }          app.UseHttpsRedirection();         app.UseAuthorization();         app.MapControllers();         app.Run();     } }`~~``

``~~### Step 5: Write Tests~~``

``~~#### GameRepositoryTests.cs~~``

``~~csharp~~``

``~~Copy code~~``

``~~`public class GameRepositoryTests {     [Fact]     public void CreateGame_ShouldAddGameToDatabase()     {         // Arrange         var options = new DbContextOptionsBuilder<GameDbContext>()             .UseInMemoryDatabase(databaseName: "GameDatabase")             .Options;          using (var context = new GameDbContext(options))         {             var repository = new GameRepository(context);             var game = new Game { GameDate = DateOnly.FromDateTime(DateTime.Today), GameLeader = "Leader" };              // Act             repository.CreateGame(game);              // Assert             var createdGame = context.Games.FirstOrDefault(g => g.GameLeader == "Leader");             createdGame.Should().NotBeNull();         }     }      [Fact]     public void AddPlayerToGame_ShouldAddPlayerToGame()     {         // Arrange         var options = new DbContextOptionsBuilder<GameDbContext>()             .UseInMemoryDatabase(databaseName: "GameDatabase")             .Options;          using (var context = new GameDbContext(options))         {             var repository = new GameRepository(context);             var game = new Game { GameDate = DateOnly.FromDateTime(DateTime.Today), GameLeader = "Leader" };             var player = new Player { FirstName = "John", LastName = "Doe" };              context.Games.Add(game);             context.Players.Add(player);             context.SaveChanges();              // Act             repository.AddPlayerToGame(game.Id, player.Id);              // Assert             var gamePlayer = context.GamePlayers.FirstOrDefault(gp => gp.GameId == game.Id && gp.PlayerId == player.Id);             gamePlayer.Should().NotBeNull();         }     }      [Fact]     public void GetPlayersInGame_ShouldReturnPlayersInGame()     {         // Arrange         var options = new DbContextOptionsBuilder<GameDbContext>()             .UseInMemoryDatabase(databaseName: "GameDatabase")             .Options;          using (var context = new GameDbContext(options))         {             var repository = new GameRepository(context);             var game = new Game { GameDate = DateOnly.FromDateTime(DateTime.Today), GameLeader = "Leader" };             var player1 = new Player { FirstName = "John", LastName = "Doe" };             var player2 = new Player { FirstName = "Jane", LastName = "Doe" };              context.Games.Add(game);             context.Players.AddRange(player1, player2);             context.GamePlayers.Add(new GamePlayer { Game = game, Player = player1 });             context.GamePlayers.Add(new GamePlayer { Game = game, Player = player2 });             context.SaveChanges();              // Act             var players = repository.GetPlayersInGame(game.Id);              // Assert             players.Should().Contain(p => p.FirstName == "John");             players.Should().Contain(p => p.FirstName == "Jane");         }     }      [Fact]     public void AddScoreToPlayerInGame_ShouldAddScoreToPlayer()     {         // Arrange         var options = new DbContextOptionsBuilder<GameDbContext>()             .UseInMemoryDatabase(databaseName: "GameDatabase")             .Options;          using (var context = new GameDbContext(options))         {             var repository = new GameRepository(context);             var game = new Game { GameDate = DateOnly.FromDateTime(DateTime.Today), GameLeader = "Leader" };             var player = new Player { FirstName = "John", LastName = "Doe" };              context.Games.Add(game);             context.Players.Add(player);             context.GamePlayers.Add(new GamePlayer { Game = game, Player = player });             context.SaveChanges();              var score = new Score { QuestionId = 1, ClappedFirst = true, PlayerAnswerQuality = 5, Points = 10 };              // Act             repository.AddScoreToPlayerInGame(game.Id, player.Id, score);              // Assert             var gamePlayer = context.GamePlayers                 .Include(gp => gp.Scores)                 .FirstOrDefault(gp => gp.GameId == game.Id && gp.PlayerId == player.Id);              gamePlayer.Scores.Should().Contain(s => s.QuestionId == 1);         }     } }`~~``

``~~This setup should give you a good foundation for working with the "Game" concept, allowing you to create games, add players to them, record scores, and retrieve players in a game using Entity Framework. Adjust the classes and methods as necessary to fit your exact requirements and architecture.`~~`