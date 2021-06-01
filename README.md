# script.database.cleaner

A program add-on for Kodi to help clean out all the streams and other 
unwanted junk that accumulates over time in the video database.

This script will connect to your video database (either MySQL or SQLite),
read the sources you have defined in your sources.xml (all the paths you
have added to Kodi containing your movies/tv-shows) and delete anything
in the 'files' table of the database that isn't in your sources.

The script can optionally back-up your SQLite database (highly recommended)
with a date and time stamp appended.  MySQL users need to manually back-up
their database.

Users can also choose (via the add-on settings) to retain or remove old 
PVR information and to retain or remove bookmarks.  As of V0.5.5 all the different
settings are documented in the file **docs.txt** which is included with the add-on
and can be found in the add-on's directory.

When run, the script will prompt with a summary window containing a list
of the paths to retain in the database. Users are encouraged to check that 
all their defined sources are indeed listed. The current settings for the add-on
are also displayed, along with the contents of any 'excludes.xml' file.
The name of the currently connected database is also displayed.
If you are using MySQL, there will also be a warning to back up your database
manually.  You have the option to either abort the script, or to clean the database. 
Clicking on 'DO IT !!' will run the cleaner, clicking on 'ABORT' will exit the script
with no changes made to the database.

Once users are happy that the script is working correctly, this summary can
be disabled in the add-on settings allowing the script to run silently.

Optionally, a user may choose to delete a specific path from their database or to
rename a path.  These settings can be found in the add-on settings under 'advanced' and
are mutually exclusive.  When enabled, the user must specify the path to be removed or the old path
and the intended new path. This can be useful if you have moved a directory on an HDD.
The addon can update the old path to the new path without having to remove any content and re-scan.
When operating in either of these modes, the add-on will not remove anything from the file table,
but will instead remove or replace the specified path in the path table of the database
and (optionally) call Kodi's built in clean library routine to clean/alter the other tables.
If a user elects to remove or rename a path then once this procedure has been completed, the
relevant setting will be automatically disabled again.  Aborting the addon at the summary page
DOES NOT reset these settings but does produce a logfile as detailed below.

From V0.5.4 a logfile containing details of the paths removed or renamed is created and
can be found in Kodi's 'temp' directory.  The log is called 'database-cleaner.log'.  If the
log already exists, it will be renamed to 'database-cleaner.old.log'.  The script will
write to the log even if you 'abort' the clean.  This allows users to do a 'dry run' to
see exactly what paths are going to be affected in the database before actually
doing so.

When the script cleaning/renaming has finished, the script calls Kodi's built in 'clean
library' routine to clean the other tables in the database.  This can also
be turned off in the settings but this is not recommended.  
*Kodi will sometimes require multiple passes to clean all the paths that it can potentially clean 
(I think this is because of "cascading" effects of removing invalid files, invalid paths and invalid 
parent paths). The option to "Run the builtin 2-6 times" option to have the built-in run repeatedly.

An option was added to the settings dialog in order to allow movie sets with fewer than two movies to be 
removed from the `sets` table. The default behaviour of the add-on (with the aforementioned option turned off)
was modified so as to exclude sets with fewer than one movie (the provided add-on did not perform any sort 
of exclusions on the sets table).

If Kodi's debugging is enabled, the script will write a few things in there.
If the script's debugging is also enabled in the settings, it will be quite
verbose as to what it is doing.

Inclusion of texturecache.py (by Milhouse):
Additional settings in the Video Database Cleaner addon to allow user to run texturecache.py 
after the Video Database Cleaner has completed cleaning the MyMovies.db.

Additional settings allow the user to:
a. Enable or Disable the running of texturecache.py at the end of the Video Database cleanup
b. Configure any of the settings available in the texturecache.cfg file
. If texturecache.py is Enabled, run it, multiple times, each time with one of the following flags:
c, C, lc, p, P, Xd, r, R, qa, qax , duplicates

Deep Clean option:
WARNING - CAN BE VEY SLOW! (it will probe every pysical file/directory as well as all that are listed in the database)
This option manually scans the locations, listed in sources.xml, and builds a list of current, legitimate directories and files. 
It doesn't know or care if directories are removable
media/network storage/mount points and ignores the 'keep bookmarks' function and doesn't work on the 'remove specific path'
operations (only on the general cleaning operations).
Once it has the list of exisiting, legitimate media files, it will compare this to the database and remove any enries that do not correspond.
*This is helpful whne there have been a lot of manually deleted files/folders that the built in Kodi Video cleaner dos not clean up.  
It will help to remove many of the "directory does not exist" erros in the kodi.log file

DISCLAIMER
==========

Whilst every effor has been made to ensure this script works correctly, the
authors take absolutely no responsibility for any damage caused to your database
by it's use in any way.  ALWAYS back up your database before running this script.
