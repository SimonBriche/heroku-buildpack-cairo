# heroku-buildpack-cairo

I am a Heroku buildpack that installs [Cairo](http://cairographics.org/) and
its dependencies ([Pango](http://www.pango.org/), [Pixman](http://pixman.org/),
[FreeType](http://www.freetype.org/),
[HarfBuzz](http://www.freedesktop.org/wiki/Software/HarfBuzz/), and
[giflib](http://giflib.sourceforge.net/)) into a dyno slug.

When used with
[heroku-buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi),
I enable subsequent buildpacks / steps to link to this library.

## Using

### Composed

You'll almost certainly want to use this in conjunction with one or more
additional buildpacks.

When creating a new Heroku app:

```bash
heroku apps:create -b https://github.com/ddollar/heroku-buildpack-multi.git

cat << EOF > .buildpacks
https://github.com/mojodna/heroku-buildpack-cairo.git
https://github.com/heroku/heroku-buildpack-nodejs.git
EOF

git push heroku master
```

When modifying an existing Heroku app:

```bash
heroku buildpack:set https://github.com/ddollar/heroku-buildpack-multi.git

cat << EOF > .buildpacks
https://github.com/mojodna/heroku-buildpack-cairo.git
https://github.com/heroku/heroku-buildpack-nodejs.git
EOF

git push heroku master
```

## Caveats

Pango, HarfBuzz, and fontconfig are only available for the `cedar-14` runtime.
(Pango is required for current versions of node-canvas.)

## Building

This uses Docker to build against Heroku
[stack-image](https://github.com/heroku/stack-images)-like images.

```bash
make
```

Artifacts will be dropped in `dist/`.  See `Dockerfile`s for build options.

## Cairo Configuration

```
cairo (version 1.14.2 [release]) will be compiled with:

The following surface backends:
  Image:         yes (always builtin)
  Recording:     yes (always builtin)
  Observer:      yes (always builtin)
  Mime:          yes (always builtin)
  Tee:           no (disabled, use --enable-tee to enable)
  XML:           no (disabled, use --enable-xml to enable)
  Skia:          no (disabled, use --enable-skia to enable)
  Xlib:          yes
  Xlib Xrender:  no (requires  http://freedesktop.org/Software/xlibs)
  Qt:            no (disabled, use --enable-qt to enable)
  Quartz:        no (requires CoreGraphics framework)
  Quartz-image:  no (disabled, use --enable-quartz-image to enable)
  XCB:           no (requires xcb >= 1.6 xcb-render >= 1.6 http://xcb.freedesktop.org)
  Win32:         no (requires a Win32 platform)
  OS2:           no (disabled, use --enable-os2 to enable)
  CairoScript:   yes
  PostScript:    yes
  PDF:           yes
  SVG:           yes
  OpenGL:        no (disabled, use --enable-gl to enable)
  OpenGL ES 2.0: no (disabled, use --enable-glesv2 to enable)
  BeOS:          no (disabled, use --enable-beos to enable)
  DirectFB:      no (disabled, use --enable-directfb to enable)
  OpenVG:        no (disabled, use --enable-vg to enable)
  DRM:           no (disabled, use --enable-drm to enable)
  Cogl:          no (disabled, use --enable-cogl to enable)

The following font backends:
  User:          yes (always builtin)
  FreeType:      yes
  Fontconfig:    yes
  Win32:         no (requires a Win32 platform)
  Quartz:        no (requires CoreGraphics framework)

The following functions:
  PNG functions:   yes
  GLX functions:   no (not required by any backend)
  WGL functions:   no (not required by any backend)
  EGL functions:   no (not required by any backend)
  X11-xcb functions: no (disabled, use --enable-xlib-xcb to enable)
  XCB-shm functions: no (requires --enable-xcb)

The following features and utilities:
  cairo-trace:                yes
  cairo-script-interpreter:   yes

And the following internal features:
  pthread:       yes
  gtk-doc:       no
  gcov support:  no
  symbol-lookup: no (requires bfd)
  test surfaces: no (disabled, use --enable-test-surfaces to enable)
  ps testing:    no (requires libspectre)
  pdf testing:   no (requires poppler-glib >= 0.17.4)
  svg testing:   no (requires librsvg-2.0 >= 2.35.0)
```

## Harfbuzz Configuration

```
Build configuration:

Unicode callbacks (you want at least one):
	Glib:			true
	ICU:			false
	UCDN:			false

Font callbacks (the more the better):
	FreeType:		true

Tools used for command-line utilities:
	Cairo:			true

Additional shapers (the more the better):
	Graphite2:		false

Platform shapers (not normally needed):
	CoreText:		false
	Uniscribe:		false

Other features:
	Documentation:		no
	GObject bindings:	false
	Introspection:		false
```
