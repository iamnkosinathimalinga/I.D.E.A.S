
DATE: 2024-07-25
TIME: 14:17
STATE: #developing 
TAGS:  [[Software-Development]] [[Code]] [[TDD]]

# NOTE

```
  [Fact]
 public void CalculatePoints_ShouldSetPointsCorrectly_WhenCalled()
 {
     // Arrange
     var determineScoreHelper = new Mock<IDetermineScoreHelper>();
     determineScoreHelper.Setup(dsh => dsh.DetermineScore(It.IsAny<bool>(), It.IsAny<PlayerAnswerQuality>()))
                         .Returns(10.0);
     var score = new Score
     {
         ClappedFirst = true,
         PlayerAnswerQuality = PlayerAnswerQuality.Correct
     };

     // Act
     score.CalculatePoints(determineScoreHelper.Object);

     // Assert
     determineScoreHelper.Verify(dsh => dsh.DetermineScore(score.ClappedFirst, score.PlayerAnswerQuality), Times.Once);
     score.Points.Should().Be(10.0);
 }

 [Fact]
 public void CalculatePoints_ShouldCallDetermineScoreMethod_OfDetermineScoreHelper()
 {
     // Arrange
     var determineScoreHelper = new Mock<IDetermineScoreHelper>();
     var score = new Score
     {
         ClappedFirst = true,
         PlayerAnswerQuality = PlayerAnswerQuality.Correct
     };

     // Act
     score.CalculatePoints(determineScoreHelper.Object);

     // Assert
     determineScoreHelper.Verify(dsh => dsh.DetermineScore(It.IsAny<bool>(), It.IsAny<PlayerAnswerQuality>()), Times.Once);
 }
```