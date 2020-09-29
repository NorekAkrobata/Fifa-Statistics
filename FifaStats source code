#Libraries

import os
import glob
import cv2
import numpy as np
import pandas as pd
from matplotlib import pyplot as plt
import pytesseract
import imutils
pytesseract.pytesseract.tesseract_cmd = r'C:\\Program Files\\Tesseract-OCR\\tesseract.exe'

#Working directory
os.getcwd()
path='C:\\Users\\Norbert\\Desktop\\fifa ss'
os.chdir(path)
fileslist = [i for i in glob.glob("*.png")]
print(fileslist)

#Functions

def getImages (path):
    os.chdir(path)
    fileslist = [i for i in glob.glob("*.png")]
    global images
    images = []
    for j in fileslist:
        temp = cv2.imread(j)
        temp = cv2.cvtColor(temp, cv2.COLOR_BGR2GRAY)
        images.append(temp)
    print('You are analyzing stats from {} fifa matches'.format(len(fileslist)))
	
def cutStatsFinal (images):
    global threshHome, threshStats, threshScoreHome, threshScoreAway
    threshHome, threshStats, threshScoreHome, threshScoreAway = [],[],[],[]
    for i in images:
        temp1, temp2 = i[150:220, 400:1500], i[440:840,1000:1770]
        temp2[0:400,135:635] = 255
        temp2 = imutils.resize(temp2, height=1000,width=1500)
        temp3, temp4 = i[150:220, 820:920], i[150:220, 1000:1100]
        temp3, temp4 = imutils.resize(temp3, height=200,width=200), imutils.resize(temp4, height=200,width=200)   
        thresh1 = cv2.threshold(temp1, 180, 255, cv2.THRESH_BINARY_INV)[1]
        thresh2 = cv2.threshold(temp2, 0 , 255, cv2.THRESH_BINARY_INV + cv2.THRESH_OTSU)[1]
        thresh2 = 255 - cv2.GaussianBlur(thresh2, (3,3), 0)
        thresh3 = cv2.threshold(temp3, 140 , 255, cv2.THRESH_BINARY)[1]
        thresh4 = cv2.threshold(temp4, 140 , 255, cv2.THRESH_BINARY)[1]
        thresh3, thresh4 = 255 - cv2.GaussianBlur(thresh3, (3,3), 0), 255 - cv2.GaussianBlur(thresh4, (3,3), 0)
        threshHome.append(thresh1)
        threshStats.append(thresh2)
        threshScoreHome.append(thresh3)
        threshScoreAway.append(thresh4)
		
def readStats(threshStats, threshScoreHome, threshScoreAway):
    config = r'--psm 6 --oem 3 -c tessedit_char_whitelist=0123456789'
    cong = r'--oem 3 --psm 6 outputbase digits'
    global Stats, Scores
    Stats, Scores = [],[]
    for i in threshStats:
        tempStats = []
        temp = pytesseract.image_to_data(i, config=config)
        for x, t in enumerate(temp.splitlines()):
            if x != 0:
                t = t.split()
                if len(t) == 12:
                    tempStats.append(int(t[11]))
        Stats.append(tempStats)        
    for i,j in zip(threshScoreHome,threshScoreAway):
        tempScore = []
        tempHome = pytesseract.image_to_data(i, config=config)
        tempAway = pytesseract.image_to_data(j, config=config) 
        for x, t in enumerate(tempHome.splitlines()):
            if x != 0:
                t = t.split()
                if len(t) == 12:
                    tempScore.append(int(t[11]))    
        for x, t in enumerate(tempAway.splitlines()):
            if x != 0:
                t = t.split()
                if len(t) == 12:
                    tempScore.append(int(t[11]))
        if not tempScore:
            Scores.append(['RQ', 'RQ'])
        else:
            Scores.append(tempScore)
			
def readHome(threshHome):
    global Side
    Home = []
    for i in threshHome:
        tempHome = []
        temp = pytesseract.image_to_data(i)
        for x, t in enumerate(temp.splitlines()):
            if x == 5:
                t = t.split()
                if len(t) == 12:
                    tempHome.append(t[11])
        Home.append(tempHome)
    Side = []
    for j in Home:
        if j[0].startswith('TEMA'):
            Side.append('Home')
        else:
            Side.append('Away')

def sortingStats(Stats, Side, Scores):
    for i in range(len(Stats)):
        if Side[i] == 'Away':
            Stats[i][0],Stats[i][1],Stats[i][2],Stats[i][3],Stats[i][4],Stats[i][5],Stats[i][6],Stats[i][7],Stats[i][8],Stats[i][9],Stats[i][10],Stats[i][11],Stats[i][12],Stats[i][13],Stats[i][14],Stats[i][15] = Stats[i][1],Stats[i][0],Stats[i][3],Stats[i][2],Stats[i][5],Stats[i][4],Stats[i][7],Stats[i][6],Stats[i][9],Stats[i][8],Stats[i][11],Stats[i][10],Stats[i][13],Stats[i][12],Stats[i][15],Stats[i][14]
            Scores[i][0], Scores[i][1] = Scores[i][1], Scores[i][0]
    return Stats, Scores

def BarCharts(results, category_names):

    labels = list(results.keys())
    data = np.array(list(results.values()))
    category_colors = plt.get_cmap('RdYlGn')(
        np.linspace(0.15, 0.85, data.shape[1]))

    fig, ax = plt.subplots(figsize=(9.2, 5))
    ax.invert_yaxis()
    ax.xaxis.set_visible(False)
    if len(labels) > 3:
        ax.set_xlim(-10, 10)
    else:
        ax.set_xlim(-100,100)

    for i, (colname, color) in enumerate(zip(category_names, category_colors)):
        widths = data[:, i]
        starts = 0
        ax.barh(labels, widths, left=starts, height=0.5,
                label=colname, color=color)
        xcenters = starts + widths / 2
        
        r, g, b, _ = color
        text_color = 'black' if r * g * b < 0.5 else 'darkgrey'
        for y, (x, c) in enumerate(zip(xcenters, widths)):
            ax.text(x, y, str(float(abs(c))), ha='center', va='center',
                    color=text_color)
    ax.legend(ncol=len(category_names), bbox_to_anchor=(0, 1),
              loc='lower left', fontsize='small')

    return fig, ax

#Main code

getImages(path)
cutStatsFinal(images)
readStats(threshStats, threshScoreHome, threshScoreAway)
readHome(threshHome)
sortingStats(Stats,Side,Scores)

Score = pd.DataFrame(Scores)
Stats = pd.DataFrame(Stats)
StatsSummary = pd.merge(Score, Stats, left_index=True, right_index=True)
StatsSummary.columns = ['GoalsW', 'Goals', 'ShootsW', 'Shoots', 'OnTargetW', 'OnTarget', 'PossesionW [%]', 'Possesion [%]', 'TacklesW', 'Tackles', 'FoulsW', 'Fouls', 'CornersW', 'Corners', 'Shot AccuracyW [%]', 'Shot Accuracy [%]', 'Pass AccuracyW [%]', 'Pass Accuracy [%]']
StatsSummary[['GoalsW','Goals']] = StatsSummary[['GoalsW','Goals']].apply(pd.to_numeric, errors='coerce')

Summary = StatsSummary[~StatsSummary['GoalsW'].isnull()]
Summary.reset_index(drop=True, inplace=True)
Summary = Summary.mean().round(1)

#print(StatsSummary)
#print(Summary)
#StatsSummary.to_excel('Stats.xlsx')

Category = ['OurTeam', 'Opponent']

ResultsSmall = {
    'Goals':[-Summary[0],Summary[1]],
    'Shoots':[-Summary[2],Summary[3]],
    'On target':[-Summary[4],Summary[5]],
    'Tackles':[-Summary[8],Summary[9]],
    'Fouls':[-Summary[10],Summary[11]],
    'Corners':[-Summary[12],Summary[13]],
}

ResultsBig = {
    'Possesion[%]':[-Summary[6],Summary[7]],
    'Shoot accuracy [%]':[-Summary[14],Summary[15]],
    'Pass accuracy [%]':[-Summary[16],Summary[17]]
}

BarCharts(ResultsSmall, Category)
BarCharts(ResultsBig, Category)