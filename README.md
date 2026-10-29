# Markdown Viewer

A Windows app by D8A AI XYZ that shows Markdown the way GitHub would, one file or a whole folder at a time. One
portable exe: nothing to install, no administrator rights. Zoe, the built-in document assistant, answers questions
about your files and cites the sections she used.

This repository publishes the **releases** and the **user guide**. The source code is developed in a private D8A
repository. A Mac version is in testing; its test builds are on the Releases page too (see [On a Mac](#on-a-mac-in-testing)).

## Download

Releases are on the [Releases page](../../releases). Each release carries:

| File | For |
|---|---|
| `MarkdownViewer-<version>-win-x64.exe` | Windows 10 or 11, 64-bit |
| `SHA256SUMS.txt` | SHA-256 checksum of the file above |

Put the exe in any folder and run it. It is not yet code-signed, so Windows SmartScreen may say "Windows protected
your PC"; **More info › Run anyway** continues. To check a download, compare its hash with `SHA256SUMS.txt`:

```powershell
Get-FileHash .\MarkdownViewer-<version>-win-x64.exe -Algorithm SHA256
```

### On a Mac (in testing)

The Mac app is not released yet. Its test builds are on the [Releases page](../../releases) too, marked
**Pre-release**; the current one is [write-mode-3](../../releases/tag/write-mode-3):

| File | For |
|---|---|
| `MarkdownViewer-write-mode-3-macOS-AppleSilicon.zip` | Macs with an Apple chip (M1 and later) |
| `SHA256SUMS.txt` | SHA-256 checksum of the zip (`shasum -a 256 <zip>` in Terminal shows yours) |

There is no Intel build at the moment. Double-click the zip, move **Markdown Viewer** to Applications (over an
earlier test build, if there is one) and open it. Apple has not signed it, so the first start stops: on macOS 15 and
later click **Done**, then **System Settings › Privacy & Security › Open Anyway**; on macOS 12 to 14 right-click the
app, **Open**, then **Open**. The release notes say what is in the build, what is not yet, and how to run its
self-test.

On a Mac the app has Write mode, and Zoe keeps your API key in the macOS Keychain (as `MarkdownViewer/anthropic`).
The first time a new test build reads a key an earlier build saved, macOS asks once: click **Always Allow**. Not
there yet: Zoe's voice, and opening a `.md` file from Finder (use Open file, Open folder, or drag it onto the window).

## What a PC needs

- Windows 10 or 11, 64-bit.
- The Microsoft Edge WebView2 Runtime, which Windows 11 already has. If the app says it is missing, install it from
  [Microsoft](https://developer.microsoft.com/microsoft-edge/webview2/).
- Nothing else: .NET is inside the exe. Settings and the recent lists live in `%LOCALAPPDATA%\MarkdownViewer`.

## Open a folder

- Click **Open folder** (<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>O</kbd>), drop a folder on the window, or start the
  app with a folder path.
- The left pane lists every Markdown file under that folder, arranged by its subfolders. Click a file to open it;
  the arrow keys move through the list.
- **Filter files** narrows the list by file or folder name. <kbd>Esc</kbd> clears it.
- The list updates by itself when files or folders are added, renamed or deleted, and the open file reloads when
  it is saved, so you can keep the viewer beside your editor.
- Next time, the app reopens the same folder on the same file.

Folders without Markdown files are left out, and so are `.git`, `node_modules`, `bin`, `obj`, `.vs`, `.idea`,
`__pycache__`, `.venv` and `venv`. A folder shows at most 20,000 files.

## Open a file

Drop a `.md` file on the window, press <kbd>Ctrl</kbd>+<kbd>O</kbd>, pick one from **Recent**, or start the app with
a path (`"Markdown Viewer.exe" notes.md`, or **Open with** in File Explorer).

## What it shows

- Tables, task lists, footnotes, alerts (`> [!NOTE]`, tip, important, warning, caution) and GitHub emoji codes.
- Code coloured by language, with a Copy button.
- ` ```mermaid ` blocks drawn as diagrams by the Mermaid bundled in the app, so nothing is fetched from the web.
  Click a diagram to enlarge, zoom and pan; its source stays below it.
- Images from the document's folder. Click one to see it fitted to the window, then zoom and pan.
- Light and dark themes, or the Windows setting.

Links to other Markdown files open in the viewer; web and mail links open in your browser or mail app. Scripts in a
Markdown file never run, and a document cannot load anything from the web.

## Ask Zoe about your documents

Click **Ask Zoe** or press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>A</kbd>. She opens beside the document.

1. Choose the scope: **Current document**, or **Entire library** when a folder is open.
2. Ask a question, or pick **Explain this document**, **Summarise** or **Compare related guidance**.
3. Click a numbered citation to open that section.

Answers use only passages from your files and cite them; when nothing matches, Zoe says so instead of guessing.
**Review documentation** checks the document or the library for broken links, missing images and sections,
unfinished parts and, with Claude, conflicting instructions and gaps. Zoe only reads your files; she never changes
them.

### Without Claude, nothing leaves your PC

Until you connect Claude, Zoe shows the passages that match your question, and the review runs the app's own checks.
Nothing is sent anywhere.

### Connect Claude

Zoe answers with Claude through the Anthropic API, on your own API key. The API is billed by Anthropic, per use; a
Claude.ai subscription does not include it.

1. Create an API key in the Anthropic Console (console.anthropic.com).
2. In Zoe's pane, click the gear to open **Zoe settings** and choose **Claude (Anthropic API)**.
3. Choose a model (**Load models** lists the ones your key can use), paste the key, and tick **Allow sending
   excerpts of my documents to Anthropic**.
4. Only if Anthropic says your key "is not scoped to a workspace", paste its **Workspace ID** (it starts with
   `wrkspc_`).
5. Click **Save**.

Each question sends the question, the earlier turns of the conversation and the matching excerpts (about 14,000
characters at most) to api.anthropic.com, never whole files. A review sends each file, up to 12,000 characters,
and some pairs of related files; the Review tab says the most it can send before it starts.

The key is kept in Windows Credential Manager (on a Mac, the macOS Keychain) as `MarkdownViewer/anthropic`, never in
the settings file, and is never shown, logged or put in an error message. **Remove saved key** in Zoe settings deletes it.

### Talk to Zoe

The microphone button dictates into the question box; the **Talk** button starts a spoken conversation, with Zoe
reading her answers aloud. Speech recognition runs on your PC with Whisper (English): your voice is never sent
anywhere and nothing is recorded. The first time you use the microphone, about 190 MB of speech model is unpacked to
`%LOCALAPPDATA%\MarkdownViewer\speech`. Windows speech is the option for other languages installed in Windows.

## Shortcuts

| Keys | Action |
|---|---|
| <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>O</kbd> | Open a folder |
| <kbd>Ctrl</kbd>+<kbd>O</kbd> | Open a file |
| <kbd>Ctrl</kbd>+<kbd>B</kbd> | Show or hide the file tree |
| <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>A</kbd> | Show or hide Zoe |
| <kbd>Ctrl</kbd>+<kbd>F</kbd> | Find, then <kbd>Enter</kbd> and <kbd>Shift</kbd>+<kbd>Enter</kbd> |
| <kbd>Ctrl</kbd>+<kbd>E</kbd> | Open in VS Code, or Notepad |
| <kbd>Ctrl</kbd>+<kbd>P</kbd> | Print or save as PDF |
| <kbd>F5</kbd> | Reload |
| <kbd>Alt</kbd>+<kbd>Left</kbd> / <kbd>Right</kbd> | Back and forward between linked files |
| <kbd>Ctrl</kbd>+wheel, <kbd>Ctrl</kbd>+<kbd>0</kbd> | Zoom, reset zoom |
| <kbd>Ctrl</kbd>+<kbd>W</kbd> | Close the window |

## Status

Version 1.2.0 is the first public release. Known limits:

- Unsigned exe, so SmartScreen warns on download and first run.
- Tested at 100% display scale only.
- Wide left-to-right diagrams are small when fitted to the page; click to enlarge them.
- Whisper understands English only.

Problems and questions: open an issue in this repository. Please include the version (the release you
downloaded) and what you did just before the problem. The app's log is `%LOCALAPPDATA%\MarkdownViewer\error.log`;
it never holds an API key.

## Third-party software

The exe includes, each under its own licence: [.NET](https://github.com/dotnet/runtime) (MIT),
[Markdig](https://github.com/xoofx/markdig) (BSD 2-Clause), [Markdown.ColorCode](https://github.com/wbaldoumas/markdown-colorcode)
and [ColorCode](https://github.com/CommunityToolkit/ColorCode-Universal) (MIT), the
[Anthropic C# SDK](https://github.com/anthropics/anthropic-sdk-csharp) (Apache 2.0), the
[WebView2 SDK](https://www.nuget.org/packages/Microsoft.Web.WebView2) (Microsoft's BSD-style licence),
[Mermaid](https://github.com/mermaid-js/mermaid) 11.17.2 (MIT), [Whisper.net](https://github.com/sandrohanea/whisper.net)
and [whisper.cpp](https://github.com/ggml-org/whisper.cpp) (MIT), the Whisper small.en model (MIT) and the Silero VAD
model (MIT), and the [Archivo](https://github.com/Omnibus-Type/Archivo) typeface (SIL Open Font License 1.1).

## Licence

No licence has been chosen yet. Until one is, the software and documents here are © 2026 D8A AI XYZ, all rights
reserved; they may be downloaded and used for evaluation. "D8A AI XYZ" and the D8A mark are trademarks of D8A AI XYZ.
