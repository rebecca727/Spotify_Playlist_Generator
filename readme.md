# README
## _Spotify Discovery Playlist Generator_

## Author: Rebecca Bui


**Downloads**

import the following by running the first cell: 
```sh
import spotipy
import spotipy.util as util
from spotipy.oauth2 import SpotifyOAuth
from spotipy.oauth2 import SpotifyClientCredentials
import spotipy.oauth2 as oauth2
import io
import pydotplus
```


**Spotify API Connection**

**_Note_**: Personal private playlist were used in the making of this project. In order to run code please replace Spotify user and playlist credentials.
- Sign up for a free Spotify for Developers account and create a new App. 
- Click on the newly created App and go to the “_Edit Settings_” tab. 
- Under “_Redirect URL_” add “https://google.com” and remember to click “_Save_” at the very bottom.
- Now in the code, install all the necessary libraries by running the first cell block
- Change the parameters _Client ID_ and _Client Secret_ within the `Set environment credentials` cell These two values can be found on your Spotify Dashboard App.
```sh
os.environ['SPOTIPY_CLIENT_ID'] = 'YOUR_CLIENT_ID'
os.environ['SPOTIPY_CLIENT_SECRET'] = 'YOUR_CLIENT_SECRET'
os.environ['SPOTIPY_REDIRECT_URI'] = 'https://google.com'
```
- If values are correct, when this portion of code is implemented, you should be redirected to a small Spotify tab confirming your account. When you click on the button, it will redirect you to Google
- Copy and paste the _URL_ of the website that you were just redirected to and insert this _URL_ into JupyterLab. 
**_Note_**: JupyterLab will appear with a prompt asking you to enter this website

**_Note_**: If you’re using your own playlist, create two playlists (Playlist 1: 100 songs you like Playlist 2: 100 songs you do not like) 
**UPDATE:** The playlists were later increased to 200 songs per list for  a larger set to learn and train from
On the playlist, go to the three buttons and click share. Click on the “Copy link to playlist” button
- Paste this URL in your browser and save the ID of this playlist 
https://open.spotify.com/playlist/ `4pPR04l8gVUmuX94m9GTQm`
    **_The highlighted portion would be the playlist’s ID_**
- Change the `user_playlist` Spotipy function’s parameters to the user ID that you’ve accessed the playlist from and change the playlist ID as well. 
```sh
sp.user_playlist('user id', playlist_id='playlist id') 
```

**_Note_**: Do this for both the bad and good playlists

**_Note_**: With all these changes, everything should run fine. Most common problem encountered is often trying to connect to Spotify and getting an access token.

**Graphs and Classifier Models**

The packages needed for each classifier are included withiin the first cell, please make sure to run this first and all following cells in sequential order to ensure the program runs.
```sh
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier 
from sklearn.metrics import accuracy_score
from sklearn.model_selection import cross_val_score
from sklearn.metrics import plot_confusion_matrix
from sklearn.ensemble import RandomForestClassifier
from sklearn import tree
```
**Testing the Model**

Spotify's _Top 50 - USA_ playlist was used on the trained model. Any playlist can be used as long as the desired playlist ID is substituted in. Insert the playlist of choice by replacing the `user id` and `playlist id` fields found below:
```sh
sampleIDs = getTrackIDs("user id","playlist id")
# Array of song features
sampleFeatures = []
for i in range(len(sampleIDs)):
    sample_data = getTrackFeatures(sampleIDs[i])
    sampleFeatures.append(sample_data)
```


**Create New Playlist with Predicted Songs**

Run the following cell once to create the playlist. Replace the `user id` and `Your Playlist Name` fields.
```sh
#Create playlist to place predicted songs
sp.user_playlist_create('user id', 'Your Playlist Name', public=False)
```
Your `playlist id` will be included in the output after running this cell. Take note of this in order to use for the next cell block. The output will look similar to the following:

```sh
{'collaborative': False,
 'description': '',
 'external_urls': {'spotify': 'https://open.spotify.com/playlist/5BmKs0mvQgyy5D8roJvrzl'},
 'followers': {'href': None, 'total': 0},
 'href': 'https://api.spotify.com/v1/playlists/5BmKs0mvQgyy5D8roJvrzl',
 'id': '5BmKs0mvQgyy5D8roJvrzl',
 'images': [],
 'name': 'Model Suggested Songs 2',
```

**Add a Playlist Description**

Additional option to add a description for your newly created playlist. Replace the `playlist id` field and insert your description under `desc`. Run this cell once to update description.
```sh
#Add playlist desciption for user
desc = "Create a description for your playlist"
sp.playlist_change_details('playlist id',name=None, public=None, collaborative=None, description=desc)
```
**Add Predictied Songs to Playlist**

Run the following code once to add the model predicted songs to your newly created playlist. Be sure to change the `playlist id` field.
```sh
#Add songs to playlist for user to listen
sp.playlist_add_items('playlist id', predictions, position=None)
```

## License

MIT


