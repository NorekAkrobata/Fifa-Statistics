# FifaStats

Creating a **statistics summary** from Fifa 20 matches based on post-match png **images**. Statistics summary can be **visualised as Pandas DataFrame, Matplotlib bar charts or simply extracted to excel** to allow non-python users to perform they own analyses. 

# How it works?

First of all, we have to make a **screenshoot** at the end of Fifa 20 match which looks exactly like the one below. More examples can be found in "Data" repository.

![Alt text](/Examples/CuttingExample.png?raw=true "Post-match ss example")

Above example also shows us what parts of image are taken to perfom further actions (colored rectangles). Team name is needed to check if our team was playing home or away.

Pre-processed images used by pytesseract to gather statistics can be seen below.

![Alt text](/Examples/TeamNameExample.PNG?raw=true "TeamNameExample")
![Alt text](/Examples/StatsExample.PNG?raw=true "StatsExample")

![Alt text](/Examples/HomeScoreExample.PNG?raw=true "HomeScoreExample")
![Alt text](/Examples/AwayScoreExample.PNG?raw=true "AwayScoreExample")

Gathered data is then placed into Pandas DataFrame. As mentioned we can either present data in form of Python Series, barcharts or export data into Excel. All examples can be find below.

![Alt text](/Examples/StatsSeriesExample.PNG?raw=true "StatsSeriesExample")

![Alt text](/Examples/StatsSmallExample.PNG?raw=true "StatsSmallExample")

![Alt text](/Examples/StatsBigExample.PNG?raw=true "StatsBigExample")

![Alt text](/Examples/StatsExcelExample.PNG?raw=true "StatsExcelExample")

# Used Libraries
- Pandas
- Numpy
- Matplotlib
- OpenCV
- Pytesseract

# ToDo List
- Update to FIFA 21 (October 2020)
- Add possibility to differentiate game-modes
- Add possibility to gather goals, assists, game ratings data for each player 