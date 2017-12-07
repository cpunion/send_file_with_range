This is a fork of [Adam Cooke](https://github.com/adamcooke)'s `send_file_with_range` gem with added support for Rails 5.1 courtesy of [Metalels](https://github.com/metalels) and support for a buffer size option courtesy of me.

------

# `send_file` with Range

Rails includes a method called `send_file` which allows you to send a file from
your Rails app to a browser. Some files, however, require that data can be
requested in partial chunks with the client providing a range of bytes to be
returned from the file. This is particularily previlent when streaming video
over HTTP.

For example, if you have a video which you want to embed, you need to be able
to return the video file in sections to the browser to allow users to skip
through the video without downloading the whole file.

This little gem just extends Rails' `send_file` method to support this.

## Installation

```ruby
gem 'send_file_with_range'
```

## Usage

To use this, you just need to pass the `range: true` option to a normal
`send_file` call.

```ruby
class VideoController < ApplicationController

  def play
    path_to_video = Rails.root.join('data', 'some-video.mp4')
    send_file path_to_video, range: true, disposition: 'inline', type: 'video/mp4'
  end

end
```

An optional `buffer_size` option can be passed which will prevent the server from crashing on large files by only reading the requested range bytes.

```ruby
send_file_with_range @medium.path, type: 'video/mp4',
                                   disposition: 'inline',
                                   range: true,
                                   buffer_size: 512_000 # Send around 512kib
```
