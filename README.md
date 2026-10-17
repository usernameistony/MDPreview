# Markit

<img src="Resources/AppIcon.png" alt="Markit" width="96" align="right">

English · [简体中文](README.zh-Hans.md)

A Markdown reader and light editor for macOS. Written with SwiftUI, AppKit and
`apple/swift-markdown`. No WebView. Called MDPreview before v2.0.

Current version: v2.0.0 · Requires macOS 14+ · MIT license

![Split view in dark mode, with the outline sidebar](docs/screenshots/split-dark.png)

---

## Features

- **Reading, editing and split view**, switched with `⌘R` / `⌘E` / `⇧⌘E`.
- **The reading view is one read-only text view.** You can select and copy across
  paragraphs and lists, search with `⌘F`, and use Look Up.
- **Rendering**: headings, bold / italic / strikethrough, inline code, `==highlight==`,
  links, block quotes, ordered / unordered / nested lists, task lists, tables,
  horizontal rules, images (local and remote).
- **Code highlighting** for Swift, Python, JavaScript / TypeScript, JSON, shell, Rust and Go.
- **Outline sidebar** built from H1–H6. Collapsible, click to jump, follows the scroll position.
- **Links**: `#heading` anchors jump within the document; relative `.md` links open in Markit.
- **Task lists**: tick a checkbox in the reading view and the source file is updated.
- **Editor** on TextKit 2 with line numbers. Markdown syntax coloring can be turned on in Settings.
- **Editing help**: `⌘B` / `⌘I` / `⌘K` / `⇧⌘K` for bold / italic / link / inline code; Return continues
  a list and exits it on an empty item; `Tab` / `⇧Tab` indent list items.
- **Images**: paste or drop an image and it is saved to an `assets/` folder next to the document, with a link inserted.
- **Settings** (`⌘,`): font, font size, line spacing, content width, light / dark / system theme.
  `⌘=` / `⌘-` / `⌘0` change the font size.
- **Export to PDF and print** (`⌥⌘P` / `⌘P`).
- **Reload on external change**: if another app changes the file, Markit reloads it.
  If you have unsaved edits, it asks first.
- **Finder previews**: Quick Look (space bar) and file thumbnails show the formatted document.
- **Languages**: English and Simplified Chinese, following the system setting.
- **Accessibility**: VoiceOver labels; stronger borders with Increase Contrast; no animations
  with Reduce Motion.
- **Document-based app**: multiple windows and tabs, `.md` file association, drag a file in to open it.
- Uses Liquid Glass on macOS 26+ and falls back to translucent materials on older systems.

## Screenshots

![Reading view with the outline sidebar](docs/screenshots/reader.png)

<table>
  <tr>
    <td width="60%"><img src="docs/screenshots/quicklook.png" alt="Quick Look in Finder"></td>
    <td width="40%"><img src="docs/screenshots/settings.png" alt="Settings"></td>
  </tr>
  <tr>
    <td align="center">Quick Look in Finder</td>
    <td align="center">Settings</td>
  </tr>
</table>

## Versions

**v1.1**
Fixed Chinese input, task toggling, nested lists and `==` highlighting bugs; added unit tests and CI.

**v1.2**
PDF export and printing; reload on external change; all UI text moved to one place.

**v1.3**
1. The reading view is now a read-only `NSTextView`: selection works across blocks, `⌘F` works while reading.
2. Settings: font, size, line spacing, content width, theme, editor line numbers and syntax coloring.
3. UI, menus and messages use a String Catalog with `en` and `zh-Hans`.
4. Finder Quick Look preview and thumbnail extensions.
5. Accessibility: VoiceOver labels, Increase Contrast, Reduce Motion.
6. English and Chinese README.

The 1.x DMGs are ad-hoc signed and not notarized.

**v1.4 — editing** (released as part of 1.5.0)
Formatting shortcuts (`⌘B` / `⌘I` / `⌘K` / `⇧⌘K`); list continuation on Return and Tab indenting;
pasted or dropped images saved to `assets/`; bold no longer colored as italic in the source view.

**v1.5 — performance, reload**
The reading view re-renders only the blocks that changed. On a 160 KB document, one edited character
takes about 30 ms instead of about 0.8 s. Reloading after an external change no longer marks the
document as edited, and a deleted file that comes back is watched again.

**v2.0 (current) — Markit, Mac App Store**
Renamed from MDPreview to Markit. Released on the Mac App Store; there is no DMG for this version.
The app runs in the sandbox: before it shows local images or opens linked files, it asks you to
allow access to the document's folder.

**Later**
Inline images, math, HTML blocks, more accurate code highlighting, more languages.

> Design notes and known gaps are in [PROJECT_SPEC.md](PROJECT_SPEC.md) (Chinese).

## Keyboard shortcuts

| Keys | Action |
| :-- | :-- |
| `⌘R` / `⌘E` / `⇧⌘E` | Reading / editing / split view |
| `⌘⌥S` | Show / hide outline |
| `⌘F` | Find (reading and editing view) |
| `⌘B` / `⌘I` / `⌘K` / `⇧⌘K` | Bold / italic / link / inline code (editing view) |
| `⌘=` / `⌘-` / `⌘0` | Larger / smaller / default font size |
| `⌘,` | Settings |
| `⌘P` / `⌥⌘P` | Print / export as PDF |
| `⌘N` / `⌘O` / `⌘S` / `⌘⇧S` | New / open / save / save as |
| `⌘W` / `⌘Q` | Close window / quit |

## Install

Install Markit from the [Mac App Store](https://apps.apple.com/app/id6821136481). It is free.

> **Note:** from v2.0, Markit is distributed only through the App Store. GitHub Releases no longer
> include a DMG. The older MDPreview DMGs (v1.5.0 and earlier) are still on
> [Releases](https://github.com/TonyT1226/MDPreview/releases), but they are not notarized and will not be updated.
> If you used MDPreview, delete `MDPreview.app` after installing Markit, so that Finder uses
> Markit's Quick Look extensions.

The Finder preview extensions become active after the app is in Applications and has been
opened once. If the space-bar preview still shows plain text, check that Markit is enabled
in System Settings › General › Login Items & Extensions › Quick Look.

## Build from source

You need full Xcode 16+ and [XcodeGen](https://github.com/yonaskolb/XcodeGen).

```bash
git clone https://github.com/TonyT1226/MDPreview.git
cd MDPreview

swift test                    # unit tests for the core library

brew install xcodegen
xcodegen generate
open MDPreview.xcodeproj       # run / preview / archive in Xcode

./package_app.sh              # or build a local DMG from the command line
```

If `swift test` fails with `no such module 'Testing'`, SwiftUI previews don't load, or the app
starts without a window, `xcode-select` is probably pointing at the Command Line Tools:

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for more.

## Privacy

Markit does not collect any data. See the [privacy policy](docs/PRIVACY.md).

## License

[MIT](LICENSE)
