---
title: "Marvel Cinematic Data Visualization With Plot.ly"
layout: post
date: 2017-06-01
image: /assets/images/markdown.jpg
headerImage: false
projects: true
blog: false
hidden: true
tag:
- data visualization
- plotly
star: true
category: blog
author: jason fong
description: Marvel Cinematic Data Visualization With Plot.ly
---

![png](/assets/images/2017-06-01-marvel-cinematic-plotly/marvel-cinematic.png)

I’m a huge sucker for Marvel cinematic and in this article I will do a fun exercise with building a simple interactive 3D network graph based on the relationship between Marvel characters. I will be using one of my favourite plotting libraries in Python, Plot.ly. Plot.ly is very easy to use and the way graphs are constructed is very intuitive. The dataset can be found on my GitHub or at the following link: <https://www.kaggle.com/csanhueza/the-marvel-universe-social-network>

### Setting up and exploring the data


```python
import plotly.plotly as py
from plotly.graph_objs import *
import plotly.offline as offline
import pandas as pd
```

Key thing to remember with Plot.ly is if you want to build graphs locally on  you computer using Jupyter notebooks, you need to initiate offline notebook mode


```python
offline.init_notebook_mode()
```

The hero-network.csv dataset contains two columns, hero1 and hero2 and represents a connection between the two characters.


```python
heros = pd.read_csv('hero-network.csv')
```


```python
heros.describe()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>hero1</th>
      <th>hero2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>574467</td>
      <td>574467</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>6211</td>
      <td>6173</td>
    </tr>
    <tr>
      <th>top</th>
      <td>CAPTAIN AMERICA</td>
      <td>CAPTAIN AMERICA</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>8149</td>
      <td>8350</td>
    </tr>
  </tbody>
</table>
</div>



Some quick data cleaning to remove empty spaces


```python
for i in range(0,2):
    heros[heros.columns[i]] = heros[heros.columns[i]].map(lambda x: x.rstrip())
```

If you are playing around with the same code and want to explore additional characters, you can use the following line to explore what type of characters are included in the dataset


```python
#heros[heros['hero1'].str.contains('DAREDEVIL')]['hero1'].unique()
```

For this exercise, I'm interested in looking at the connection between some of the marvel characters and villains that we've see in theatres!


```python
avengers_name = ['HULK/DR. ROBERT BRUC', 'BLACK WIDOW/NATASHA', 'CAPTAIN AMERICA', 'IRON MAN/TONY STARK', 
                 'WAR MACHINE II/PARNE', 'HAWKEYE | MUTANT X-V', 'FALCON/SAM WILSON', 
                 'SCARLET WITCH/WANDA', 'VISION', 'ANT-MAN II/SCOTT HAR', 'SPIDER-MAN/PETER PAR', 
                 "BLACK PANTHER/T'CHAL", 'DR. STRANGE/STEPHEN', 'THOR IV/DARGO', 'FURY, COL. NICHOLAS', 
                 'QUICKSILVER/PIETRO M'
                ]

villain_name = [ 'BUCKY/BUCKY BARNES', 'MALEKITH/MALCOLM KEI', 'THANOS', 'ULTRON',
                 'LOKI [ASGARDIAN]', 'BARON MORDO/KARL MOR', 'DORMAMMU', 'RED SKULL/JOHANN SCH']

all = []
all.extend(avengers_name)
all.extend(villain_name)
```

### First lets just explore The Avenger's social circle

We do a quick count on how many relationships each avengers have and how many comic books they appeared in


```python
avenger_info = {'hero': [], 'buddies': []}
for i in avengers_name:
    avenger_info['hero'].append(i)
    avenger_info['buddies'].append(len(heros[heros['hero1']==i]))
```


```python
avenger_df = pd.DataFrame(avenger_info, columns = ['hero', 'buddies', 'comics'])
```

Building a grouped bar chart in Plotly is very simple. Each set of bar is treated as seperate data, we define the x and y values and the aesthetics for each group of bars. Then we define the layout like axes, chart title etc. Lastly we pass data and layout into Plot.ly's figure function to build the graph


```python
buddies = Bar(
    x = avenger_df['hero'],
    y = avenger_df['buddies'],
    name = 'buddies',
    marker=dict(
        color='rgba(234, 35, 40, 0.7)',
        line=dict(
            color='rgba(234, 35, 40, 1.0)',
            width=1
        )
    )
)

data = [buddies]
layout = Layout(
    barmode = 'group',
    bargroupgap=0.1,
    title =  'Avengers - Buddies'
)

fig = Figure(data = data, layout = layout)
```


```python
offline.iplot(fig)
```

<iframe width="900" height="500" frameborder="0" scrolling="no" src="//plot.ly/~datafong/8.embed"></iframe>


We can see our fellow Captain Steve Rogers is quite popular along with friendly neighborhood Spiderman and Mr. Tony Stark. One weird data point here is Thor who in my mind is should be quite popular... This might be because there are several Thor characters in the dataset, representing different Thors from different universes, as well as the comic book universe being different from cinematics. 

### Now lets build the network graph for our Avengers and villains

First off, there are a whole lot of characters in our dataset, lets just keep our avengers and villains


```python
def keep_avengers(x, characters):
    if ((x['hero1'] in characters) and (x['hero2'] in characters)):
        return 1
    else:
        return 0
```


```python
heros['keep'] = heros.apply(lambda x: keep_avengers(x, all), axis=1)
heros = heros[heros['keep']==1].drop('keep', axis=1).reset_index().drop('index', axis=1)
```

Next we use the igraph libary which is a library for high-performance graph generation and analysis. For more information on installation, visit <http://igraph.org/python/>


```python
import igraph as ig
```

One of the requirement to build this network graph is to express the source and destination nodes as integer values. So we start off with encoding our heros into numbers


```python
heros.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>hero1</th>
      <th>hero2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SPIDER-MAN/PETER PAR</td>
      <td>HULK/DR. ROBERT BRUC</td>
    </tr>
    <tr>
      <th>1</th>
      <td>QUICKSILVER/PIETRO M</td>
      <td>SCARLET WITCH/WANDA</td>
    </tr>
    <tr>
      <th>2</th>
      <td>QUICKSILVER/PIETRO M</td>
      <td>IRON MAN/TONY STARK</td>
    </tr>
  </tbody>
</table>
</div>




```python
Edges = []
mapper = {}
character_group = []

unique_heros = pd.concat([heros['hero1'], heros['hero2']]).unique()
num_unique_heros = len(unique_heros)

for i in range(num_unique_heros):
    mapper[unique_heros[i]] = i
```


```python
heros['hero1_node'] = heros['hero1'].map(lambda x: mapper[x])
heros['hero2_node'] = heros['hero2'].map(lambda x: mapper[x])
```


```python
heros.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>hero1</th>
      <th>hero2</th>
      <th>hero1_node</th>
      <th>hero2_node</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>SPIDER-MAN/PETER PAR</td>
      <td>HULK/DR. ROBERT BRUC</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>QUICKSILVER/PIETRO M</td>
      <td>SCARLET WITCH/WANDA</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>QUICKSILVER/PIETRO M</td>
      <td>IRON MAN/TONY STARK</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



Next I'd like to seperate our good guys from the bad guys visually, so let's group them up.


```python
for j in unique_heros:
    character_group.append(1) if (j in avengers_name) else character_group.append(0)
```

Now were ready to build our nodes and links. We start off with links by putting our (source, destination) for every node into Edges and pass Edges into our igraph. The layout function here defines the overall structure of our network graph and we use 'sphere'. There are other options like 

- gfr, grid_fr, grid_fruchterman_reingold: grid-based Fruchterman-Reingold layout

- kk, kamada_kawai: Kamada-Kawai layout

- kk_3d, kk3d, kamada_kawai_3d: 3D Kamada-Kawai layout

Play around with some of these to see the different structures.


```python
for i in range(len(heros)):
    Edges.append((heros['hero1_node'][i], heros['hero2_node'][i]))
```


```python
G = ig.Graph(Edges, directed=False)
layt=G.layout('sphere', dim=3)
```

This part looks a little intimidating and complicated but its not so bad. Layout function helps us define the layout of the network graph, what we are doing here is populating our X, Y, Z coordinates for each node and edge to be placed into our 3D space


```python
Xn=[layt[k][0] for k in range(num_unique_heros)]# x-coordinates of nodes
Yn=[layt[k][1] for k in range(num_unique_heros)]# y-coordinates
Zn=[layt[k][2] for k in range(num_unique_heros)]# z-coordinates
Xe=[]
Ye=[]
Ze=[]
for e in Edges:
    Xe+=[layt[e[0]][0],layt[e[1]][0], None]# x-coordinates of edge ends
    Ye+=[layt[e[0]][1],layt[e[1]][1], None]
    Ze+=[layt[e[0]][2],layt[e[1]][2], None]
```

Same as how we built the bar chart, we define our data & layout and pass into the Figure function. Instead of bar chart, we are using Scatter3d for our data


```python
trace1=Scatter3d(x=Xe,
               y=Ye,
               z=Ze,
               mode='lines',
               line=Line(color='rgb(125,125,125)', width=1),
               hoverinfo='none'         
            )

trace2=Scatter3d(x=Xn,
               y=Yn,
               z=Zn,
               mode='markers',
               name='actors',
               marker=Marker(symbol='dot',
                             size=13,
                             color=character_group,
                             colorscale=[
                                [0, 'black'],
                                [1, 'red']],
                             line=Line(color='rgb(50,50,50)', width=0.5),
                             opacity = 0.8
                             ),
               text=unique_heros,
               hoverinfo='text'  
            )
```


```python
axis=dict(showbackground=False,
          showline=False,
          zeroline=False,
          showgrid=False,
          showticklabels=False,
          title='',
          showspikes = False
          )
```


```python
layout = Layout(
         title="Marvel Cinematic - Social Network",
         width=1000,
         height=700,
         showlegend=False,
         scene=Scene(
         xaxis=XAxis(axis),
         yaxis=YAxis(axis),
         zaxis=ZAxis(axis)
        ),
        hovermode='closest'   
)
```


```python
data=Data([trace1, trace2])
fig=Figure(data=data, layout=layout)
```


```python
offline.iplot(fig)
```

<iframe width="900" height="800" frameborder="0" scrolling="no" src="//plot.ly/~datafong/6.embed"></iframe>