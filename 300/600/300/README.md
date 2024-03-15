# 300 - Rendering

See also [Creating automated music video with nexrender](https://www.youtube.com/watch?v=E64dXZ_AReQ) by [Douglas Prod](https://www.youtube.com/channel/UCDFTT_oX6VwmANKMng0-NUA).

See the documentation of NexRender at https://github.com/inlife/nexrender/ .

**NOTE**: We can store our Adobe After Effects project file (*.aep) in GitHub (here: https://github.com/vanHeemstraCreations/movie-digital-twin/movie-digital-twin.aep) and use its URL with NexRender to do the rendering. Automated version control!!

## Job

A job is a single working unit in the nexrender ecosystem. It is a json document, that describes what should be done, and how it should be done. Minimal job description always should contain a pointer onto Adobe After Effects project, which is needed to be rendered, and a composition that will be used to render.

The pointer is src (string) field containing a URI pointing towards specified file, followed by composition (string) field, containing the name of the composition that needs to be rendered.

Note: check out supported protocols for src field.

```
// myjob.json
{
    "template": {
        "src": "file:///users/myuser/documents/myproject.aep",
        "composition": "main"
    }
}
```

or for remote file accessible via http

```
// myjob.json
{
    "template": {
        "src": "http://example.com/myproject.aep",
        "composition": "main"
    }
}
```

Or in our case:

```
// myjob.json
{
    "template": {
        "src": "https://github.com/vanHeemstraCreations/movie-digital-twin/movie-digital-twin.aep",
        "composition": "digital-twin"
    }
}
```

Hence, ```digital-twin``` is the name of our main composition, it includes the whole movie. It is prefered to use a movie name, rather then ```main``` as Adobe After Effect will allow you to use the Comp(osition) Name when it starts a render, so the result of our render would thus automatically be named ```digital-twin.mp4``` instead of the ambiguous ```main.mp4```.

Submitting this data to the binary will result in start of the rendering process:

```
$ nexrender-cli '{"template":{"src":"file:///home/documents/myproject.aep","composition":"main"}}'
```

Or in our case:

```
$ nexrender-cli '{"template":{"src":"https://github.com/vanHeemstraCreations/movie-digital-twin/movie-digital-twin.aep","composition":"digital-twin"}}'
```

*Note*: on MacOS you might need to change the permissions for downloaded file, so it would be considered as an executable.
You can do it by running: ```$ chmod 755 nexrender-cli-macos```

or more conveniently using the ```--file``` option

```
$ nexrender-cli --file myjob.json
```

*Note*: its recommended to run ```nexrender-cli -h``` at least once, to read all useful information about available options.

More info: [@nexrender/cli](https://github.com/inlife/nexrender/blob/master/packages/nexrender-cli)

**After Effects 2023 (and newer)**

Please note that for After Effects 2023, it's vital to set up an Output Module, even if you want to rely on the default output module. After Effects 2023 rendering binary (aerender) in a lot of cases will not render a composition unless it has a configured output module. Additionally, AE2023 now allows rendering directly to mp4, so consider setting up a custom value for ```outputExt``` as well. To do that, take a look at following example:

```
// myjob.json
{
    "template": {
        "src": "file:///users/myuser/documents/myproject_ae2023.aep",
        "composition": "main",
        "outputModule": "H.264 - Match Render Settings - 15 Mbps",
        "outputExt": "mp4",
        "settingsTemplate": "Best Settings"
    }
}
```

Or in our case:

```
{
    "template": {
        "src": "https://github.com/vanHeemstraCreations/movie-digital-twin/movie-digital-twin.aep",
        "composition": "digital-twin",
        "outputModule": "H.264 - Match Render Settings - 15 Mbps",
        "outputExt": "mp4",
        "settingsTemplate": "Best Settings"
    }
}
```

More, see https://github.com/inlife/nexrender/
