Sure! Here’s the content for macOS.md as a Markdown file, ready for you to save or commit:

# Building on macOS

This guide explains how to build the project on macOS, including important configuration notes and common issues.

---

## Prerequisites

- Xcode and Command Line Tools installed
- Homebrew installed for package management
- Required dependencies installed via Homebrew, e.g.:

  ```bash
  brew install spdlog fmt

	•	Make sure your environment variables include the Homebrew paths:

export CPPFLAGS="-I/opt/homebrew/include"
export LDFLAGS="-L/opt/homebrew/lib"
export PKG_CONFIG_PATH="/opt/homebrew/lib/pkgconfig"

Adjust paths if your Homebrew prefix differs.

⸻

Key Configuration Changes for macOS

1. malloc.h header handling
	•	macOS does not provide <malloc.h>.
	•	All code including <malloc.h> is wrapped with:

#ifdef HAVE_MALLOC_H
#include <malloc.h>
#endif


	•	The configure.ac script sets HAVE_MALLOC_H to 0 (undefined) on macOS, avoiding compilation errors.

2. C++ standard library linkage
	•	The C++ standard library on macOS (Clang) is libc++.
	•	The build system sets HOST_CPP_LIB to -lc++ automatically on macOS to ensure correct linking.

3. Dependencies via Homebrew or Conda
	•	If you installed dependencies like spdlog or fmt with Homebrew or Conda, make sure to export the appropriate environment variables before building:

export CPPFLAGS="-I/opt/homebrew/include"
export LDFLAGS="-L/opt/homebrew/lib"
export PKG_CONFIG_PATH="/opt/homebrew/lib/pkgconfig"


	•	This ensures make and pkg-config find the correct headers and libraries.

⸻

Build Steps
	1.	Run ./configure to generate Makefiles and check system capabilities.
	2.	Build the project:

make


	3.	(Optional) Install the built artifacts:

make install



⸻

Troubleshooting
	•	malloc.h not found error:
Confirm that the source code has #include <malloc.h> guarded by #ifdef HAVE_MALLOC_H. Re-run ./configure to update macros.
	•	spdlog/spdlog.h not found:
Ensure CPPFLAGS includes the directory where spdlog headers reside, e.g.,

export CPPFLAGS="-I/opt/homebrew/include"


	•	Linker errors related to C++ stdlib:
Verify HOST_CPP_LIB is set to -lc++ for macOS. This is handled automatically if the configure script is up to date.

⸻

Summary

This project has been tested to build cleanly on macOS by:
	•	Disabling use of <malloc.h>
	•	Setting appropriate C++ standard library flags
	•	Using environment variables to point to dependency includes and libraries

If you encounter any build issues, please check that your environment variables and dependencies are correctly set, and rerun ./configure.

⸻

Last updated: 2025-07-11