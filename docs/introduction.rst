Getting Started
===============

.. |br| raw:: html

   <br />

Python bindings for the Plex API.

* Navigate local or remote shared libraries.
* Mark shows watched or unwatched.
* Request rescan, analyze, empty trash.
* Play media on connected clients.
* Get URL to stream stream h264/aac video (playable in VLC,MPV,etc).
* Plex Sync Support.
* Plex Audio Support.

Installation
------------

.. code-block:: python

    pip install plexapi


Getting a PlexServer Instance
-----------------------------

There are two types of authentication. If you are running on a separate network
or using Plex Users you can log into MyPlex to get a PlexServer instance. An
example of this is below. NOTE: Servername below is the name of the server (not
the hostname and port).  If logged into Plex Web you can see the server name in
the top left above your available libraries.

.. code-block:: python

    from plexapi.myplex import MyPlexAccount
    account = MyPlexAccount.signin('<USERNAME>', '<PASSWORD>')
    plex = account.resource('<SERVERNAME>').connect()  # returns a PlexServer instance

If you want to avoid logging into MyPlex and you already know your auth token
string, you can use the PlexServer object directly as above, but passing in
the baseurl and auth token directly.

.. code-block:: python

    from plexapi.server import PlexServer
    baseurl = 'http://plexserver:32400'
    token = '2ffLuB84dqLswk9skLos'
    plex = PlexServer(baseurl, token)


Usage Examples
--------------

.. code-block:: python

    # Example 1: List all unwatched movies.
    movies = plex.library.section('Movies')
    for video in movies.search(unwatched=True):
        print(video.title)


.. code-block:: python

    # Example 2: Mark all Conan episodes watched.
    plex.library.get('Conan (2010)').markWatched()


.. code-block:: python

    # Example 3: List all clients connected to the Server.
    for client in plex.clients():
        print(client.title)


.. code-block:: python

    # Example 4: Play the movie Avatar on another client.
    # Note: Client must be on same network as server.
    avatar = plex.library.section('Movies').get('Avatar')
    client = plex.client("Michael's iPhone")
    client.playMedia(avatar)


.. code-block:: python

    # Example 5: List all content with the word 'Game' in the title.
    for video in plex.search('Game'):
        print('%s (%s)' % (video.title, video.TYPE))


.. code-block:: python

    # Example 6: List all movies directed by the same person as Jurassic Park.
    movies = plex.library.section('Movies')
    jurassic_park = movies.get('Jurassic Park')
    director = jurassic_park.directors[0]
    for movie in movies.search(None, director=director):
        print(movie.title)


.. code-block:: python

    # Example 7: List files for the latest episode of Friends.
    thelastone = plex.library.get('Friends').episodes()[-1]
    for part in thelastone.iterParts():
        print(part.file)


.. code-block:: python

    # Example 8: Get a URL to stream a movie or show in another client
    jurassic_park = plex.library.section('Movies').get('Jurassic Park')
    print 'Run running the following command to play in VLC:'
    print 'vlc "%s"' % jurassic_park.getStreamUrl(videoResolution='800x600')


.. code-block:: python

    # Example 9: Get audio/video/all playlists
    for playlist in self.plex.playlists():
        print(playlist.title)


FAQs
----

**Q. Why are you using camelCase and not following PEP8 guidelines?** |br|
A. This API reads XML documents provided by MyPlex and the Plex Server.
We decided to conform to their style so that the API variable names directly
match with the provided XML documents.


**Q. Why don't you offer feature XYZ?** |br|
A. This library is meant to be a wrapper around the XML pages the Plex
server provides. If we are not providing an API that is offerered in the
XML pages, please let us know! -- Adding additional features beyond that
should be done outside the scope of this library.


**Q. What are some helpful links if trying to understand the raw Plex API?** |br|
https://github.com/plexinc/plex-media-player/wiki/Remote-control-API |br|
https://forums.plex.tv/discussion/104353/pms-web-api-documentation |br|
https://github.com/Arcanemagus/plex-api/wiki |br|
