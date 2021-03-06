# Desktop Workflow

The desktop workflow brings all of the required processing software to your computer. It's designed to remove [GitHub](https://github.com) as a dependency so that you can use any web hosting platform for publishing your open textbooks. In this tutorial, we'll walk you through the process of setting up your computer and running basic processing commands. 

**Prerequisites**

You'll need a Windows, macOS, or Linux computer with the ability to install open source software packages from the command line (we'll teach you how!). For example, on a Windows computer, you'll need the ability to login as an administrator to install certain programs and enable advanced operating system features. 

::: aside :::

Lantern depends on [GNU Bash](https://www.gnu.org/software/bash/) and [GNU Make](https://www.gnu.org/software/make/). These programs work fine on macOS and Linux computers, but they require some extra programs in order to run on Windows computers. We recommend the [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about) for which we provide some guidance below.   

:::

## Setup on Windows

_You can skip to the next section if you are using macOS or Linux._

Lantern uses software that requires a [Unix-like](https://en.wikipedia.org/wiki/Unix-like) operating system. Since Windows does not behave like a Unix system, we'll need to install one before working with Lantern. Luckily, recent versions of Windows 10 support the [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about), which allows you to install and run a full [Linux distribution](https://en.wikipedia.org/wiki/Linux_distribution) within your normal Windows environment. 

Microsoft maintains [instructions on how to install WSL on a Windows 10 computer](https://docs.microsoft.com/en-us/windows/wsl/install-win10). We recommend installing WSL 2 with the [latest Ubuntu version is 20.04 LTS](https://www.microsoft.com/en-us/p/ubuntu-2004-lts/9n6svws3rx71?rtc=1&activetab=pivot:overviewtab), but WSL 1 and any other Linux distribution might work too. The instructions assume you will have some familiarity with the Windows command line interface, specifically [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.1). 

We highly recommend following [Microsoft's instructions for a manual installation](https://docs.microsoft.com/en-us/windows/wsl/install-win10) as written, but we also provide an opinionated set of instructions below. 

### Installing Windows Subsystem for Linux (WSL) with Ubuntu

WSL is a feature in Windows that lets you run a Linux operating system _within_ your normal Windows computing environment. Linux operating systems are available in a [variety of _distributions_](https://en.wikipedia.org/wiki/Linux_distribution#Widely_used_GNU-based_or_GNU-compatible_distributions). Each distribution is a collection of software packages that use the Linux kernel. This set of instructions will install WSL with [Ubuntu](https://ubuntu.com/) 20.04 LTS as the Linux distribution.   

1. Checking Your System Type: Most Windows computers have either x64 or ARM64 systems. If you're not sure which one you're using, follow these steps:

- Hit the Windows Key
- Type "PowerShell"
- Press Enter
- Type in the prompt: `systeminfo | find "System Type"`
- Press Enter

_You should see a message that says something like: ` x64-based PC`_. 

1. Checking Your Windows 10 Version: There are several **versions** and **builds** of Windows 10. If you're not sure which version and build you have, follow these steps:_

- Hit the Windows Key
- Hit the "R" key
- Press Enter
- Type "winver"
- Press Enter

_The pop-up message will include both version and build numbers for your Windows 10 software._ 

- For x64 systems: you need version 1903 or higher, with Build 18362 or higher.
- For ARM64 systems: you need version 2004 or higher, with Build 19041 or higher.
- Builds lower than 18362 do not support WSL 2. See if you can update your version of Windows.

**If you meet the system requirements, you can move ahead to the next steps. If you don't, try using the online workflow instead.**

1. Open PowerShell as an Administrator

- Hit the Windows Key
- Type "PowerShell"
- Select "Run as Administrator"
- Login with an Administrator username and password

1. Enable WSL on Your Computer

In PowerShell, run:

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

1. Enable Virtual Machine Features

In PowerShell, run:

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

1. Restart your computer

1. Based on your system type (e.g. x64), Download the latest Linux kernel update package [from Microsoft](https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-4---download-the-linux-kernel-update-package)

1. Set WSL 2 as your default version

In PowerShell, run:

```
wsl --set-default-version 2
```

1. Download Ubuntu 20.04 LTS from the [Microsoft Store](https://www.microsoft.com/store/apps/9n6svws3rx71)

- From the Store page, click on "Get"
- Wait for the download to finish; it will take a few minutes
- Click on the Launch button when the download finishes

1. Congrats! You now have an Ubuntu Operating System within your Windows computer!

This will launch a new terminal window for Ubuntu. In the task bar, we recommend you right-click the Ubuntu icon and pin the program to your task bar (or Start menu) for easy access to it. 

1. Create a User for Ubuntu

The first time you open the Ubuntu terminal, you will be prompted to create a new user profile. This user is separate from the user profile you use to log in to your Windows computer. You can set this to whatever you want; just be sure to remember the password!

1. Update Your Ubuntu System Packages

It's usually a good idea to keep your system packages up to date with the latest versions. Since you have a clean install of Ubuntu 20.04 LTS, it's worth checking to make sure all software dependences are current with their latest release. We can do this with a few commands:

In the terminal, run (you may be prompted to enter the password you set for your Ubuntu user):

```
sudo apt update
```

In the terminal, run (you will be asked to confirm the installation of new versions of software; enter `Y` and press enter):

```
sudo apt upgrade
```

1. Enable Easy Copying and Pasting in the Ubuntu Terminal

- Right-click the top bar of the Ubuntu terminal
- Select "Properties"
- In the Edit options, check the box for "Use Ctrl+Shift+C/V as Copy/Paste"

This will let you copy text from the terminal with `Ctrl+Shift+C` and paste text into the Terminal prompt with `Ctrl+Shift+V`. This will be very helpful during the Homebrew installation process.

## Install Homebrew

[Homebrew](brew.sh) is a software package manager that simplifies the process for installing, uninstalling, and upgrading software from the command line. Visit [https://brew.sh/](https://brew.sh/) for instructions on installing Homebrew. 

Be sure to read through the "Next Steps:" prompt in your terminal after the initial Homebrew install is complete. Run the commands that Homebrew provides to "Add Homebrew to your PATH" and "Install the Homebrew dependences". Check your terminal for the exact commands.

## Install Pandoc

We can use Homebrew to install [Pandoc](https://pandoc.org) and [Pandoc Crossref](https://github.com/lierdakil/pandoc-crossref). 

Pandoc is our main document converter. We use Pandoc to convert manuscript files from Word processing formats to Markdown, then Markdown to our publishing output formats (e.g. HTML, LaTeX, DOCX, etc.). Pandoc Crossref is a filter that helps us label and number equations, figures, and images in order to produce links within our textbook projects. 

We can install both of these with Homebrew by executing the following command: 

```sh
brew install pandoc pandoc-crossref
```

## Install TinyTeX

_You can skip this section if you already have LaTeX installed on your system._

Lantern uses [$LaTeX$](https://www.latex-project.org/) to make PDFs. $Latex$ is a document production system that is commonly used for formatting mathematical notation and advanced typesetting. 

Lantern uses a minimal LaTeX distribution called [TinyTeX](https://yihui.org/tinytex/). Follow the installation instructions on the TinyTeX homepage for your system:

### Windows WSL with Ubuntu:

```sh
wget -qO- "https://yihui.org/tinytex/install-bin-unix.sh" | sh
```

Now reload the terminal session with:

```
source ~/.profile
```

You now have have a minimal version of LaTeX installed. You can now use common LaTeX commands, including `tlmgr`, `pdflatex`, and `xelatex` (You won't need to know these commands to use Lantern, but just letting you know that they are available). 

### macOS:

```
sudo chown -R $(whoami) /usr/local/bin
curl -sL "https://yihui.org/tinytex/install-bin-unix.sh" | sh
```

## Text Editing

There are two main types of documents we use to write and edit text: [plain-text](https://en.wikipedia.org/wiki/Plain_text) and [rich text](https://en.wikipedia.org/wiki/Formatted_text). Plain text exposes the characters within a document, whereas rich text displays the formatting features and styles. 

| File Contents | File Extension          | Editors                    |
| ------------- | ----------------------- | -------------------------- |
|   Plain text  | `.xml`, `.html`, `.md`  | Notepad, TextEdit, VS Code |
|   Rich text   | `.docx`, `.rtf`, `.odt` | Microsoft Word, Scrivener  |

Most of us are trained to use rich text editors: emails, word documents, content management systems (e.g. WordPress). This is for good reason: they're easy to use and we need them for everyday things. For academic publishing purposes, plain text offers some advantages over rich text, as Tenen and Wythoff ([2014](https://doi.org/10.46430/phen0041)) explain: 

> Plain text both ensures transparency and answers the standards of long-term preservation. MS Word may go the way of [Word Perfect](https://en.wikipedia.org/wiki/WordPerfect) in the future, but plain text will always remain easy to read, catalog, mine, and transform. Furthermore, plain text enables easy and powerful versioning of the document, which is useful in collaboration and organizing drafts. Your plain text files will be accessible on cell phones, tablets, or, perhaps, on a low-powered terminal in some remote library. Plain text is backwards compatible and future-proof. Whatever software or hardware comes along next, it will be able to understand your plain text files.

### Installing Visual Studio Code

We recommend installing [Visual Studio Code (VS Code)](https://code.visualstudio.com/) as your text editor, but you're welcome to use any other text editor you prefer. Visit the website to [download](https://code.visualstudio.com/) and [install](https://code.visualstudio.com/docs/setup/setup-overview) VS Code. When prompted to "Select Additional Tasks" during installation, double-check that the Add to PATH option is checked so you can easily open a folder in WSL using the `code` command from your terminal. We also think it's helpful to check the box for the other Additional Tasks, including:

- Add "Open with Code" action to Windows Explorer file context menu
- Add "Open with Code" action to Windows Explore directory context menu
- Register Code as an editor for supported file types

**Windows Users**

VS Code can be set up to work with your Windows Subsytem for Linux (WSL) setup. The first time you launch VS Code, it will likely detect your WSL setup and ask if you'd like to install recommended extensions. Click the "Install" button when prompted.

![](#)

If you didn't see that prompt when opening VS Code, [follow these instructions](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode#:~:text=Visit%20the%20VS%20Code%20install,WSL%20using%20the%20code%20command.) on configuring youre VS Code editor with your WSL instance. You can skip the Git and Windows Terminal sections as those are optional.  

## Creating Your Desktop Workspace

We'll need a place on our computers to store our textbook files. Let's create a folder to contain our Lantern projects. For these steps, open your terminal:

Check your current directory:

```
pwd
```

_This will return the full path to your prompt's current directory._

Create a new directory:

```
mkdir lantern-projects
```

Change into that directory:

```
cd lantern-projects
```

## Download Lantern

Now that you have all of the required software for the desktop workflow, you're ready to download the Lantern files to your computer. [Lantern](https://github.com/nulib-oer/lantern) is a Git repository hosted on [GitHub](https://github.com). We won't get too much into Git or Github, but we will use it to download the files. Git is pre-installed with Ubuntu and macOS (if it's not on your macOS, run `brew install git` in your terminal). You can download Lantern by running this command:

```
git clone https://github.com/nulib-oer/lantern.git
```

_Navigate to the new folder containing Lantern files_

```sh
cd lantern
```

_Open this folder in VS Code_

```sh
code .
```

![Screenshot of VS Code open to the Lantern folder]()
_These icons are generated by the [vscode-icons extension](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons), which is highly recommended!_

**You have now set up your desktop workspace for using Lantern!**

## Lantern Files and Folders

Lantern comes with a few files and folders you'll be using to add and edit your manuscript content. All of these files and folders are contained in the `source` directory. These are the **only** files and folders you will need to edit to use Lantern for your textbook project.

![Screenshot of the `source` directory](source-dir.png)

- `chapters/`: This folder contains the textbook chapter files

- `images/`: This folder contains the textbook images for figures, logos, covers, etc.

- `preprocess/`: If you add `.docx`, `.odt`, or `.tex` files to this folder, Lantern will convert these files to Markdown and add them to the `chapters/` folder with the `make markdown` command (more on this below)

- `metadata.yml`: This file defines the bibliographic metadata for your textbook

- `references.bib`: This file contains the bibliography for your textbook in [BibTex format](http://www.bibtex.org/Format/)

## Metadata

Textbooks need bibliographic metadata in order to be indexed by search engines and library catalogs. Lantern stores metadata about the textbook in a YAML file. The information stored in the YAML file will be used to fill the templates for each of the publication formats. 

::: aside :::

### YAML Primer

YAML is to JSON what Markdown is to HTML. It's a more human-readable (and human-writable) way to express and store data. 

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/cdLNKUoMc6c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

YAML needs to be valid, so if you ever hit an error, it's a good idea to check the YAML code you're using with a [YAML validation tool](https://jsonformatter.org/yaml-validator). 

:::

- Open the `metadata.yml` file to view the available metadata fields.

![Screenshot of the Metadata file in VS Code](metadata.png)

- Edit the file by replacing the placeholder metadata values with your textbook's information 

- When you're finished with your edits, save the file: Control + S 

You can fill out the this file using [all available metadata fields](#).

## Chapters

The `chapters/` folder includes two sample chapters formatted in Markdown for your reference: `01-introduction.md` and `02-chapter-example.md`. Lantern looks for files in this folder to build out the chapters of the textbook. 

By default, Lantern sets the order of the chapters using the file name. You can set the order of the chapters by adding a numeric prefix to the file name, `00-`, `01-`, `02-`, and so on. Anything after the hyphen (`-`) in the file name can be used for your reference to quickly identify the file. It is a best practice to use all lowercase words separated by hyphens (`-`) in filenames. For example:

1. Preface = `00-preface.md`
1. Chapter One = `01-introduction.md`
1. Chapter Two = `02-chapter-example.md`
1. Chapter Three = `03-probability.md`
1. Chapter Four = `04-binomial-distribution.md`

The title of the chapter as it displays in the final textbook is set by the first line of the chapter file. The first line of each chapter file must begin with one hashtag symbol (`#`) to represent a first-level heading (or `<h1>` if you're familiar with [HTML headings](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements)). For example, the chapter file `01-introduction.md` contains the first line: `# Introduction to Vegetable Lasagna`, which renders thusly:

**HTML:**

# Introduction to Vegetable Lasagna {.unlisted}

::: summary :::

You can preview the HTML rendering of the Markdown within VS Code by pressing `Ctrl+Shift+V` with your keyboard. Lantern provides some special syntax for specific chapter components, but basic Markdown will render properly, such as headings, paragraphs, emphasis, italics, lists, links, tables, and images. 

![Screenshot of Markdown Preview (with Math)](dw_markdown-preview.png)

_The Math rendering in the preview screen is handled by the VS Code [Markdown+Math extension](https://marketplace.visualstudio.com/items?itemName=goessner.mdmath)._

:::

**PDF:**

![](chapter-title.png)

**LaTeX:**

```
\chapter{Introduction to Vegetable Lasagna}
```

### Converting Word Processing Formats to Markdown

Most OER authors are not writing their manuscripts in Markdown (yet!), so we'll need to convert from more common file formats. The [example chapters](https://drive.google.com/drive/folders/1Fl__DhDXDFyoPmwX0CHpfj10qhOY3t0k?usp=sharing) are Google Docs that can be downloaded as `.docx` or `.odt` files, which are the file types that most word processing software use. 

Lantern has includes a micro-workflow that will help you convert raw manuscript files from `.docx` or `.odt` to Markdown (`.md`).

- Download the example chapter files from the Google Drive, ignoring the README file, which we don't need
- Save the files to the `source/preprocess/` folder in your Lantern project directory

- In your terminal window, run this command:

```
make markdown
```

This command will run a script that will use Pandoc to convert the `.docx` or `.odt` files in the `source/preprocess/` folder to Markdown files that will be saved in the `source/chapters/` folder. 

You can now edit any of these files using your text editor. 

### Editing Chapter Files

The conversion process between word processing formats and markdown won't be perfect, so you may need to spend some time correcting any formatting errors or removing any unnecessary markup. 

- Open on the "03-probability.md" file in the `source/chapters/` folder with VS Code. We'll need to edit this file because there's a problem with the title of the chapter. 

The title of this particular chapter is not formatted properly. Instead of marking the title as a heading, the title is formatted as `**bold**`. This is a common problem with word processing formats, wherein headings are representing _visually_ but not _semantically_. Markdown uses specific syntax to mark contents as headings.

- Change the `**Probabilities**` heading to a proper markdown heading: `# Probabilities`

- There is another heading error in the file. Change `**Introduction to Probability Standard**` in line 19 to `## Introduction to Probability Standard` so that it represents a "Heading 2", or section heading.
- Scroll down to the bottom of the page and click on the "Commit changes" button to save these changes to your textbook

## Building Publication Outputs

```
make textbook
```

You will now have a new directory in your project folder called `public`. In it, you will find all of your textbook files in HTML, PDF, LaTeX, and DOCX formats. You can share these files however you'd like, including by uploading these files to any web hosting services as they are. 

## Uploading to a Web Server

