# spotify-downloader

Spotify song downloader using API created by spotifydown.com

The script `src/spotify_dl.py` can be run in interactive mode or CLI mode.

## Interactive mode

When run without any arguments, interactive mode is used.  The user is prompted for URLs of songs or playlists.  If a playlist is given, the user has the option to download individual songs from that playlist or all of them as well as the ability to see the songs in the playlist prior to making a decision.  The default download directory is the user's `Downloads/` directory, e.g. `C:\Users\[USER]\Downloads\`.  The user is prompted if they want to change the directory prior to downloading the songs.

## CLI mode

```shell
usage: spotify_dl.py [-h] [-u URLS [URLS ...]] [-f TEMPLATE] [-o OUTPUT] [-c] [-s] [-k CONFIG_FILE] [--retry-failed-downloads RETRY_FAILED_DOWNLOADS] [--debug]

optional arguments:
  -u URLS [URLS ...], --urls URLS [URLS ...]
                        URL(s) of Sptofy songs, albums, or playlists to download. If an album or playlist is given, append "|[TRACK NUMBERS]" to URL to specify which tracks to download. Example:
                        'https://open.spotify.com/playlist/mYpl4YLi5T|1,4,15-' to download the first, fourth, and fifteenth to the end. If not specified, all tracks are downloaded.
  -f TEMPLATE, --filename TEMPLATE
                        Specify custom filename. Use the following tags inside quotation marks: {artist}, {title}, {track_num}
			Example: --filename "{track_num} - {title}". If not specified, filename = "{title} - {artist}". Note that changing this will cause tracks downloaded using a different 
                        template to not be recognized.
  -o OUTPUT, --output OUTPUT
                        Path to directory where tracks should be downloaded to
  -c, --create-dir      Create the output directory if it does not exist.
  -s, --skip-duplicate-downloads
                        Don't download a song if the file already exists in the output directory.
  -k CONFIG_FILE, --config-file CONFIG_FILE
                        Path to JSON containing download instructions.
  --retry-failed-downloads RETRY_FAILED_DOWNLOADS
                        Number of times to retry failed downloads.
  --debug               Debug mode.
```

### Cfg file

Not to be confused with the "config" file below (bad name, I know. I'll change it at some point), The user can define a file named `.spotify_dl.cfg` in their home directory (in Windows, it's `C:\Users\[Your user]\`) to define settings that will always be used by the downloader.  

```
[Settings]
default_download_location="C:\Users\me\Desktop\folder"
```

Currently, only `default_download_location` is supported


### Config file

Example of JSON file used to contain download instructions:

```json
[
  {
    "url": "https://open.spotify.com/playlist/mYpl4YLi5T"
  },
  {
    "url": "https://open.spotify.com/playlist/mYpl4YLi5T2",
    "output_dir": "/path/to/dir/",
    "create_dir": true,
    "filename_template": "{id} - {artist} - {title}"
  },
  {
    "url": "https://open.spotify.com/playlist/mYpl4YLi5T3",
    "skip_duplicate_downloads": true
  }
]

_The argument `--retry-failed-downloads` does not need to be put into the config JSON.  It should be run when using the `--config-file` arg, e.g., `spotify_dl --config-file path/to/config.json --retry-failed-downloads 3`_
```
