DATE: 2024-07-29
TIME: 09:46
STATE:
TAGS: [[P.U]] [[Work 1]] [[Software-Development]]
# NOTE

Today, I will work on implementing the Game Entity.

& here's the generated plan: 
- **Define Entities**: Create `Game`, `Player`, and `GamePlayer` entities.
- **Configure Many-to-Many Relationship**: Set up the many-to-many relationship between `Game` and `Player`.
- **Create DbContext and Mappings**: Update the `DbContext` to include these entities.
- **Implement Repositories and Services**: Implement methods to create games, add players to games, record scores, and retrieve players in a game.
- **Update the Program**: Ensure the DI container is correctly configured.
- **Testing**: Write tests for the new functionality.