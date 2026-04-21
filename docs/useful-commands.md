# Useful Commands

## Handling files

### Creating a new file
```bash
i3new [-s <i> [i] [i] [i]] [-o <n> [n] [n] [n]] [-val <s>] <S>
```
This command creates a new file with the specified parameters. The options allow you to set various attributes of the file, such as size, output format, and more.
Most important options:
- `-s <i> [i] [i] [i]`: Set the size of the file in each dimension.
- `-fmt <s>`: Specify the format of the file (e.g., `mrc`, `img`).
- `-val <s>`: Set the initial value for the file (e.g., `0` for zeros).

### Applying Math Operations
```bash
i3compute -fmt mrc output_file.mrc = input_file.mrc \* 2 # Mutiply the file by 2, \* is used to escape the * character in bash
i3compute -fmt mrc output_file.mrc = - input_file.mrc # Invert the contrast of the file, useful for opening in chimerax
i3compute -fmt mrc output_file.mrc = input_file.mrc \* input_file2.mrc # Multiply the files element-wise, useful for masking
```

### Masking a File
```bash
i3mask [-minv] [-mw <r> [r] [r]] [-md <r> [r] [r]] [-ms <r> [r] [r]]  file_to_mask.mrc
```
options:
- `-mw <r> [r] [r]`: Set rectangular mask width in each dimension.
- `-md <r> [r] [r]`: Set oval mask diameter in each dimension.
- `-ms <r> [r] [r]`: Set decay radius for soft edge in each dimension.
- `-minv <k>`: Invert masking, useful for masking out a region instead of keeping it.

### Getting Alignment and Classification Info
```bash
i3datalist -cls gem-004-class.i3d gem-004-class-004 > cycle04-004.trf
```
This command extracts the alignment and classification information from the specified `.i3d` file and saves it in a `.trf` file, which can be used for further processing in the protocol.
I have a trf2star code that converts that file into a star file for relion.

---

[← Back: Example Workflow](step-by-step.md)
