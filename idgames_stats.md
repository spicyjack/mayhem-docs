## idgames_stats.pl #

Gather statistics on files in the `idGames Archive`.

- Statistics about what's stored in idGames Archive
- Number of WAD files
- What game they're for, Doom, Doom2, Heretic, Hexen, Strife
- What levels are in each wad
- What textures are used in each wad
- When the WAD was added to the `idGames` archive
  - Use `fullsort.gz` to build a transaction log of database updates
    that show when a file was uploaded to the `idGames` archive

## Questions to ask ##
- How to store data in as small a space as possible?
  - Binary representation?
  - Tokenization

vim: filetype=markdown shiftwidth=2 tabstop=2
