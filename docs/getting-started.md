## Getting started

Core Banking BASIC Tools adds editing, navigation, compilation and debugging of MultiValue BASIC sources (`.b`, `.component`) to IntelliJ IDEA 2025.3 and later, for Java-based runtimes such as TAFJ. Editor features work on their own; compilation, routine/insert completion and debugging use your local runtime installation.

### 1. Install and configure

1. Install the plugin from **Settings | Plugins | Marketplace** and restart the IDE.
2. Open the folder that contains your BASIC sources as a project.
3. Open **File | Project Structure | Facets**, click **+**, choose **TAFJ** and select your module.
4. Set **TAFJ home** to the installation directory (the one containing `conf/` and `lib/`). The plugin reads `conf/tafj.properties` from there.
5. Click **OK**. Inserts, compiled routines and components are indexed in the background; the first run reads every library jar once and caches the result, later starts are fast.

The facet is stored in the module file (`.iml`), so it can be shared with your team through version control.

### 2. Write code

- **Highlighting**: keywords, built-in functions, labels, `@` system variables, directives, strings, numbers and comments. EQU constants and COMMON variables declared in the file or its included inserts are highlighted too. Colors: **Settings | Editor | Color Scheme | TAFJ Basic**.
- **Statement completion**: press **Ctrl+Space** at the start of a line for keywords and block templates (`IF … THEN … END`, `FOR … NEXT`, `LOOP … REPEAT`, `BEGIN CASE`, `READ`, `LOCATE`, …); after `GOSUB` / `GOTO` it lists the labels of the file.
- **CALL completion**: after `CALL` it lists project routines and the routines of the installed libraries, with their parameters.
- **Components**: `$PACKAGE` and `$USING` complete component names. Typing a component name and a dot completes its methods (with arguments), constants, properties and tables, respecting public / module / private scope.
- **Inserts**: `$INSERT` completes insert names from the project, the insert and precompile directories and the library jars.
- **New file**: **File | New | TAFJ File** creates a Routine, Program, Function or Component from a template.
- **Formatting**: **Code | Reformat Code** (Ctrl+Alt+L) and Enter indent in the common house style: headers, labels and section `RETURN` in column 0, nested blocks, continuation lines. Indent size and tabs: **Settings | Editor | Code Style | TAFJ Basic**.
- **Comments**: **Ctrl+/** toggles `*` comments on the selected lines.

### 3. Navigate

- **Ctrl+Click** (or **Ctrl+B**) on a `GOSUB` / `GOTO` label jumps to the label.
- **Ctrl+Click** on `CALL NAME` opens the routine source, or its compiled class when the routine comes from a library.
- **Ctrl+Click** on an `$INSERT` name opens the insert; on an EQU constant or COMMON variable it jumps to the declaration.
- **Ctrl+Click** on `$PACKAGE` / `$USING` opens the component definition.
- The **Structure** tool window (Alt+7) and the **File Structure** popup (Ctrl+F12) list the labels of the file.
- **Quick Documentation** (Ctrl+Q, or hover) explains BASIC keywords and built-in functions with an example.

### 4. Compile

The plugin runs the compiler of your installation in one background process per project, so there is no JVM start per file.

- **Errors while typing**: syntax errors and warnings reported by the compiler are underlined in the editor and listed in the **Problems** tool window, also for unsaved changes.
- **Build | Build Project** compiles the sources changed since the last successful build (and those that include a changed project insert); **Build | Rebuild Project** compiles everything.
- **Compile with TAFJ** (editor or Project view context menu) compiles the selected files.
- Problems appear in the **Build** tool window; double-click to jump to the line. Errors inside an insert are shown on the `$INSERT` line.

### 5. Debug

Debugging needs `temn.tafj.runtime.enable.debug=true` in the runtime configuration.

- **Local program**: right-click a `PROGRAM` source and choose **Debug**, or create a **TAFJ Debug** run configuration. The program is compiled first, then started in a local runtime that connects to the IDE.
- **Application server**: start the server with `-Djbc.debug=<port>` and `temn.tafj.runtime.enable.jbc.debugging=true`, then create a **TAFJ Remote Debug** configuration with the host and port. Each BASIC session appears as its own thread; stopping only disconnects, the server keeps running.
- **Listen mode**: tick **Listen mode** in the remote configuration and start the Java process with `-Djbc.debug=<ide-host>:<port>` to debug BASIC code called from Java.
- Supported: line breakpoints, step over / into / out, run to cursor, variables (COMMON grouped, dynamic and dimensioned arrays expandable), set value, evaluate variable names.

### Keyboard shortcuts

| Shortcut | Action |
|---|---|
| Ctrl+Space | Statement, `CALL`, component and insert completion |
| Ctrl+Click / Ctrl+B | Go to label, routine, insert, declaration or component |
| Ctrl+Q | Quick Documentation |
| Ctrl+Alt+L | Reformat Code |
| Ctrl+/ | Toggle comment |
| Ctrl+F12 | Labels of the file |
| Ctrl+F9 / Shift+F9 | Build / Debug |

Shortcuts are those of the default Windows / Linux keymap; on macOS use Cmd instead of Ctrl.

### Troubleshooting

- **No compiler errors, no CALL completion**: check that the module has a TAFJ facet and that **TAFJ home** points to a directory with `conf/tafj.properties` and `lib/`. The facet editor validates the path.
- **An insert or routine is not found**: the plugin uses the insert, precompile, classes and generated-jar directories configured in `tafj.properties`. Project files take precedence over the installation.
- **"Missing subroutine" when debugging**: build the project (Ctrl+F9) so that all called routines are compiled.
- **Debugger does not stop**: check the debug properties above and that the breakpoint is on an executable line.

### Requirements

- IntelliJ IDEA 2025.3 or later. Build integration (Build / Rebuild Project, Compile with TAFJ) uses the bundled Java plugin; everything else, including the debugger, needs only the platform.
- A local runtime installation for compilation, library completion and debugging.

*Independent project, not affiliated with or endorsed by the vendors of the supported runtimes. All trademarks belong to their respective owners.*
