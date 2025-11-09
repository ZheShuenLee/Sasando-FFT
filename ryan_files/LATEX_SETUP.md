# LaTeX Compilation Setup Guide

## Option 1: VS Code/Cursor with LaTeX Workshop Extension (Recommended)

### Step 1: Install LaTeX Distribution

**macOS:**
```bash
# Install MacTeX (full distribution, ~4GB)
brew install --cask mactex

# OR install BasicTeX (minimal, ~100MB) and add packages as needed
brew install --cask basictex

# If using BasicTeX, initialize tlmgr (TeX Live Manager)
sudo tlmgr update --self
```

**Alternative: Install via MacTeX website**
- Download from: https://www.tug.org/mactex/
- This includes all packages and is easier for beginners

### Step 2: Install LaTeX Workshop Extension

1. Open VS Code/Cursor
2. Go to Extensions (Cmd+Shift+X)
3. Search for "LaTeX Workshop"
4. Install by James Yu

### Step 3: Configure Auto-Build (Already Done)

The `.vscode/settings.json` file has been created with:
- **Auto-build on save**: Enabled (`"latex-workshop.latex.autoBuild.run": "onSave"`)
- **PDF viewer**: Opens in tab within editor
- **SyncTeX**: Double-click in PDF to jump to source

### Step 4: Compile Your Document

1. Open `ryan_writeup_v1.tex` in Cursor/VS Code
2. The document will **automatically compile on save**
3. View PDF by clicking the preview button in the LaTeX Workshop sidebar
4. Or use Command Palette (Cmd+Shift+P) → "LaTeX Workshop: View LaTeX PDF"

### Real-Time Compilation

With the current settings:
- **Auto-compile on save**: Every time you save the .tex file, it recompiles
- **PDF updates automatically**: The PDF viewer refreshes with changes
- **SyncTeX enabled**: Click in PDF to jump to corresponding line in source

## Option 2: Overleaf (Online, No Installation)

1. Go to https://www.overleaf.com
2. Create free account
3. New Project → Upload Project
4. Upload `ryan_writeup_v1.tex`
5. Compiles automatically as you type
6. Download PDF when done

**Pros:**
- No installation needed
- Real-time collaboration
- Automatic compilation
- Access from anywhere

**Cons:**
- Requires internet
- File size limits on free tier

## Option 3: Command Line (Terminal)

If you have LaTeX installed:

```bash
cd ryan_files
pdflatex ryan_writeup_v1.tex
# Run twice for references
pdflatex ryan_writeup_v1.tex
```

Output: `ryan_writeup_v1.pdf`

## Troubleshooting

### "pdflatex: command not found"
- Install MacTeX or BasicTeX (see Step 1 above)
- Restart terminal/editor after installation

### Missing Packages
If you get errors about missing packages:
```bash
# With MacTeX/BasicTeX
sudo tlmgr install <package-name>

# Common packages you might need:
sudo tlmgr install enumitem
sudo tlmgr install xcolor
sudo tlmgr install listings
```

### Extension Not Working
1. Check that LaTeX Workshop extension is installed and enabled
2. Reload window: Cmd+Shift+P → "Developer: Reload Window"
3. Check Output panel: View → Output → Select "LaTeX Workshop"

## Quick Start Checklist

- [ ] Install MacTeX or BasicTeX
- [ ] Install LaTeX Workshop extension in Cursor/VS Code
- [ ] Open `ryan_writeup_v1.tex`
- [ ] Save the file (Cmd+S) - should auto-compile
- [ ] View PDF in the editor tab

## Recommended Workflow

1. **Write in .tex file** - Edit `ryan_writeup_v1.tex`
2. **Save (Cmd+S)** - Auto-compiles
3. **View PDF** - Opens in tab, updates automatically
4. **SyncTeX** - Double-click PDF to jump to source line
5. **Iterate** - Make changes, save, see updates instantly

## Additional LaTeX Workshop Features

- **Build LaTeX project**: Cmd+Alt+B
- **View LaTeX PDF**: Cmd+Alt+V
- **Clean up auxiliary files**: Cmd+Alt+C
- **SyncTeX forward**: Cmd+Click in source → jumps to PDF
- **SyncTeX backward**: Double-click in PDF → jumps to source

