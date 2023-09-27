FFmpeg README
=============

FFmpeg is a collection of libraries and tools to process multimedia content
such as audio, video, subtitles and related metadata.

## Libraries

* `libavcodec` provides implementation of a wider range of codecs.
* `libavformat` implements streaming protocols, container formats and basic I/O access.
* `libavutil` includes hashers, decompressors and miscellaneous utility functions.
* `libavfilter` provides means to alter decoded audio and video through a directed graph of connected filters.
* `libavdevice` provides an abstraction to access capture and playback devices.
* `libswresample` implements audio mixing and resampling routines.
* `libswscale` implements color conversion and scaling routines.

## Tools

* [ffmpeg](https://ffmpeg.org/ffmpeg.html) is a command line toolbox to
  manipulate, convert and stream multimedia content.
* [ffplay](https://ffmpeg.org/ffplay.html) is a minimalistic multimedia player.
* [ffprobe](https://ffmpeg.org/ffprobe.html) is a simple analysis tool to inspect
  multimedia content.
* Additional small tools such as `aviocat`, `ismindex` and `qt-faststart`.

## Documentation

The offline documentation is available in the **doc/** directory.

The online documentation is available in the main [website](https://ffmpeg.org)
and in the [wiki](https://trac.ffmpeg.org).

### Examples

Coding examples are available in the **doc/examples** directory.

## License

FFmpeg codebase is mainly LGPL-licensed with optional components licensed under
GPL. Please refer to the LICENSE file for detailed information.

## Contributing

Patches should be submitted to the ffmpeg-devel mailing list using
`git format-patch` or `git send-email`. Github pull requests should be
avoided because they are not part of our review process and will be ignored.

## How Genetec Built FFmpeg

Within a Visual Studio command line, run:

32 bits: "%VCINSTALLDIR%\Auxiliary\Build\vcvars32.bat"
64 bits: "%VCINSTALLDIR%\Auxiliary\Build\vcvars64.bat"
Launch msys2 with msys2_shell.cmd -use-full-path to keep the environment variables. Confirm the right compiler has been loaded by running cl.exe. It will say 'for x86/x64' depending on which is loaded.

Place yourself at the root of your FFmpeg repository.

Set the PKG_CONFIG_PATH to the location of your additionnal libraries:

32 bits: export PKG_CONFIG_PATH=/c/tools/build32/lib/pkgconfig
64 bits: export PKG_CONFIG_PATH=/c/tools/build64/lib/pkgconfig
Create a sourcelink.json file at the root of the repository so we get source code when debugging:

 {
   "documents": {
     "/c/git/Genetec.Video.Ffmpeg/*": "https://raw.githubusercontent.com/FFmpeg/FFmpeg/<commitId>/*"
   }
 }
Replace /c/git/Genetec.Video.Ffmpeg with the absolute directory of your FFmpeg repository. And replace the <commitId> with the full commit id of the FFmpeg version you are using. You can retrieve it with git rev-parse n4.3. For example, the version 4.3 has commit id 8e12af29d1a3f95c9e952d78354e3c8b1c0431a8
In the msys console, configure the FFmpeg build with the needed flags:

32 bits: ./configure --toolchain=msvc --target-os=win32 --arch=x86 --enable-shared --disable-static --prefix=/c/tools/build32 --enable-libdav1d --extra-ldflags="/SOURCELINK:sourcelink.json" --disable-mediafoundation --extra-cflags="-MD"
64 bits: ./configure --toolchain=msvc --target-os=win64 --arch=x86_64 --enable-shared --disable-static --prefix=/c/tools/build64 --enable-libdav1d --extra-ldflags="/SOURCELINK:sourcelink.json" --disable-mediafoundation --extra-cflags="-MD"
make -j install

Retrieve the .pdb files generated in the repository. They are not copied to the installation folder automatically:

32 bits: find . -name \*.pdb -exec cp {} /c/tools/build32/bin \;
64 bits: find . -name \*.pdb -exec cp {} /c/tools/build64/bin \;
You are now ready to copy the build output to the Genetec.AvCodec.Native working directory and build the other platform.

Before building the other version you may want to do a make clean.