# Application Bundles

Gershwin uses __application bundles__ to manage native applications. Whenever possible, application bundles are used. XDG-style `.desktop` files are considered legacy and should be avoided.

## Benefits

Applications shipped as application bundles can

* Easily be moved around in the filesystem (relocation)
* Easily be copied to e.g., another machine or a network share
* Easily be copied on the local machine to get a second instance of the same application (e.g., to make changes to it while keeping the original around)
* Easily be deleted by moving to the Trash
* Easily be managed without the need for a package manager
* Easily be modified (e.g., changing the icon) without any side effects to other parts of the system
* Easily be distributed (e.g., in archives like zip files or in disk images) without the need for packaging
* Easily be understood by switchers coming from other operating systems with similar application distribution formats

### Wrappers for legacy packages

Gershwin supports applications that are not shipped in bundle formats yet. These can be bridged by __wrapper bundles__, application bundles that merely launch (but do not contain) the payload application (which may be installed in traditional ways).

The __appwrap__ tool ships with Gershwin that automates the creation of such wrappers.

Please note that the use of wrapper bundles is discouraged and is only available as a bridge technology for backward compatibility with existing application packages.

### Making an application load privately bundled libraries

If an application is supposed to load privately bundled libraries, one must patch it so that it loads privately bundled libraries from a path relative to itself (`$ORIGIN`) rather than from `/usr/local/lib`:

```console
$ sed -i -e 's|/usr/local/lib|$ORIGIN/../lib|g' usr/local/bin/falkon
$ ln -s usr/local/lib .
$ rm usr/local/bin/falkon-e
```

This works because by coincidence the string `/usr/local/lib` has the exact same length as `$ORIGIN/../lib`. If this was not the case, one would need to either specify the rpath at compilation time, or use a tool such as `patchelf`.

### Avoiding absolute paths

For an application to be fully relocatable in the filesystem, one must take care that no absolute paths to data files (e.g., those in `/usr/share/<APPNAME>` get compiled in.

In Qt applications, void [`QStringList QStandardPaths::standardLocations(QStandardPaths::AppDataLocation);`](http://doc.qt.io/qt-5/qstandardpaths.html). According to the [Qt documentation](http://doc.qt.io/qt-5/qstandardpaths.html), this resolves to `"~/.local/share/<APPNAME>", "/usr/local/share/<APPNAME>", "/usr/share/<APPNAME>"` but clearly `/usr` is not where these things are located in an `.app` bundle.

Instead, use [`QString QCoreApplication::applicationDirPath()`](http://doc.qt.io/qt-5/qcoreapplication.html#applicationDirPath) and construct a _relative_ path to `../share/<APPNAME>` from there.

For an example, see:
https://github.com/KaidanIM/Kaidan/commit/da38011b55a1aa5d17764647ecd699deb4be437f

## Credits

Application directories and application bundles have been used by many desktop-oriented operating systems, including RISC OS, NeXTStep/OPENSTEP, Mac OS X, and various other systems. Classic Macintosh System used single-file applications that kept resources in the Resource Fork.

* https://en.wikipedia.org/wiki/Application_directory
* http://rox.sourceforge.net/desktop/AppDirs.html
* https://en.wikipedia.org/wiki/Resource_fork
