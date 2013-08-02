## idgames_sync.pl ##
- See also **https://github.com/spicyjack/App-idGamesSync/issues**
- Split `idgames_sync.pl` into separate files/modules
  - Add a target to the `Makefile` that can recombine all the files back into
    one file
  - Go one step further and compact the file?
    - Not really needed, since the file won't be downloaded?
    - Would it help speed up the parser at all?
- The local copy of the archive is not verified against the ls-laR.gz file, so
  files that are not in the ls-laR.gz file are not deleted from the local host
  (also, see test cases below)
- Using --dry-run only compares ls-laR.gz files, not all of the files in the
  archive
  - Downloaded ls-laR.gz file is not used for comparing the local archive
    unless it's replaced with the --sync switch
- Set taint mode in idgames_sync.pl, and then untaint any environment
  variables as they are used
- paths used in script are UNIX-y paths, need to add support for Windows paths
- build a standalone .exe
  - using Camelbox/PAR/pp
  - Use `App::FatPacker` to build packed archives
- add a --wads-only switch for syncing only wads/levels, no extras, or maybe
  make that the default, and add a switch for syncing everything
  - see `idgas-tools.git` for a sum_all_text_files.sh script, which has a list
    of directories to sync for just WAD files
- Separate the idgames_sync.pl script into individual classes, and write a
  "recombinator" script for combining all of the classes into one script in
  order to make it easier to distribute
- Test cases
  - Deleting of files that are no longer present in the ls-laR.gz file
    - /incoming directory
    - /newstuff directory
- Create a list of files on the local filesystem that will be used to compare
  against files listed in the ls-laR.gz file; this way, you will know if any
  files need to be deleted because they no longer appear in the ls-laR.gz file

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
- Test the script on Windows, test filepaths, and see what breaks (L1735)

## Mayhem API Protocol ##
- Sender sends a message
- Receiver acknowledges the message with 'OK' and the original sender's
  verb/action word if the receiver can handle the message; otherwise, responds
  with 'ERR' and original sender's verb/action word

## Done ##
???/26Oct2011 - Test LWP::UserAgent changes

vim: filetype=markdown shiftwidth=2 tabstop=2
