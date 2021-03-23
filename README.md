# Update Archillect Wallpapers

This was part of a larger project that wasn't (hasn't been?) finished, but
I'll put it up here as is for now.

## What it does

Running this script will:

1. create `~/images/wallpapers` and `~/tmp` directories if they don't
exist
2. download the latest images from
[@archillect on Twitter](https://twitter.com/archillect) to
`~/images/archillect`
3. copy the latest images to `~/images/wallpapers`
4. create a compressed tarball of all archillect images in
`~/tmp/archillect.tar.gz`

## Why do this

This was part of the data collection process for a supervised learning
project.

In the meantime it's a useful way to have interesting wallpapers. On a Mac,
after you've run `refresh.sh`:

1. `System Preferences...` -> `Desktop & Screensaver` -> `Desktop`
2. Create a new entry under `Folders` and set it to `~/images/wallpapers`
3. Due to the variety of images aspect ratios, for best results choose
`Stretch to Fill Screen`.
4. Set `Random order` and `Change picture` frequency as desired.

## Okay, I want to set it up

Depending on your OS, you could use `crontab(1)` or `launchctl(1)` or simply
run the script periodically. In essence:

### First time

Ensure you have the latest version of `pip`:

`python3 -m pip install --user --upgrade pip`

Clone this repository and create a Python virtual environment:

```sh
git clone https://github.com/benkant/roboraster.git
cd roboraster
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```

This will install the [twitter-photos](https://twitter-photos.shichao.io)
package. In order to access the Twitter API, you'll need to create a Twitter
App in the [Twitter Developer Portal](https://developer.twitter.com/en/apps).

After you've created an app, create a file `~/.twphotos` with the following
details from the `Keys and tokens` tab of your Twitter app's details page:

```INI
[credentials]
consumer_key = your_consumer_key
consumer_secret = your_consumer_secret
access_token_key = your_access_token_key
access_token_secret = your_access_token_secret
```

### Collect new wAreZ

Again, from `crontab(1)`/similar or manually:

```sh
cd roboraster
./refresh.sh
```

The script will take care of activating the Python environment. It's
very simple, has no options, and is designed for a single purpose. Hack at
will.

The `twphotos` script provided by the `twitter-photos` package has a
multithreaded option (`twphotos -r`), however at time of printing there
exists a race condition such that we get a non-zero exit and uncaught
exceptions. As this script's initial purpose didn't require speed, no
workarounds or extensive debugging has been done. The author points to the
existence of the flag should it be of interest.

```C
/*
 * Copyright (c) 2021 Benjamin Giles <btgiles@gmail.com>
 *
 * Permission to use, copy, modify, and distribute this software for any
 * purpose with or without fee is hereby granted, provided that the above
 * copyright notice and this permission notice appear in all copies.
 *
 * THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
 * WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
 * MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
 * ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
 * WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
 * ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
 * OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
 */
```
