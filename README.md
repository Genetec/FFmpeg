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


## How Genetec built FFmpeg
	## Building ffmpeg

	You will have to follow the whole procedure twice, once for 64 bits and once for 32 bits.

	1. Within a Visual Studio command line, run:  
	32 bits: `"%VCINSTALLDIR%\Auxiliary\Build\vcvars32.bat"`  
	64 bits: `"%VCINSTALLDIR%\Auxiliary\Build\vcvars64.bat"`
	2. Launch msys2 and confirm the right compiler has been loaded by running `cl.exe`. It will say `for x86/x64` depending on which is loaded. Then place yourself at the root of your ffmpeg repository.  
	  Use `msys2_shell.cmd -use-full-path` to start it using the same environment as the current command line.
	3. Prepare the PKG_CONFIG location at the location of your additionnal libraries:  
	32 bits: `export PKG_CONFIG_PATH=/c/tools/pkg-config/build/lib/pkgconfig`  
	64 bits: `export PKG_CONFIG_PATH=/c/tools/pkg-config/build64/lib/pkgconfig`
	4. Create a `sourcelink.json` file at the root of the repository so we get source code when debugging:  
	`{"documents":{"/c/git/ffmpeg/*":"https://raw.githubusercontent.com/FFmpeg/FFmpeg/<commitId>/*"}}`  
	Replace `/c/git/ffmpeg` with the absolute directory of your ffmpeg repository. And replace the <commitId> with the full commit id of the ffmpeg version you are using. You can retrieve it with `git rev-parse n4.3`. For example, the version 4.3 has commit id `8e12af29d1a3f95c9e952d78354e3c8b1c0431a8`
	5. In the msys console, configure the ffmpeg build with the needed flags:  
	32 bits: `./configure --toolchain=msvc --target-os=win32 --arch=x86 --enable-shared --disable-static --prefix=./build32 --enable-libdav1d --extra-ldflags="/SOURCELINK:sourcelink.json" --disable-mediafoundation`  
	64 bits: `./configure --toolchain=msvc --target-os=win64 --arch=x86_64 --enable-shared --disable-static --prefix=./build64 --enable-libdav1d --extra-ldflags="/SOURCELINK:sourcelink.json" --disable-mediafoundation`
	6. `make install -j8` (Feel free to replace the 8 with your number of cores)
	7. You can retrieve the .pdb files in the repository output. They are not copied to the build*/ folder automatically
	8. You are now ready to copy the build output to the Genetec.AvCodec.Native working directory and build the other platform.
	9. Before building the other version you may want to do a `make clean`