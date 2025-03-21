# Project Notes:
    The project structure is as follows:
    1. Documentation is located in the ExternalDocumentation/ directory.
    2. Build (CMakeLists.txt) are located in the each directory containing compileable code.
    3. Source code is in the following directories:
        a. CommonUtlities/ contains a common set of utilities (used across multiple components).
        b. CPU/ contains all the code that implements the emulation of the Digital Alpha AXP 21264 CPU.  This
           directory contains the following sub-directories:
              i. Caches - contains the Icache and Dcache code
              ii. Cbox - contains the code that interfaces between the rest of the CPU, the Bcache, and System
              iii. Ebox - contains the integer execution code
              iv. Fbox - contains the floating-point execution code
              v. Ibox - contains the instruction processing code, accessing the iCache
              vi. Mbox - contains the dCache processing code
        c. DECaxp/ contains all the code that implements application main program and SROM generation program.
        d. Devices/ contains the code that implements various devices connected to the emulator
        e. Motherboard/ contains the code that implements the system where the CPUs, Memory, and devices are
           connected.
        f. Tests/ contains all the unit testing code.
    4. Data files are located in the DataFiles/ folder.
    5. Binary files from compiling are placed in the build/ folder (note: GIT ignores this folder and its contents)

    When building the project:
    1. Create the build directory
    2. From within the build/ directory, run `cmake ..` prior to trying to build the emulator code.
    3. Run `make`.  The resulting executable code will be locating in the build/Executables directory.

# Coding Rules:
    The code is written entirely in the C language.  The ANSI C compiler option
    should be utilized.  When in doubt, look at existing code for an example of
    how the code should be formatted.

    Coding Style:
        Basics:
            blank lines:    Blank lines should not have any whitespace
                            characters.
            tab length:     Each tab is 4 space characters long and comprised
                            of spaces only (no tabs).
            white space:    There will be no whitespace characters at the end
                            of a line.
            line length:    A line of code should not exceed 80 characters
                            long.  If at all possible, and code clarity is not
                            lost, a line of code which exceeds 80 characters
                            should be broken up into multiple lines.
        Comments:
            module comment: Every module/header file will begin with a
                            copyright comment, also containing a module/header
                            description and revision history.  The first
                            revision history entry will be V01.000 and say
                            "Initially written.".
            line comments:  Only '/* */' can be used for Line comments.  Line
                            comments should either be on their own line or at
                            the and of a line containing code.  If a line
                            comment is on its own line, then the start of the
                            comment is in line with the code underneath.  Also,
                            these line comments will be proceeded by a blank
                            line.
            multiline comments:
                            There will be a blank line before a multiline
                            comment. The comment, itself, will start with '/*',
                            where the '/' is inline with the underlying code.
                            The comment start will be on it's own line.
                            The comment will end with '*/', where the '*' will
                            be inline with the '*' on on the comment start and
                            be ib its own line.
                            Lines in between the start and end comment will
                            a '*', which is inline with the '*' on both the
                            comment start and end.  If an in-between comment
                            does not have any text following it, then the white
                            space rule above shall be followed.  Otherwise, one
                            or more sace/tab characters will follow the '*',
                            prior to the text of the comment.
                            There will be no blank line following a multi-line
                            comment, unless the comment stands along and is not
                            specifically comment in the subsequent code.
                            A variation on the multiline comment mat be used,
                            where the opening comment '/*' be be followed by
                            multiple '*', and inbetweem comments can contain
                            space characters prior to a final '*', and the
                            comment end can be preceeded by mutliple '*', all
                            to form a box.
            function comments:
                            All functions will be proceeded by a multiline
                            comment.  The function comment will have the
                            following format:
``` {.c}
                                /*
                                 * functionName
                                 *  Function description.
                                 *
                                 * Input Parameters:
                                 *  param1:
                                 *      Input Param1 description.
                                 *  paramn:
                                 *      Input Paramn description.
                                 * Output Parameters:
                                 *  paramx:
                                 *      Output Paramx description.
                                 *  paramy:
                                 *      Output Paramy description.
                                 *
                                 * Return Value:
                                 *  None or a list of potential return values
                                 *  and what they mean.
                                 */
```
                            A parameter is this both an input and output
                            parameter may be listed in both areas.
        Files:
            header file:    After the copyright comment. the first two lines
                            should be the following:
``` {.c}
                                #ifndef _HEADER_FILE_NAME_DEFS_
                                #define _HEADER_FILE_NAME_DEFS_
```
                            The last line should be the following (note a tab
                            character between #endif and '/*'):
``` {.c}
                                #endif    /* _HEADER_FILE_NAME_DEFS_ */
```
                            There shall be at least one header file for each
                            module.  A module file of the name foobar.c, shall
                            have a header file of foobar.h.  This header file
                            will include all the files needed by the module.
                            The module will include this header file.
                            For external entry points, either the module header
                            file or a separate header file (with a file name
                            sufficient to determine what the header file
                            contains) will be utilized.
                            A header file will contain no code that is compiled
                            within the header file.
            includes:       All includes are listed at the top of the header
                            and module file.  Conditional includes may be done
                            here as well.
                            A header file should contain all the includes
                            required for it to be successfully compiled (even
                            by itself).
            module file:    A module file will have a similar format to a
                            header file, except it will not contain the
                            '#ifndef ... #endif' lines.
                            A module file can contain everything a header file
                            includes, plus code that will be compiled into an
                            executeable.
                            All global variables (module or beyond), will be
                            declared at the top of the module (after any
                            includes).
                            All functions/routines/procedures will be preceeded
                            by a function comment (see above).
        Code:
            All code will have the following format (note locations of '{' and
            white space):
``` {.c}
            /*
             * functionName
             *  Function description.
             *
             * Input Parameters:
             *  param1:
             *      Input Param1 description.
             *
             * Output Parameters:
             *  None.
             *
             * Return Value:
             *  1:  Item not found.
             *  2:  Close enough item found.
             *  3:  Item found.
             */
            int funct(int param)
            {
                int retVal = 1;
                int ii;

                /*
                 * Loop until param and retVal are not equal.
                 */
                while (param == retVal)
                {

                    /*
                     * Loop through param until we find what we came for.
                     */
                    for (ii = 0; ii < param; ii++)
                    {
                        if (param)
                        {
                            retVal = 2;        // Preliminary value.
                        }
                        else
                        {
                            retVal = 3;        // What we came for.
                            break;            // get out of for loop.
                        }
                    }

                    /*
                     * Retest while condition to see if we are done.
                     */
                    continue;
                }

                /*
                 * Return what we found back to the caller.
                 */
                return(RetVal);
            }
```
