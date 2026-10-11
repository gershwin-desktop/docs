# Using Gershwin frameworks

How to link the frameworks that Gershwin ships from your own tool, app or
bundle. All examples below were built against the installed frameworks with
zero warnings.

## Where things live

Everything installs to the SYSTEM domain. Never install to LOCAL.

| What | Path |
| --- | --- |
| Frameworks | `/System/Library/Frameworks/<Name>.framework` |
| Headers | `/System/Library/Headers/<Name>/` (a symlink into the framework's `Headers`) |
| Shared libraries | `/System/Library/Libraries/lib<Name>.so` |
| Pref panes | `/System/Library/Bundles/<Name>.prefPane` |

Import headers as `#import <Name/Header.h>`. Link with `-l<Name>` (the
GNUstep Makefiles already add the framework library path). Start every
GNUmakefile with `include $(GNUSTEP_MAKEFILES)/common.make` and, in a
Gershwin component, set `GNUSTEP_INSTALLATION_DOMAIN = SYSTEM`.

Build and run an example:

```sh
. /System/Library/Makefiles/GNUstep.sh
gmake
```

## Frameworks at a glance

| Framework | For | Main classes | Source |
| --- | --- | --- | --- |
| PackageManager | Install/remove packages and AppImages on any supported OS | `GWPackageManager`, `GWOSDetector`, `GWHeaderDatabase` | `gershwin-components/PackageManager` |
| ItemFlow | OpenGL carousel of images (cover flow) | `ItemFlowView` | `gershwin-components/ItemFlow` |
| GSAssistantFramework | Step-by-step assistant windows | `GSAssistantWindow`, `GSAssistantStep`, `GSSelectionStep`, `GSIntroductionStep` | `gershwin-components/Assistants/AssistantFramework` |
| PreferencePanes | Pref pane bundles for System Preferences | `NSPreferencePane` | `gershwin-systempreferences/PreferencePanes` |
| FSNode | File system model and file views | `FSNode`, `FSNodeRep`, `FSNIcon`, `FSNIconsView`, `FSNListView` | `gershwin-workspace/FSNode` |
| Inspector | Content viewer plugin protocol for the Workspace inspector | `ContentViewersProtocol` | `gershwin-workspace/Inspector` |
| MDKit | Metadata queries | `MDKQuery`, `MDKQueryManager`, `MDKWindow` | `gershwin-workspace/GWMetadata/MDKit` |
| Operation | Copy, move, delete file operations | `Operation` | `gershwin-workspace/Operation` |
| MessagesKit | Chat backends and accounts | `MSGAccount`, `MSGBackend`, `MSGChannel`, `MSGMessage` | installed only |
| AVFoundation | Audio and video playback and capture | `AVPlayer`, `AVAudioPlayer`, `AVCaptureSession` | `libs-av` |
| Pantomime | Mail protocols (IMAP, POP3, SMTP) | `CWIMAPStore`, `CWFolder`, `CWMessage` | installed only |
| StepTalk | Scripting | `STEnvironment`, `STEngine`, `STScript` | `libs-steptalk` |

Backend libraries in `gershwin-components/Libraries` (`SoundBackend`,
`NetworkBackend`, `KeyboardBackend`, `MouseBackend`, `EnergyBackend`,
`CPUGovernorBackend`, `ColorProfileBackend`) are plain shared libraries,
not frameworks. Their headers are in `/System/Library/Headers/<Name>/` and
you link them with `-l<Name>`, for example `-lSoundBackend`.

## PackageManager

Use it to check for and install packages without caring which distribution
or BSD runs. `GWOSDetector` tells you the OS and package manager family;
`GWPackageManager` (use `sharedManager`) installs, removes and queries.
Install calls need authorization and return `NO` with an `NSError` in
`GWPackageManagerErrorDomain` on failure. Pass an object implementing
`GWInstallProgressHandler` to get progress.

`GNUmakefile`:

```make
include $(GNUSTEP_MAKEFILES)/common.make

TOOL_NAME = pkgcheck
pkgcheck_OBJC_FILES = main.m
pkgcheck_TOOL_LIBS = -lPackageManager
ADDITIONAL_OBJCFLAGS += -fobjc-arc -Wall -Wno-nullability-completeness

include $(GNUSTEP_MAKEFILES)/tool.make
```

`main.m`:

```objc
#import <Foundation/Foundation.h>
#import <PackageManager/GWOSDetector.h>
#import <PackageManager/GWPackageManager.h>

int main(int argc, const char *argv[])
{
  @autoreleasepool {
    NSLog(@"OS: %@, family: %@, arch: %@",
          [GWOSDetector currentOSIdentifier],
          [GWOSDetector packageManagerFamily],
          [GWOSDetector currentArchitecture]);

    NSArray *wanted = @[@"git", @"curl"];
    NSArray *missing =
      [[GWPackageManager sharedManager] missingPackagesFrom:wanted];
    NSLog(@"Missing packages: %@", missing);
  }
  return 0;
}
```

`-Wno-nullability-completeness` hides warnings from the framework's own
header, which mixes annotated and unannotated pointers.

## FSNode

`FSNode` is a file system item with lazily loaded attributes. Create one
with `+nodeWithPath:`, list children with `-subNodes`, and ask it
`-isDirectory`, `-isApplication`, `-isPackage`, `-displayName`, `-fileSize`.
`FSNodeRep` caches icons and settings; `FSNIconsView` and `FSNListView`
are ready-made views of a node's children.

`GNUmakefile`:

```make
include $(GNUSTEP_MAKEFILES)/common.make

TOOL_NAME = nodelist
nodelist_OBJC_FILES = main.m
nodelist_TOOL_LIBS = -lFSNode
ADDITIONAL_OBJCFLAGS += -fobjc-arc -Wall

include $(GNUSTEP_MAKEFILES)/tool.make
```

`main.m`:

```objc
#import <Foundation/Foundation.h>
#import <FSNode/FSNode.h>

int main(int argc, const char *argv[])
{
  @autoreleasepool {
    FSNode *dir = [FSNode nodeWithPath:@"/System/Library"];
    for (FSNode *node in [dir subNodes]) {
      NSLog(@"%@ %@", [node isDirectory] ? @"dir " : @"file",
            [node name]);
    }
  }
  return 0;
}
```

## ItemFlow

`ItemFlowView` is an `NSOpenGLView` showing images on a carousel. Give it a
`dataSource` (item count and an `NSImage` per index) and optionally a
`delegate` (told which item was selected). Call `-reloadData` after the data
changes, or `-updateTexturesForIndices:` when only some pictures change.
It needs OpenGL, so link `-lGL -lGLU` as well.

`GNUmakefile`:

```make
include $(GNUSTEP_MAKEFILES)/common.make

APP_NAME = FlowDemo
FlowDemo_OBJC_FILES = main.m
FlowDemo_GUI_LIBS = -lItemFlow -lGL -lGLU
ADDITIONAL_OBJCFLAGS += -fobjc-arc -Wall

include $(GNUSTEP_MAKEFILES)/application.make
```

`main.m`:

```objc
#import <AppKit/AppKit.h>
#import <ItemFlow/ItemFlow.h>

@interface FlowDemo : NSObject <ItemFlowViewDataSource, ItemFlowViewDelegate>
@property (nonatomic, strong) NSWindow *window;
@property (nonatomic, strong) ItemFlowView *flow;
@property (nonatomic, strong) NSArray *images;
@end

@implementation FlowDemo

- (void)applicationDidFinishLaunching:(NSNotification *)note
{
  self.images = @[[NSImage imageNamed:@"NSApplicationIcon"],
                  [NSImage imageNamed:@"NSFolder"],
                  [NSImage imageNamed:@"NSComputer"]];
  NSRect frame = NSMakeRect(100, 100, 600, 300);
  self.window = [[NSWindow alloc]
    initWithContentRect:frame
              styleMask:NSTitledWindowMask | NSClosableWindowMask
                backing:NSBackingStoreBuffered
                  defer:NO];
  self.flow = [[ItemFlowView alloc]
    initWithFrame:[[self.window contentView] bounds]];
  self.flow.dataSource = self;
  self.flow.delegate = self;
  [[self.window contentView] addSubview:self.flow];
  [self.flow reloadData];
  [self.window makeKeyAndOrderFront:nil];
}

- (NSUInteger)numberOfItemsInItemFlowView:(ItemFlowView *)view
{
  return [self.images count];
}

- (NSImage *)itemFlowView:(ItemFlowView *)view imageAtIndex:(NSUInteger)index
{
  return self.images[index];
}

- (void)itemFlowView:(ItemFlowView *)view didSelectItemAtIndex:(NSUInteger)index
{
  NSLog(@"Selected item %lu", (unsigned long)index);
}

@end

int main(int argc, const char *argv[])
{
  @autoreleasepool {
    NSApplication *app = [NSApplication sharedApplication];
    FlowDemo *demo = [[FlowDemo alloc] init];
    [app setDelegate:(id)demo];
    [app run];
  }
  return 0;
}
```

## PreferencePanes

Subclass `NSPreferencePane` to add a pane to System Preferences. Override
`-loadMainView` (or provide a nib via `-mainNibName`), call `-setMainView:`
and `-mainViewDidLoad`. Use `-willSelect`, `-didSelect`, `-shouldUnselect`
and `-didUnselect` for the pane life cycle. Lay out the view with the
metrics from the `gershwin-ui-metrics` guidance (640 x 440 pane area).
Panes install to `/System/Library/Bundles/<Name>.prefPane`; see
`Sound/GNUmakefile` for a complete install rule.

`GNUmakefile`:

```make
include $(GNUSTEP_MAKEFILES)/common.make

BUNDLE_NAME = Hello
BUNDLE_EXTENSION = .prefPane
Hello_OBJC_FILES = HelloPane.m
Hello_PRINCIPAL_CLASS = HelloPane
Hello_BUNDLE_LIBS = -lPreferencePanes
ADDITIONAL_OBJCFLAGS += -fobjc-arc -Wall

include $(GNUSTEP_MAKEFILES)/bundle.make
```

`HelloPane.m`:

```objc
#import <AppKit/AppKit.h>
#import <PreferencePanes/PreferencePanes.h>

@interface HelloPane : NSPreferencePane
@end

@implementation HelloPane

// Build the view in code instead of loading a nib.
- (NSView *)loadMainView
{
  NSView *view = [[NSView alloc] initWithFrame:NSMakeRect(0, 0, 640, 440)];
  NSTextField *label = [[NSTextField alloc]
    initWithFrame:NSMakeRect(24, 200, 592, 22)];
  [label setStringValue:@"Hello from a preference pane"];
  [label setEditable:NO];
  [label setBezeled:NO];
  [label setDrawsBackground:NO];
  [view addSubview:label];
  [self setMainView:view];
  [self mainViewDidLoad];
  return view;
}

@end
```

## Other frameworks

- GSAssistantFramework: build an assistant from steps (`GSAssistantStep` subclasses such as `GSSelectionStep` and
  `GSEnhancedProgressStep`) shown in a `GSAssistantWindow`. `Assistants/` has working examples such as
  `BackupAssistant` and `NetworkSetupAssistant`; copy their GNUmakefile.
- Inspector: implement `ContentViewersProtocol` (`-canDisplayPath:`,
  `-displayPath:`) in a bundle to add a viewer to the Workspace inspector.
- MDKit, Operation: used by Workspace; import
  `<MDKit/MDKit.h>` and `<Operation/Operation.h>` and link `-lMDKit` or
  `-lOperation`.
- MessagesKit, Pantomime, StepTalk, AVFoundation: link
  `-lMessagesKit`, `-lPantomime`, `-lStepTalk`, `-lAVFoundation` and import
  from `<Name/Name.h>` (Pantomime uses the `CW*.h` headers).

## TerminalKit

There is no TerminalKit framework in the Gershwin tree today, so none is
installed. The terminal view is part of the Terminal app
(`gershwin-terminal`: `TerminalView`, `TerminalWindow`). Until it is split
out as a framework, copy those classes to embed a terminal view.
