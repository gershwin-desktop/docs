# Reviewer Guide

## Introduction

The Gershwin Desktop is a desktop experience that can run on Unix-like operating systems such as GhostBSD, FreeBSD, Artix, Arch, Devuan, Debian, etc.. Its goal is to provide a friendly, highly usable, and visually appealing system for desktop and notebook computers. The system is designed to be easy to use, efficient, with an emphasis on minimalism and simplicity. 

Its target audience are switchers from other desktop operating systems, to which it aims to provide an environment in which they can feel instantly at home.

The purpose of this guide is to provide an overview of Gershwin, as well as to offer guidance on how to use the system's features and capabilities. The guide includes information on system requirements, installation, user interface, features, performance, support options, and more. When reviewing Gershwin, try to look at it from a perspective of long-time Mac users, particularly those who are doing creative work like graphics, design, video editing, or 3D modeling and printing but who would like to do so using open source applications. This guide should provide you with a thorough understanding of the system and its functionality and should give you a starting point for your own exploration.

## System Requirements

System requirements depend on the operating system you are running the Gershwin Desktop on.

When reviewing or benchmarking Gershwin, it is important to note that virtual machines may not provide an accurate representation of the system's stability, feature set, and performance. Therefore, **we recommend that reviewers use real hardware** for these purposes. Running Gershwin on actual hardware will provide a more authentic experience and allow reviewers to test the system's capabilities in a more realistic environment. Additionally, using real hardware will help to ensure that the system's hardware support is properly evaluated. For these reasons, we strongly advise against conducting tests and benchmarks in a virtual environment, even though VirtualBox guest drivers are preinstalled.

## User Interface

The six-part series on [Linux desktop usability](https://medium.com/@probonopd/make-it-simple-linux-desktop-usability-part-1-5fa0fb369b42) by Gershwin's founder eventually lead to the design and development of Gershwin. The series explored aspects of  desktop usability, from user interface design to application packaging and distribution. The insights expressed in the series helped inform the design decisions behind Gershwin, with a focus on simplicity, ease of use, and discoverability. The intended result is a user-friendly and efficient operating system that is easy to use and customize, even for non-technical users.

The Gershwin user interface is designed to be intuitive, functional, and discoverable, with a range of features and tools to meet the needs of both new and experienced users. Here are some key concepts:

- **Command key** left to the spacebar: On systems with a Mac-style keyboard, the Command key is located to the left of the spacebar, which can be more comfortable for Mac users. When Gershwin is used with a PC keyboard, the Alt key functions as the Command key, being located in the same physical location on the keyboard.

- **Global menu bar**: Gershwin features a global menu bar, which is a menu bar that is located at the top of the screen, rather than within the application window. This provides for a consistent user experience across applications, even if those applications are written in non-native frameworks such as Gtk or Electron.

- **Keyboard layout and language autodetection**: When using a Mac or a [Raspberry Pi Keyboard and Hub](https://www.raspberrypi.com/products/raspberry-pi-keyboard-and-hub/), Gershwin will automatically detect the keyboard layout and language, removing the need to configure it manually.

- **Menu search and launcher**: Gershwin includes a powerful menu search and launcher, which can be used to quickly find and launch applications, files, and menu commands. It is located in the top left corner of the global menu bar and can be a true productivity booster, as it allows the system to be operated virtually only by using the keyboard.

- **Application bundles**: Applications on Gershwin are structured as self-contained bundles, which can be easily installed, removed, and updated.

## Features

- **Resource efficiency**: Gershwin is designed to be lightweight and efficient, with moderate resource usage, making it suitable for use on a range of hardware configurations. Compare the RAM usage to other similar systems but be aware that as of early 2026, optimization on RAM usage has not yet started.

- **Zeroconf (mDNS-SD)**: Gershwin makes use of this technology throughout the system to advertise and browse network resources, like servers, printers, and the like.


## Particularities

* Please refer to the global menu bar at the top of the screen in Gershwin as the "Menu", as those who have grown up using a Macintosh will likely feel right at home with this naming convention. Avoid terminology like "panel", as the target audience of Gershwin may not be familiar with it. Similarly, please refer to the "desktop", "documents", and "folders" rather than the "wallpaper", "files", and "directories".

* The file manager in Gershwin, Workspace, is intentionally kept simple and is aspiring to grow into a truly [spatial](https://arstechnica.com/gadgets/2003/04/finder/) file manager over time. Which means that every document and folder has one physical location on the screen, and one location only. When opening an empty folder, it will show a blank window. This is because there is nothing to display in an empty folder, much like how a text editor shows a blank window when opening a new text file.

* Gershwin uses black text on white background. With the intoduction of modern graphical user interfaces, black text on white background (like paper, "WYSIWYG") was a revelation compared to the amber or green screens of MS-DOS PCs.

* In Gershwin, there is no concept of "launchers." Instead, it uses symlinks to applications, which can be created by dragging and dropping an application from its folder to the desktop. Keep in mind that Gershwin aims to keep things simple and straightforward for switchers from the Mac, who don't have conceptions of "launchers".
