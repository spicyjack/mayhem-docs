## idgames_sync.pl ##

- See also **https://github.com/spicyjack/App-idGamesSync/issues**
- Rewrite IDGames Sync using _Moo_ and removing _File::Stat::Ls_
- Write a "recombinator" script for combining all of the classes into one
  script in order to make it easier to distribute
- Set taint mode in idgames_sync.pl, and then untaint any environment
  variables as they are used
- Test cases
  - Deleting of files that are no longer present in the ls-laR.gz file
    - /incoming directory
    - /newstuff directory
- Create a list of files on the local filesystem that will be used to compare
  against files listed in the ls-laR.gz file; this way, you will know if any
  files need to be deleted because they no longer appear in the ls-laR.gz file

## Script options and defaults ##
! = script defaults

Script reports:
- ! files in the tarball but missing on disk
- ! files that have different sizes between the tarball and disk
- ! files that have the same size in both places
- files on disk but not listed in the tarball; note that this mode would
- require scanning the filesystem at some point in order to compare what's
  on disk and not in the tarball
- all of the above reports

Output types:
- simple - one file per line, with status flags in the left hand side
- full - one line per file/directory attribute
- ! more - filename, date/time, size on one line, file attributes on the next
  line

## Object/Class Notes ##
- Role::Dir::Attribs - local/archive directory attributes
- Role::FileDir::Attribs - local/archive file/directory attributes
- Role::LocalFileDir - methods and attributes for interacting with local
  files; exists, local_path
- Archive::File - a file in the archive
- Archive::Directory - a directory in the archive, can contain one or more
  file and/or directory objects
- Local::File - a file on the filesystem
- Local::Directory - a directory on the filesystem, can contain one or more
  file and/or directory objects
- Reporter - writes reports based on the type of report specified by the
  user

## Building a standalone .exe ##
- using Camelbox/PAR/pp
- https://metacpan.org/module/pp
- https://metacpan.org/module/RSCHUPP/PAR-1.007/lib/PAR/Tutorial.pod
- Use `App::FatPacker` to build packed archives
- See `public.git/notes/perl_app_building.md` for `PAR` usage notes


    C:\Users\manningb\src\App-idGamesSync.git\bin>pp --verbose --bundle 
      --module Tie::Hash::NamedCapture --module Pod::Usage --module Pod::Text 
      --output idgames_sync.bundled.exe idgames_sync.pl

    C:\Users\manningb\src\App-idGamesSync.git\bin>pp --verbose 
      --module Tie::Hash::NamedCapture --module Pod::Usage --module Pod::Text 
      --output idgames_sync.exe idgames_sync.pl

## Release Todos ##
- Update copyrights in source files
- update version numbers
- Build packed archives (`App::Fatpacker`, `PAR`)

### Testing ###
- Describe what messages the object will send and receive in the docs (above),
  then write tests that exercise the messages described in the docs
- Try to compare a downloaded ls-laR.gz file when the ls-laR.gz file doesn't
  exist on the local filesystem; the downloaded copy should become the local
  copy, and the rest of the files should be downloaded based on the ls-laR.gz
  file
- Test untainting $ENV{TEMP} (L1726)

## Mayhem API Protocol ##
- Sender sends a message
- Receiver acknowledges the message with 'OK' and the original sender's
  verb/action word if the receiver can handle the message; otherwise, responds
  with 'ERR' and original sender's verb/action word

## Done ##
23Oct2013 - Split `idgames_sync.pl` into separate files/modules
- Add a target to the `Makefile` that can recombine all the files back into
  one file
- Go one step further and compact the file?
  - Not really needed, since the file won't be downloaded?
  - Would it help speed up the parser at all?

12Sep2013 - Deleting files in the local copy of the archive
- The local copy of the archive is not verified against the ls-laR.gz file, so
  files that are not in the ls-laR.gz file are not deleted from the local host
  (also, see test cases below)
  - _Github issue #41_ - Files in `/newstuff` are not deleted when the file is
    moved out of `/newstuff` in `ls-laR.gz`
  - _Github issue #5_ - Add deleting of files off of the local filesystem
    using the `--delete` switch

26Oct2011 - Test LWP::UserAgent changes
- Using --dry-run only compares ls-laR.gz files, not all of the files in the
  archive
  - Downloaded ls-laR.gz file is not used for comparing the local archive
    unless it's replaced with the --sync switch

20Aug2013 - Windows Path issues
- paths used in script are UNIX-y paths, need to add support for Windows paths
  - Also added support for determining user names on Windows

30Aug2013 - (GitHub issue #25) Sync WAD directories only by default 
- add a --wads-only switch for syncing only wads/levels, no extras, or maybe
  make that the default, and add a switch for syncing everything
  - see `idgas-tools.git` for `sum_all_text_files.sh` and `idgas/docs.git`
    `idgas_notes.md` files, which have a list of directories to sync for just
    WAD files

27Aug2013 - (GitHub issue #16) Create standalone binary
- Test the script on Windows, test filepaths, and see what breaks (L1735)

vim: filetype=markdown shiftwidth=2 tabstop=2
