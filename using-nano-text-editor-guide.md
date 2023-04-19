# Text Editors
> In order to edit and save text files, we need a text editor. Programmers use text editors to write programs, but because programmers often work in graphical user environments, they may often use graphical text editors or graphical Integrated Development Environments (IDEs). It might be that if you work in systems librarianship, that you will often use a graphical text editor, but knowing something about how to use command line-based editors can be helpful.
> Plain Text is a basic way to store human-readable text
> Upon examination, most of the files in a single .docx file are plain text that are marked up in XML. The files are packaged as a .docx file and then rendered by an application, commonly Microsoft Word, but any application that can read .docx files will do.
* think a digitial typewriter *

# Nano Text Editor
> The nano text editor is one of the user-friendliest of the text editors available on the Linux command line, but it still requires some adjustment as a new command line user. The friendliest thing about nano is that it is modeless, which is what you're already accustomed to using. This means nano can be used to enter and manipulate text without changing to insert or command mode. It is also friendly because, like many graphical text editors and software, it uses control keys to perform its operations.

- Vim and ed(1) are text editors that can also be used

#### Note on Text Editors
> A modal text editor has modes such as insert mode or command mode. In insert mode, the user types text as anyone would in any kind of editor or word processor. The user switches to command mode to perform operations on the text, such as find and replace, saving, cutting and pasting but cannot insert text as they would in insert mode. Switching between modes usually involves pressing some specific keys. In Vim and ed(1), my text editors of choice, the user starts in command mode and switches to insert mode by pressing the letter i or the letter a. The user may switch back to command mode by pressing the Esc key in Vim or by pressing the period in a new line in ed(1).

## Control Keys in Nano

> For example, instead of Ctrl-c or Cmd-c to copy text, in nano you press the M-6 key (press Alt, Cmd, or Esc key and 6) to copy. Then to paste, you press Ctrl-u instead of the more common Ctrl-v. Fortunately, nano lists the shortcuts at the bottom of the screen.
> The shortcuts listed need some explanation, though. The carat mark is shorthand for the keyboard's Control (Ctrl) key. Therefore to Save As a file, we write out the file by pressing Ctrl-o (although Ctrl-s will work, too). The M- key is also important, and depending on your keyboard configuration, it may correspond to your Alt, Cmd, or Esc keys. To search for text, you press ^W, If your goal is to copy, then press M-6 to copy a line. Move to where you want to paste the text, and press Ctrl-u to paste.

** Start nano by typing nano **
- If the file doesn't exist, this will create it. If it does exit, then the above command will open it.
- One of the other tricky things about nano is that the menu bar (really just a crib sheet, so to speak) is at the bottom of the screen instead of at the top, which is where we are mostly accustomed to finding it these days. Also, the nano program does not provide pop up dialog boxes. Instead, all messages from nano, like what to name a file when we save it, appear at the bottom of the screen.
- Lastly, nano also uses distinct terminology for some of its functions. The most important function to remember is the Write Out function, which means to save.

