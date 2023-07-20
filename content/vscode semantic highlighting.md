https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide

Yes, you can indeed install a Visual Studio Code extension from a local directory. Here's how:

1. First, make sure you have Node.js installed in your system as it's required for running the `vsce` package, a command line tool for packaging VS Code extensions. If you haven't, you can download it from [here](https://nodejs.org/).

2. Next, install the `vsce` package globally using npm (which is included in Node.js) by running the following command in your terminal:

   ```bash
   npm install -g vsce
   ```

3. Once you have `vsce` installed, navigate to your extension's directory:

   ```bash
   cd path/to/your/extension
   ```

4. Then package your extension using `vsce`. This will create a `.vsix` file (VS Code's extension file format) in your extension's directory:

   ```bash
   vsce package
   ```

5. After the `.vsix` file is created, open VS Code and click on the extensions view icon on the Sidebar (or press `Ctrl+Shift+X`).

6. Click on the `...` More Actions menu in the top-right corner of the Extensions view, and select `Install from VSIX...`.

7. Locate the `.vsix` file in your filesystem and click `Install`.

That's it! You've installed your VS Code extension from a local directory. Now you should be able to test it and debug it locally. When you make changes, you'll have to repackage and reinstall the extension to test the new version.